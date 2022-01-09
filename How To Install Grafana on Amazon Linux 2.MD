 ```
sudo tee /etc/yum.repos.d/grafana.repo<<EOF
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
EOF
```

```
 cat /etc/yum.repos.d/grafana.repo
```

```
sudo yum repolist
```
```
sudo yum install -y grafana
```
```
rpm -qi grafana
```
```
sudo systemctl enable --now grafana-server
```
```
systemctl status grafana-server.service
```

```
<punlic-ip><3000>
#username = admin password = admin
```
## If you cant access
```
sudo firewall-cmd --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
```



