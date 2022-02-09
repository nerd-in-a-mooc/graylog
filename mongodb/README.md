# MongoDB : A straightforward configuration to get Graylog up and ready

Our Graylog and MongoDB hosts are called 
- graymongo1.nerd.mooc
- graymongo2.nerd.mooc
- graymongo3.nerd.mooc

## Install
> :warning: On the three Nodes !

```
apt install -y gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" > /etc/apt/sources.list.d/mongodb-org-4.4.list
apt update && apt install -y mongodb-org
systemctl enable mongod && systemctl start mongod
systemctl status mongod -l --no-pager
```
## Configuration
> :warning: On the three Nodes !

Before we go any further, let's save the main configuration file.
```
mv /etc/mongod.conf /etc/mongod.conf.bak
```

Let's now edit `/etc/mongod.conf` with your favorite text editor and paste this configuration file inside.
Dont forget to change the hostname at line 23.

## Replica Shard
> :warning: On ONE node !

Connect to MongoDB

`mongo`

This node will become master when the cluster first starts.
```
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

## RBAC

`mongo`
`use admin`

Create a *user admin* which will be granted all permissions an all databases
```
db.createUser(
{
   user: "UserAdminNIAM",
   pwd: passwordPrompt(),
   roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
}
)
```

Create a *cluster admin* which will be granted all permissions an the cluster
```
db.createUser(
{
   user: "ClusterAdminNIAM",
   pwd: passwordPrompt(),
   roles: [ { role: "clusterAdmin", db: "admin" } ]
}
)
```

Create a *graylog-user* which will be use to connect graylog to the MongoDB replica Shard
`use graylog-database`
```
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
