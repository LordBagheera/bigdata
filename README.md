# bigdata
Pseudo cluster for big data operations, simulating enterprise system

Thanks to BDE2020 for base images and documentation-

this includes (so far)
* Apache Nifi orchestrator
* Spark cluster with 2 executors and history server
* SSH server as client and edge/ingest server
* Hadoop HDFS cluster with 2 nodes
* Hadoop Nodemanager
* Hadoop YARN
* MySql Server (pending hive integration)
* soon phpmyadmin
* FTP server (alpine ftp//thanks to @delfer)

This cover most of the services that I use as part of my data engineer day2day work.