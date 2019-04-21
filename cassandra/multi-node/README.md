# Steps for creating two nodes within a cluster and both nodes will replicates there data with each other

**Machine-one :**

Add `broadcast address` in cassandra yaml file under environment labels, in broadcast address there will be `host IP` as bellow

```bash
environment:
    - CASSANDRA_BROADCAST_ADDRESS=192.168.43.78
```

**Machine-two :**

In machine two we just have to add `broadcast address` and `cassandra seeds`, in cassandra seeds we will provide ip of machine one or master node.

```bash
environment:
    - CASSANDRA_SEEDS=192.168.43.78
    - CASSANDRA_BROADCAST_ADDRESS=192.168.43.47
```
