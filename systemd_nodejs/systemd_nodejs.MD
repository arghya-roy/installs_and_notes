```
vi /etc/systemd/system/anyname.service
```
```
[Unit]
Description=HTTP Hello World
After=network.target
[Service]
User=ec2-user
Environment=NODE_ENV=production
WorkingDirectory=/path/where/name.js/located
ExecStart=/usr/bin/node name.js
[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
```
```
systemctl start index.service
```
