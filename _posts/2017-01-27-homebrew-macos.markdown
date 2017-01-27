---
layout: post
title:  "Homebrew on macOS Sierra"
date:   2017-01-27 00:00:00 -0600
categories: docs
---
#### A guide for installing and configuring [Homebew](http://brew.sh){:target="_blank"} on macOS Sierra.

| Software         | Versions                       |
| :--------------- | :----------------------------- |
| macOS            | **10.12.3**                    |
| Xcode            | **8.2.1**                      |
| Homebrew         | **1.1.8**                      |

#### 1. Install Command Line Tools for Xcode

There are three options to install the command line tools for XCode:

  1. [Download](http://developer.apple.com/downloads){:target="_blank"} command line tools (login required)

  2. Install [Xcode](https://itunes.apple.com/us/app/xcode/id497799835){:target="_blank"} from App Store

  3. Install directly (easiest):

{% highlight bash %}
$ xcode-select --install
{% endhighlight %}

```
xcode-select: note: install requested for command line developer tools
```

Verify gcc

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

#### 2. Install Homebrew

{% highlight bash %}
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

#### 3. Verify /usr/local/bin is in the PATH environment variable

{% highlight bash %}
$ echo $PATH
{% endhighlight %}

````
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
````

If not add edit your ~/.bash_profile and add /usr/local/bin to your PATH:

{% highlight bash %}
export PATH="/usr/local/bin:$PATH"
{% endhighlight %}

Restart your shell or:
{% highlight bash %}
$ source ~/.bash_profile
$ echo $PATH
{% endhighlight %}

#### 4. Optionally disable Homebrew analytics

By default Homebrew analytics](https://git.io/brew-docs){:target="_blank"} now collects your usage history.

Edit your ~.bash_profile:

{% highlight bash %}
export HOMEBREW_NO_ANALYTICS=1
{% endhighlight %}

#### 5. Optionally set a Homebrew Github API Token

If you use brew often you may receive an API rate limit exceeded error message. You can set the HOMEBREW_GITHUB_API_TOKEN environment variable to correct this.

Generate a new Github [personal access token.](https://github.com/settings/tokens){:target="_blank"}

Edit your ~.bash_profile:

{% highlight bash %}
export HOMEBREW_GITHUB_API_TOKEN=<your_personal_access_token>
{% endhighlight %}

#### 6. Test Homebrew

Check if homebrew is installed:
{% highlight bash %}
$ brew --version
{% endhighlight %}
```
Homebrew 1.1.8
Homebrew/homebrew-core (git revision f753a; last commit 2017-01-26)
```

Update homebrew:
{% highlight bash %}
$ brew update
{% endhighlight %}
```
Already up-to-date.
```

Check for any problems:
{% highlight bash %}
$ brew doctor
{% endhighlight %}
