---
layout: post
title: Send emails using R
---

I wanted to test the [mailR](https://github.com/rpremraj/mailR) package as I was looking for a way to get the results of some of my R programs that run remotely.

The [mailR](https://github.com/rpremraj/mailR) package is, as defined by his author Rahul Premraj, a utility to send emails from the R programming environment. The package has multiple features including the ability to attach files from the local file system to the email.

First you'll need to install the mailR package (and rJava if not already installed) then load it:

{% highlight R linenos=table%}
#install the mailR package
install.packages('rJava')
install.packages('mailR')
#load the library
library('mailR')
{% endhighlight %}

For the current test I'll generate two distributions of 1000 numbers and plot their histograms on pdf files:

{% highlight R linenos=table%}
#set the pseudo random number generator seed so we can get the same results
#generate 1000 gaussian/uniform numbers
set.seed(1); gNum <- rnorm(1000)
set.seed(1); uNum <- runif(1000)
#path where the pdf will be stored locally, make sure you use a defined path
pdfFileName1 <- '/tmp/histoGauss.pdf'
pdfFileName2 <- '/tmp/histoUnif.pdf'

#plot the first pdf
pdf(file = pdfFileName1)
hist(gNum, main='gaussian distribution')
graphics.off()

#plot the second pdf
pdf(file = pdfFileName2)
hist(uNum, main='uniform distribution')
graphics.off()
{% endhighlight %}

<p align="center">
  ![_config.yml]({{ site.baseurl }}/images/)
</p>



