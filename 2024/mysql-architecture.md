# Evolution of MySQL database architecture

## The journey to MySQL InnoDB ClusterSet

Best Practices in 2024: Config parameters: binary logs, log format row and gtids, not file/pos

### First Iteration
one single InnoDB setting with default durability settings
Drawback: incident can be catastrophic, not ready for production

### Second Iteration
DB more important. RTO -> hours, RPO -> 1 day
Backups with MySQL Shell Dump & Load Utility rather than mysqldump (single vs multi threaded). Physical & Logical Backups

### Third Iteration
Reduce RPO to minutes
Enable binary logs: log_bin ON and sync_binlog 1
Enables point in time recovery. Restore data & replay binary logs. User deleting data: restore before the delete and skip deletes
Best practice: purge binary log files after backup.

### Fourth Iteration
Heavy workload, loose less than a second of data
Improved point in time recovery: enable GTIDs, save binlogs in realtime (OCI, S3...)
Create a dedicated user for binlogs
On another system mysqlbinlog read from remote server. Enables keeping all bin logs and restore at any point in time

### Fifth Iteration
Make DB restorable in minutes
InnoDB ReplicaSet, async replication. No need to provision a replica with a backup.
MySQL shell to replace client. Connect with MySQL shell on master, `dba.configureReplicaSetInstance` then `rs=dba.createReplicaSet('myreplicaset)'` then on replica `dba.configureReplicaSetInstance()` then on master `rs.addInstance('mysql-2)'`

MySQL router

### Sixth Iteration
Automate everything, no data lost. MySQL InnoDB Cluster
Group Replication, automatic failover. Operated by shell
```
dba=dropMetadataSchema()
cluster=dba.createCluster('mycluster')'
```
3 servers, cam tolerate 1 failure. If 2 crashes, cannot recover because of consensus.

### Seventh Iteration
Data Center recovery: MySQL InnoDB ClusterSet: 2 group replications in 2 datacenters
```
cluster=dba.getCluster()
cs=cluster.createClusterSet('domain')
cluster2=cs.createReplicaCluster()
cluster2.addInstance()
```

### Consistency levels
Consistency levels: default, single master. Multi primary works but more conflicts possible.
Default consistency level is eventual, writes will eventually reach all the nodes.
Since MySQL 8.0.16, possible to set sync point at read or write or both

### MySQL Innovation Releases

Scaling Reads, router supports Read Replicas since 8.1
Read/Write Split Evolution since 8.2. Same port for read and writes, no need to separate in the app.
