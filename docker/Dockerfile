#Docker file build java
# For Java 11, try this
FROM adoptopenjdk/openjdk11:alpine-jre

# Move Maven build -> finalName
ARG JAR_FILE=target/spring-boot-web.jar

WORKDIR /opt/app

#Copy file to dockerimage
COPY ${JAR_FILE} app.jar


ENTRYPOINT ["java","-jar","app.jar"]


