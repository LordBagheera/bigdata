FROM  alpine
LABEL Maintainer="Erik Hurtado"
LABEL Email="hurtado.erik@gmail.com"
LABEL Description="Nifi, based on alpine linux"

RUN apk add --no-cache bash openssh sed vim openjdk8 zip tar ca-certificates
ENV USER root 
ENV PASSWORD ThisIsAnUnS3cur3P4ssw0rd
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV PATH $JAVA_HOME/bin:$PATH
ENV NIFI_HOME /opt/nifi

# Download and install Nifi
RUN wget https://archive.apache.org/dist/nifi/1.17.0/nifi-1.17.0-bin.zip -O /opt/nifi-1.17.0-bin.zip ; \
    cd /opt ; \
    unzip nifi-1.17.0-bin.zip ; \
    ln -s nifi-1.17.0 nifi ; \
    mkdir $NIFI_HOME/database_repository && \
    mkdir $NIFI_HOME/flowfile_repository && \
    mkdir $NIFI_HOME/content_repository && \
    mkdir $NIFI_HOME/provenance_repository && \
    mkdir $NIFI_HOME/xsl && \
    mkdir $NIFI_HOME/extra_lib && \
    sed -i 's/127.0.0.1/0.0.0.0/' ${NIFI_HOME}/conf/nifi.properties; \
    sed -i 's/http/1.1/http/1.2/' ${NIFI_HOME}/conf/nifi.properties; \
    ${NIFI_HOME}/bin/nifi.sh set-single-user-credentials ${USER} ${PASSWORD} ; \
    mkdir /root/.ssh

# Copy id's to autoconnect via ssh
COPY ./conf/known_hosts /root/.ssh/
COPY ./conf/id_rsa /root/.ssh/
RUN chmod 0600 /root/.ssh/id_rsa /root/.ssh/known_hosts
VOLUME ["$NIFI_HOME/conf", "$NIFI_HOME/database_repository", "$NIFI_HOME/flowfile_repository", "$NIFI_HOME/content_repository", "$NIFI_HOME/provenance_repository"]

EXPOSE 8443
WORKDIR ${NIFI_HOME}
CMD ["bin/nifi.sh", "run"]