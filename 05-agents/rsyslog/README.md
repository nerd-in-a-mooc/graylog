# 🐂 RSYSLOG

Rsyslog is the go-to tool for logging in Debian. This is fairly easy to use - TLS can be painful though.
You can explore the main configuration file to define what you want to log.
Default configuration is a very good starting point to begin with.

Feel free to try these configurations.

> ☣️ These configuration use the so called *rainer script* which supersede the old format. The old format `@.@ xxx.xxx.xxx.xxx` is was already considered as obsolete 10 years ago.

**Remember to restart the service when you are done.**
```shell
systemctl restart rsyslogd.service
```

## 🍐 RSYSLOG/TLS
This considerd as the best practice. Logs will be encrypted on the network and will be decrypted by NGINX.

**You need to install this**
```shell
apt -y install rsyslog-gnutls
```

**Then you need to create a configuration under**
```shell
/etc/rsyslog.d/whateveruwant-tls.conf
```

**And paste [this configuration](rsyslog-tls.conf) if you like it.**

## 🍏 RSYSLOG/TCP
If you don't have certificate or if you don't want to use TLS, you should use TCP.

**You need to create a configuration under**
```shell
/etc/rsyslog.d/whateveruwant-tcp.conf
```
**And paste [this configuration](rsyslog-tcp.conf) if you like it.**

## 🥝 RSYSLOG/UDP
If for some odd reason you cannot either use TLS or TCP, you can use UDP. This is not recommanded.

**You need to create a configuration under**
```shell
/etc/rsyslog.d/whateveruwant-udp.conf
```
**And paste [this configuration](rsyslog-tcp.conf) if you like it.**
