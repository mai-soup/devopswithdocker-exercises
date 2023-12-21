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