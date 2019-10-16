~/.bash_profile
```
monitoring()
{
        ssh remote_server -L 3001:172.18.4.10:80
}
```
/etc/kibana/kibana.yml 
```
...
server.basePath: "/kibana"
...
```
 vim /etc/grafana/grafana.ini
```
...
[server]
protocol = http
http_port = 3000
root_url = %(protocol)s://local_ip_add/grafana/
...
```
/etc/nginx/conf.d/monitoring.conf
```
server {
  listen 80 default_server;
#  server_name monitoring.ru _;
  root /usr/share/nginx/www;
  index index.html index.htm;

  location /grafana/ {
        proxy_pass http://localhost:3000/;
  }
  
  location /kibana/ {
        proxy_pass http://localhost:5601/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        rewrite ^/awesome/(.*)$ /$1 break;
  }
}

```
