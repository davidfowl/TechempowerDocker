FROM maven

WORKDIR /app

COPY servlet/pom.xml .
COPY servlet/src ./src

# produce assets
#RUN mvn clean install

# install resin

ENV RVER 4.0.53
ENV RESIN resin-${RVER}
ENV RESIN_HOME=${RESIN}

RUN cp -r $JAVA_HOME/include $JAVA_HOME/jre/bin/ \
    && curl -L -o /tmp/resin-${RVER}.tar.gz http://www.caucho.com/download/resin-${RVER}.tar.gz \
    && tar -x -C . -f /tmp/resin-${RVER}.tar.gz

# build runtime image
#FROM openjdk:8
#WORKDIR /app
#COPY --from=servlet-build-env /app/target/ ./
#ENTRYPOINT ["java", "-server", "-XX:+UseNUMA", "-XX:+UseParallelGC", "-XX:+AggressiveOpts", "-jar", "netty-example-0.1-jar-with-dependencies.jar"]