FROM maven AS build-env

WORKDIR /app

COPY pom.xml .
COPY src ./src

# produce assets
RUN mvn clean compile assembly:single

# build runtime image
FROM openjdk:8
WORKDIR /app
COPY --from=build-env /app/target/ ./
ENTRYPOINT ["java", "-server", "-XX:+UseNUMA", "-XX:+UseParallelGC", "-XX:+AggressiveOpts", "-jar", "netty-example-0.1-jar-with-dependencies.jar"]