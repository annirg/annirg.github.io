
mkdir -p /data/app/elk/{elasticsearch,logstash,kibana}
mkdir -p /data/app/elk/elasticsearch/{data,plugins}
useradd -s /sbin/nologin -u 1000 elasticsearch
chown 1000:1000 -R /data/app/elk/elasticsearch/
docker pull elasticsearch:7.6.1
docker pull logstash:7.6.1
docker pull kibana:7.6.1

Elasticsearch
vim /etc/sysctl.conf
vm.max_map_count=262144
sysctl -p
docker run -d -p 9200:9200 -p 9300:9300 --name es -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx1g" -v /data/app/elk/elasticsearch/data:/usr/share/elasticsearch/data -v /data/app/elk/elasticsearch/plugins:/usr/share/elasticsearch/plugins -v /data/app/elk/elasticsearch/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml elasticsearch:7.6.1


docker run -d -p 9100:9100 --name es-head mobz/elasticsearch-head:5

vim /data/app/elk/elasticsearch/elasticsearch.yml
cluster.name: "xinglong"
http.cors.enabled: true
http.cors.allow-origin: "*"
network.host: 0.0.0.0


docker restart es es-head

