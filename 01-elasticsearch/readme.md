# 🦦 Elasticsearch


Elasticsearch is a NoSQL database search engine developed in Java, which Graylog depends on. 

For a production environment, one should use a dedicated node / cluster.

It's possible to start with a one node cluster and then and more nodes along the way.


## 🥑 Installation


**Add the keys, the source list, then update and install Elasticsearch.**
> ⚠️ Be careful : Graylog only works with OSS version !
```shell
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -

echo "deb https://artifacts.elastic.co/packages/oss-7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list

apt update && apt install -y elasticsearch-oss
```


## 🥦 Configuration

**Before we break anything, let's save the configuration file.**

```shell
mv /etc/elasticsearch/elasticsearch.yml /etc/elasticsearch/elasticsearch.yml.bak
```

Then edit this file with your favorite text editor...

`/etc/elasticsearch/elasticsearch.yml`

...And copy [this configuration file](elasticsearch.yml).

Have a look at [Bootstrapping a cluster](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-discovery-bootstrap-cluster.html) from Elasticsearch documentation.

## 🧇 Fine tuning

### 🍂 JVM heap Size
Graylog recommends not using more than 50% of the available RAM. 

Elasticsearch also tells us to set the minimum and maximum heap space to the same value.

Let's edit `/etc/elasticsearch/jvm.options` and set these two values. For a 16 GB machine, settings would be :

```shell
-Xms8g
-Xmx8g
```

### 🍁 SWAP Memory

According to Elasticsearch : 

> Swapping is very bad for performance, for node stability, and should be avoided at all costs. It can cause garbage collections to last for minutes instead of milliseconds and can cause nodes to respond slowly or even to disconnect from the cluster. In a resilient distributed system, it’s more effective to let the operating system kill the node.

So let's turn off the swap :

```shell
swapoff -a
```

## 🥜 Start the service

Load, enable and start the service.

```shell
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
systemctl status elasticsearch.service -l --no-pager
```

## 🥯 Check the service

**By typing this command...**
```shell
curl -X GET "localhost:9200/?pretty"
```

**... You should get an output similar to this !**
```json
{
  "name" : "Graylog-NIAM",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "uK0IYMAqQOyUXCr8Ld92YQ",
  "version" : {
    "number" : "7.10.2",
    "build_flavor" : "oss",
    "build_type" : "deb",
    "build_hash" : "747e1cc71def077253878a59143c1f785afa92b9",
    "build_date" : "2021-01-13T00:42:12.435326Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```