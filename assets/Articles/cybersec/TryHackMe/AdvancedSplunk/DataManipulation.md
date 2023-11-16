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

