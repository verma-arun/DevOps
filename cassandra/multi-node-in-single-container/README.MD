# CREATING MULTI NODE CASSANDRA CLUSTER

**cluster_name**
(Default: Test Cluster) The name of the cluster. This setting prevents nodes in one logical cluster from joining another. All nodes in a cluster must have the same value.

**listen_address**
(Default: localhost) The IP address or hostname that Cassandra binds to for connecting to other Cassandra nodes. Set this parameter or listen_interface, not both. You must change the default setting for multiple nodes to communicate:


**CASSANDRA_SEEDS**
The tag `CASSANDRA_SEEDS` is used for creating connection bitween nodes, we just give the service name of first node in second node under the environment label, just like bellow:

```bash
environment:
    - CASSANDRA_SEEDS=cassandra-node-one
```

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

## In bellow example two nodes are run in single cluster and by giving different volumes path the data will replicate in both locations

**Sample for multi-nodes cluster of cassandra**

```bash
version: '2'
services:
    test1-cassandra-node-one:
        image: cassandra:3.9
        volumes:
            - ~/volumes/jhipster/node-one/cassandra/:/var/lib/cassandra/data
        ports:
            - 7000:7000
            - 7001:7001
            - 7199:7199
            - 9042:9042
            - 9160:9160
        environment:
            - CASSANDRA_BROADCAST_ADDRESS=192.168.43.78
            - LISTEN_ADDRESS=192.168.43.78
            - RPC_ADDRESS= 0.0.0.0
    test1-cassandra-node-two:
        image: cassandra:3.9
        volumes:
            - ~/volumes/jhipster/node-two/cassandra/:/var/lib/cassandra/data
        environment:
            - CASSANDRA_SEEDS=test1-cassandra
```