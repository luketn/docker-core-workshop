# docker-core-workshop
What is docker at its core and how to build efficient docker containers.

What is docker - discuss the Linux kernel, and the shared access to the single kernel from all docker containers on a host.

What is docker not? A virtual machine! Even though it is possible to create things that look a lot like virtual machines, you really shouldn't. 
They're huge and waste a lot of valuable resources, time and money.

From SCRATCH! Demo how to create a usable Docker container from scratch, using a 
statically compilable language. Build a hello world C program container. Look at
the size.

?Useful from SCRATCH. Demo creating something that is actually useful, e.g. a statically linked Golang
program with an HTTP server.?  
https://www.mongodb.com/docs/drivers/go/current/fundamentals/crud/read-operations/query-document/  
https://www.sohamkamani.com/golang/json/ 
https://zetcode.com/golang/http-server/  

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
