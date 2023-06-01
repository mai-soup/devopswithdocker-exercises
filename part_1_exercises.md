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
