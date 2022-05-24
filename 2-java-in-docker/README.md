
Java in docker. Limitation: Java depends on a few additional libraries beyond the kernel, and can't (easily) be statically linked.
So we need a minimal operating system, which has a package manager to install Java and its dependencies. Enter Alpine!
Create a minimal Java docker image, using Alpine and the Alpine JRE package.

```
FROM alpine

RUN apk upgrade --no-cache && \
    apk add --no-cache openjdk17-jre-headless

WORKDIR /opt/app

# Add layers of files to the docker image. Order is significant, from least changing to most changing.
ADD lib/ lib/
ADD log4j.properties .
ADD app.jar .

# Hint to docker hosts about ports being used
EXPOSE 8080/tcp

# Java run statement
CMD java -XX:+UseContainerSupport \
         -XX:InitialRAMPercentage=40.0 \
         -XX:MinRAMPercentage=20.0 \
         -XX:MaxRAMPercentage=90.0 \
         -Dlog4j.configuration=file:/opt/app/log4j.properties \
         -cp /opt/app:/opt/app/*:/opt/app/lib/* \
         com.mycodefu.EntryPoint
```
