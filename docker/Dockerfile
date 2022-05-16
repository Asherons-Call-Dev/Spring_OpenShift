####
# This Dockerfile is used in order to build a container that runs the Spring Boot application
#
# Build the image with:
#
# docker build -f docker/Dockerfile -t springboot/sample-demo .
#
# Then run the container using:
#
# docker run -i --rm -p 8081:8081 springboot/sample-demo
####
FROM quay.io/devfile/maven:3.8.1-openjdk-17-slim

WORKDIR /build

# Build dependency offline to streamline build
#COPY pom.xml .
COPY build.gradle .
#RUN mvn dependency:go-offline
#RUN gradle --offline
#RUN ./gradlew --offline clean build -test
#RUN ~/.gradle

COPY src src
#RUN mvn package -Dmaven.test.skip=true
RUN gradle --offline clean build -test

FROM openjdk:11-jdk
COPY --from=0 /build/target/demo-0.0.1-SNAPSHOT.jar /app/target/demo-0.0.1-SNAPSHOT.jar

EXPOSE 8081
ENTRYPOINT [ "java", "-jar", "/app/target/demo-0.0.1-SNAPSHOT.jar", "--server.port=8081" ]
