FROM       java:8-jre
MAINTAINER Michael Scheetz <michael.scheetz@hpe.com>

ENV SONATYPE_WORK /sonatype-work
ENV NEXUS_VERSION 3.0.0-b2015110601

RUN mkdir -p /opt/sonatype/nexus \
  && curl --fail --silent --location --retry 3 \
    https://sonatype-download.global.ssl.fastly.net/nexus/oss/nexus-${NEXUS_VERSION}-bundle.tar.gz \
  | gunzip \
  | tar x -C /tmp nexus-${NEXUS_VERSION} \
  && mv /tmp/nexus-${NEXUS_VERSION}/* /opt/sonatype/nexus/ \
  && mkdir -p ${SONATYPE_WORK}/etc \
  && mv /opt/sonatype/nexus/etc/* ${SONATYPE_WORK}/etc \
  && mkdir -p ${SONATYPE_WORK}/data \
  && mv /opt/sonatype/nexus/data/* ${SONATYPE_WORK}/data \
  && mkdir -p ${SONATYPE_WORK}/deploy \
  && rm -rf /tmp/nexus-${NEXUS_VERSION}

RUN useradd -r -u 200 -m -c "nexus role account" -d ${SONATYPE_WORK} -s /bin/false nexus

RUN chown -R nexus:root /opt/sonatype/nexus \
  && chown -R nexus:root ${SONATYPE_WORK} 


VOLUME ${SONATYPE_WORK}

EXPOSE 8081
WORKDIR /opt/sonatype/nexus
USER nexus





ENV MAIN org.apache.karaf.main.Main
ENV OPTS -Dkaraf.startLocalConsole=true -Dkaraf.startRemoteShell=true

ENV JAVA_MIN_MEM 256M
ENV JAVA_MAX_MEM 768M
ENV KARAF_OPTS -Djava.net.preferIPv4Stack=true

ENV DEFAULT_JAVA_OPTS -Xms${JAVA_MIN_MEM} -Xmx${JAVA_MAX_MEM} -XX:+UnlockDiagnosticVMOptions -XX:+UnsyncloadClass 
ENV DEFAULT_JAVA_OPTS -Xverify:none ${DEFAULT_JAVA_OPTS}
ENV JAVA_OPTS ${DEFAULT_JAVA_OPTS}

ENV KARAF_HOME /opt/sonatype/nexus
ENV KARAF_BASE ${SONATYPE_WORK}
ENV KARAF_DATA ${SONATYPE_WORK}/data
ENV KARAF_ETC ${SONATYPE_WORK}/etc
ENV CLASSPATH lib/karaf-org.osgi.core.jar:lib/karaf-nexus-branding.jar:lib/karaf-jaas-boot.jar:lib/karaf.jar


CMD java \
   ${JAVA_OPTS} -Dkaraf.instances="${KARAF_HOME}/instances" \
  -Dkaraf.home=${KARAF_HOME} -Dkaraf.base=${KARAF_BASE} -Dkaraf.data=${KARAF_DATA} -Dkaraf.etc=${KARAF_ETC} \
  -Djava.io.tmpdir=${KARAF_DATA}/tmp -Djava.util.logging.config.file=${KARAF_ETC}/java.util.logging.properties \
   ${KARAF_OPTS} ${OPTS} -classpath ${CLASSPATH} ${MAIN} "$@"
