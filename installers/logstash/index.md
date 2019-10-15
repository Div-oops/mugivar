# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
```
sudo yum install logstash-7.1.1
```
```
sudo systemctl enable logstash.service
```

```
etc/logstash/conf.d/nginx_access.conf

input {
    beats {
        port => "5044" } }

filter {
    grok {
        match => [ "message" , "%{COMBINEDAPACHELOG}+%{GREEDYDATA:extra_fields}"]
        overwrite => [ "message" ]
    }
    mutate {
        convert => ["response", "integer"]
        convert => ["bytes", "integer"]
        convert => ["responsetime", "float"]
    }
    geoip {
        source => "clientip"
        target => "geoip"
        add_tag => [ "nginx-geoip" ]
    }
    date {
        match => [ "timestamp" , "dd/MMM/YYYY:HH:mm:ss Z" ]
        remove_field => [ "timestamp" ]
    }
    useragent {
        source => "agent"
    }
}

output {
    elasticsearch {
        hosts     => "localhost:9200"
        index    => "nginx-access-%{+YYYY.MM.dd}"
    }
}
#    stdout { codec => rubydebug }}
```
