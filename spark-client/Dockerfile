FROM  bde2020/spark-base:3.3.0-hadoop3.3
LABEL Maintainer="Erik Hurtado"
LABEL Email="hurtado.erik@gmail.com"
LABEL Description="based on BDE2020 spark base image"
LABEL Description="alpine linux + openjdk8 + spark 3.3.0 + hadoop 3.3 to use spark shell and send jobs to BDE2020 based spark cluster"

RUN apk add --no-cache openssh sed vim; \
    cd /etc/ssh/; ssh-keygen -A; \
    sed -i 's/^#\(Port\) .*/\1 2222/' /etc/ssh/sshd_config; \
    sed -i 's/^#\(PermitRootLogin\) .*/\1 yes/' /etc/ssh/sshd_config; \
    sed -i 's/^\(UsePAM yes\)/# \1/' /etc/ssh/sshd_config

RUN { \
    echo '#!/bin/bash -eu'; \
    echo 'ln -fs /usr/share/zoneinfo/${TZ} /etc/localtime'; \
    echo 'echo "root:${ROOT_PASSWORD}" | chpasswd'; \
    echo 'exec "$@"'; \
    } > /usr/local/bin/entry_point.sh; \
    chmod +x /usr/local/bin/entry_point.sh;

COPY ./hadoop-3.2.1.tar.gz /opt/
RUN cd /opt; tar zxvf hadoop-3.2.1.tar.gz; ln -s hadoop-3.2.1 hadoop
COPY ./sbt-1.7.1.tgz /opt/
RUN cd /opt; tar zxvf sbt-1.7.1.tgz; ln -s sbt-1.7.1 sbt
COPY ./*.xml /opt/hadoop/etc/hadoop/
COPY ./spark-defaults.conf /spark/conf/
COPY image_env_setup.sh /root/
RUN echo 'source /root/image_env_setup.sh' >> ~/.bashrc ; \
    echo 'source /root/image_env_setup.sh' >> ~/.profile; \
    mkdir /root/.ssh
COPY ./authorized_keys /root/.ssh/
COPY ./id_rsa /root/.ssh/
RUN chmod 0600 /root/.ssh/id_rsa

ENV TZ America/Santiago
ENV ROOT_PASSWORD defaults
EXPOSE 2222
EXPOSE 22
ENTRYPOINT ["entry_point.sh"]
CMD    ["/usr/sbin/sshd", "-D", "-e"]