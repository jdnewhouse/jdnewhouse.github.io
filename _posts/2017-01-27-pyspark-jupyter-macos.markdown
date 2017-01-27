---
layout: post
title:  "PySpark with Jupyter on macOS Sierra"
date:   2017-01-27 00:00:00 -0600
categories: docs
---
#### A guide for setting up PySpark with [The Jupyter Notebook](http://jupyter.org/){:target="_blank"} on macOS Sierra.

| Software         | Versions                       |
| :--------------- | :----------------------------- |
| macOS            | **10.12.3**                    |
| Xcode            | **8.2.1**                      |
| Oracle JDK       | **1.8 (1.8.0_112)**            |
| Python           | **3.5.2 (Anaconda 4.2.0)**     |
| Jupyter          | **4.2.0 (Anaconda 4.2.0)**     |
| Apache Spark     | **2.1.0**                      |
| Homebrew         | **1.1.8**                      |


## Prerequisites

[Install Oracle's JDK 1.8.0](/docs/2017/01/27/java-macos.html){:target="_blank"} if it's not already installed:

#### 1. Test Java

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

#### 2. Test and update Homebrew

[Install Homebrew](/docs/2017/01/27/homebrew-macos.html){:target="_blank"} if it's not already installed.

{% highlight bash %}
$ brew --version
{% endhighlight %}
```
Homebrew 1.1.8
Homebrew/homebrew-core (git revision f753a; last commit 2017-01-26)
```

{% highlight bash %}
$ brew update
{% endhighlight %}
```
Already up-to-date.
```

{% highlight bash %}
$ brew doctor
{% endhighlight %}

#### 3. Verify gcc

{% highlight bash %}
$ gcc -v
{% endhighlight %}

```
Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 8.0.0 (clang-800.0.42.1)
Target: x86_64-apple-darwin16.4.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

## Anaconda

#### 1. Install Anaconda

[Download](https://www.continuum.io/downloads){:target="_blank"} Anaconda 4.2.0 for OSX (Python 3.5) and install. (Graphical or Command-Line Installer)

#### 2. Verify Anaconda is in PATH environment variable

Verify [/your_path]/anaconda/bin was added to PATH environment variable in ~/.bash_profile:

{% highlight bash %}
$ echo $PATH
{% endhighlight %}

````
[/your_path]/anaconda/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
````

If not add it your ~/.bash_profile

{% highlight bash %}
export PATH="[/your_path]/anaconda/bin:$PATH"
{% endhighlight %}

Restart your shell or:
{% highlight bash %}
$ source ~/.bash_profile
$ echo $PATH
{% endhighlight %}

#### 3. Verify Jupyter

Start Jupyter:

{% highlight bash %}
$ jupyter notebook
{% endhighlight %}

Browser should have opened to [http://localhost:8888](http://localhost:8888){:target="_blank"}

![jupyter](/assets/imgs/jupyter.png)

Close Jupyter **(control-c)**

## Spark

#### 1. Install Apache Spark

{% highlight bash %}
$ brew update
$ brew install apache-spark
{% endhighlight %}

````
...
/usr/local/Cellar/apache-spark/2.1.0: 1,276 files, 213.2M, built in 20 seconds
````

#### 2. Test pyspark

{% highlight bash %}
$ pyspark
{% endhighlight %}

```
Python 3.5.2 |Anaconda 4.2.0 (x86_64)| (default, Jul  2 2016, 17:52:12)
[GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
17/01/26 18:16:48 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/01/26 18:16:51 WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.1.0
      /_/

Using Python version 3.5.2 (default, Jul  2 2016 17:52:12)
SparkSession available as 'spark'.
>>>
```

Quit PySpark  

{% highlight bash %}
>>> quit()
{% endhighlight %}


## PySpark with Jupyter Notebook

#### 1. Create home folder for Jupyter notebooks

{% highlight bash %}
$ mkdir ~/notebooks
{% endhighlight %}

#### 2. Create pyspark-jupyter bash script

Create pyspark-jupyter file

{% highlight bash %}
$ touch /usr/local/bin/pyspark-jupyter
$ chmod +x /usr/local/bin/pyspark-jupyter
{% endhighlight %}

Edit /usr/local/bin/pyspark-jupyter

{% highlight bash %}
#!/usr/bin/env bash

# NOTEBOOKS_HOME environment variable
export NOTEBOOKS_HOME=~/notebooks

# Set pyspark driver for jupyter
export PYSPARK_DRIVER_PYTHON="jupyter"

# Set port to 8888 & notebooks directory
export PYSPARK_DRIVER_PYTHON_OPTS="notebook --port=8888 --notebook-dir=$NOTEBOOKS_HOME"

# Launch pyspark
pyspark --master local --packages com.databricks:spark-csv_2.10:1.5.0 --executor-memory 6400M --driver-memory 6400M
{% endhighlight %}

Note the following arguments used with PySpark:

  1. master: local (Spark master is local)
  2. packages: --packages com.databricks:spark-csv_2.10:1.5.0
  3. executor-memory: 6400M
  4. driver-memory: 6400M

#### 3. Start PySpark with Jupyter

{% highlight bash %}
$ pyspark-jupyter
{% endhighlight %}

```
[I 12:05:16.373 NotebookApp] [nb_conda_kernels] enabled, 2 kernels found
[I 12:05:16.761 NotebookApp] ✓ nbpresent HTML export ENABLED
[W 12:05:16.761 NotebookApp] ✗ nbpresent PDF export DISABLED: No module named 'nbbrowserpdf'
[I 12:05:16.795 NotebookApp] [nb_anacondacloud] enabled
[I 12:05:16.798 NotebookApp] [nb_conda] enabled
[I 12:05:16.801 NotebookApp] Serving notebooks from local directory: [your_path]/notebooks
[I 12:05:16.801 NotebookApp] 0 active kernels
[I 12:05:16.801 NotebookApp] The Jupyter Notebook is running at: http://localhost:8888/
[I 12:05:16.801 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
```
#### 4. Test PySpark with Jupyter

[Download Frankenstein](https://www.gutenberg.org/ebooks/){:target="_blank"} or any book of choice and save if to your ~/notebooks folder.

Create a new notebook in Jupyter (Python default) or [download](/assets/notebooks/pyspark_test.ipynb){:target="_blank"} this example.

{% highlight python %}
# Source file to read
textFile = sc.textFile("84.txt")
{% endhighlight %}

{% highlight python %}
# Number of items in this RDD
textFile.count()
{% endhighlight %}

````
7653
````

{% highlight python %}
# First item in this RDD
textFile.first()
{% endhighlight %}

````
"Project Gutenberg's Frankenstein, by Mary Wollstonecraft (Godwin) Shelley"
````

{% highlight python %}
# Search for a specific term
numSearch = textFile.filter(lambda line: 'Frankenstein' in line).count() # How many lines contain text
print (numSearch)
{% endhighlight %}

````
30
````

{% highlight python %}
# Word count

counts = textFile.flatMap(lambda line: line.split(" ")) \
    .map(lambda word: (word.lower() \
        .replace('http', '') \
        .replace(' ', '') \
        .replace('\'', '') \
        .replace(':', '') \
        .replace('[', '') \
        .replace(']', '') \
        .replace('-', '') \
        .replace('(', '') \
        .replace(')', '') \
        .replace('!', '') \
        .replace('?', '') \
        .replace('.', '') \
        .replace('#', '') \
        .replace('%', '') \
        .replace('$', '') \
        .replace('*', '') \
        .replace('?', '') \
        .replace('@', '') \
        .replace('^', '') \
        .replace('"', '') \
        .replace(',', '') \
        .replace('_', '') \
        .replace('\\', '') \
        .replace('/', '') \
        .replace('0', '') \
        .replace('1', '') \
        .replace('2', '') \
        .replace('3', '') \
        .replace('4', '') \
        .replace('5', '') \
        .replace('6', '') \
        .replace('7', '') \
        .replace('8', '') \
        .replace('9', '') \
        .replace(';', ''), 1)) \
    .reduceByKey(lambda a, b: a + b) \
    .sortByKey()

output = counts.collect()

for (word, count) in output:
    print("%s: %i" % (word, count))
{% endhighlight %}
