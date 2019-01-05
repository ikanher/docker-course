# Part 1 - Excercises 1.1-1.8

## 1.1

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
075f73ca9e56        nginx               "nginx -g 'daemon of…"   18 seconds ago      Exited (0) 7 seconds ago                       festive_bardeen
a4a30b55bee5        nginx               "nginx -g 'daemon of…"   19 seconds ago      Exited (0) 4 seconds ago                       compassionate_davinci
f473c6e43b79        nginx               "nginx -g 'daemon of…"   21 seconds ago      Up 20 seconds              80/tcp              priceless_joliot
```

## 1.2

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

## 1.3

```bash
$ docker run --name install-curl -it ubuntu:16.04
root@e4fbbd1d31f9:/# apt-get update > /dev/null
root@e4fbbd1d31f9:/# apt-get -y install curl > /dev/null
debconf: delaying package configuration, since apt-utils is not installed
root@e4fbbd1d31f9:/# $
$ docker commit install-curl ubuntu:16.04
sha256:5203dfc0b111156b101684ab0766b5282ada43444844850fde19c676ed60278f
$ docker run -it --rm ubuntu:16.04 sh -c 'read website; sleep 3; curl http://$website'
helsinki.fi
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body></html>
```

## 1.4

Dockerfile:
```bash
FROM ubuntu:16.04

RUN apt-get update && apt-get install -y curl

WORKDIR /mydir
COPY curler.sh .
CMD ["./curler.sh"]
```

Run:
```bash
$ docker run --rm -it curler
```

## 1.5

Dockerfile:
```
FROM ubuntu:16.04
EXPOSE 5000

WORKDIR /mydir
RUN apt-get update && apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
COPY frontend-example-docker .
WORKDIR /mydir/frontend-example-docker
RUN npm install
CMD ["npm", "start"]
```

Run:
```bash
$ docker run --rm -p 5000:5000 -d frontend
```

## 1.6

Dockerfile:
```
FROM ubuntu:16.04
EXPOSE 8000

WORKDIR /mydir
RUN apt-get update && apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs
COPY backend-example-docker .
WORKDIR /mydir/backend-example-docker
RUN npm install
CMD ["npm", "start"]
```

Run:
```bash
$ docker run --rm --mount source=vol-ex1.6,target=/mydir -p 8000:8000 -d backend
```

## 1.7

Backend:
```
$ docker run -e "FRONT_URL=http://localhost:5000/" --rm --name backend --mount source=vol-ex1.6,target=/mydir -p 8000:8000 -d backend
```

Frontend:
```
$ docker run -e "API_URL=http://localhost:8000/" --rm --name frontend -p 5000:5000 -d frontend
```

## 1.8

[Categorized Bookmarks](https://hub.docker.com/r/ikanher/categorized-bookmarks)

