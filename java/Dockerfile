ARG OS_VERSION=3.8

FROM alpine:${OS_VERSION}-glibc-cn

ARG JAVA_REPO=http://172.17.0.1/java
ARG JAVA_VERSION_MAJOR=8
ARG JAVA_VERSION_MINOR=202
ARG JAVA_PACKAGE=jdk

ENV JAVA_HOME=/opt/java \
    PATH=${PATH}:/opt/java/bin \
    LANG=zh_CN.UTF-8

RUN set -ex && \
    apk -U upgrade && \
    apk add java-cacerts && \
    mkdir /opt && \
    curl -o /tmp/java.tar.gz ${JAVA_REPO}/${JAVA_VERSION_MAJOR}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz && \
    # Install jdk
    gunzip /tmp/java.tar.gz && \
    tar -C /opt -xf /tmp/java.tar && \
    mv /opt/j* $JAVA_HOME && \
    if [ "$JAVA_PACKAGE" = "jre" ]; then \
        JRE_PATH=$JAVA_HOME; \
    else \
        find $JAVA_HOME -maxdepth 1 -mindepth 1 | grep -v jre | grep -v include | xargs rm -rf && \
        JRE_PATH=$JAVA_HOME/jre && \
        cd $JAVA_HOME && rm -fr ./bin && ln -s ./jre/bin ./bin; \
    fi && \
    # Install jce
    curl -o /tmp/jce_policy.zip ${JAVA_REPO}/${JAVA_VERSION_MAJOR}/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
    cd /tmp && unzip /tmp/jce_policy.zip && \
    cp -v /tmp/UnlimitedJCEPolicy*/*.jar $JRE_PATH/lib/security && \
    sed -i s/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/ $JRE_PATH/lib/security/java.security && \
    sed -i /^securerandom\.source\=file\:/s@/dev/urandom@/dev/./urandom@ $JRE_PATH/lib/security/java.security && \
    # Clean env
    # apk del glibc-i18n && \
    rm -rf $JAVA_HOME/*README* \
           $JAVA_HOME/*src.zip \
           $JAVA_HOME/man \
           $JAVA_HOME/bin/appletviewer \
           $JAVA_HOME/bin/extcheck \
           $JAVA_HOME/bin/pack200 \
           $JAVA_HOME/bin/unpack200 \
           $JAVA_HOME/bin/ws* \
           $JAVA_HOME/lib/missioncontrol \
           $JAVA_HOME/lib/visualvm \
           $JAVA_HOME/lib/*javafx* \
           $JRE_PATH/plugin \
           $JRE_PATH/bin/javaws \
           $JRE_PATH/bin/jjs \
           $JRE_PATH/bin/orbd \
           $JRE_PATH/bin/pack200 \
           $JRE_PATH/bin/policytool \
           $JRE_PATH/bin/rmid \
           $JRE_PATH/bin/rmiregistry \
           $JRE_PATH/bin/servertool \
           $JRE_PATH/bin/tnameserv \
           $JRE_PATH/bin/unpack200 \
           $JRE_PATH/lib/javaws.jar \
           $JRE_PATH/lib/deploy* \
           $JRE_PATH/lib/desktop \
           $JRE_PATH/lib/*javafx* \
           $JRE_PATH/lib/*jfx* \
           $JRE_PATH/lib/amd64/libdecora_sse.so \
           $JRE_PATH/lib/amd64/libprism_*.so \
           $JRE_PATH/lib/amd64/libfxplugins.so \
           $JRE_PATH/lib/amd64/libglass.so \
           $JRE_PATH/lib/amd64/libgstreamer-lite.so \
           $JRE_PATH/lib/amd64/libjavafx*.so \
           $JRE_PATH/lib/amd64/libjfx*.so \
           $JRE_PATH/lib/ext/jfxrt.jar \
           $JRE_PATH/lib/ext/nashorn.jar \
           $JRE_PATH/lib/oblique-fonts \
           $JRE_PATH/lib/plugin.jar \
           /tmp/* /var/cache/apk/* && \
    ln -sf /etc/ssl/certs/java/cacerts $JRE_PATH/lib/security/cacerts
