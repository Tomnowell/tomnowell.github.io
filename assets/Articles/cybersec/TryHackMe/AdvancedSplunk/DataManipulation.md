# Splunk: Data Manipulation

## Learn How to Parse and Manipulate data in Splunk

## SOC Level 2 Learning Pathway

### Introduction

Here we're going to walk through the TryHackMe room (Splunk: Data Manipulation)[https://tryhackme.com/room/splunkdatamanipulation]. Part of the Advanced Splunk module on the SOC Level 2 learning path.

We're expected to already have knowledge of regex and Splunk basics.

### Task 2 - Scenario and Lab Instructions

We are John.

Here we can start the machine. Unfortunately we're not given login credentials. looking at the /etc/shadow file it looks like the ubuntu user is not able to authenticate with a password (it's hash is preceeded by a ! symbol). I reset the ubuntu user password and sshed in anyway.

From the machines terminal in split screen browser:

> sudo passwd ubuntu
> Enter a new password

We can now ssh in from our machine over the VPN using:

> ssh ubuntu@VM_IP_ADDRESS
> and entering our new password when prompted
> We can elevate to root with
> sudo su


*Q1: Connect to the Lab.*

> NO answer needed

*Q2: How many Python scripts are present in the ~/Downloads/scripts directory?*

> cd Downloads/scripts
> ls
> count 'em!

<details>

  <summary>Spoiler warning: Answer</summary>
    
    3

</details>

### Task 3 - Splunk Data Processing: Overview

Some reading to do. Don't skip it!

*Q1: Understand the Data Processing steps and move to the next task.*

> No answer needed

### Task 4 - Exploring Splunk Configuration Files

More reading about config files and stanzas. Stanzas are how individual data points are expressed, processed and indexed within Splunk.

*Q1: Which stanza is used in the configuration files to break the events after the provided pattern?*

> Break and after give us a good hint here

<details>

  <summary>Spoiler warning: Answer</summary>
    
    BREAK_ONLY_AFTER

</details>

---

*Q2: Which stanza is used to specify the pattern for the line break within events?*

> It's another breaker and the task info spells it out

<details>

  <summary>Spoiler warning: Answer</summary>
    
    LINE_BREAKER

</details>

---

*Q3: Which configuration file is used to define transformations and enrichments on indexed fields?*

> the keyword here is transformations

<details>

  <summary>Spoiler warning: Answer</summary>
    
    transforms.conf

</details>

---

*Q4: Which configuration file is used to define inputs and ways to collect data from different sources?*

> keyword: inputs

<details>

  <summary>Spoiler warning: Answer</summary>
    
    inputs.conf

</details>

---

### Task 5 - Creating a Simple Splunk App


> We are told that installed splunk instances are located here:
> /opt/splunk/etc/apps

<details>

  <summary>Spoiler warning: Answer</summary>
    
    /opt/splunk/etc/apps/THM

</details>

---

### Task 6 - Event Boundaries - Understanding The Problem

Do work through the examples, creating the example DataApp - you'll need it later on.

Here's a summary of the steps from creation to event boundaries.

> In the Splunk interface home click on the cog wheel near Apps as directed
> Name the app DataApp (to follow the example) and give some random values for name
> After saving your new app click Launch in the actions column
> From your root shell prompt navigate to:
> /opt/splunk/etc/apps/DataApp/default
> vim inputs.conf (or whatever editor you prefer)
> i for insert mode, and copy the 4 lines of code that will check the vpnlogs
> escape to exit insert mode
> :wq - save and quit
> mv /home/ubuntu/Downloads/scripts/vpnlogs ../bin/ # copy the scrip to the apps bin directory
> /opt/splunk/bin/splunk restart # restart Splunk # Give it a couple of minutes before trying to access in the browser
> search: index=main sourcetype=vpnlogs and set Time setting - All time (real-time)

Okay we have logs coming in but there seem to be multiple data points in each log. We need to establish event boundaries. We do this from a props.conf file.

> vim props.conf (or your preferred editor)
> i for insert mode and copy the 3 lines of code given in the task instructions
> /opt/splunk/bin/splunk restart # restart Splunk again

You should see the logs comming in nicely formatted now.



*Q1: Which configuration file is used to specify parsing rules?*

> Back in Task 3 we learned about the configuration file that defines data parsing settings for specific sourcetypes or data sources.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    props.conf

</details>

---

*Q2: What regex is used in the above case to break the Events?*

> In the task materials a site is used to generate a regex that finds the event boundary - either CONNECTED or DISCONNECTED

<details>

  <summary>Spoiler warning: Answer</summary>
    
    (CONNECTED|DISCONNECTED)

</details>

---

*Q3: Which stanza is used in the configuration to force Splunk to break the event after the specified pattern?*

> We can see from the screenshots that multiple events are being treated as a single event. We need to get in there and break each event after it's 'Action' value.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    MUST_BREAK_AFTER

</details>

---

*Q4: If we want to disable line merging, what will be the value of the stanza SHOULD_LINEMERGE?*

> We can see in the props.conf file that the current value of SHOULD_LINEMERGE is true. The opposite of true is...

<details>

  <summary>Spoiler warning: Answer</summary>
    
    false

</details>

---

## Task 7 - Parsing Multi-line Events

Let's take a look at some longer events and how the event boundaries might be established and defined.

---

*Q1: Which stanza is used to break the event boundary before a pattern is specified in the above case?*

> Look at the section that explains how to define the event boundary and shows the entry for the props.conf file. The last line is our man.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    BREAK_ONLY_BEFORE

</details>


*Q2: Which regex pattern is used to identify the event boundaries in the above case?*

> Look at the props.conf - what is the value assigned to the stanza. Note that we need to use a backslash '\' to escapethe square bracket. Simply, we need to tell regex that these square brackets are not stating a range of values which is the syntax for regex but that we actually mean a litteral square bracket.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    \[Authentication\]

</details>

---

### Task 8 - Masking Sensitive Data

Everyone has some sensitive data - whether it's financial information, an important private number like a US social security number or even just an email address or telephone number you don't want to publish for everyone to see.

But sometimes sensitive data is a matter of standards compliance or even legal compliance.

*Q1: Which stanza is used to break the event after the specified regex pattern?*

> Take a look at the props.conf file

<details>

  <summary>Spoiler warning: Answer</summary>
    
    MUST_BREAK_AFTER

</details>

---

*Q2: What is the pattern of using SEDCMD in the props.conf to mask or replace the sensitive fields?*

> Again look in the props.conf file

<details>

  <summary>Spoiler warning: Answer</summary>
    
    s/oldValue/newValue/g

</details>

---

### Task 9 - Extracting Custom Fields

We're going to do some exercises that practice step 4 that was introduced in Task 3. In particular, we will specify our own sourcetypes for our data.

---

### Conclusion

An interesting room. After the fuss I made about not being given credentials, I now realise we don't really need to use the machine at all until task 9. The screenshots along with the writing contain all the answers or the information you need to get the answers. I'd like to see some more challenging activities using the VM to solidify knowledge and gain a little experience not just play along at home.

All in all I enjoyed it. Looks like I'll be getting my practical challenge in the next room. 

Next up - Fixit
