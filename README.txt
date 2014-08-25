vagrant up

then, in a web browser, go to:

http://localhost:8080

to get Kibana.


to stick in some apache logs, from localhost, do this:

nc localhost 8333 < /var/log/apache/access.log


syslog listener is mapped to localhost:8500






Reference:

https://github.com/ck99/docker-elasticsearch
https://github.com/ck99/docker-logstash
https://github.com/ck99/docker-kibana
