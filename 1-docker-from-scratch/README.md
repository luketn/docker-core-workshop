# Docker FROM scratch
This is a little demo to show what docker is at its core. 

Let's build a simple container from scratch with a statically compiled binary executable as the only contents.

Docker is a containerization technology. It allows you to run a program in a container, and then run that program on a different host.

Start by running a new Alpine Linux container:
```shell
docker run --name alpine-builder -v $(pwd):/src -it --rm alpine /bin/sh
```

Next install clang and tools to build a C executable:
```shell
apk add --no-cache gcc clang g++ binutils
```

Next build a statically linked executable from main.c:
```shell
cd /src
clang -static -o main main.c
ls -lah main
```

Why statically linked? Because we're going to take this executable and bundle it into a completely empty Docker image, and we want the only dependency to be the Linux kernel.

As you can see, the executable is only ~150KB:
```shell
-rwxr-xr-x    1 root     root      149.4K May 24 16:41 main
```

Next we'll use a multi-stage Dockerfile to build the executable, and copy it to an empty (scratch) image.

```shell
docker build -t docker-from-scratch .
docker images -f "reference=docker-from-scratch"
```

You can see the size of the built docker image is basically the same as the binary executable:
```shell
REPOSITORY            TAG       IMAGE ID       CREATED       SIZE
docker-from-scratch   latest    96e8f257d629   2 hours ago   153kB
```

The important takeaway of this is that Docker isn't a virtual machine - it's a container having a set of files which share a common instance of the Linux kernel on a host machine. In this case it's your computer but often in production it will be a cloud-managed host with many containers all sharing the resources of the server and running on that shared kernel they all use.

The files can include all of content files in a particular distribution of Linux, like Alpine (very lightweight) or Centos (very heavy). These files include services, utilities, shells and libraries. 

You can see the relative sizes of some common base container images like this:
```shell
docker pull ubuntu
docker pull centos
docker pull debian
docker pull alpine
docker images -f "reference=ubuntu" -f "reference=debian" -f "reference=centos" -f "reference=alpine"
```
Output:
```shell
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
debian       latest    5c17865bcd0f   13 days ago    118MB
ubuntu       latest    f3d495355b4e   3 weeks ago    69.2MB
alpine       latest    3fb3c9af89a9   7 weeks ago    5.32MB
centos       latest    e6a0117ec169   8 months ago   272MB
```

Note the massive difference between a minimal Linux distro like Alpine and a full-featured Linux distro like Centos.

Most of the time, this additional weight won't add anything to your process - and will not be executed by the container.

With containers, every byte counts too - as it will need to be copied around, and will slow down the start-up time and potentially the runtime of your application. In addition, each additional capability of the base image adds to the surface area for attack, meaning you have more potential vulnerabilities that could be exploited.

Most of the time when you are using containers you won't have this idealised 150KB static binary, but it's good to understand that this is all you need to run a process. 

If you use a statically compiled language like C++, Go, Rust etc... you can statically link your code to the Linux kernel and have a lean and mean container. If you do need an operating system, consider Alpine Linux as a good starting point for a lean foundation.


Further reading:
https://www.baeldung.com/linux/docker-containers-evolution

Further exercise:
Try building something useful from SCRATCH. e.g. a statically linked Golang program with an HTTP server connecting to a MongoDB backend database.  
https://www.mongodb.com/docs/drivers/go/current/fundamentals/crud/read-operations/query-document/
https://zetcode.com/golang/http-server/
https://gobyexample.com/json
