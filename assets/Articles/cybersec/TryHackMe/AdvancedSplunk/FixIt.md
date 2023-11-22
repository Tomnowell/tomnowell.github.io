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

I used [regex101](http://regex101.com) to come up with the answer. We need to split before [network_log].
