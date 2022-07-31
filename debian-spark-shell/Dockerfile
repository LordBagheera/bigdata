FROM debian:stable
LABEL Maintainer="Erik Hurtado"
LABEL Email="hurtado.erik@gmail.com"
LABEL Description="Debian + openjdk8 + spark 3.3.0 + hadoop 3.3 to use spark shell and send jobs to BDE2020 based spark cluster"
RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get install -y build-essential tzdata apt-transport-https ca-certificates dirmngr gnupg gnupg1 gnupg2; apt clean
RUN apt-get install -y wget gnupg gnupg1 gnupg2 software-properties-common; apt clean

RUN wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | apt-key add -
RUN add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/
RUN apt-get update ; apt-get install -y adoptopenjdk-8-hotspot; apt clean

RUN mkdir /var/run/sshd; \
    apt install -y openssh-server; \
    sed -i 's/^#\(Port\) .*/\1 2222/' /etc/ssh/sshd_config; \
    sed -i 's/^#\(PermitRootLogin\) .*/\1 yes/' /etc/ssh/sshd_config; \
    sed -i 's/^\(UsePAM yes\)/# \1/' /etc/ssh/sshd_config; \
    apt clean;

RUN { \
    echo '#!/bin/bash -eu'; \
    echo 'ln -fs /usr/share/zoneinfo/${TZ} /etc/localtime'; \
    echo 'echo "root:${ROOT_PASSWORD}" | chpasswd'; \
    echo 'exec "$@"'; \
    } > /usr/local/bin/entry_point.sh; \
    chmod +x /usr/local/bin/entry_point.sh;

COPY image_env_setup.sh /root/
RUN echo 'source /root/image_env_setup.sh' >> ~/.bashrc
COPY spark-3.3.0-bin-hadoop3.tgz /opt/
RUN cd /opt; tar zxvf spark-3.3.0-bin-hadoop3.tgz; ln -s spark-3.3.0-bin-hadoop3 spark
COPY hadoop-3.2.1.tar.gz /opt/
RUN cd /opt; tar zxvf hadoop-3.2.1.tar.gz; ln -s hadoop-3.2.1 hadoop
COPY sbt-1.7.1.tgz /opt/
RUN cd /opt; tar zxvf sbt-1.7.1.tgz; ln -s sbt-1.7.1 sbt

ADD core-site.xml /opt/hadoop/etc/hadoop/hdfs-site.xml
ADD core-site.xml /opt/hadoop/etc/hadoop/core-site.xml


ENV TZ America/Santiago
ENV ROOT_PASSWORD root
ENV SPARK_HOME /opt/spark
EXPOSE 2222
ENTRYPOINT ["entry_point.sh"]
CMD    ["/usr/sbin/sshd", "-D", "-e"]