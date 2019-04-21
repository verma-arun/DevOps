# Run Elasticsearch.yml through docker-compose up.

Enter into the server 1 :

```bash
docker-compose -f docker-compose-node-one.yml up -d

```

Enter into the server #2
```bash
docker-compose -f docker-compose-node-two.yml up -d
```

# when it shows virtual memory error then run this command

```bash
  sudo sysctl vm.max_map_count=262144
```
it sets virtual memory temporarly you have set it again after restart the system 


#when you are doing docker-compose -f -- up first time it will gives directory access
#permision denied error, to resolve this we have to give permission to user

```bash
  sudo chown username path_of_host(where we are mounting the data)
```
# We can set flags by mounting the elasticsearch-config file

```bash
  - ./filename.yml:/usr/share/elasticsearch/config/elasticsearch.yml
```