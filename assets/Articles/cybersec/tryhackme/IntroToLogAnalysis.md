# Intro to Log Analysis

## An intro to log analysis, best practices, and essential tools for effective detection and response.

## SOC Level 2 Learning Pathway

### Introduction

This room concludes the Log Analysis module of the SOC Level 2 learning pathway.

While there is no room VM there are three tasks with file downloads and I recommend downloading these to an environment where you are comfortable manipulating from the command line. For me that's a bash or similar shell along with built in Linux programs such as cat, sort, grep. I'm sure this is equally do-able from a windows Command prompt or from Powershell. In fact diving deeper into powershell is on my to do list, but for now, I'm going to be doing this in my Ubuntu installation in WSL.

As in my other write-up or walkthroughs I am not going to just repeat the information in the room.

### Log Analysis Basics

Understanding the information that can be stored in logs is important. Understanding what kind of logs to look for when looking for certain information is useful. Knowing why logs are useful and in what contexts they can be used is also important.

Most things that happen on a computer can be logged. This ranges from application activity to firewall logs to database logs to server logs and everything else. Okay, solitaire may not keep extensive logs of each move you do, but it probably could with a little modification. 

Understanding that every log is a perspective on activities within an environment is important and leads us to realise that they are essential for understanding our own systems, investigating anomalies and detecting threats.

### Investigation Theory

A lot of information about time. It's pertinent, too. A timeline is almost certainly essential in any security investigation. Syncronising and normalising these details is essential to correlate and enrich each log with extra perspective from other logs. Here we are introduced to timestamps, timelines and super timelines. Super timeline was a new concept for me at this point. It can be thought of as a consolidated timeline showing a single view of events across different components and systems. 

Cool name: *super timeline.*

To facilitate consolidating timelines we are introduced to Plaso (link) a python tool to automate the creation of timelines from various log sources. Thanks to Kristinn Gudjonsson and other contributors to the project.

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

