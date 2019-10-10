---
permalink: /index.html
---
# [Home](https://div-oops.github.io/mugivar)  [Installers](https://div-oops.github.io/mugivar/installers)  
[SQL](https://div-oops.github.io/mugivar/SQL/)


```
если вдруг кто-то встретит ошибку function public.mamonsu_count_wal_files() does not exist, то чинить надо вот так:
CREATE OR REPLACE FUNCTION public.mamonsu_count_wal_files()
RETURNS BIGINT AS $$
WITH list(filename) as (SELECT * FROM pg_catalog.pg_ls_dir('pg_wal'))
SELECT
    COUNT(*)::BIGINT
FROM
    list
WHERE filename not like 'archive_status'
$$ LANGUAGE SQL SECURITY DEFINER;
```
**nmap**
```yml

Доступные ip в подсети
nmap -v -sn -n 172.18.200.0/24 -oG -

```
**ports**
```yml

netstat -tlnp

```
**Установленные пакеты**
```yml

rpm -qa|grep <packadge_name>

```
**rsync**
```yml

rsync --archive --verbose --progress <имя пользователя>@<ip адрес>:<путь к папке или файлу на серваке> <путь к файлу или папке на твоём серваке>

```
**scp**
```yml

$ scp user@remote.host:file.txt /some/local/directory
$ scp file.txt user@remote.host:/some/remote/directory

```
**ssh web**
```yml

sudo vim ~/.bash_profile 

grafana_server()
{
        ssh p-pvk-perm-backup -L 3000:172.18.4.10:3000
}

source ~/.bash_profile
$ grafana_server
localhost:3000

```
**nginx subPath kibana or grafana**
```yml

/etc/nginx/conf.d/name.conf

server {
  listen 80;
  server_name monitoring.ru;
  root /usr/share/nginx/www;
  index index.html index.htm;

  location /sed/grafana/ {
        allow   85.26.168.125;
#       deny    all;
        proxy_pass http://localhost:3000/;
  }

  location /sed/kibana/ {
        proxy_pass http://localhost:5601/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        rewrite ^/awesome/(.*)$ /$1 break;
  }
}

/etc/kibana/kibana.yml

server.basePath: "/sed/kibana"

/etc/grafana/grafana.ini

protocol = http
domain = monitoring.ru
root_url = %(protocol)s://%(domain)s/sed/grafana/

```
**Ansible vars**
```yml

CPU: ansible_processor_vcpus
RAM: ansible_memtotal_mb
{{ var (+, -, *, /) number }}
```
**Монтирование диска и создание разделов**
```yml

fdisk /dev/sdb
pvcreate /dev/sdb1
vgcreate monitoring /dev/sdb1
lvcreate —name lv_monitoring   -l +100%FREE monitoring
mkfs.xfs /dev/mapper/monitoring-lv_monitoring   
mkdir /mnt/monitoring
#/etc/fstab
/dev/mapper/monitoring-lv_monitoring            /mnt/monitoring         xfs     defaults        0 0

```
