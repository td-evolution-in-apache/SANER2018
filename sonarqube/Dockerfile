FROM openjdk:8-jdk

ENV TZ=Europe/Amsterdam \
    SONAR_VERSION=6.4 \
    SONARQUBE_HOME=/opt/sonarqube \
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Set timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Http port
EXPOSE 9000

RUN set -x \
    && cd /opt \
    && curl -o sonarqube.zip -fSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/* \
    && rm -rf $SONARQUBE_HOME/extensions/plugins/* \
    && rm -rf $SONARQUBE_HOME/data/es

VOLUME "$SONARQUBE_HOME/data"

WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
COPY plugins $SONARQUBE_HOME/extensions/plugins/
ENTRYPOINT ["./bin/run.sh"]

