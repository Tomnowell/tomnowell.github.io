# Intro to Log Analysis

## An intro to log analysis, best practices, and essential tools for effective detection and response.

## SOC Level 2 Learning Pathway

### Introduction

This room, [Intro to Log Analysis](https://tryhackme.com/room/introtologanalysis) concludes the Log Analysis module of the SOC Level 2 learning pathway.

While there is no room VM there are three tasks with file downloads and I recommend downloading these to an environment where you are comfortable manipulating from the command line. For me that's a bash or similar shell along with built in Linux programs such as cat, sort, grep. I'm sure this is equally do-able from a windows Command prompt or from Powershell. In fact diving deeper into powershell is on my to do list, but for now, I'm going to be doing this in my Ubuntu installation in WSL.

As in my other write-up or walkthroughs I am not going to just repeat the information in the room.

---

### Task 2 - Log Analysis Basics

Understanding the information that can be stored in logs is important. Understanding what kind of logs to look for when looking for certain information is useful. Knowing why logs are useful and in what contexts they can be used is also important.

Most things that happen on a computer can be logged. This ranges from application activity to firewall logs to database logs to server logs and everything else. Okay, solitaire may not keep extensive logs of each move you do, but it probably could with a little modification. 

Understanding that every log is a perspective on activities within an environment is important and leads us to realise that they are essential for understanding our own systems, investigating anomalies and detecting threats.

---

### Task 3 - Investigation Theory

A lot of information about time. It's pertinent, too. A timeline is almost certainly essential in any security investigation. Synchronising and normalising these details is essential to correlate and enrich each log with extra perspective from other logs. Here we are introduced to timestamps, timelines and super timelines. Super timeline was a new concept for me at this point. It can be thought of as a consolidated timeline showing a single view of events across different components and systems. 

Cool name: **super timeline.**

To facilitate consolidating timelines we are introduced to [Plaso](https://github.com/log2timeline/plaso), a python tool to automate the creation of timelines from various log sources. Thanks to Kristinn Gudjonsson and other contributors to the project.

Visualisation is very important. We touched on Elastic Stack (Kirbana) and Splunk in the SOC 1 Learning Path but it was quite brief. Anyway those neat charts are back and I for one like the way that visualisations can easily highlight outlier data values or anomalies.

Log monitoring and alerts are very important and proper implementation can allow a swift and effective response with clear escalation procedures incidents can be addressed promptly and correctly by the right people.

When confronted with anomalous data external research and threat intelligence allow us to understand what we are seeing and proactively take steps to remediate the situation.

*Q1: What's the term for a consolidated chronological view of logged events from diverse sources, often used in log analysis and digital forensics?*

> Well, I thought it was a cool name :)

<details>

  <summary>Spoiler warning: Answer</summary>
  
    super timeline

</details>

*Q2: Which threat intelligence indicator would 5b31f93c09ad1d065c0491b764d04933 and 763f8bdbc98d105a8e82f36157e98bbe be classified as?*

> They certainly look like some kind of hash.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    file hashes

</details>

---

### Task 4 - Detection Engineering

Next we turn our attention to noticing patterns within logs. Being able to notice (or even better automate detection of) patterns that suggest unwanted or anomalous behaviour is essential for analysing logs. 

First we learn where some common logs are stored. Logs for web servers, databases, PHP web applications, OS logs and firewall logs.

We then look at some common patterns including user behaviour and attack signatures including SQL injection, XSS and path traversal.

*What is the default file path to view logs regarding HTTP requests on an Nginx server?*

> This is stated earlier in the Task...

<details>

  <summary>Spoiler warning: Answer</summary>
  
    /var/log/nginx/access.log

</details>


*A log entry containing %2E%2E%2F%2E%2E%2Fproc%2Fself%2Fenviron was identified. What kind of attack might this infer?*

> All those %2E look like URL encoded periods and a few %2F which represent forward slashes. It looks like someone is navigating through a file system.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    path traversal

</details>

---

### Task 5 - Automated vs. Manual Analysis

Here we learn about some tools that automate log analysis. Many of the modern tools use 'AI'. Likely they use machine learning to recognise trends and patterns. It's noted that the 'AI' landscape is evolving and solutions will likely become more effective with further development. 

The advantages and disadvantages of automated and manual analysis are considered. Basically automated is quick but expensive and highly dependent on the effectiveness of the detection model or AI, manual analysis is 'cheap' in terms of tooling and contributes to the organisational understanding of the employee conducting the analysis however it is time consuming (and therefore also expensive) and prone to human error.

*Q1: A log file is processed by a tool which returns an output. What form of analysis is this?*

> If a tool does the analysis for you it is...

<details>

  <summary>Spoiler warning: Answer</summary>
  
    automated

</details>

*Q2: An analyst opens a log file and searches for events. What form of analysis is this?*

> Um...the opposite

<details>

  <summary>Spoiler warning: Answer</summary>
  
    manual

</details>

---

# Task 6 - Log Analysis Tools: Command Line

Yay some task files to download! The examples are all done in bash on Linux so I'll do this in WSL. It's mostly an introduction to some Linux tools. You've probably met cat, less, wc, cut, tail, uniq, sort, sed, awk and grep before. We used most of them in the [Intro to Logs](https://tomnowell.github.io/IntroToLogs.html) room. Here's a chance to brush up on their usage with some examples!

I definitely recommend trying these exercises before looking at my hints or answers. If you're not used to using these tools on the command line (like me) it may seem daunting at first. I panicked! But, it's easier than it looks!

*Q1: Use a combination of the above commands on the apache.log file to return only the URLs. What is the flag that is returned in one of the unique entries?*

> This probably isn't the cleanest way to do this - but if you cut the 7th column you get just the path and the flag is pretty obvious sitting there near the bottom of the list.
```bash 
cut -d ' ' -f 7 apache.log
```

<details>

  <summary>Spoiler warning: Answer</summary>
  
    c701d43cc5a3acb9b5b04db7f1be94f6

</details>

*Q2: In the apache.log file, how many total HTTP 200 responses were logged?*

> use awk to select all 200 response codes and pipe it to wc - the first value is our count  
```bash
awk '$9 == 200' apache.log | wc
```

<details>

  <summary>Spoiler warning: Answer</summary>
  
    52

</details>

*Q3: In the apache.log file, which IP address generated the most traffic?*

> this should isolate the ip addresses, sort them and count the occurrences

```bash
cut -d ' ' -f 1 apache.log | sort -n -r | uniq -c
```  

<details>

  <summary>Spoiler warning: Answer</summary>
  
    145.76.33.201

</details>

*Q4: What is the complete timestamp of the entry where 110.122.65.76 accessed /login.php?*

> This command returns just one log entry - the timestamp is the answer!

```bash
grep "/login.php" apache.log | grep "110.122.65.76"
```

<details>

  <summary>Spoiler warning: Answer</summary>
  
    31/Jul/2023:12:34:40 +0000

</details>

---

### Task 7 - Log Analysis Tools: Regular Expressions

Regular expressions often strike fear into the noobs - I include myself in that number so it's time to face the fear and get some regex exp. In fact we often use regex alongside grep with the -E parameter - so it's not all that scary. Regex is a wonderful tool, it just looks quite cryptic and needs some practice. Let's get stuck in!

There is a linked room to learn and practice [regular expressions](https://tryhackme.com/room/catregex) - I did it a while ago, so I could use a refresher.

Download the file if you want to practice on it, but the questions actually don't really require it. I'd like a bit of practice though.

*How would you modify the original grep pattern above to match blog posts with an ID between 22-26?*

> We will take just the regex part of the command in the inverted commas and change it so that instead of finding values between 10 and 19 it will select values between 22 and 26. The original command is: 

```bash
grep -E 'post=1[0-9]' apache-ex2 log
```

<details>

  <summary>Spoiler warning: Answer</summary>
  
    post=2[2-6]

</details>

*What is the name of the filter plugin used in Logstash to parse unstructured log data?*

> This name was actually in the tech news cycle recently - unfortunately because of a certain E. Musk. And not related to this plugin.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    grok

</details>

---

### Task 8 - Log Analysis Tools: CyberChef

Wow, I did *not* know that Cyberchef was created by GCHQ - Nice one GB secret service. Perhaps Ms. Moneypenny and Q have become more open in their old age!

It makes sense to keep track of what people are trying to decode, though. So, I'm sure they're doing a *lot* of data collection through the site. If you are decoding sensitive stuff perhaps don't use Cyberchef to do it. Otherwise it's a very handy tool.

*Locate the "loganalysis.zip" file under /root/Rooms/introloganalysis/task8 and extract the contents.*

> I'm not exactly sure what they mean with this question - perhaps they've changed the room without changing all the questions - luckily this one doesn't actually require an answer

 
*Upload the log file named "access.log" to CyberChef. Use regex to list all of the IP addresses. What is the full IP address beginning in 212?*

> We're presented with the following regex to find ip addresses in the information for the task, so this is pretty straightforward. Just add a regex module to the recipe and change the output format to list matches only then search for the given octet '212'. I used the provided regular expression: 

```regex
\b([0-9]{1,3}\.){3}[0-9]{1,3}\b 
```

<details>

  <summary>Spoiler warning: Answer</summary>
  
    212.14.17.145

</details>

*Using the same log file from Question #2, a request was made that is encoded in base64. What is the decoded value?*

> Well, I couldn't get this to work properly in CyberChef - I found a string that certainly looks like base64 in a GET request: 'VEhNe0NZQkVSQ0hFRl9XSVpBUkR9== ' I used the bash command:
```bash
 echo 'VEhNe0NZQkVSQ0hFRl9XSVpBUkR9== ' | base64 -d
 ```
> to decode it and that did actually come out with the answer but it also stated that it was an invalid input. 
> I then reversed the direction and encoded the answer with:

```bash
 echo 'THM{THIS_THIS_IS_NOT_THE_ACTUAL_FLAG}' | base64
```

> which resulted in the encoded string:  
> VEhNe0NZQkVSQ0hFRl9XSVpBUkR9Cg==  
> Perhaps there is some different character encoding...
> I then tried it again in Cyberchef and it worked with either encoded string, so perhaps Cyberchef just froze in my browser.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    THM{CYBERCHEF_WIZARD}

</details>

*Using CyberChef, decode the file named "encodedflag.txt" and use regex to extract by MAC address. What is the extracted value?*

> Again Cyberchef makes this too easy! First we need to decode 'from base64'. I then tried making a regex for mac addresses until I found that Cyberchef has an 'extract MAC address' function. Apply it, and we're left with the answer.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    08-2E-9A-4B-7F-61

</details>

### Task 9 - Log Analysis Tools: Yara and Sigma

In this task, we are introduced to the tool, Sigma. It uses Yara rules to find information in log files using pattern matching. We came across [Yara](https://tryhackme.com/room/yara) in the SOC 1 learning path.

*What languages does Sigma use?*

> Well Sigma uses the Yara rule structure which in turn uses the .... language.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    YAML

</details>

*What keyword is used to denote the "title" of a Sigma rule?*

> The answer is literally in the question.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    title

</details>

*What keyword is used to denote the "name" of a rule in YARA?*

> Have a look at an example, the word that preceeds the title is...

<details>

  <summary>Spoiler warning: Answer</summary>
  
    rule

</details>

---

### Conclusion

Well that was quite a bit of work. It took me a few days to fit in around my full time job. But it was worth it. Lots of ways to explore logs and some idea of what to look for. 
