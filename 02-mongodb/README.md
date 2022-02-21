# 🦍 MongoDB

## A straightforward configuration to get Graylog up and ready

Graylog works with 4.x MongoDB instances. Don't try using newest versions of MongoDB. It might not break your instance, but you're not sure what will happen.

Our Graylog and MongoDB hosts are called

- graymongo01.nerd.mooc
- graymongo02.nerd.mooc
- graymongo03.nerd.mooc

## 🍍 Install

> ⚠️ On the three Nodes !

```shell
apt install -y gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" > /etc/apt/sources.list.d/mongodb-org-4.4.list
apt update && apt install -y mongodb-org
systemctl enable mongod && systemctl start mongod
systemctl status mongod -l --no-pager
```
## 🥨 Configuration
> ⚠️ On the three Nodes !

**Before we go any further, let's save the main configuration file.**
```shell
mv /etc/mongod.conf /etc/mongod.conf.bak
```

**Let's now edit `/etc/mongod.conf` with your favorite text editor and paste this configuration file inside.**
Dont forget to change the hostname at line 23 !

### 🌻 Replica Shard
> ⚠️ On ONE node !

**Connect to MongoDB**

```shell
mongo
```

**This node will become master when the cluster first starts.**
```sql
rs.initiate(
{
   _id : "rs01",
   members: [
      { _id: 0, host: "graymongo1:27017" },
      { _id: 1, host: "graymongo2:27017" },
      { _id: 2, host: "graymongo3:27017" }
   ]
}
)
```

### 🌼 RBAC

**Connect to MongoDB**
```shell
mongo
```

**Change to the *admin* database**
```sql
use admin
```

**Create a *user admin* which will be granted all permissions an all databases**

```sql
db.createUser(
{
   user: "UserAdminNIAM",
   pwd: passwordPrompt(),
   roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
}
)
```

**Create a *cluster admin* which will be granted all permissions an the cluster**

```sql
db.createUser(
{
   user: "ClusterAdminNIAM",
   pwd: passwordPrompt(),
   roles: [ { role: "clusterAdmin", db: "admin" } ]
}
)
```

**Create a *graylog-user* which will be use to connect graylog to the MongoDB replica Shard**
`use graylog-database`

```sql
db.createUser(
{
   user: "graylog-user",
   pwd: passwordPrompt(),
   roles: [
      { role: "readWrite", db: "graylog-database" },
      { role: "dbAdmin", db: "graylog-database" } ]
}
)
```

## 🌰 Secure

Let's now create a keyfile that the nodes will use as an authentication system. This is a password shared only by the nodes in the MongoDB cluster, allowing cluster members to authenticate themselves. It is also possible to use an internal CA to generate x.509 certificates or to use "real" certificates. This higher form of security is more than desirable for a cluster using an unsecured network. 

> ⚠️ On ONE node !

**Create a directory to add the key and then generate the key.**

```shell
mkdir -p /var/lib/graylog-database/mongo-security/
openssl rand -base64 768 > /var/lib/graylog-database/mongo-security/mongo-key
```

Paste that key in the same directory on the other machines, considering you did this on `graymongo1.nerd.mooc`.

**Create the same directory then paste the key and then chmod/chown it !**
```shell
ssh root@graymongo2.nerd.mooc "mkdir -p /var/lib/graylog-database/mongo-security/" &&
scp /var/lib/graylog-database/mongo-security/mongo-key root@graymongo2.nerd.mooc:/var/lib/graylog-database/mongo-security/mongo-key &&
ssh root@graymongo3.nerd.mooc "chown -R mongodb:mongodb /var/lib/graylog-database/mongo-security/ && chmod -R 700 /var/lib/graylog-database/mongo-security/"
```
```shell
ssh root@graymongo3.nerd.mooc "mkdir -p /var/lib/graylog-database/mongo-security/" &&
scp /var/lib/graylog-database/mongo-security/mongo-key root@graymongo3.nerd.mooc:/var/lib/graylog-database/mongo-security/mongo-key &&
ssh root@graymongo3.nerd.mooc "chown -R mongodb:mongodb /var/lib/graylog-database/mongo-security/ && chmod -R 700 /var/lib/graylog-database/mongo-security/"
```

> ⚠️ On the three nodes !

Edit the configuration file to allow secure authentication by uncommneting lines 29 to 31. `Transition to Auth : true` lets us test our configuration by allowing both secured / unsecured communication between nodes.

```yml
security:
  keyFile: /var/lib/graylog-database/mongo-security/mongo-key
  transitionToAuth: true
```

> ⚠️ On the SECONDARY nodes !

```shell
systemctl restart mongod.service
```

> ⚠️ On the PRIMARY node !

**Connect to MongoDB**
```shell
mongo
```

**Then switch this node to secodary**
```sql
rs.stepdown()
```

**Restart the service**
```shell
systemctl restart mongod.service
```

Then on all nodes, comment the `transitionToAuth: true` to disable unsecured connections.

**Edit `/etc/mongod.conf`**
```yml
security:
  keyFile: /var/lib/graylog-database/mongo-security/mongo-key
# transitionToAuth: true
```

**Restart all the nodes**
```shell
systemctl restart mongod.service
```
and you're good !
