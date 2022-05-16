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
# FROM quay.io/devfile/maven:3.8.1-openjdk-17-slim

# WORKDIR /build

# # Build dependency offline to streamline build
# #COPY pom.xml .
# COPY build.gradle .
# #RUN mvn dependency:go-offline
# #RUN gradle --offline
# #RUN ./gradlew --offline clean build -test
# #RUN ~/.gradle

# COPY src src
# #RUN mvn package -Dmaven.test.skip=true
# RUN gradle clean build -test

# FROM openjdk:11-jdk
# COPY --from=0 /build/target/demo-0.0.1-SNAPSHOT.jar /app/target/demo-0.0.1-SNAPSHOT.jar

# EXPOSE 8081
# ENTRYPOINT [ "java", "-jar", "/app/target/demo-0.0.1-SNAPSHOT.jar", "--server.port=8081" ]

#
# Set a variable that can be used in all stages.
#
ARG BUILD_HOME=/Spring_OpenShift

#
# Gradle image for the build stage.
#
FROM gradle:jdk11 as build-image

#
# Set the working directory.
#
ARG BUILD_HOME
ENV APP_HOME=$BUILD_HOME
WORKDIR $APP_HOME

#
# Copy the Gradle config, source code, and static analysis config
# into the build container.
#
COPY --chown=gradle:gradle build.gradle settings.gradle $APP_HOME/
COPY --chown=gradle:gradle src $APP_HOME/src
COPY --chown=gradle:gradle config $APP_HOME/config

#
# Build the application.
#
RUN gradle --no-daemon build

#
# Java image for the application to run in.
#
FROM openjdk:12-alpine

#
# Copy the jar file in and name it app.jar.
#
ARG BUILD_HOME
ENV APP_HOME=$BUILD_HOME
#COPY --from=build-image $APP_HOME/build/libs/gradle-docker-example.jar app.jar
COPY --from=0 /build/target/demo-0.0.1-SNAPSHOT.jar /app/target/demo-0.0.1-SNAPSHOT.jar

#
# The command to run when the container starts.
#
ENTRYPOINT java -jar app.jar
