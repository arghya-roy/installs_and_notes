```
sudo wget https://github.com/prometheus/prometheus/releases/download/v2.32.1/prometheus-2.32.1.linux-amd64.tar.gz
``` 
``` 
tar xvfz prometheus-*.tar.gz
``` 
``` 
cd prometheus-*
``` 
# By default prometheus.yml has only configured to monitor own server but we can add more target later
``` 
./prometheus --config.file=prometheus.yml
```
If it showing that this port is already in use
 ```
 lsof -i :9090
 sudo kill -9 <pid-it will be shown after running previous command>
 ```
```
<publicip>:<9090>
```

# To add another target group in prometheus.yml file -
```
- targets: ['192.168.x.x:9100','192.168.x.y:9100']
```
