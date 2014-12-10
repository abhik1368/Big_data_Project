Big_data_Project
================
For configuring Mongo DB across multiple machine Cloudmesh Python API is used . The vmstart.py code starts the ubuntu machines as shard machines in India grid and write, executes the filescript.sh.

Config servers are lie the brains of the cluster: they hold all of the metadata about which servers hold what data.Thus, they must be set up first and the data they hold is extremely important. Each config server should be on a separate physical machine. For the config server and mongos router separatelya VMs are started and Mongo DB is installed on it. The config servers must be started before any of the mongos processes, as mongos pulls its configuration from them. Config servers are standalone mongod processes, and it is started as the same way as normal mongod processes. For production purposes it is recomended to use 3 config servers and for testing puporses one config server is fine to go with. Also we start the shard servers on each of the VMs using the command given below. After shard servers,config servers and and router server is ready we copy the ssh keys to each of the machines for secure data transfer between machines.

<p>
<code>
mongod --configsvr --dbpath=/data/configdb
mongod --shardsvr --dbpath=/data/shardb
</code>
</p>
