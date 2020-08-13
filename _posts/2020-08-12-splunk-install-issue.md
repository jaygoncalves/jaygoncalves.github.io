---
title: That first time Splunk Error
author: muqduq
date: 2020-08-12
categories: [Blogging, Demo]
tags: [splunk, learning]
---

## Splunk Error on fresh install

* * *

Splunk is quite possibly the most exciting thing I have ever played around with! So how I got here... I'm taking Splunk's free-tier fundamentals course. The course covers essential Splunk enterprise use and setup. I downloaded the free trial of Splunk Enterprise on my Ubuntu Server; I was excited to finally play around with this software. Upon installation, I got the following error...

```
jason@admin-workstation:/opt/splunk/bin$ sudo ./splunk start --accept-license

Warning: cannot create "/opt/splunk/var/log/splunk"

Warning: cannot create "/opt/splunk/var/log/introspection"

Warning: cannot create "/opt/splunk/var/log/watchdog"
Warning: cannot create "/opt/splunk/etc/licenses/download-trial"
```
If my years as a diesel mechanic have taught me anything, it was to NOT GIVE UP. I used my resources and tapped into my Google-Foo and researched my issue. I found a few possible fixes on StackOverflow, but none of that worked out for me. Many of the users with problems were running their instance on a Windows Machine. I finally came across this [Splunk Documentation](https://docs.splunk.com/Documentation/Splunk/8.0.5/Admin/RunSplunkassystemdservice)

I entered the following code into my terminal and voila! I was able to fix my issue.  
Basically, what I believe is happening here is that this command kills my previous instance of Splunk (never knew I had one running), change ownership of all the Splunk files in the directory recursively using the -R flag. Then, I had to configure systemd to run Splunkd as a service, start the service, and accept the license.

```    
sudo pkill -f splunk
sudo chown -R splunk:splunk /opt/splunk
sudo ./splunk enable boot-start -user splunk -systemd-managed 1 --accept-license
sudo systemctl start Splunkd

```
So, what is systemd? This is what I got from Splunk. 
> systemd is a system startup and service manager that is widely deployed as the default init system on most major Linux distributions. You can configure systemd to manage processes, such as splunkd, as services, and allocate system resources to those processes under cgroups.

Please stick around for more muqduq shenanigans! 


*** 







