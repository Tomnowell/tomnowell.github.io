# Splunk: Exploring SPL

## Learn and explore the basics of the Search Processing Language.

## TryHackMe SOC 2 Learning Pathway

### Introduction

We met Splunk back on the SOC 1 path. (In this room)[https://tryhackme.com/room/splunkexploringspl] we focus on SPL, the Search Processing Language.

Processing queries using a specified language reduces human error and ensures repeatability and allows processes to be documented and automated.

### Task 2: Connect with the Lab

You can interact with this room's machine directly from the browser by following the provided link. Of course you can connect through the vpn or attackbox if you prefer.

It's worth noting that the machine is painfully slow - maybe grab a coffee when you're actually getting it to do anything!

*Q1: Connect with the Lab.*
> No answer required.

*Q2: What is the name of the host in the Data Summary tab?*
> Calling this a tab is a little misleading in my opinion. On the search page there is a 'Data Sumamry' button which brings up an overlayed section with the summary (not really a tab). 
>I tried setting the index=windowslogs but it seems we need index=windowslogs\* and remember to set the time period to 'all time' rather than the default 'past 24 hours'*

<details>

  <summary>Spoiler warning: Answer</summary>
    
    cyber-host

</details>

### Task 3: Search & Reporting App Overview

Still learning our way around the interface. 

*In the search History, what is the 7th search query in the list? (excluding your searches from today)*

> Expand down the search history '>' list and count down 7 + however many searches you have done so far.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    index=windowslogs | chart count(EventCode) by Image

</details>

*In the left field panel, which Source IP has recorded max events?*

> Find the Source Address field in the left hand column and note the first address that isn't a wildcard address or loopback.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    172.90.12.11

</details>

> To the right of the search bar is a dropdown that allows us to filter by date or time. Hopefully you already have set it to 'all time'. Now we can use it to specify a 'Date time range'. Select this option and set the date as April 15th 2022 for both start and end. Set the start time to 08:05:00.000 and end to 08:06:00.000. The number of events is shown under the search bar with a little green tick to its left.

<details>

  <summary>Spoiler warning: Answer</summary>
    
    134

</details>

### Task 4: Splunk Processing Language Overview

I'm a little confused is it search processing language or Splunk processing language. There are so many acronyms it's difficult to keep track of them all. Or is it SSPL, SPlunk Search Processing Language? 

Operators within the language will look familiar to anyone who has done any coding and boolean logic operators are written in capitals in English (rather than in a logic notation). And is AND not && or ^. Or is OR not || or V as is sometimes the case in other languages.

Wildcards in strings are handled like in regexes with an asterisk.

*How many Events are returned when searching for Event ID 1 AND User as \*James\*?*

> Our search string ends up looking like this index=windowslogs EventID=1 AND User=*James*

<details>

  <summary>Spoiler warning: Answer</summary>
    
    4

</details>

*How many events are observed with Destination IP 172.18.39.6 AND destination Port 135?*

> Well I ran the following query but I found 8 events with those details:
> index=windowslogs DestAddress="172.18.39.6" AND DestPort=135
> Not sure why my numbers don't match. It is true that there are 4 unique EventRecievedTime events within the search

<details>

  <summary>Spoiler warning: Answer</summary>
    
    4 (But I think the answer should be 8)

</details>

*What is the Source IP with highest count returned with this Search query?
Search Query: index=windowslogs  Hostname="Salena.Adam" DestinationIp="172.18.38.5"*

> This is too easy, they even give you the search term. Just search it and find the SourceIP in the Interesting Fields section on the left hand side. There are two entries. The answer has 17 counts...

<details>

  <summary>Spoiler warning: Answer</summary>
    
    172.90.12.11

</details>

*In the index windowslogs, search for all the events that contain the term cyber how many events returned?*

> Running the search for index=windowslogs cyber returns no results

<details>

  <summary>Spoiler warning: Answer</summary>
    
    0

</details>

*Now search for the term cyber\*, how many events are returned?*

> Running the search for index=windowslogs cyber* gives our answer. Wildcards have a lot of power!

<details>

  <summary>Spoiler warning: Answer</summary>
    
    12256

</details>

### Task 5: Filtering the Results in SPL

_fields_
Here we learn how to specify the fields we wish to select within the SPL. We can pipe from the search query term using the | character like in most shells. Fields are added with a + and removed with a -

> Eg.
> index=windowlogs | field + SourceIP + DestAddress - sourcetype

_search_
We can use the pipe and the command search to look for raw text in piped results. For example in the last task we searched for the term cyber and cyber* after specifying the index. This worked, but in a larger search it may be more clear to specify.

> index=windowlogs | search cyber
> or
> index=windowlogs | search cyber*

_dedup_
Removes duplicate fields from search results

_rename_

> rename field as new_field

*What is the third EventID returned against this search query?*

> Again this is too easy, the search query is give. Let's take a look at it though. we view it as a table that shows _time, EventId, Hostname and SourceName fields then we pipe to a reversing function that reverses the order.
> The EventID of third item down is...

<details>

  <summary>Spoiler warning: Answer</summary>
    
    4103

</details>

*Use the dedup command against the Hostname field before the reverse command in the query mentioned in Question 1. What is the first username returned in the Hostname field?*

> We're just going to add a section of pipe before the reverse that says dedup Hostname like this:
> index=windowslogs | table _time EventID Hostname SourceName | dedup Hostname | reverse

<details>

  <summary>Spoiler warning: Answer</summary>
    
    Salena.Adam

</details>

### Task 6: SPL - Structuring the Search Results

You might want to reduce the number of events you're viewing at one time. Or you're initially only interested in the start or the end of the list. Functions such as head and tails can give a snapshot of the start or end of the search results. If you've ever used Python and Pandas to do some data analysis you'll probably be pretty comfortable with these commands.

*Using the Reverse command with the search query index=windowslogs | table _time EventID Hostname SourceName - what is the HostName that comes on top?*

> just pipe the given search string to reverse:
> index=windowslogs | table _time EventID Hostname SourceName | reverse
<details>

  <summary>Spoiler warning: Answer</summary>
    
    James.browne

</details>

*What is the last EventID returned when the query in question 1 is updated with the tail command?*

> Initially I thought they wanted the tail of the reversed table but that didn't fit so just pipe to tail on the given search string and present the EventID as the answer.
> index=windowslogs | table _time EventID Hostname SourceName | tail

<details>

  <summary>Spoiler warning: Answer</summary>
    
    4103

</details>

*Sort the above query against the SourceName. What is the top SourceName returned?*

> Again here use the original search query and pipe to sort SourceName:
> index=windowslogs | table _time EventID Hostname SourceName | sort SourceName

<details>

  <summary>Spoiler warning: Answer</summary>
    
    Microsoft-Windows-Directory-Services-SAM

</details>

### Task 7: Transformational Commands in SPL

There's a lot of great information here on how to get statistical data using SPL and transforming the search. Also they touch on generating charts. 

*Q1: List the top 8 Image processes using the top command -  what is the total count of the 6th Image?*

> The Images field contains lists of processes. The following command will get the top 8. The questions asks for the 6th so count down 6 and then hover over to get the count.

> index=windowslogs| top limit=8 Image

<details>

  <summary>Spoiler warning: Answer</summary>
    
    196

</details>

*Q2: Using the rare command, identify the user with the least number of activities captured?*

> We start with the search:
> index=windowslogs*| rare limit=20 ActivityID
> Clicking on the result gets us the new search term
> index=windowslogs* ActivityID="{4F259F18-BCE1-0000-35FD-7393808AD601}"
> We can see that the UserName for this activity is:

<details>

  <summary>Spoiler warning: Answer</summary>
    
    James

</details>

*Q3: Create a pie-chart using the chart command - what is the count for the conhost.exe process?*

> I used the following search:
> index=windowslogs | chart count by Image

<details>

  <summary>Spoiler warning: Answer</summary>
    
    70

</details>

### Recap and Conclusion

SPL seems pretty intuitive and the Splunk interface is very clean and also intuitive. I like it! I guess these are simple examples but it definitely has some easy to memorize usage patterns especially if you're quite comfortable in bash and/or Python. I was a little confused by the meaning of dedup until I realised it stood for de-duplicate *facepalm*. Why this couldn't be called unique (or uniq as is quite often used) I don't know. If you do know please let me know!

Anyway, I really enjoyed this room. See you next time!