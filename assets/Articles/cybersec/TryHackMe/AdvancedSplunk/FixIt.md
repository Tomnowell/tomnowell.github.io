# Fixit

## Fix the log parsing issue and analyze the logs in Splunk.

## SOC Level 2 Learning Pathway

### Introduction

We are still John.

This is the challenge room to complete the Advanced Splunk module. We have to fix a bunch of problems with the Splunk configuration. 

As there is only one 'task' in this room. I will separate each question out to its own heading.

We are told there are three levels to this room.

Level 1 - We will fix the event boundaries.

Level 2 - Extract new fields.

Level 3 - Analyse the data.

Once the VM has started and Splunk has had a couple of minutes to start up. You can access it from the split screen or your own machine via VPN at:

> http://MACHINE_IP:8000

Like the last rooms, if you want to access the machine via ssh you can reset the password for the ubuntu user in split screen view terminal and then ssh in using those credentials.

In split screen terminal:

```bash
sudo su
passwd ubuntu
```

Set a new password.

In your own terminal connected to the THM VPN.

```bash
ssh ubuntu@MACHINE_IP
```

type the new password when ready and elevate to root

```bash
sudo su
```

Let's go!

### Q1: What is the full path of the FIXIT app directory?

We learned where apps are installed by default in the previous room. This will also be the same location in which we created our DataApp in the previous room.  

<details>

  <summary>Spoiler warning: Answer</summary> 
    /opt/splunk/etc/apps/fixit

</details>

---

### Q2: What Stanza will we use to define Event Boundary in this multi-line Event case?

If we take a look at how the events are displayed we can see individual events are being split into two. So we are aiming for an action that only splits in certain cases.

<details>

  <summary>Spoiler warning: Answer</summary>
    BREAK_ONLY_BEFORE

</details>

### Q3: In the inputs.conf, what is the full path of the network-logs script?

Let's look at the input.conf file. It's in the default directory for the fixit app.

```bash
cat /opt/splunk/etc/apps/fixit/default/inputs.conf
```

<details>

  <summary>Spoiler warning: Answer</summary>
    /opt/splunk/etc/apps/fixit/bin/network-logs
</details>

---

### Q4: What regex pattern will help us define the Event's start?

I used [regex101](http://regex101.com) to come up with the answer. We need to split before each '[network_log]'. Note: Square brackets [] denote a range of values in regex, so we need to escape the square brackets with backslashes.

<details>
  <summary>Spoiler warning: Answer</summary>
    \[Network-log\]
</details>

### Q5: What is the captured domain?

We don't actually need to implement the props.conf file yet, but one currently doesn't exist and it would be easier to read the logs with it. Let's implement it

```bash
cd /opt/splunk/etc/apps/fixit/default/  
echo -e "[network_logs]
SHOULD_LINEMERGE = true
BREAK_ONLY_BEFORE = \[Network-log\]" > props.conf
```

Restart Splunk and our events should be correctly separated.

```bash
/opt/splunk/bin/splunk restart
```

We can see a domain appearing again and again.

<details>
  <summary>Spoiler warning: Answer</summary>
    Cybertees.THM
</details>

---

### Q6,7,8,9: How many ... are captured in the logs?

To answer this question, we're going to need to extract a 'country' field from the log. Let's get to work making a regex that separates each field logically.

Currently we have the long string that includes a lot of information. One example from the log file is:

> User named Patricia Allen from Custom department accessed the resource Cybertees.THM/signup.html from the source IP 192.168.1.3 and country Australia at: Wed Nov 22 05:01:31 2023

From this we can reason that some useful fields to extract would be:

- Username 
- Department
- Resource Accessed
- Source IP
- Country
- Date
- Time
- Year

Note: In the task materials only 5 fields are suggested and I couldn't get my regex for all the fields I just mentioned working. I will leave it here because I'd love feedback if someone knows why it didn't work. The first 5 fields worked fine and then I was getting $5, $6 output in Splunk for the latter fields.

I put the whole thing into regex101 and got to work designing a regex to separate out each value. I started with the Username mostly borrowed from the exercise in the previous room.

Here's the vicious snake that I came up with:

```regex
User\snamed\s([\w\s]+)\sfrom\s([\w\s]+)\sdepartment\saccessed\sthe\sresource\s([^\s]+)\sfrom\sthe\ssource\sIP\s([\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}]+)\sand\scountry\s\n([\w\s]+)\sat:\s(.{3}\s.{3}\s\d{2})\s(\d{2}:\d{2}:\d{2})\s(\d{4})
```

I'm sure there's a much neater way to do that, please let me know your solution! I realise the IP address could be obtained with just ([^/s]+) but this way seemed more explicit. What do you think? Is it better to be concise or explicit when dealing with regular expressions? I always prefer the extra clarity of longer expressions when writing code but regular expressions often look obtuse anyway (to me). I'm torn on which is better and I'd love to know better.

Now we have the regex let's extract those fields. I counted the colour coded fields in regex101 to do a sanity check. I extracted 8 fields in total. Should Date and Year be separate? I'm not sure exactly how to combine them. I think they should probably be joined because if one is searching for events on say, May 22nd 2022 we could just search one field. Another user might mistakenly believe that values in the Date field are already distinguished by year and end up with values from multiple years in their search. I don't think we need to do it for these questions but I've noted it for further investigation. I imagine the solution will involve using sed to adjust the string to have date and year combined and then adjusting the regular expression to capture it as one field.

While the regex I created seemed to work all values from $5 (Country) on were not processed correctly. I'm not sure what was going on - probably an error in my regex or a difference in how Splunk interprets the regex. Please let me know if you can clear that up for me. I decided to focus on the fields suggested in the task ie.

-Username
-Country
-Source_IP
-Department 
-Domain (Resource)

Note: I think the error was due to not adding /g global tag at the end?
So here's my new regex:

```regex
User named ([\w\s]+)\sfrom\s([\w\s]+)\sdepartment[\w\s]+resource\s([^\s]+)\sfrom[\w\s]+IP\s(\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3})\sand country\s([\w\s]+)\sat.
```

A little shorter than the last one. We can write our transforms.conf file like so:

```bash
echo -e "[networks_custom_fields]
REGEX = User named ([\w\s]+)\sfrom\s([\w\s]+)\sdepartment[\w\s]+resource\s([^\s]+)\sfrom[\w\s]+IP\s(\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3})\sand country\s([\w\s]+)\sat.*
FORMAT = Username::\$1 Department::\$2 Domain::\$3 Source_IP::\$4 Country::\$5
WRITE_META = true" > transforms.conf
```

Next, add the link in the props.conf

```bash
echo "TRANSFORM-networks = networks_custom_fields" >> props.conf
```

Now let's make our fields.conf
```bash
echo -e "[Username]
INDEXED=true

[Department]
INDEXED=true

[Domain]
INDEXED=true

[Source_IP]
INDEXED=true

[Country]
INDEXED=true" > fields.conf 
```

And we're ready to go!

```bash
/opt/splunk/bin/splunk restart
```

> Count!  
> index=main sourcetype="network_logs" |  stats dc (Country)  
> Or just look left - the value should appear next to the field name in grey on the left hand side.

> Again look left.

> Left hand side or use distinct-count dc(Username) as before.

### Which configuration files were used to fix our problem? [Alphabetic order: File1, file2, file3]

We wrote to three files to perform the previous four questions. If you know your ABC then you will know that f comes before t which comes before p.

<details>
  <summary>Spoiler warning: Answer</summary>
    fields.conf, transforms.conf, props.conf
</details>

### What are the TOP two countries the user Robert tried to access the domain from? [Answer in comma-separated and in Alphabetic Order][Format: Country1, Country2]?

I'm sure there's an easier way to do this:

> index=main sourcetype="network_logs" Username="Robert Wilson" \ top limit=0 Country | fields Country count
> The top 2 values are the answer:

<details>
  <summary>Spoiler warning: Answer</summary>
    Canada, United States
</details>

Let's use wildcards to search for a Domain containing the secret-document.pdf string.

> index=main sourcetype="network_logs" Domain=*secret-document.pdf*

<details>
  <summary>Spoiler warning: Answer</summary>
    Sarah Hall
</details>

### Conclusion

It was really fun and educational to come up with the regular expression for this task. It's elevated my regex skills and I feel much more fluent in the regex lingo. Of course, it has also taught me a lot about Splunk and how it can be configured to extract meaningful data.

That's the end of the Advanced Splunk module we are now experts...almost. Next up Elk!