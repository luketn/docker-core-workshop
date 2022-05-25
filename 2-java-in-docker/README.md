# Building Java Programs to run on Docker
Java and containers are a great pairing - the typically long-lived run time of a container allows the Java Virtual Machine (JVM) which executes Java code to optimise itself over time and creates awesome performance for services that improves over time. 

There are a few key considerations to running Java programs in a container though:  
* The JVM depends on glibc, and can't be statically linked so it requires at least a minimal Linux distribution.
* Size is still a key factor, so you should consider the dependencies you add and keep them minimal.
* Consider the layered file system that a container image is composed of, and layer the image starting with the least-changing files and build up to the most-changing files. I.e. your code should go on the very last layer.

I've followed the Getting Started guide to build a Quarkus Java microservice:  
https://quarkus.io/guides/getting-started

I already initialized project which was created using the quick start artifact:
```shell
mkdir quarkus
cd quarkus
mvn io.quarkus.platform:quarkus-maven-plugin:2.9.1.Final:create \
    -DprojectGroupId=com.mycodefu \
    -DprojectArtifactId=getting-started \
    -Dextensions="resteasy-reactive"
cd getting-started
```

Then built a deployable package with Maven:
```shell
./mvnw clean package
```

The resulting built app is checked in to the repository.

We can build the Java docker container like this:
```shell
docker build -t quarkus-quick-start .
```

And then run it like this:
```shell
docker run -it --rm -p 8080:8080 quarkus-quick-start
```

Let's take a look at the Dockerfile and walk through each part to understand what it means and why use it...

**Further reading:**  

Here's a great cheat-sheet to good practices building Java docker images:   
https://snyk.io/blog/best-practices-to-build-java-containers-with-docker/

