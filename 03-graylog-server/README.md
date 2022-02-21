# 🐌 Graylog

  * [🌲 Install](#---install)
  * [🌵 Configuration](#---configuration)
    + [🌸 Master Node - *Line 47*](#---master-node----line-47-)
    + [🏵️ Password secret - *Line 57*](#----password-secret----line-57-)
    + [🌺 Root Password - *Line 68*](#---root-password----line-68-)
    + [🌻 Timezone - *Line 76*](#---timezone----line-76-)
    + [🌼 Bind Address - *Line 105*](#---bind-address----line-105-)
    + [🌷 Elasticserach hosts - *Line 190*](#---elasticserach-hosts----line-190-)
    + [🌱 MongoDB URI - *Line 550*](#---mongodb-uri----line-550-)
  * [🍞 Start the server](#---start-the-server)

Graylog store its configuration in one big huge monolithic configuration file.

Instead of using a premade file, one should make a copy of the original configuration file an then work on the copy.

We will walk you through this.


## 🌲 Install


**First, you will need those packets :**

```shell
apt install -y dirmngr openjdk-11-jre-headless pwgen uuid-runtime
```

- [dirmngr](https://packages.debian.org/en/sid/dirmngr)
- [openjdk-11-jre-headless](https://packages.debian.org/bullseye/openjdk-11-jre-headless)
- [pwgen](https://packages.debian.org/bullseye/pwgen)
- [uuid-runtime](https://packages.debian.org/bullseye/uuid-runtime)

**Then let's go and install Graylog !**

```shell
wget https://packages.graylog2.org/repo/packages/graylog-4.2-repository_latest.deb
dpkg -i graylog-4.2-repository_latest.deb
apt update && apt install -y graylog-server graylog-integrations-plugins 
rm dpkg graylog-4.2-repository_latest.deb
```

## 🌵 Configuration


**Let's save the main configuration file before breaking anything :**

```shell
cp /etc/graylog/server/server.conf /etc/graylog/server/server.conf.bak
```

**Edit the configuration file *before* starting the service**

### 🌸 Master Node - *Line 47*

> ⚠️ On ONE node !

**One node will be set to master.**

```yml
is_master = true
```

> ⚠️ On the other nodes !

```yml
is_master = false
```

### 🏵️ Password secret - *Line 57*
> ⚠️ On ONE node !

**You must set a password here to secure the stored password.**

```yml
password_secret = 
```

Let's do that by typing the following command..

```shell
pwgen -N 1 -s 96
```

..and paste the output at line 57 of the configuration file *on the other nodes* ⚠️ ! This way, all of the nodes will share the same.

### 🌺 Root Password - *Line 68*
```yml
root_password = 
```
**The hash of the global admin password.**

```shell
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```
**Then enter your root password when prompted.** 

The output will be the hash that you will need to copy on *all nodes* ⚠️ !

### 🌻 Timezone - *Line 76*
This is the timezone I currently live in.
```yml
root_timezone = Europe/Paris
```


### 🌼 Bind Address - *Line 105*
On which interface do you want Graylog to be accessible.
```yml
http_bind_address = ip.of.the.node
```
If you have several NIC, you should use one of them to bind the graylog server. If you set `0.0.0.0`, Graylog server should be joined all all of the interfaces available on this machine.

### 🌷 Elasticserach hosts - *Line 190*
**This is the list of our elasticsearch instances.**
```yml
elasticsearch_hosts = http://elastic1.nerd.mooc:9200,http://elastic2.nerd.mooc:9200,http://elastic3.nerd.mooc:9200, 
```


### 🌱 MongoDB URI - *Line 550*
**This is the list of our MongoDB instances.**
```yml
mongodb_uri = mongodb://graylog-user:password@graymongo1:27017,graymongo2:27017,graymongo3:27017/graylog?replicaSet=rs01
```



## 🍞 Start the server
```shell
systemctl enable graylog-server
systemctl start graylog-server
systemctl status graylog-server
```

**And you should be set !**