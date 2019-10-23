# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
Debager grok https://grokdebug.herokuapp.com/
Grok https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns
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
        break_on_match => false
        match => {
        "message" => [
                "%{IPORHOST:[nginx_json][remote_addr]} %{DATA:[nginx_json][content_type]} %{DATA:[nginx_json][forwarded_for]} \[%{HTTPDATE:[nginx_json][timestamp]}\] \"%{DATA:[nginx_json][request]}\" %{NUMBER:[nginx_json][status]} %{NUMBER:[nginx_json][body_bytes_sent]} \"%{DATA:[nginx_json][http_referrer]}\" \"%{DATA:[nginx_json][http_user_agent]}\"",
                "%{IPORHOST} - %{DATA} \[%{HTTPDATE}\] \"%{WORD:[nginx_json][request_method]} %{DATA:[nginx_json][url]} %{DATA:[nginx_json][http_version]}\" %{NUMBER:[nginx_json][upstream_status]} %{NUMBER} \"%{WORD}://%{DATA:[nginx_json][host]}\/%{DATA}\" \"%{DATA} \(%{DATA:[nginx_json][http_user_agent_parsed][os]} %{DATA}\) %{DATA}\) %{DATA:[nginx_json][http_user_agent_parsed][name]}\/%{DATA:[nginx_json][http_user_agent_parsed][major]}\.%{DATA:[nginx_json][http_user_agent_parsed][minor]}\.%{DATA:[nginx_json][http_user_agent_parsed][path]}\.%{DATA}",
                "&DNSID=%{DATA:[nginx_json][DNSID]}&%{DATA}",
                "&DNSID=%{DATA:[nginx_json][DNSID]}\" %{DATA}",
                "\?%{DATA:[nginx_json][args]}&DNSID=%{DATA:[nginx_json][DNSID]}&user_id=%{NUMBER:[nginx_json][page_user_id]}&gen_time=%{NUMBER:[nginx_json][page_gen_time]}&load_time=%{NUMBER:[nginx_json][page_load_time]}"
#               "%{IPORHOST:[nginx_json][remote_ip]} - %{DATA:[nginx_json][user_name]} \[%{HTTPDATE:[nginx_json#][access_time]}\] \"%{DATA:[nginx_json][request]}\" %{NUMBER:[nginx_json][status]} %{NUMBER:[nginx_json][body_bytes_sent]} \"%{DATA:[nginx_json][url]}\" \"%{DATA:[nginx_json][http_referrer]}\""
#               "%{IPORHOST:[nginx_json][remote_ip]} - %{DATA:[nginx_json][user_name]} \[%{HTTPDATE:[nginx_json#][access_time]}\] \"%{DATA:[nginx_json][request_method]} \/%{DATA:q}\/\?%{DATA:e} %{DATA:[nginx_json][http_version]}\""
        ]                                                                                                      #
        }
        remove_tag => [ "_grokparsefailure" ]
        add_tag => [ "access" ]
    }
    date {
        match => [ "[nginx_json][timestamp]" , "dd/MMM/YYYY:HH:mm:ss Z" ]
    }
    prune {
        whitelist_names => ["timestamp", "host", "nginx_json", "http_user_agent_parsed", "tags", "log"]
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
ERROR
[ERROR] 2019-10-17 12:34:01.891 [[main]>worker3] useragent - Uknown error while parsing user agent data {:exception=>#<TypeError: cannot convert instance of class org.jruby.RubyHash to class java.lang.String>, :field=>"agent", :event=>#<LogStash::Event:0x57cae72d>}

РЕШЕНИЕ
```
    useragent {
        source => "user_agent"
    }
```
###Проверка конфига
```
cd /usr/share/logstahs
bin/logstash -f /etc/logstash/conf.d/nginx-access.conf -t
```
