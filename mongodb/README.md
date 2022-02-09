# MongoDB : A straightforward configuration to get Graylog up and ready

> :warning: On the three Nodes !

### Install

```
apt install -y gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" > /etc/apt/sources.list.d/mongodb-org-4.4.list
apt update && apt install -y mongodb-org
systemctl enable mongod && systemctl start mongod
systemctl status mongod -l --no-pager
```
### Configuration

Before we go any further, let's save the main configuration file.
```
mv /etc/mongod.conf /etc/mongod.conf.bak
```

Let's now edit `/etc/mongod.conf` with your favorite text editor and paste this configuration file inside.
Dont forget to change the hostname at line 23.

