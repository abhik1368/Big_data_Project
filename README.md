Big_data_Project
================
For configuring Mongo DB across multiple machine Cloudmesh Python API is used . The vmstart.py code starts the ubuntu machines as shard machines in India grid and write, executes the filescript.sh.

Config servers are lie the brains of the cluster: they hold all of the metadata about which servers hold what data.Thus, they must be set up first and the data they hold is extremely important. Each config server should be on a separate physical machine. For the config server and mongos router separatelya VMs are started and Mongo DB is installed on it. The config servers must be started before any of the mongos processes, as mongos pulls its configuration from them. Config servers are standalone mongod processes, and it is started as the same way as normal mongod processes. For production purposes it is recomended to use 3 config servers and for testing puporses one config server is fine to go with. Also we start the shard servers on each of the VMs using the command given below. After shard servers,config servers and and router server is ready we copy the ssh keys to each of the machines for secure data transfer between machines.

```
mongod --configsvr --dbpath=/data/configdb
mongod --shardsvr --dbpath=/data/shardb
```

Also when we start up config servers, we do not use the --replSet option: config servers are not members of a replica set. The --configsvr option indicates to the mongod that VM is used as a config server. It is not strictly required, as all it does is change the default port mongod  which listens on to 27019 and the default data directory to /data/configdb. Once we have the shard servers and confif servers running we start the mongos router server which needs to know where the config servers are so mongos is started with the --configdb option:

```
mongos --configdb config-1:27019 > -f /var/lib/mongos.conf
```

mongos runs on port 27017. Note that it does not need a data directory. Adding shards to the mongos is easy once in the shell the command below adds three shards,

```
sh.addShard("server-1:27017,server-2:27017,server-3:27017")
```

Mongo DB won’t distribute your data automatically until we explicitly tell which database and collections to shard. We explicitly shard the chembldb database,molecules collection and mfp\_counts collection. On the mongos shell the database and collection are sharded by,
```
db.runCommand({enablesharding: "chembldb"});
sh.shardCollection("chembldb.molecules", { "_id": "hashed" } )
sh.shardCollection("chembldb.mfp1_counts", { "_id": "hashed" } )  
```
