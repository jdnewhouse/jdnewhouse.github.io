---
layout: post
title:  "Oracle JDK on macOS Sierra"
date:   2017-01-27 00:00:00 -0600
categories: docs
---
#### A guide for installing the [Oracle JDK](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html){:target="_blank"} on macOS Sierra.

| Software         | Versions                       |
| :--------------- | :----------------------------- |
| macOS            | **10.12.3**                    |
| Oracle JDK       | **1.8 (1.8.0_112)**            |


#### 1. Install Oracle JDK 1.8

  [Download](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html){:target="_blank"} and install the Oracle JDK 1.8 for "Mac OS X"

#### 2. Set JAVA_HOME environment variable

Add JAVA_HOME to your ~.bash_profile:

{% highlight bash %}
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
{% endhighlight %}

Restart your shell or:
{% highlight bash %}
$ source ~/.bash_profile
{% endhighlight %}


#### 3. Test Java

  Verify JAVA_HOME is set:

{% highlight bash %}
$ echo $JAVA_HOME
{% endhighlight %}
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_112.jdk/Contents/Home
```

  Verify Java is installed properly:

{% highlight bash %}
$ java -version
{% endhighlight %}
```
java version "1.8.0_112"
Java(TM) SE Runtime Environment (build 1.8.0_112-b16)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b16, mixed mode)
```
