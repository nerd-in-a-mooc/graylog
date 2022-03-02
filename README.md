# 🦮 Graylog is awesome...

But you might need a little help to get use to it, like I did. I use Graylog on a day to day basis. So I try to gather informations that I would have like to find when I first get started with it.
It can get confusing to build a FOSS SIEM solution from scratch, even more if you are not familiar with Elasticsearch and MongoDB (both needed). 
This repository is a constent WIP and tries to be a decent place to start for beginners.

## 🥗 As we said 

Graylog is based on three softwares :

- Elasticsearch
- MongoDB
- Graylog (duh)

## 🍅 What you'll find here

You will find good configurations files and explanation that will walk you through the set up, the configuration and the daily use of Graylog.

You will either be able to start with a fully **functional cluster** made of

- 1 NGINX Loadbalancer
- 3 Graylog/MongoDB nodes
- 3 Elasticseaerch nodes 

**Or a smaller one** that you will easily be able to scale out

- 1 NGINX Loadbalancer
- 1 Graylog/MongoDB node
- 1 Elasticsearch node

## 🧅 Proceed in that order to get everything running
1. [Elasticsearch](01-elasticsearch/readme.md)
2. [MongoDB](02-mongodb/README.md)
3. [Graylog Server](03-graylog-server/README.md)
4. [NGINX](04-nginx/readme.md)
5. [Agents](05-agents/README.md)
6. [Pipeline Rules](https://github.com/nerd-in-a-mooc/graylog/tree/main/06-pipelines-rules#readme)
7. [GeoIP](07%20-%20geoip/README.md)
8. [ContentPack]
9. [Notification]
