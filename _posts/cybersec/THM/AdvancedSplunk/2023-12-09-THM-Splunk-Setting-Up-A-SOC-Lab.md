---
layout: post
title:  "TryHackMe - Splunk: Setting Up a SOC Lab"
date:   2023-12-09 00:00:00 +0900
categories: [TryHackMe, SOC 2 Path, Splunk]
comments: true
---

##### Explore Splunk Beyond Basics

---

<div> <img src="/images/splunk2.jpeg" alt="Computer tech cartoon" /> </div>

---
### Introduction

This sounds fun. We get to install Splunk and set it up on a Linux box and a Windows box! Let's get started! Today, we're looking at the Try Hack Me room [Splunk: Setting Up a SOC Lab](https://tryhackme.com/room/splunklab) on the SOC 2 Learning pathway.

You can connect to this room's VMs via the browser, which is what I did. I like to full screen the browser and put it on a different desktop so that I can (in Windows) just press ctrl+Win and arrow keys to go between the VM and the room page. Unfortunately THM didn't provide RDP or ssh credentials. I'd like to at least have ssh for installation and interacting with the CLI. The lag on the terminal in the browser is painful, but it's a minor complaint.

### Task2 - Splunk: Setting up a Lab

This is more like a continued introduction. Let's proceed!

### Task 3 - Splunk: Deployment on Linux Server

*Note: There are two VMs for this room and I recommend setting some time aside to work through all the steps on each machine in one go. The reason being that later tasks rely on work performed in previous tasks. Eg. Task 6 - configuring the forwarder on linux requires the installation steps performed in task 2. It took me about an hour each to work through the machines while taking notes. I consider myself on the slow side, especially while learning - so you may be able to get through much faster.*

Here are the steps I took:

```bash
cd Downloads/splunk
sudo su
tar xvzf splunk_installer.tgz
mv splunk /opt/
cd /opt/splunk/bin
./splunk start --accept-license
```

```
Go ahead and create a user account
Wait for setup to finish
Access your splunk instance on:
http://coffely:8000
Or if you're using the VPN http://VM_IP_ADDRESS:8000
Sign in with the credentials you made on installation*
```

**Q1: What is the default port for Splunk?**

*We can see the port used to access splunk after the : in the splunk URL*

<details>
  <summary>Spoiler warning: Answer</summary>  
  8000
</details>

---

### Task 4 - Splunk: Interacting with CLI

We'll go back to the terminal for interacting with the CLI and stay as root. Or if you aren't root then:

```bash
sudo su 
```

Move to the main splunk directory to run CLI commands:

```bash
cd /opt/splunk
```

If you haven't started splunk already you can run start like this:

```bash
./bin/splunk start
```

And stop like so:
```bash
./bin/splunk/stop
```

I won't list all the CLI instructions you can use - but I recommend making a note them, understanding them and considering how and when they might be used. Have a look at the help command and get used to using it. Notably we can run:

```bash
./bin/splunk help [command]
```

To get help on any individual command. Nice!

---

**Q1: In Splunk, what is the command to search for the term coffely in the logs?**

*We can see how to run a search query in the task information*

<details>
  <summary>Spoiler warning: Answer</summary>
  ./bin/splunk search coffely
</details>

---

**Q2: Use the help command to explore different help options and their syntax.**

*No answer needed*

---

### Task 5 - Splunk: Data Ingestion

For data ingestion we're introduced to forwarders. These come in two varieties: Heavy forwarders and universal forwarders. Heavy forwarders apply a filter, analyse or modify the logs in some way before forwarding to the SIEM. Universal forwarders on the other hand are lightweight and just sends the log data from the host to the SIEM.

The forwarder is installed separately (of course) because it is designed to collect logs from a host machine and send them to the SIEM.

We only have one VM so we're going to install the forwarder here. Let's go!

I opened a new shell for this but if you don't want to then navigate to the ubuntu user's Downloads/splunk folder

```bash
cd /home/ubuntu/Downloads/splunk # get to the splunk download
sudo su # If you're not already root
tar xvzf splunkforwarder.tgz
mv splunkforwarder /opt/
cd /opt/splunkforwarder
./bin/splunk start --accept-license
```

```
Enter new credentials
You may get an error that the management port is already bound
Select y and set a new port number - I followed the example and chose 8090
```

---

**Q1: What is the default port, on which Splunk Forwarder runs on?**

*If you get the error that the port is already bound - it should show the default port.*

<details>
  <summary>Spoiler warning: Answer</summary>
  8089
</details>

---

### Task 6 - Configuring Forwarder on Linux

Change VM_IP_ADDRESS to your VM's IP address:

```bash
./bin/splunk add forward-server VM_IP_ADDRESS:9997
./bin/splunk add monitor /var/log/syslog -index Linux_host
```

To view the inputs:

```bash
/opt/splunkforwarder/etc/apps/search/local/inputs.conf
```
See the logs in splunk go to search and search the index Linux_host

```index="linux_host"```

*Note: We don't need to change the time period as we're collecting this data now so past 24 hours default is fine.*

---

**Q1: Follow the same steps and ingest /var/log/auth.log file into Splunk index Linux_logs. What is the value in the sourcetype field?**

*I'm Not sure about this answer I added the auth.log file and the sourcetype for those logs was auth-too_small but that is not the answer - check the sourcetype for the other log file*

<details>
  <summary>Spoiler warning: Answer</summary>
  syslog
</details>

---

**Q2: Create a new user named analyst using the command adduser analyst. Once created, look at the events generated in Splunk related to the user creation activity. How many events are returned as a result of user creation?**

*At first I misunderstood this question. I thought we need to add a user to splunk. But, we need to add a user to splunkforwarder. Splunk doesn't accept the adduser command. This should have been my first warning.*

*So, within splunkforwarder directory run:*
```bash
adduser analyst
# Or any name you like - fill in the requested info for the new user
# If you set the time window to show only recent log entries - this is the easiest way to see which log entries were
# added as a result of the adduser command. They should be at the top of your log entries and all from the /var/log/auth.# log source
```

<details>
  <summary>Spoiler warning: Answer</summary>
  6
</details>

---

**Q3: What is the path of the group the user is added after creation?**

*One of our log entries should give the answer to this question:*
*coffely groupadd[38041]: group added to...*

<details>
  <summary>Spoiler warning: Answer</summary>
  /etc/group
</details>

---

That's the end of the Linux VM. *Phew* quite a lot to ingest, punny. But all good fun!

### Task 7 - Splunk: Installing on Windows

We're going to connect using the browser again.

*Open the Downloads folder and run the splunk_instance installer.*

The machine will whir away for a while considering it's next move before presenting the user with a chance to read the user license agreement and view or change the default install options. I thoroughly read the EULA before ticking the box as I am sure you will - and I left all the default options.

```
Tick EULA box and click Next
Add name and password for administrator account
Next!
```

About 5 minutes later the install finished and I was prompted to open a browser to view splunk. I declined this time.

You can connect to the splunk instance in the browser either on the local loopback address from the browser 127.0.0.1:8000 or over the VPN from attackbox or your computer using http://VM_IP_ADDRESS:8000. Almost the same as for the linux box I guess just coffely doesn't resolve to localhost.

Alright! We're done here time to move on to the forwarders!

**Q1: What is the default port Splunk runs on?**

*We just asked this question for the Linux version. I guess it could use different ports. But why would it? It doesn't!*

<details>
  <summary>Spoiler warning: Answer</summary>
  8000
</details>

---

**Q2: Click on the Add Data tab; how many methods are available for data ingestion?**

*Before trying to search anything click on the add data button on the first splunk page. There are only a few ways to add data*

<details>
  <summary>Spoiler warning: Answer</summary>
  3
</details>

---

**Q3Click on the Monitor option; what is the first option shown in the monitoring list?**

*Click on the Monitor option. What's at the top of the list?*

<details>
  <summary>Spoiler warning: Answer</summary>
  Local Event Logs
</details>

---

### Task 8 - Installing and Configuring Forwarder

In splunk, configure the receiver as shown in the room info.

*Settings -> Forwarding and Receiving -> New Receiving Port -> 9997 (or whatever port you like)*

Then install the splunk forwarder. It's already in the Downloads folder.

**Q1: What is the full path in the C:\Program Files where Splunk forwarder is installed?**

*The install path was given when we checked the default install path and accepted the EULA. It's also in a screen shot in the task materials. Remember Windows file slashes go the wrong way '\\'*

<details>
  <summary>Spoiler warning: Answer</summary>
  C:\Program Files\SplunkUniversalForwarder
</details>

**Q2: What is the default port on which Splunk configures the forwarder?**

*This is also stated as a part of the setup procedure and we already set it as a listen port within splunk.*

<details>
  <summary>Spoiler warning: Answer</summary>
  C:\Program Files\SplunkUniversalForwarder
</details>

---

### Task 9 - Splunk: Ingesting Windows Logs

We need to get some data into splunk. We won't use the CLI this time. Back to the browser.

```
Settings -> Add Data -> Forward
```

#### Select Forwarders_

```
Select Forwarders -> Available host(s)
Click on the Windows coffeylab entry - it will then appear in selected host(s) too
dd a new Server Class Name - I called it coffee_lab as that was used in the example.
Next
```

#### Select Source

```
Local Event Logs -> (Application & Security & System) Click on each - they will appear in the selected items pane.
Next
```

#### Input Settings

```
Create New Index
Enter a name - I used win_logs to follow the example
Next
```

#### Review

```
OK.
Next
```

#### Done

```
Submit
```

---

**Q1: While selecting Local Event Logs to monitor, how many Event Logs are available to select from the list to monitor?**

*Did you count how many options there were for Local Event Logs. I didn't, but that scrollbar doesn't go down far. There were:*

<details>
  <summary>Spoiler warning: Answer</summary>
  5
</details>

---

**Q2: Search for the events with EventCode=4624. What is the value of the field Message?**

*search with the following query and open any of the log entries to find the message field.*

```
index="win_logs" EventCode=4624
```

<details>
  <summary>Spoiler warning: Answer</summary>
  An account was successfully logged on.
</details>

### Task 10 - Ingesting Coffely Web Logs

Nearly there, we've got a forwarder set up. Now let's look at ingesting some logs from a web server. You don't actually need to do this to answer the room's questions but it's probably good to know how to monitor a webserver's log files.

```Settings -> Add Data```

#### Select Forwarders

```
From available host(s) select Windows coffeylab
Add a new Server Class Name - I followed the example and named it web_logs
Next
```

#### Select Sources

*Note: The directory name is given but the log filename given ends in \* which represents a number.*

Let's look in the directory and find what file we need to add. Check out the directory in Explorer:

```
C:\inetpub\logs\LogFiles\
```

*Note: My VM instance had a log file called W3SVC1 - the example on the task instructions uses W3SVC2. Yours may be different.*

```
Files & Directories -> C:\inetpub\logs\LogFiles\W3SVC1
Next
```

#### Input Settings

Looks like this is a Microsoft IIS Server:

```
Select -> IIS 
Index -> win_logs
Next
```

#### Review

```
OK
Next
```

#### Done

``` 
Submit 
```

From these logs (and from the question posed) we see that there is a secret-flag.html page

*Note: The creator of this website is probably paying a bit too much for their coffee and, more worryingly, does not know how to make a cappuccino. Luckily, they know about Splunk so we'll let that go.*

**Q1: In the lab, visit http://coffely.thm/secret-flag.html; it will display the history logs of the orders made so far. Find the flag in one of the logs.**

*Go to http://coffely.thm/secret-flag.html and look at the orders. The flag is in the message section*

<details>
  <summary>Spoiler warning: Answer</summary>
  {COffely_Is_Best_iN_TOwn}
</details>

### Conclusion

The Windows VM was a little sluggish in installing Splunk and the forwarder. But the Windows VM was very straightforward after doing the Linux VM. All in all it was good fun and an interesting lesson!

Next up: [Splunk: Dashboards and Reports](http://tomnowell.com/Dash)
