# Part 3 Excercices

## 3.1

Size before
```
backend                         latest              1387fd519189        About a minute ago   342MB
frontend                        latest              eb224b31e1d5        3 seconds ago       402MB
```

Size after
```
backend                         latest              ca01d024ee37        23 seconds ago      269MB
frontend                        latest              91e400259517        20 seconds ago      364MB
```

Backend Dockerfile
```
FROM ubuntu:18.04

WORKDIR /app
COPY ./backend-example-docker/package.json ./package.json

RUN apt-get update && \
    apt-get install -y curl gnupg && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt-get install -y nodejs && \
    apt-get purge -y --auto-remove curl gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    npm install

COPY ./backend-example-docker ./

EXPOSE 8000

CMD ["npm", "start"]
```

Frontend Dockerfile
```
FROM ubuntu:18.04

WORKDIR /app
COPY ./frontend-example-docker/package.json .

RUN apt-get update && \
    apt-get install -y curl gnupg && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt install -y nodejs && \
    apt-get purge -y --auto-remove curl gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    npm install

COPY ./frontend-example-docker ./

EXPOSE 5000

CMD ["npm", "start"]
```

## 3.2

Dockerfile
```
FROM ubuntu:18.04

WORKDIR /videos

RUN apt-get update && \
    apt-get install -y \
    ffmpeg \
    python3 \
    python3-pip \
    wget && \
    pip3 install yle-dl && \
    rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/usr/local/bin/yle-dl"]
```

Running
```
$ docker run --rm -v $PWD:/videos yle-dl https://areena.yle.fi/1-3395140
```

## 3.3

Frontend
```
FROM ubuntu:18.04

WORKDIR /app
COPY ./frontend-example-docker/package.json .

RUN apt-get update && \
    apt-get install -y curl gnupg && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt install -y nodejs && \
    apt-get purge -y --auto-remove curl gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    npm install && \
    useradd -m app && \
    chown -R app /app

COPY ./frontend-example-docker ./

EXPOSE 5000

USER app

CMD ["npm", "start"]
```

Backend
```
FROM ubuntu:18.04

WORKDIR /app
COPY ./backend-example-docker/package.json ./package.json

RUN apt-get update && \
    apt-get install -y curl gnupg && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt-get install -y nodejs && \
    apt-get purge -y --auto-remove curl gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    npm install && \
    useardd -m app && \
    chown -R app /app

COPY ./backend-example-docker ./

EXPOSE 8000

USER app

CMD ["npm", "start"]
```

## 3.4

Sizes before
```
frontend                        latest              d794f5f5a311        12 minutes ago      364MB
backend                         latest              8169967b7a99        26 hours ago        269MB
```

Sizes after
```
frontend                        latest              e22f882f4d18        33 seconds ago      234MB
backend                         latest              1d68eb1a164f        30 seconds ago      140MB
```

Frontend
```
FROM node:10-alpine

WORKDIR /app
COPY ./frontend-example-docker/package.json .

RUN npm install && \
    chown -R node /app

COPY ./frontend-example-docker ./

EXPOSE 5000

USER node

CMD ["npm", "start"]
```

Backend
```
FROM node:10-alpine

WORKDIR /app
COPY ./backend-example-docker/package.json ./package.json

RUN npm install && \
    chown -R node /app

COPY ./backend-example-docker ./

EXPOSE 8000

USER node

CMD ["npm", "start"]
```

## 3.5

Before
```
FROM python

WORKDIR /opt/categorized-bookmarks
RUN apt-get update && apt-get install -y git
RUN git clone https://github.com/ikanher/categorized-bookmarks .
RUN python3 -m venv venv
RUN /bin/bash -c "source venv/bin/activate"
RUN pip install -r requirements.txt
CMD ["gunicorn", "--preload", "--bind", "0.0.0.0:80", "--workers", "1", "application:app"]
```

After
```
FROM python:3.7-alpine

COPY ./categorized-bookmarks /app

WORKDIR /app

RUN apk add --no-cache libpq postgresql-dev && \
    apk add --no-cache --virtual .build-deps build-base python3-dev libffi-dev && \
    pip install -r requirements.txt --no-cache-dir && \
    apk --purge del .build-deps && \
    adduser -S app && \
    chown -R app /app

USER app

CMD ["gunicorn", "--preload", "--bind", "0.0.0.0:5000", "--workers", "1", "application:app"]
```

