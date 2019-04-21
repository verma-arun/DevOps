**Volumes :**
The data in cassandra is volatile, which means when we down the container data will delete as well, for saving the data permanently, we just have to add `volumes:` label in our cassandra.yml file, under the `volumes:` label we have to give path of the disk where data will be saved. for example:

```bash
volumes:
    - /var/db/simba-infra/cassandra-one/:/var/lib/cassandra/data
```

**PORTS :** All ports are defined bellow:

* 7000 port for cluster communication
* 7001 if SSL is enable
* 7199 for JMX
* 9042 for native protocol client
* 9160 is Cassandra client port (Thrift).

**Sample for single node of cassandra**

```bash
version: '2'
services:
    urvaraapm-cassandra:
        image: cassandra:3.9
        volumes:
            - /var/db/UrvaraApm/cassandra/:/var/lib/cassandra/data
        ports:
            - 7000:7000
            - 7001:7001
            - 7199:7199
            - 9042:9042
            - 9160:9160
```