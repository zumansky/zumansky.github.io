---
layout: post
title: Send emails using R
---

I wanted to test the [mailR](https://github.com/rpremraj/mailR) package as I was looking for a way to get the results of some of my R programs that run remotely.

The [mailR](https://github.com/rpremraj/mailR) package is, as defined by his author Rahul Premraj, a utility to send emails from the R programming environment. The package has multiple features including the ability to attach files from the local file system to the email.

First you'll need to install the mailR package then load it. Note that installing mailR package requires installing rJava. This is not always straightforward but plenty Stack Overflow's posts should help you to deal with that.

{% highlight R %}
#install the mailR package
install.packages('mailR')
#load the library
library('mailR')
{% endhighlight %}

For the current test I'll generate two distributions of 1000 numbers and plot their histograms on pdf files:

```R
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
```

{% highlight R %}
send.mail(from = 'sender@gmail.com',
		  to = c('recipient@gmail.com'),
		  subject = 'Subject of my email',
		  body = 'Body of my email',
		  smtp = list(host.name = 'smtp.gmail.com', port = 465, 
					  user.name = 'sender@gmail.com', passwd = 'senderpassword', 
					  ssl = TRUE),
		  authenticate = TRUE,
		  send = TRUE,
		  attach.files = c(pdfFileName1, pdfFileName2),
		  file.names = c('file1.pdf', 'file2.pdf'), # optional parameter
		  )
{% endhighlight %}



