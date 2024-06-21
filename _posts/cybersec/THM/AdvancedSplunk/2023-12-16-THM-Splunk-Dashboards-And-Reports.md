---

layout: post

title: "TryHackMe - Splunk: Dashboards and Reports"

date:   2023-12-16 00:00:00 +0900

categories: [TryHackMe, SOC 2 Path, Splunk]

comments: true

---

<div> <img src="/images/splunk3.webp" alt="Computer tech cartoon" /> </div>

#### Creating Dashboards and Reports in Splunk.

## Introduction

This is a writeup for the TryHackMe room (Splunk: Dashboards and Reports)[https://tryhackme.com/room/splunkdashboardsandreports]. It's probably best to go through the Introductory rooms first and these are linked to from the Introduction task under Pre-requisites.

I'm excited for this room. I love those SIEM dashboards. You feel like your commanding a starship! Let's gOoO!
 
---
## Task 2 - Organizing Data in Splunk

This task is mostly just a warning to anyone who hasn't yet had to deal with larger datasets. It can be overwhelming and it's easy to miss stuff when there is so much noise. So, as they state it's usually a good idea to search within a specific time-frame to reduce the number of data points, the load on the Splunk search head.

---

**Q1: Which search term will show us results from all indices in Splunk?**

*Hint: \* is a wildcard and we set the index with an = sign.*  

<details>
	<summary>Spoiler warning: Answer</summary>
	index=*
</details>

---
## Task 3 - Creating Reports for Recurring Searches

Here we get to play with a VM and good news, we don't need to use a remote desktop or VPN. We can access the machine directly from our main browser through the subdomains of thmlabs.com - give it a few minutes - you may get a bad gateway error even when the machine has booted.

Reports are useful when we have search queries that we need to run again and again.

Let's get into the questions!

---

**Q1: Create a report from the network-server logs host that lists the ports used in network connections and their count. What is the highest number of times any port is used in network connections?**

*Hint: We don't actually need to create a report to get this info here's the cheat method first:*

- Search: index=network_logs host="network-server"
- On the left side click on # port - the top 10 values are listed along with the counts.

Okay let's make the report anyway because it's good practice!

- Search: index=network_logs host="network-server" \| stats count by port
- Save as -> Report
- Give your report a title and description if you wish
- We can see our report. It's in ascending order we can click the arrow for descending and we see 4 ports are used X times.
 
<details>
	<summary>Spoiler warning: Answer</summary>
	5
</details>

---

**Q2: While creating reports, which option do we need to enable to get to choose the time range of the report?**

- In the Save as dialogue the last option allows us to apply a time range.

<details>
	<summary>Spoiler warning: Answer</summary>
	Time Range Picker
</details>

---
## Task 4 - Creating Dashboards for Summarizing Results

Dashboards are good for collecting reports and visualisations in one place.

Let's get straight into the questions - we're making dashboards.

---  

**Q1: Create a dashboard from the web-server logs that show the status codes in a line chart. Which status code was observed for the least number of times?**

I'm not sure why we need to put this on a dashboard. Let's cheat and get the answer. Then when we know what we're looking for we'll make a dashboard.

- index="web-server"
- Find and click status_code in the fields on the left column
- Click rare (or top values there are only 8)
- The answer will be the first in the list (the rarest port number)

Okay now that we've answered the question lets make a line graph and add it to a dashboard.

We can get our statistics with the search:

- index="web_logs" host="web-server" \| rare status_code
- visualisations lets us choose a line chart as instructed.
- When you're happy with your chart click 'Save As'
- and either New Dashboard or add to an existing Dashboard.

<details>
	<summary>Spoiler warning: Answer</summary>
	400
</details>

--- 

**Q2: What is the name of the traditional Splunk dashboard builder?**

*Hint: When you first make a dashboard you are invited to choose between the snazzy new dashboard builder and the older one that is not called traditional, antiquated, over-the-hill or obsolete.*

<details>
	<summary>Spoiler warning: Answer</summary>
	classic
</details>  

---
## Task 5 - Alerting on High Priority Events

Alright! We're on the home straight. Let's learn about alerts and priority.

Looking at the alerts tab on the VM we see:

> "This feature is not available with your installed set of licenses."


It's a little disappointing to learn that licensing issues stop us from doing practical work setting up alerts.

Anyway let's read the info and answer the questions!

---

**Q1: What feature can we use to make Splunk take some actions on our behalf?**

*Hint: These are actions caused by ...*

<details>
	<summary>Spoiler warning: Answer</summary>
	trigger actions  
</details>

---

**Q2: Which alert type will trigger the instant an event occurs?**

*Hint: Well if an alert is triggered at the instant it happens in...*

<details>
	<summary>Spoiler warning: Answer</summary>
	real-time
</details>

---

**Q3: Which option, when enabled, will only send a single alert in the specified time even if the trigger conditions re-occur?**

*Hint: If you want to reduce the flow of something you can choke it. Another word for this is...*

<details>
	<summary>Spoiler warning: Answer</summary>
	throttle
</details>

---

## Conclusion

I enjoyed most of this room. It actually took me a long time, because I had a lot of distractions but it wasn't very difficult. I don't think the practical exercises really showed off how reports or dashboards are used in real life, but it is nice to have a simple introduction and I did learn a lot.

See you next time!
