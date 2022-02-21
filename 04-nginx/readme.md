# 🦑 NGINX

We will use NGINX as a loadbalancer both for log inputs and HTTP connections. It will also be our SSL termination point.

## 🥭 Install

**The installation is pretty straightforward.**
```
apt -y install nginx
```

## 🍎 Configuration

**We will basically need two configuration files.**

- One main configuration for log inputs.
- Another one for the web GUI.

**You will find three fully working configuration files :**

- [nginx.conf](nginx.conf) were you'll both tls/plain inputs.
- [graylog-https.conf](graylog-https.conf) to work with TLS.
- [graylog-http.conf](graylog-http.conf) to work without TLS (shame).