# 🐌 Graylog

The configuration of Graylog is build in one huge big monolithic file. Instead of using a premade file, one should make a backup of the original one and work on a copy.

## 🌲 Install

**First, you will need those packets**
```shell
apt install -y dirmngr openjdk-11-jre-headless pwgen uuid-runtime
```

**Then let's go and install Graylog !**
```shell
wget https://packages.graylog2.org/repo/packages/graylog-4.2-repository_latest.deb
dpkg -i graylog-4.2-repository_latest.deb
apt update && apt install -y graylog-server graylog-integrations-plugins 
rm dpkg graylog-4.2-repository_latest.deb
```

## 🌵 Configuration

**Let's save the main configuration file before breaking anything.**
```shell
cp /etc/graylog/server/server.conf /etc/graylog/server/server.conf.bak
```

**Edit the configuration file *before* starting the service**

Graylog NEEDS some tweaks before starting.

> ⚠️ On ONE node !

### 🌸 Master Node - *Line 47*
```yml
is_master = true
```
**One node will be set to master.**

*Type this on other nodes.*
```yml
is_master = false
```

### 🏵️ Password secret - *Line 57*
```yml
password_secret = 
```

You must set a password here to secure the stored password. 

**Let's do that by typing the following command..**
```shell
pwgen -N 1 -s 96
```
**..and paste the output at line 57 of the configuration file *on all nodes* ⚠️ ! This way, all of the nodes will share the same.**

### 🌺 Root Password - *Line 68*
```yml
root_password = 
```
This is the hash of the global admin password.

**Type this command :**
```shell
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```
**Then enter your root password when prompted.** The output will be the hash that you will need to copy on the three nodes ⚠️ !

### 🌻 Timezone - *Line 76*
```yml
root_timezone = Europ/Paris
```
This is the timezone I currently live in.

### 🌼 Bind Address - *Line 105*
```yml
http_bind_address = ip.of.the.node
```
If you have several NIC, you should use one of them to bind the graylog server.

### 🌷 Elasticserach hosts - *Line 190*
```yml
elasticsearch_hosts = http://elastic1.nerd.mooc:9200,http://elastic2.nerd.mooc:9200,http://elastic3.nerd.mooc:9200, 
```
**This is the list of our elasticsearch instances.**

### 🌱 MongoDB URI - *Line 550*
```yml
mongodb_uri = mongodb://graylog-user:password@graymongo1:27017,graymongo2:27017,graymongo3:27017/graylog?replicaSet=rs01
```

**This is the list of our MongoDB instances.**

## 🍞 Start the server
```shell
systemctl enable graylog-server
systemctl start graylog-server
systemctl status graylog-server
```
