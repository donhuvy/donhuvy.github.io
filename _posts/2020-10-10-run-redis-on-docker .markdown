---
layout: post
title:  "Run Redis in Docker"
date:   2020-10-10 22:21:00 +0700
categories: Redis Docker
---
Run Redis in Docker

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

<img src="https://raw.githubusercontent.com/donhuvy/donhuvy.github.io/master/images/install-telnet.png" alt="Java roadmap" class="inline"/>

<!-- 
{% highlight xml %}

{% endhighlight %}
-->
