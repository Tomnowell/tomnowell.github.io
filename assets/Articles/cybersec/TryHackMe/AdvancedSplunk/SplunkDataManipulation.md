# Splunk: Data Manipulation

## Learn How to Parse and Manipulate Data in Splunk

## SOC Level 2 Learning Pathway

### Introduction

Here we're going to walk through the TryHackMe room [Splunk: Data Manipulation](https://tryhackme.com/room/splunkdatamanipulation). Part of the Advanced Splunk module on the SOC Level 2 learning path.

We're expected to already have knowledge of regex and Splunk basics.

### Task 2 - Scenario and Lab Instructions

We are John.

Here we can start the machine. Unfortunately we're not given login credentials. looking at the /etc/shadow file it looks like the ubuntu user is not able to authenticate with a password. The ! preceding the passwords SHA512 hash denotes that this user cannot authenticate by password.  

*Q1: Connect to the Lab.*

If you do want to connect via ssh then you can follow my method here. Let me know if you have another way in! I reset the ubuntu user password and sshed in with the new password.

From the machines terminal in split screen browser:

```bash
sudo passwd ubuntu # Enter a new password when prompted
```

We can now ssh in from our machine over the VPN using:

```bash
ssh ubuntu@VM_IP_ADDRESS # enter our new password when prompted

sudo su # Elevate to root when ssh is connected
```

> No answer needed

---

*Q2: How many Python scripts are present in the ~/Downloads/scripts directory?*

```bash
cd Downloads/scripts
ls
```

> count 'em!  

<details>
    <summary>Spoiler warning: Answer</summary>
    3
</details>

Just a quick note here. We'll need these scripts later on so let's copy them to the splunk scripts directory now
```bash
cp -a /home/ubuntu/scripts/ /opt/splunk/etc/apps/DataApp/bin/ # -a preserves file attributes
```

---

### Task 3 - Splunk Data Processing: Overview

Some reading to do. Don't skip it!

*Q1: Understand the Data Processing steps and move to the next task.*

> No answer needed

---

### Task 4 - Exploring Splunk Configuration Files

More reading about config files and stanzas. Stanzas are how individual data points are expressed, processed and indexed within Splunk.

*Q1: Which stanza is used in the configuration files to break the events after the provided pattern?*

> 'Break' and 'after' give us a good hint here.

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

```bash
/opt/splunk/bin/splunk start # Start Splunk from a root shell
```

> In the Splunk interface home click on the cog wheel near Apps as directed  
> Name the app DataApp (to follow the example) and give some random values for name  
> After saving your new app click Launch in the actions column  
> From your root shell prompt navigate to:  

```bash
cd /opt/splunk/etc/apps/DataApp/default
vim inputs.conf # (or whatever editor you prefer)
```

> Copy the 4 lines of code that will check the vpnlogs
> :wq - save and quit

```bash
mv /home/ubuntu/Downloads/scripts/vpnlogs ../bin/ # copy the scrip to the apps bin directory
/opt/splunk/bin/splunk restart # restart Splunk # Give it a couple of minutes before trying to access in the browser
```

> search: index=main sourcetype=vpnlogs and set Time setting - All time (real-time)

Okay we have logs coming in but there seem to be multiple data points in each log. We need to establish event boundaries. We do this from a props.conf file.

```bash 
vim props.conf (or your preferred editor)
```

> Copy the 3 lines of code given in the task instructions

```bash
/opt/splunk/bin/splunk restart # restart Splunk again
```

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

There seems to be a typo where we are instructed to create a fields.conf file. However, at one point this is referred to as 'Fields.conf'. I think the lowercase version should be used if only to keep consistency with other conf file naming used in these examples.

To answer the questions in this task we're going to need to apply what we learned to extract fields from the purchase logs.

This task took me quite a while to get through because I kept getting pulled away to do other things just when I got the machine set up. I'll assume that anyone following this is also starting this task from a newly spawned VM. So, from the newly started bare machine.

In the terminal:

```bash
sudo su
/opt/splunk/bin/splunk start
```

> In the browser:  
> Navigate to http://MACHINE_IP:8000  
> Create the DataApp  
> Launch the DataApp  

Back in the terminal as root:

```bash
cp -a /home/ubuntu/Downloads/scripts/. /opt/splunk/etc/apps/DataApp/bin/

cd /opt/splunk/etc/apps/DataApp/default 
```

Write the inputs.conf file:

```bash
echo -e "[script:///opt/splunk/etc/apps/DataApp/bin/purchase-details]
interval = 5
index = main
source = purchase_logs
sourcetype= purchase_logs
host = order_server" > inputs.conf
```

Write the props.conf file:

```bash
echo -e "[purchase_logs]
SHOWLD_LINEMERGE=true
MUST_BREAK_AFTER = \d{4}\.
TRANSFORM-purchase = purchase_custom_fields" > props.conf
```

Write the transforms.conf file:

```bash
echo -e "[purchase_custom_fields]
REGEX = User\s([\w\s]+).(made a purchase with credit card).(\d{4}-\d{4}-\d{4}-\d{4}).
FORMAT = Username::$1 Message::$2 CardNumber::$3
WRITE_META = true" > transforms.conf
```

Write the fields.conf file:

```bash
echo -e "[Username]
INDEXED = true

[Message]
INDEXED = true

[CardNumber]
INDEXED = true" > fields.conf
```

Restart splunk:

```bash
/opt/splunk/bin/splunk restart
```

As you can see I only added the purchase_logs sourcetype for this task in the inputs.conf file.  In the props.conf file I defined the section to look for in the transforms.conf file. In the transforms.conf file we define the regex to separate the fields. I realise specifically matching the message string is probably not going to be reliable in a real situation if there are multiple payment methods, but it works for our tasks here. In the transforms.conf file we also define the format of the data specifying how the strings are ordered. Finally the fields.conf file we set every field defined in transforms.conf to be indexed. Finally we restart Splunk.

*Q1: Create a regex pattern to extract all three fields from the VPN logs.*

> No answer needed - But, anyway, the regex is defined earlier in the task materials.

*Q2 Extract the Username field from the sourcetype purchase_logs we worked on earlier. How many Users were returned in the Username field after extraction?*

> If you have followed the steps above you should be ready to go.  
> In Splunk, search:  
> index-main sourcetype="purchase_logs"  
> The number of Usernames appears in the interesting fields section on the left side.  

<details>

  <summary>Spoiler warning: Answer</summary>
    
    14

</details>

---

*Q2: Extract Credit-Card values from the sourcetype purchase_logs, how many unique credit card numbers are returned against the Credit-Card field?*

> Again, the number of unique values appears in the interesting fields section on the left side.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    16

</details>

---

### Recap and Conclusion

I kept getting pulled away in this room so it took a while to get through it. But it was a great learning tool and I feel much more comfortable configuring Splunk to ingest new and different data sources, creating detailed events with meaningful event boundaries and using regular expressions to extract interesting fields. Splunk also has a built-in 'Extract New Fields' function which could be worth exploring for those who dislike creating their own regular expressions. I quite enjoy the challenge of learning regex. With the help of [regex101](https://regex101.com), I find the process fun and satisfyingd. A little like a jigsaw puzzle.

I might be a bit slower to upload the SOC path 2 as I'll be doing the [Advent of Cyber 2023](https://tryhackme.com/room/adventofcyber2023). But there should be enough time for a room or two over the festive season.

---

Next up in the series - Fixit
