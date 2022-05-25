FROM alpine

RUN apk upgrade --no-cache && \
    apk add --no-cache openjdk17-jre-headless

WORKDIR /application
COPY ./quarkus/getting-started/target/quarkus-app /application

# Hint to docker hosts about ports being used
EXPOSE 8080/tcp

# Java run statement
CMD java -XX:+UseContainerSupport \
         -XX:InitialRAMPercentage=40.0 \
         -XX:MinRAMPercentage=20.0 \
         -XX:MaxRAMPercentage=90.0 \
         -jar quarkus-run.jar
