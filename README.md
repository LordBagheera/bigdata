# bigdata
Pseudo cluster for big data operations, simulating enterprise system

Thanks to BDE2020 for base images and documentation-

this includes (so far)
* Homebrew Apache Nifi orchestrator 
* Homebrew SSH server as client and edge/ingest server
* Hadoop HDFS cluster with 2 nodes from BDE2020
* Hadoop YARN with 1 cluster configured and history server from BDE2020
* Hadoop Nodemanager from BDE2020
* MySql Server
* Phpmyadmin

NIFI server: 
    docker name : nifi
    url https://localhost:8443/nifi/login
        user: root
        password: ThisIsAnUnS3cur3P4ssw0rd
    In order to first connect to edge from nifi you must:
        -open bash cli to nifi container 'docker exec -ti nifi /bin/bash'
        -run first ssh connection to edge in order to approve new host: 'ssh -p2222 root@edge-1'
        -answer 'yes' when prompted

EDGE Server: 
    docker name : edge-1
    SSH server at localhost port 2222
        user: root
        password: defaults
    Contains an HDFS installation, and a local spark environment
    Configured to be transparent with YARN and namenode
    Spark commands use YARN as master by default

HDFS Cluster: 
    docker name : namenode, datanode-1, datanode-2
    url http://localhost:9870
    ACL's disabled
    is necesary to create some dirs in order to startup, see the logs
    TODO: specify complete steps

HADOOP YARN server: docker name : resourcemanager, historyserver
    url http://localhost:8088/cluster
    ACL's disabled
    can monitoring status of application and serve correct yarn logs to EDGE

HADOOP Nodemanager:
    docker name : nodemanager
    url http://localhost:8042
    ACL's disabled
    configured as 1 cluster/1 rack

MySql Server:
    docker name : mysql
    SQL server available at localhost:3306
        user: root
        password: defaults
    vanilla configuration

phpmyadmin:
    docker name : phpmyadmin
    url http://localhost:8888/
        user & password as set on MySql

This cover most of the services that I use as part of my data engineer day2day work.