FROM tomcat:8-jre8

LABEL maintainer="Roy Kim <roy.kim@navercorp.com>"

#ARG PINPOINT_VERSION=${PINPOINT_VERSION:-1.9.0-SNAPSHOT}
#ARG INSTALL_URL=${INSTALL_URL:-https://github.com/gwagdalf/binary-collection/raw/master/monitor/pinpoint/id-and-pwd/11/pinpoint-collector-${PINPOINT_VERSION}.war}
ARG PINPOINT_VERSION=${PINPOINT_VERSION:-1.8.4}
ARG INSTALL_URL=https://github.com/naver/pinpoint/releases/download/${PINPOINT_VERSION}/pinpoint-collector-${PINPOINT_VERSION}.war

COPY /build/scripts/start-collector.sh /usr/local/bin/

RUN apt-get update && \
    apt-get install -y dos2unix && \
    apt-get install -y net-tools

RUN chmod a+x /usr/local/bin/start-collector.sh \
    && curl -SL ${INSTALL_URL} -o pinpoint-collector.war \
    && rm -rf /usr/local/tomcat/webapps \
    && mkdir -p /usr/local/tomcat/webapps \
    && unzip pinpoint-collector.war -d /usr/local/tomcat/webapps/ROOT \
    && rm -rf pinpoint-collector.war \
    && sed -i "s/8005/9005/g" /usr/local/tomcat/conf/server.xml \
    && sed -i "s/8080/9080/g" /usr/local/tomcat/conf/server.xml \
    && sed -i "s/8009/9009/g" /usr/local/tomcat/conf/server.xml \
    && sed -i "s/8443/9443/g" /usr/local/tomcat/conf/server.xml

RUN dos2unix /usr/local/bin/start-collector.sh

ENTRYPOINT ["/usr/local/bin/start-collector.sh"]

