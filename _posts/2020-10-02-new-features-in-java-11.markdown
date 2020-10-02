---
layout: post
title:  "New features in Java 11"
date:   2020-10-02 21:45:00 +0700
categories: java blog
---
New features in Java 11:

<img src="https://raw.githubusercontent.com/donhuvy/donhuvy.github.io/master/images/java_roadmap.png" alt="Java roadmap" class="inline"/>

Old way

{% highlight java %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.URL;
import java.net.URLConnection;

public class Java10varOld {

    public static void main(String[] args) throws IOException {
        URL mp = new URL("https://mpfamily.vn/");
        URLConnection connection = mp.openConnection();
        Reader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
    }

}
{% endhighlight %}

New way

{% highlight java %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.URL;

public class Java10varNew {

    public static void main(String[] args) throws IOException {
        var mp = new URL("https://mpfamily.vn/");
        var connection = mp.openConnection();
        var reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
    }

}
{% endhighlight %}

go to http://localhost:4000
