# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
```
vim /etc/yum.repos.d/grafana.repo

[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```
```
sudo yum install grafana
```
```
sudo systemctl start grafana-server.service 
sudo systemctl enable grafana-server.service 

```