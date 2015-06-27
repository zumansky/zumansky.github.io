---
layout: post
title: Send emails using R
---

I wanted to test the [mailR](https://github.com/rpremraj/mailR) package as I was looking for a way to get the results of some of my R programs that run remotely.

The [mailR](https://github.com/rpremraj/mailR) package as defined by  Rahul Premraj is a utility to send emails from the R programming environment. The package has multiple features including the ability to attach files from the local file system to the email.

First you'll need to install the mailR package (and rJava if not already installed) then load it:

```R
#install the mailR package
install.packages('rJava')
install.packages('mailR')
#load the library
library('mailR')
```
<p align="center">
  ![_config.yml]({{ site.baseurl }}/images/)
</p>



