# Exercises

## 3.1
The workflow can be found in this repo's `.github/workflows`. Here is the Docker Compose file used to test it:
```yml
services:
  app:
    image: maisoup/testing:latest
    ports:
      - 8080:8080
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - WATCHTOWER_POLL_INTERVAL=30
      - WATCHTOWER_LABEL_ENABLE=true
    depends_on:
      - app
```

## 3.2
The repo is [Pink Noise Generator](https://github.com/mai-soup/pink-noise-generator). Please note that the containerised version link is the Render one.

## 3.3
```bash
#!/bin/bash

# expects 2 args
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <github_username/github_repo> <dockerhub_username/docker_image>"
    exit 1
fi

github_repo="$1"
docker_image="$2"

git clone "https://github.com/$github_repo" || { echo "Error: Unable to clone the GitHub repository."; exit 1; }

repo_name=$(basename "$github_repo")
cd "$repo_name" || { echo "Error: Unable to change to the repository directory."; exit 1; }

docker build -t "$docker_image" . || { echo "Error: Unable to build Docker image."; exit 1; }

docker login || { echo "Error: Unable to log in to Docker Hub."; exit 1; }

docker push "$docker_image" || { echo "Error: Unable to push Docker image to Docker Hub."; exit 1; }

# cleanup
cd ..
rm -rf "$repo_name"

echo "Build and push completed successfully."
exit 0
```

## 3.4
```Dockerfile
FROM docker:dind

WORKDIR /app
ADD ./builder.sh .

RUN chmod +x ./builder.sh
RUN apk add git

ENTRYPOINT ["/app/builder.sh"]
```

```sh
#!/bin/sh

# expects 2 args
if [[ -z "${DOCKERHUB_USERNAME}" ]]; then
    echo "Set the DOCKERHUB_USERNAME environment variable."
    exit 1
fi

if [[ -z "${DOCKERHUB_TOKEN}" ]]; then
    echo "Set the DOCKERHUB_TOKEN environment variable."
    exit 1
fi

if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <github_username/github_repo> <dockerhub_username/docker_image>"
    exit 1
fi

github_repo="$1"
docker_image="$2"

git clone "https://github.com/$github_repo" || { echo "Error: Unable to clone the GitHub repository."; exit 1; }

repo_name=$(basename "$github_repo")
cd "$repo_name" || { echo "Error: Unable to change to the repository directory."; exit 1; }

docker build -t "$docker_image" . || { echo "Error: Unable to build Docker image."; exit 1; }

docker login -u "${DOCKERHUB_USERNAME}" -p "${DOCKERHUB_TOKEN}" || { echo "Error: Unable to log in to Docker Hub."; exit 1; }

docker push "$docker_image" || { echo "Error: Unable to push Docker image to Docker Hub."; exit 1; }

# cleanup
cd ..
rm -rf "$repo_name"

echo "Build and push completed successfully."
exit 0
```

## 3.5
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

RUN adduser -D appuser

RUN chown appuser .

USER appuser

CMD ["serve", "-s", "-l", "5000", "build"]
```

```Dockerfile
# backend
FROM golang:1.16-alpine

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .
ENV REQUEST_ORIGIN http://localhost:5000
RUN go build

RUN adduser -D appuser

RUN chown appuser .

USER appuser

CMD ["./server"]
```

### 3.6
Before optimisations (though I started with node and golang alpine base images in the first place), the frontend image is 554 MB and the backend is 447 MB.
After merging the RUN commands, the sizes are the same.

### 3.7
As I had used Alpine base images before already for improved speed, I rewrote the Dockerfiles to use the non-alpine images.
The backend image increased from 447 MB to 1.07 GB, and the frontend from 554 MB to 1.35 GB.
### 3.8
After separating the build and serve stages, the frontend image comes down to 129 MB.
### 3.9
The backend image got down to 18.1 MB with the following Dockerfile:
```Dockerfile
FROM golang:1.16-alpine as build-stage

WORKDIR /usr/src/app

COPY . .

RUN apk update && apk add --no-cache git && \
  go get -d -v && \
  go build -o ./build

FROM scratch

EXPOSE 8080
ENV REQUEST_ORIGIN http://localhost:5000

WORKDIR /usr/src/app

COPY --from=build-stage /usr/src/app/build /usr/src/app

CMD ["./server"]
```
### 3.10
Optimised container size for my [Pink Noise Generator app](https://github.com/mai-soup/pink-noise-generator). Links to Dockerfiles: [before](https://github.com/mai-soup/pink-noise-generator/blob/f6d1fe6ff51abb707cc1cde62ab587673c9856ce/Dockerfile) and [after](https://github.com/mai-soup/pink-noise-generator/blob/da28082763ccf2445286b3eeacf2cc319f39a866/Dockerfile).

### 3.11
Skipping this one, worked on it for a couple hours and ended up feeling like I didn't even have enough knowledge of Kubernetes to even know what I was supposed to be searching for.
