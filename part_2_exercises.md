# Exercises

## 2.1

```yml
services:
  simple-web-service:
    image: devopsdockeruh/simple-web-service
    volumes:
      - ./log.txt:/usr/src/app/text.log
```

## 2.2
```yml
services:
  simple-web-service:
    image: devopsdockeruh/simple-web-service
    command: server
    ports:
      - 8080:8080
```

## 2.3

```yml
services:
  frontend:
    ports:
      - 5000:5000
    build: ./frontend

  backend:
    build: ./backend
    ports:
      - 8080:8080
```