#Crontab for any users

Debian:
```shell
cd /var/spool/cron/crontabs/ && grep -v '^#'  -r 
```

Citrix XenServer:
```shell
cd /var/spool/cron/ && grep -v '^#'  -r 
```
