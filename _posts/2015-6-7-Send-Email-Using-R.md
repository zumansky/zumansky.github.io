---
layout: post
title: Send emails using R
---

I wanted to test the [mailR](https://github.com/rpremraj/mailR) package as I was looking for a way to get the results of some of my R programs that run remotely.

As defined by his author Rahul Premraj, [mailR](https://github.com/rpremraj/mailR) is a utility to send emails from the R programming environment. The package has multiple features including the ability to attach files from the local file system to the email.

First you'll need to install the mailR package and load it. Note that mailR requires installing [rJava](http://cran.r-project.org/web/packages/rJava/index.html). This is not always straightforward but plenty of Stack Overflow's posts should help you with that.

{% highlight R %}
#install the mailR package
install.packages('mailR')
#load the library
library('mailR')
{% endhighlight %}

For the current test I'll generate two distributions of 1000 numbers and plot their histograms on pdf files:

{% highlight R %}
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

The send.mail function allows you to send an email via SMTP server with one or more attachments. You'll need to setup the emails of the sender and the recipient and include the username and password. Note that for this to work you will need to change an option in gmail by removing extra security [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps).
 
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
		  file.names = c('file1.pdf', 'file2.pdf') # optional parameter
		  )
{% endhighlight %}

As I don't really like the idea of putting the password in the code of my R program, I've first tried to use the aspmx.l.google.com server that works for gmail recipients only but does not require authentication. 

{% highlight R %}
send.mail(from = 'sender@gmail.com',
          to = c('recipient@gmail.com'),
          subject = "Subject of the email",
          body = "Body of the email",
          smtp = list(host.name = "aspmx.l.google.com", port = 25),
          authenticate = FALSE,
          send = TRUE,
          attach.files = c(pdfFileName1, pdfFileName2),
          file.names = c('file1.pdf', 'file2.pdf') # optional parameter
          )
{% endhighlight %}

If you get the following error with the previous code, check whether you have enabled [SMTP access for gmail](https://www.google.com/settings/security/lesssecureapps) or you need to configure R to connect via a proxy. 

{% highlight R %}
Error in ls(envir = envir, all.names = private) : 
  invalid 'envir' argument
{% endhighlight %}

If you’re still having the same issue you can set the debug parameter of send.mail to TRUE to get detailed logs and check the [tickets on github](https://github.com/rpremraj/mailR/issues). 

I did get the previous error initially, when I was trying to run it from home. After a while, I’ve figured out that my IPS was blocking the access to port 25 and I managed to make it work from a different location.

In addition to attaching files mailR has other interesting features as sending HTML formatted emails. Please refer to it's repository on [github](https://github.com/rpremraj/mailR) for more information.

The codeblocks of this post are available [here](https://github.com/zumansky/dev/blob/master/R/SendEmail/sendEmails.r).



