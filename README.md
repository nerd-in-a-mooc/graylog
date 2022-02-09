# Graylog is awesome...

But you might need a little help, like I did. I use Graylog on a day to day basis. So I try to gather informations that I would have like to have when I first get started with it.
It can get confusing to build a FOSS SIEM solution from scratch, even more if you don't know how Elasticsearch or MongoDB work. This repo is a constent WIP and tries to be a decent place to start for beginners.

## As we said 

Graylog is based on three softwares :

- Elasticsearch
- MongoDB
- Graylog (duh)

## What you'll find here

You will find good configurations files and explanation that will walk you through those.

You will either be able to start with a fully **functionnal cluster** made of

- 1 NGINX Loadbalancer
- 3 Graylog/MongoDB nodes
- 3 Elasticseaerch nodes 

**Or a smaller one** that you will easily be able to scale out

- 1 NGINX Loadbalancer
- 1 Graylog/MongoDB node
- 1 Elasticsearch node
