FROM openjdk:8-jdk-alpine
RUN sh -c 'mkdir -p /app/properties/'
WORKDIR /app
# COPY ./src/main/resources/application.properties /app/application.properties
COPY ./target/gs-spring-boot-docker-0.1.0.jar /app/gs-spring-boot-docker-0.1.0.jar
RUN sh -c 'ls /app'
ENTRYPOINT ["java","-jar","-Dspring.config.location=file:/app/properties/application.properties","/app/gs-spring-boot-docker-0.1.0.jar"] 
