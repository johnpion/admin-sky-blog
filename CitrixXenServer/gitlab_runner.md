# Gitlab-Runner

## Lets Encrypt SSL

```
wget https://letsencrypt.org/certs/isrgrootx1.pem -O /etc/pki/ca-trust/source/anchors/isrgrootx1.crt
update-ca-trust enable
update-ca-trust extract
```
