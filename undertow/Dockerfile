FROM maven AS undertow-build-env

WORKDIR /app

COPY undertow/pom.xml .
COPY undertow/src ./src

# produce assets
RUN mvn clean package

# build runtime image
FROM openjdk:8
WORKDIR /app
COPY --from=undertow-build-env /app/target/ ./
ENTRYPOINT ["java", "-jar", "hello-undertow.jar", "NO_DATABASE"]