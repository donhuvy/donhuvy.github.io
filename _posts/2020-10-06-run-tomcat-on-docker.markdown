---
layout: post
title:  "Run Tomcat in Docker"
date:   2020-10-06 00:22:00 +0700
categories: Tomcat Docker Java
---
Run Tomcat in Docker

```
docker run --rm --name tomcat-server -p 8080:8080 tomcat:latest
docker exec -it tomcat-server /bin/bash
mv webapps webapps2
mv webapps.dist/ webapps
exit
```

From web browser, go to: [http://localhost:8080](http://localhost:8080/)

<!-- 
{% highlight xml %}

{% endhighlight %}
-->
