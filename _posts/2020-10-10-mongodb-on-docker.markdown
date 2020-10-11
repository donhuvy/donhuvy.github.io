---
layout: post
title:  "MongoDB on Docker"
date:   2020-10-10 22:21:00 +0700
categories: Redis Docker
---
MongoDB in Docker

File `docker-compose.yml`

```yml
version: "3.8"

services:
  redis:
    image: redis
    volumes:
      - ./data:/data
    ports:
      - 6379:6379
```


```cmd
docker pull redis
docker-compose up
docker-compose up -d
docker container ls
telnet localhost 6379
```

<img src="https://raw.githubusercontent.com/donhuvy/donhuvy.github.io/master/images/install_telnet.png" alt="Instll telnet" class="inline"/>

Telnet, type `PING` then press Enter key (you will not see text), then see result: `PONG`. Type `quit` to exit.

At folder where has file `docker-compose.yml` , type command:

```cmd
docker-compose stop redis
```
See what is running

```cmd
docker container ls
```
You will see, docker redis was stoped.

```
docker image prune -a
docker-compose up
```

Docker RedisInsight

```
docker run -v redisinsight:/db -p 8001:8001 redislabs/redisinsight:latest
```
Wait about 6 minutes (at internet speed at 22:30) for downloading, unzip, install, starting.


go to: [http://localhost:8001/](http://localhost:8001/) (auto open web browser)

You also maybe want use Redis for Windows (not official support: [https://github.com/MicrosoftArchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.msi](https://github.com/MicrosoftArchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.msi)). It will install at `C:\Program Files\Redis\`

<!-- 
{% highlight xml %}

{% endhighlight %}
-->
