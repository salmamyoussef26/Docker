# Lab 2

### Problem 1:

```

FROM ubuntu:23.04

RUN  apt -y update
RUN  apt -y install nginx

COPY index.html /var/www/html
COPY index.tar /var/www/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
--------------------------------------
 sudo docker build -t mynginx:3.0 .
 sudo docker run -p 8080:80 --name testnginx2 mynginx:3.0

```

### Problem 2:

* Single-staged:

```
FROM node:alpine3.16

#folder app inside the container
WORKDIR /app

#copy src "my_react_app files" >> dest "path inside the container /app"
COPY package*.json ./

#install libs and dependencies
RUN npm install

#copy src "code " >> dest "/app"
COPY . .

EXPOSE 3000
#start app
CMD [ "npm", "start" ]
```

* Multi-staged:

```
FROM node:alpine3.16 AS builder

#folder app inside the container
WORKDIR /app

#copy src "my_react_app files" >> dest "path inside the container /app"
COPY package*.json ./

#install libs and dependencies
RUN npm install

#copy src "code " >> dest "/app"
COPY . .

EXPOSE 80
#start app
RUN npm run build


FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html

ENTRYPOINT [ "nginx", "-g", "daemon off;" ]
```

