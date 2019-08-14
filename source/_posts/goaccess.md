---
title: Goaccess
date: 2019-08-14 15:18:06
tags:
---
### Server Monitoring
Have you ever wondered that watching logs of your apache2 can be a big headache. Then you are the audience for this post.
In order to do this we are going to use an open source tool called goAccess.

### Install goAccess
If you are luckily on Ubuntu you can just use ```apt-get``` like so:
```
sudo apt-get install goaccess
```
If you aren't on ubuntu or face some issues you can refer goaccess's [documentation](https://goaccess.io/download#distro).

### Configuring
You can make a few changes to the configuration to avoid specifying everything while starting up. I am on apache2 virtual hosting multiple tomcat installations. You can refer [here](../apache2setup) for more details on how to get that up and running.
Anyways in my setup I opened the configuration and did the following changes:
```
vi /usr/local/etc/goaccess/goaccess.conf
```
Uncomment the following lines:
```
date-format %d/%b/%Y
```
```
log-format %h %^[%d:%t %^] "%r" %s %b "%R" "%u"
```

### Running
You can run goaccess as a command line (not so cool) or on the browser (cooler) or on the browser with real-time updates (coolest).
Let's go through these methods one by one:

#### Command line
Once the configuration is modified you can just run goaccess with the following command specifying the configuration file and the location of the log file. If you want to view old log files you can try specifying that:
```
goaccess -p /usr/local/etc/goaccess/goaccess.conf /var/log/apache2/access.log
```

#### Browser
In order to see these details on a browser you can generate a static file with the following command:
```
goaccess -p /usr/local/etc/goaccess/goaccess.conf /var/log/apache2/access.log -a -o /root/vv/report.html
```
You can then serve this report.html with a simple python one-liner:
```
python3 -m http.server 8000
```

#### Browser with realtime
In order to see the same cool view that you get on the browser but with real-time updates you can try the following command:
```
goaccess -p /usr/local/etc/goaccess/goaccess.conf /var/log/apache2/access.log -o /root/vv/report.html --real-time-html
```
You can then serve the same html using any server like so:
```
python3 -m http.server 8000
```
