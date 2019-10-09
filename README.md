### Полезные ссылки
## SQL:
[PostgreSQL: справочник по командам psql, pg_dump, pg_restore](https://proft.me/2013/06/9/postgresql-spravochnik-po-komandam-psql-pg_dump/)
[Полезные команды MySQL](http://gentooway.ru/2009/11/poleznye-komandy-mysql#comments)
[mysql: полезные команды и настройки](https://proft.me/2011/07/19/mysql-poleznye-komandy-i-nastrojki/)
### Шпаргалки
**nmap**
```
Доступные ip в подсети
nmap -v -sn -n 172.18.200.0/24 -oG -
```
**ports**
```
netstat -tlnp
```
**Установленные пакеты**
```
rpm -qa|grep <packadge_name>
```
**rsync**
```
rsync --archive --verbose --progress <имя пользователя>@<ip адрес>:<путь к папке или файлу на серваке> <путь к файлу или папке на твоём серваке>
```
**scp**
```
$ scp user@remote.host:file.txt /some/local/directory
$ scp file.txt user@remote.host:/some/remote/directory
```
**ssh web**
```
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
```
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
```
CPU: ansible_processor_vcpus
RAM: ansible_memtotal_mb
{{ var (+, -, *, /) number }}
```
**Монтирование диска и создание разделов**
```
fdisk /dev/sdb
pvcreate /dev/sdb1
vgcreate monitoring /dev/sdb1
lvcreate —name lv_monitoring   -l +100%FREE monitoring
mkfs.xfs /dev/mapper/monitoring-lv_monitoring   
mkdir /mnt/monitoring
#/etc/fstab
/dev/mapper/monitoring-lv_monitoring            /mnt/monitoring         xfs     defaults        0 0
```
