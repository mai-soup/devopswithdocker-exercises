# Exercises

## 1.1 Getting Started

```console
mai@pop-os:~/code/docker-course/section_1$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS      NAMES
d35f90a8d206   httpd     "httpd-foreground"       27 seconds ago   Exited (0) 2 seconds ago              suspicious_brown
d2b7c5868c97   redis     "docker-entrypoint.s…"   42 seconds ago   Up 39 seconds              6379/tcp   sharp_poincare
3312f90d39c5   nginx     "/docker-entrypoint.…"   57 seconds ago   Up 55 seconds              80/tcp     magical_chandrasekhar
```

## 1.2 Cleanup

```console
mai@pop-os:~/code/docker-course/section_1$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
mai@pop-os:~/code/docker-course/section_1$ sudo docker image list
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
mai@pop-os:~/code/docker-course/section_1$ 
```
## 1.3 Secret message

```console
mai@pop-os:~/code/docker-course$ sudo docker exec -it f5 bash
root@f5edb3406f7c:/usr/src/app# ls
server  text.log
root@f5edb3406f7c:/usr/src/app# tail -f ./text.log
2023-06-01 19:54:51 +0000 UTC
2023-06-01 19:54:53 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2023-06-01 19:54:55 +0000 UTC
```

## 1.4 Missing dependencies

```console
mai@pop-os:~/code/docker-course$ sudo docker run -it ubuntu apt update && sudo apt install curl && sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [566 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [108 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [36.3 kB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [449 kB]
Get:9 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [925 kB]  
Get:10 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]          
Get:11 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
Get:12 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [42.2 kB]
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1162 kB]
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [849 kB]
Get:16 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [450 kB]
Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [27.0 kB]
Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [49.4 kB]                                             
Fetched 24.9 MB in 6s (3936 kB/s)                                                                                                 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
11 packages can be upgraded. Run 'apt list --upgradable' to see them.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
curl is already the newest version (7.81.0-1ubuntu1.10).
curl set to manually installed.
The following packages were automatically installed and are no longer required:
  libatopology2 libfftw3-single3
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 164 not upgraded.
Input website:
helsinki.fi
Searching..
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.20.1</center>
</body>
</html>

```

## 1.5 Sizes of images
```console
mai@pop-os:~/code/docker-course$ sudo docker images
REPOSITORY                          TAG       IMAGE ID       CREATED       SIZE
devopsdockeruh/simple-web-service   ubuntu    4e3362e907d5   2 years ago   83MB
devopsdockeruh/simple-web-service   alpine    fd312adc88e0   2 years ago   15.7MB
```

```console
mai@pop-os:~/code/docker-course$ sudo docker run -it devopsdockeruh/simple-web-service:alpine
Starting log output
Wrote text to /usr/src/app/text.log
Wrote text to /usr/src/app/text.log
Wrote text to /usr/src/app/text.log
mai@pop-os:~/code/docker-course$ sudo docker ps
CONTAINER ID   IMAGE                                      COMMAND                 CREATED          STATUS          PORTS     NAMES
1b8070593098   devopsdockeruh/simple-web-service:alpine   "/usr/src/app/server"   13 seconds ago   Up 11 seconds             hungry_northcutt
mai@pop-os:~/code/docker-course$ sudo docker exec -it 1b sh
/usr/src/app # ls
server    text.log
/usr/src/app # tail -f ./text.log
2023-06-01 20:16:10 +0000 UTC
2023-06-01 20:16:12 +0000 UTC
2023-06-01 20:16:14 +0000 UTC
2023-06-01 20:16:16 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2023-06-01 20:16:18 +0000 UTC
2023-06-01 20:16:20 +0000 UTC
2023-06-01 20:16:22 +0000 UTC
2023-06-01 20:16:24 +0000 UTC
2023-06-01 20:16:26 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2023-06-01 20:16:28 +0000 UTC
```

## 1.6 Hello Docker Hub
```console
mai@pop-os:~/code/docker-course$ sudo docker run -it devopsdockeruh/pull_exercise
Unable to find image 'devopsdockeruh/pull_exercise:latest' locally
latest: Pulling from devopsdockeruh/pull_exercise
8e402f1a9c57: Pull complete 
5e2195587d10: Pull complete 
6f595b2fc66d: Pull complete 
165f32bf4e94: Pull complete 
67c4f504c224: Pull complete 
Digest: sha256:7c0635934049afb9ca0481fb6a58b16100f990a0d62c8665b9cfb5c9ada8a99f
Status: Downloaded newer image for devopsdockeruh/pull_exercise:latest
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
``` 

## 1.7 Image for Script

```Dockerfile
FROM ubuntu:20.04

RUN apt update && \
	apt install -y curl

WORKDIR /usr/src/app

COPY script.sh .

CMD ./script.sh
```

## 1.8 Two Line Dockerfile
```Dockerfile
FROM devopsdockeruh/simple-web-service:alpine
CMD ["server"]
```

```bash
[mai@mochi code]$ docker run web-server
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /*path                    --> server.Start.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

## 1.9 Volumes
```bash
[mai@mochi devopswithdocker-exercises]$ docker run -v "/home/mai/web.log:/usr/src/app/text.log" devopsdockeruh/simple-web-service:alpine
```

## 1.10 Ports Open
```bash
[mai@mochi devopswithdocker-exercises]$ docker run -p 8080:8080 web-server
```

## 1.11 Spring
```Dockerfile
FROM openjdk:8

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .

RUN ./mvnw package

CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
```

## 1.12 Hello, Frontend!
```Dockerfile
FROM node:16-alpine

EXPOSE 5000

WORKDIR /usr/src/app

COPY . .

# to fix error https://stackoverflow.com/questions/69693907/error-err-package-path-not-exported-package-subpath-lib-tokenize-is-not-d
RUN rm package-lock.json
RUN npm install

RUN npm run build

RUN npm install -g serve

CMD ["serve", "-s", "-l", "5000", "build"]
```

## 1.13 Hello, Backend!
```Dockerfile
FROM golang:1.16-alpine

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .

RUN go build

CMD ["./server"]
```

```bash
[mai@mochi example-backend]$ docker build . -t backend && docker run -p 8080:8080 backend
```

## 1.14 Environment
```Dockerfile
# backend
FROM golang:1.16-alpine

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .
ENV REQUEST_ORIGIN http://localhost:5000
RUN go build

CMD ["./server"]
```

```Dockerfile
# frontend
FROM node:16-alpine

EXPOSE 5000

WORKDIR /usr/src/app

COPY . .

RUN rm package-lock.json
RUN npm install

ENV REACT_APP_BACKEND_URL http://localhost:8080
RUN npm run build

RUN npm install -g serve

CMD ["serve", "-s", "-l", "5000", "build"]
```

```bash
[mai@mochi example-frontend]$ docker build . -t frontend && docker run -p 5000:5000 frontend
```

```bash
[mai@mochi example-backend]$ docker build . -t backend && docker run -p 8080:8080 backend
```