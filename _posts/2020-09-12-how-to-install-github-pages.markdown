---
layout: post
title:  "How to install GitHub pages"
date:   2020-10-04 08:45:00 +0700
categories: github blog
---
Need install:

- [Ruby+Devkit 2.6.6-1 (x64)](https://rubyinstaller.org/downloads/)

- [Ruby gem 3.1.4](https://rubygems.org/rubygems/rubygems-3.1.4.zip)

- [Visual Studio Code 1.49](https://code.visualstudio.com/)

Check version of programming language, plug-in:

{% highlight cmd %}
$ ruby -v
ruby 2.6.6p146 (2020-03-31 revision 67876) [x64-mingw32]

$ gem -v
3.1.4
{% endhighlight %}

{% highlight cmd %}
gem install jekyll bundler
jekyll new myblog
cd myblog
bundle exec jekyll serve
{% endhighlight %}

go to [http://localhost:4000](http://localhost:400)
