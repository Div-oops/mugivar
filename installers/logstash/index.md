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
        match => {
        "message" => [
                "%{IPORHOST:[nginx_json][remote_ip]} - %{DATA:[nginx_json][user_name]} \[%{HTTPDATE:[nginx_json][access_time]}\] \"%{DATA:[nginx_json][request]}\" %{NUMBER:[nginx_json][status]} %{NUMBER:[nginx_json][body_bytes_sent]} \"%{DATA:[nginx_json][http_referrer]}\" \"%{DATA:[nginx_json][http_user_agent]}\"",
                "%{IPORHOST} - %{DATA} \[%{HTTPDATE}\] \"%{WORD:[nginx_json][request_method]} %{DATA:[nginx_json][url]} HTTP/%{NUMBER:[nginx_json][http_version]}\" %{NUMBER} %{NUMBER} \"%{WORD}://%{DATA:[nginx_json][host]}\/%{DATA}\" \"%{DATA} \(%{DATA:[nginx_json][http_user_agent_parsed][os]} %{DATA}\) %{DATA}\) %{DATA:[nginx_json][http_user_agent_parsed][name]}\/%{DATA:[nginx_json][http_user_agent_parsed][major]}\.%{DATA:[nginx_json][http_user_agent_parsed][minor]}\.%{DATA:[nginx_json][http_user_agent_parsed][path]}\.%{DATA}"
        ]
        }
        remove_tag => [ "_grokparsefailure" ]
        add_tag => [ "nginx_access" ]
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
        source => "user_agent"
    }
}

output {
    elasticsearch {
        hosts     => "172.18.200.230:9200"
        index    => "nginx-access-%{+YYYY.MM.dd}"
    }
} 
#    stdout { codec => rubydebug }}

```
ERROR
[ERROR] 2019-10-17 12:34:01.891 [[main]>worker3] useragent - Uknown error while parsing user agent data {:exception=>#<TypeError: cannot convert instance of class org.jruby.RubyHash to class java.lang.String>, :field=>"agent", :event=>#<LogStash::Event:0x57cae72d>}

РЕШЕНИЕ
```
    useragent {
        source => "user_agent"
    }
```
