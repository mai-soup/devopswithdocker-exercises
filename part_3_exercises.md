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