# bigdata
Pseudo cluster for big data operations, simulating enterprise system

Thanks to BDE2020 for base images and documentation-

this includes (so far)
* Homebrew Apache Nifi orchestrator 
* Homebrew SSH server as client and edge/ingest server
* Hadoop HDFS cluster with 2 nodes from BDE2020
* Hadoop YARN with 2 nodes configured and history server from BDE2020
* Hadoop Nodemanager from BDE2020
* HIVE2 server
* Jupyter with spark support(local mode only) and hdfs support

NIFI server: 
    *docker name : nifi

    *url https://localhost:8443/nifi/login
    *    user: root
    *    password: ThisIsAnUnS3cur3P4ssw0rd
    *In order to first connect to edge from nifi you must:
    *    -open bash cli to nifi container 'docker exec -ti nifi /bin/bash'
    *    -run first ssh connection to edge in order to approve new host: 'ssh -p2222 root@edge-1'
    *    -answer 'yes' when prompted

EDGE Server: 

    * Docker name : edge-1
    * SSH server at localhost port 2222
    *    user: root
    *    password: defaults
    * Contains an HDFS installation, and a local spark environment
    * Configured to be transparent with YARN and namenode
    * Spark commands use YARN as master by default

HDFS Cluster: 

    * Docker name : namenode, datanode-1, datanode-2
    *   url http://localhost:9870
    * ACL's disabled
    * TODO: specify complete steps

HADOOP YARN server:

    * Docker name : resourcemanager, historyserver
    *   url http://localhost:8088/cluster
    *   ACL's disabled
    * Can monitoring status of application and serve correct yarn logs to EDGE

HADOOP Nodemanager 1 & 2:

    * Docker name : nodemanager-1, nodemanager-2
    *    url http://localhost:8042
    *    url http://localhost:8043
    *    ACL's disabled
    * Configured as 1 cluster/1 rack

Hive:   

    * Docker name : hive-server, hive-metastore, hive-metastore-postgresql
    * HIVE 2 Server
    * HIVE 2 metastore
    * HIVE 2 postgress metastore
    * ACL's disabled

Jupyter:

    * Docker name : jupyspark
    *    url http://localhost:8888   I
    *    see docker container log for token

This cover most of the services that I use as part of my data engineer day2day work.