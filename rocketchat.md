# RocketChat

**/etc/systemd/system/rocketchat.service:**

```
[Unit]
Description=RocketChat
After=network.target

[Service]
Type=simple
ExecStart=/opt/rocketchat/start.sh
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=rocketchat

[Install]
WantedBy=multi-user.target
```
