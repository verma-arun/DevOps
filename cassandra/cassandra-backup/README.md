# Steps for creating backup file of cassandra and store the backup file to minio bucket

firstly we have to mount the volume of data where cassandra saves it's files
the main thing we have to backup is commit_logs by the commit_logs we can restore
our data

```
volumes:
    - /var/db/cassandra/:/var/lib/cassandra
```

By add above flag in compose file for mount the data of cassandra from container to host

When we have to flush all the data of commitlogs and memtable to sstables, to avoid loss of recent changes, for this we use this command

```bash
docker exec -it containerId bash
nodetool flush -- keyspaceName
exit
```

Now we have the data which we going to back up now we are going to make compress file (tar.gz or tar.xz) data which we are going to backup, for achiving this we are going to create .sh script for creating tar and upload it to minio bucket

```
tar -czvf ~/Desktop/cassandra_backup.tar.gz /var/db/UrvaraApm/cassandra/
```