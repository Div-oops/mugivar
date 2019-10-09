# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
```
sudo yum install logstash-7.1.1
```
```
sudo systemctl enable logstash.service
```

```
etc/logstash/conf.d/input.conf

input {
  beats {
    port => 5044
  }
}

/etc/logstash/conf.d/output.conf

output {
        elasticsearch {
            hosts    => "localhost:9200"
            index    => "nginx-%{+YYYY.MM.dd}"
        }
	#stdout { codec => rubydebug }
}

/etc/logstash/conf.d/filter.conf

filter {
 if [type] == "nginx_access" {
    grok {
        match => { "message" => "%{IPORHOST:remote_ip} - %{DATA:user} \[%{HTTPDATE:access_time}\] \"%{WORD:http_method} %{DATA:url} HTTP/%{NUMBER:http_version}\" %{NUMBER:response_code} %{NUMBER:body_sent_bytes} \"%{DATA:referrer}\" \"%{DATA:agent}\"" }
    }
  }
  date {
        match => [ "timestamp" , "dd/MMM/YYYY:HH:mm:ss Z" ]
  }
}

```