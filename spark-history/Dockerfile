FROM  bde2020/spark-history-server:3.3.0-hadoop3.3
LABEL Maintainer="Erik Hurtado"
LABEL Email="hurtado.erik@gmail.com"
LABEL Description="based on BDE2020 spark history server image"
LABEL Description="alpine linux + openjdk8 + spark 3.3.0 + hadoop 3.3 to use history server from hdfs repository"

RUN sed -i 's/\/tmp\/spark-events/hdfs:\/\/namenode:8020\/tmp\/spark-events/' /history-server.sh