# Original Dockerfile at: https://github.com/SonarSource/docker-sonarqube/blob/52981f8b2efdbfe18ffacaae687af7a4f05c15a5/7.7-community/Dockerfile

FROM openjdk:8

ENV SONAR_VERSION=7.1 \
    SONARQUBE_HOME=/opt/sonarqube \
    # Database configuration
    # Defaults to using H2
    # DEPRECATED. Use -v sonar.jdbc.username=... instead
    # Drop these in the next release, also in the run script
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

RUN groupadd -r sonarqube && useradd -r -g sonarqube sonarqube

# grab gosu for easy step-down from root
RUN set -x \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture)" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture).asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && (gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
        || gpg --batch --keyserver ipv4.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4) \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -rf "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

# In case you want to use a local version of the installer instead of downloading one
# uncomment the following line and comment the next "RUN" command
#COPY sources/sonarqube-$SONAR_VERSION.zip /opt/sonarqube.zip

RUN set -x \
    && (gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE \
	    || gpg --batch --keyserver ipv4.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE) \
    && cd /opt \
    && curl -o sonarqube.zip -fSL https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && curl -o sonarqube.zip.asc -fSL https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    && gpg --batch --verify sonarqube.zip.asc sonarqube.zip 

# Installing sonarqube
RUN set -x \
    && cd /opt \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && chown -R sonarqube:sonarqube sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*

COPY assets/plugins/branch-2.0.0.jar $SONARQUBE_HOME/extensions/plugins/branch-2.0.0.jar

VOLUME "$SONARQUBE_HOME/data"

WORKDIR $SONARQUBE_HOME
COPY assets/scripts/run.sh $SONARQUBE_HOME/bin/
USER sonarqube
ENTRYPOINT ["./bin/run.sh"]