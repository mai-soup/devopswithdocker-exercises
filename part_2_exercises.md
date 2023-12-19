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

## 2.4
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
    environment:
      - REDIS_HOST=redis
    depends_on:
      - redis

  redis:
    image: redis
    restart: unless-stopped
```

## 2.5
```bash
$ docker compose up --scale compute=2
```

## 2.6
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
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
    depends_on:
      - redis
      - postgres

  redis:
    image: redis
    restart: unless-stopped

  postgres:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=postgres
```

## 2.7
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
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
    depends_on:
      - redis
      - postgres

  redis:
    image: redis
    restart: unless-stopped

  postgres:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - ./data:/var/lib/postgresql/data
```

## 2.8 (and 2.10, on accident)
```yml
services:
  frontend:
    build: ./frontend
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:80

  backend:
    build: ./backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - REQUEST_ORIGIN=http://localhost:80
    depends_on:
      - redis
      - postgres

  redis:
    image: redis
    restart: unless-stopped

  postgres:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - ./data:/var/lib/postgresql/data

  nginx:
    image: nginx
    ports:
      - 8080:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - frontend
      - backend
```

## 2.9
For the `docker-compose.yml`, I moved the environment variables here from the Dockerfiles, as well as fixed the `REQUEST_ORIGIN` var in the backend service to have the correct port.
```yml
services:
  frontend:
    build: ./frontend
    environment:
      - REACT_APP_BACKEND_URL=http://localhost:80

  backend:
    build: ./backend
    environment:
      - REDIS_HOST=redis
      - POSTGRES_HOST=postgres
      - REQUEST_ORIGIN=http://localhost:8080
    depends_on:
      - redis
      - postgres

  redis:
    image: redis
    restart: unless-stopped

  postgres:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - ./data:/var/lib/postgresql/data

  nginx:
    image: nginx
    ports:
      - 8080:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - frontend
      - backend
```
Most of the updates were in `nginx.conf`:
```
events { worker_connections 1024; }

http {
  server {
    listen 80;

    location / {
      proxy_pass http://frontend:5000/;
    }

    # configure here where requests to http://localhost/api/...
    # are forwarded
    location /api/ {
      proxy_set_header Host $host;
      proxy_pass http://backend:8080/;
    }

    location /ping {
      proxy_pass http://backend:8080/ping;
    }

    location /messages {
      proxy_pass http://backend:8080/messages;
    }
  }
}
```
The Dockerfiles stayed mostly the same:
```Dockerfile
# backend
FROM golang:1.16-alpine

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .
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

RUN npm run build

RUN npm install -g serve

CMD ["serve", "-s", "-l", "5000", "build"]
```

## 2.10
See 2.8 above.