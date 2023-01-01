# Lab 3

## Problem 1

```
version: '3.8'
services:
  nginx-react:
    image: nginxreactapp-i:1.0

    build:
      context: .
      dockerfile: Dockerfile

    container_name: ngixreactapp-c
    ports:
      - "80:80"
```

## Problem 2

* app.py
  
```
from flask import Flask
from redis import Redis
app = Flask(__name__)
redis = Redis(host='redis', port=6379)
@app.route('/')
def hello():
    redis.incr('hits')
    counter = str(redis.get('hits'),'utf-8')
    return "This webpage has been viewed "+counter+" time(s)"
if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

* reauirements.txt

```
flask
redis
```

* Dockerfile
  
```
FROM python:3.11.0a6-alpine3.15
WORKDIR /code
COPY requirements.txt /code
RUN pip install -r requirements.txt --no-cache-dir
COPY . /code
CMD python app.py
```

* docker-compose

```
services:
  redis: 
    image: redislabs/redismod
    ports:
      - '6379:6379' 
  redisinsight:
    image: redislabs/redisinsight:latest
    ports:
      - '8001:8001'
  web:
        build: .
        ports:
            - "5000:5000"
        volumes:
            - .:/code
        depends_on:
            - redis


```