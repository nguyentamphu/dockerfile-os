# Dockerfile for `Ruby`
`Dockerfile` for `Ruby environtment` each `Operation system` (`Ubuntu, Centos`)

### Note: I'm using `Powershell on Windows 10`

- **Ubuntu 16.04**

    *Usage:*
1) Move to `ubuntu1604` directory
```bash
PS C:\Users\phunt\workspace\docker\ubuntu1604> cd ubuntu1604

PS C:\Users\phunt\workspace\docker\ubuntu1604> ls

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        19-Apr-18     15:27                supervisord
-a----        19-Apr-18     16:09           1062 Dockerfile
```

2) Build `image` from `Dockerfile`
```bash
PS C:\Users\phunt\workspace\docker\ubuntu1604> docker build -t ubuntu1604-ruby .
```

```bash
PS C:\Users\phunt\workspace\docker\ubuntu1604> docker images
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
ubuntu1604-ruby                     latest              12079e28efb3        26 minutes ago      1.12GB
```

3) Run `container` from above image `ubuntu1604-ruby`
```bash
PS C:\Users\phunt\workspace\docker\ubuntu1604> docker run -itd --name ubuntu1604-ruby -p 2222:22 ubuntu1604-ruby
```
   > * `-d`: run `daemon` mode.
   > * `-p`: `mapping` port `ssh` from `host` to `container`
     (ssh from `host`: `ssh root@localhost -p2222`, and then input `password`).
   > * After this `command` completed, the `mysql` service will be start, but it takes a few minutes.
     
4) Exec into above `container` (`ubuntu1604-ruby`)
```bash
PS C:\Users\phunt\workspace\docker\ubuntu1604> docker exec -it ubuntu1604-ruby bash
```

*To be continue...*
