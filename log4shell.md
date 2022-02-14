# ⚠️ Update against log4shell

## Graylog

```shell
apt update && apt -y install graylog-server
```

🐈‍ Do not overwrite (select N). You have custom configurations you need to preserve. If you overwrite you risk taking down your Graylog instance altogether!

## Elasticsearch

🐦 If Elasticsearch version >= 6.x you're all good.

If not :

``` shell
echo "-Dlog4j2.formatMsgNoLookups=true" /etc/elasticsearch/jvm.options
```
