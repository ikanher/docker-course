# Part 2 Excercices

## 2.1

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: frontend
        ports:
            - 5000:5000
        container_name: frontend
        environment:
            - API_URL=http://localhost:8000

    backend:
        build: backend
        ports:
            - 8000:8000
        volumes:
            - vol-ex2.1:/mydir
        container_name: backend
        environment:
            - FRONT_URL=http://localhost:5000

volumes:
    vol-ex2.1:
```

frontend/Dockerfile:
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

backend/Dockerfile:
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

## 2.2

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: frontend
        ports:
            - 5000:5000
        container_name: frontend
        environment:
            - API_URL=http://localhost:8000

    backend:
        build: backend
        ports:
            - 8000:8000
        volumes:
            - vol:/mydir
        container_name: backend
        environment:
            - FRONT_URL=http://localhost:5000
            - REDIS=redis
    redis:
        image: redis

volumes:
    vol:
```

## 2.3

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: frontend
        ports:
            - 5000:5000
        container_name: frontend
        environment:
            - API_URL=http://localhost:8000

    backend:
        build: backend
        ports:
            - 8000:8000
        volumes:
            - vol:/mydir
        container_name: backend
        environment:
            - FRONT_URL=http://localhost:5000
            - REDIS=redis
            - DB_USERNAME=webpack-app
            - DB_PASSWORD=webpack-app
            - DB_NAME=webpack-app
            - DB_HOST=postgres
        depends_on:
            - postgres

    redis:
        image: redis

    postgres:
        image: postgres
        environment:
            - POSTGRES_USER=webpack-app
            - POSTGRES_PASSWORD=webpack-app
            - POSTGRES_DB=webpack-app

volumes:
    vol:
```

## 2.4

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: ml-kurkkumopo-frontend
        ports:
            - 3000:3000
        container_name: frontend

    backend:
        build: ml-kurkkumopo-backend
        ports:
            - 5000:5000
        volumes:
            - model:/src/model
        container_name: backend
        depends_on:
            - training

    training:
        build: ml-kurkkumopo-training
        volumes:
            - model:/src/model
            - imgs:/src/imgs

volumes:
    model:
    imgs:
```

## 2.5

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: frontend
        container_name: frontend
        environment:
            - API_URL=http://localhost/api

    backend:
        build: backend
        volumes:
            - vol:/mydir
        container_name: backend
        environment:
            - DEBUG=express:*
            - REDIS=redis
            - DB_USERNAME=webpack-app
            - DB_PASSWORD=webpack-app
            - DB_NAME=webpack-app
            - DB_HOST=postgres
        depends_on:
            - postgres

    redis:
        image: redis

    postgres:
        image: postgres
        environment:
            - POSTGRES_USER=webpack-app
            - POSTGRES_PASSWORD=webpack-app
            - POSTGRES_DB=webpack-app

    nginx:
        image: nginx
        volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf:ro
        ports:
            - 80:80
        depends_on:
            - backend
            - frontend

volumes:
    vol:
```

## 2.6

docker-compose.yml:
```
version: '3.5'

services:
    frontend:
        build: frontend
        container_name: frontend
        environment:
            - API_URL=http://localhost/api

    backend:
        build: backend
        volumes:
            - vol:/mydir
        container_name: backend
        environment:
            - DEBUG=express:*
            - REDIS=redis
            - DB_USERNAME=webpack-app
            - DB_PASSWORD=webpack-app
            - DB_NAME=webpack-app
            - DB_HOST=postgres
        depends_on:
            - postgres

    redis:
        image: redis
        volumes:
            - redis-data:/data

    postgres:
        image: postgres
        environment:
            - POSTGRES_USER=webpack-app
            - POSTGRES_PASSWORD=webpack-app
            - POSTGRES_DB=webpack-app
        volumes:
             - postgres-data:/var/lib/postgresql/data

    nginx:
        image: nginx
        volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf:ro
        ports:
            - 80:80
        depends_on:
            - backend
            - frontend

volumes:
    vol:
    redis-data:
    postgres-data:
```
