# Logstash: Data Processing Unit

## Learn how to collect, process and transform data with Logstash.

## SOC Level 2 Learning Pathway

### Introduction

Wow, this looks like a long room - better get started!

We're going to start by installing the ELK stack. This consists of:

- Elastic Search
- Logstash
- Kibana

Elasticsearch is the underlying search and analytics engine.

Logstash is a server-side data processing pipeline that.

Kibana is a data visualisation tool and dashboard that provides the user interface for the elk stack.

*Q1: Move to the next task.*

> No answer needed  

### Task 2 - Connect with the Lab

Start the room VM and either use the Attack box or ssh in over the VPN. I chose the latter as credentials are provided and I like my terminal.

We'll want to be root user so:

```bash
sudo su
```

Note: We only really need the VM for task 3, 4 and 5. But they follow on from one another, so it's best to at least get that far in one attempt. If your VM isn't a snail then you should be okay. Recently I've had some machines that grind along like a sloth in slow-mo. I found certain times of day are better than others and changing VPN locations hasn't helped. I guess that THM have some maximum spend setting on AWS so busy times result in slow machines. It's difficult to tell.

*Q1: Connect with the Lab*

> No answer needed.

### Task 3 - Elasticsearch Installation and Configuration

We start off installing Elasticsearch.

If you're not root already then switch to root user. All the install files are in the /home/tools/ directory.

Let's start the installer.

```bash
cd /home/tools/elasticsearch
dpkg -i elasticsearch.deb
```

Take a note of the important information displayed after the package has installed:

In my case I noted the password. Of course, your password will be different.

The generated password for the elastic built-in superuser is : oSpz_0G6xYa5Al1Yya*y 

We're also instructed to note a couple of useful commands.

Reset the password of the elastic built-in superuser with:

Here's a summary of the steps to install.

```bash
/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

Generate an enrollment token for Kibana instances with:

```bash
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

Elasticsearch is installed now we will set it to start whenever there is a system restart. 

Still as root user:

```bash
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
systemctl status elasticsearch.service
```

Next, we move onto configuration:

```bash
cd /etc/elasticsearch
```

Edit the elasticsearch.yml file. In the network section change network.host by removing the preceding '#' and changing the IPv4 address to loopback 127.0.0.1 also uncomment the http.port.

> network.host: 127.0.01
> http.port: 9200

Restart elasticsearch:

```bash
systemctl restart elasticsearch.service
```
Here's a summary of the steps to install.

```bash
cd /home/tools/elasticsearch
dpkg -i elasticsearch.deb
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
nano /etc/elasticsearch/elasticsearch.service
# Edit IP Address & uncomment port
systemctl restart elasticsearch.service
```

*Q1: What is the default port Elasticsearch runs on?*

> It's right there in the elasticsearch.yml file

<details>

  <summary>Spoiler warning: Answer</summary>

  9200

</details>

---

*Q2: What version of Elasticsearch we have installed in this task?*

> This is shown at installation.

<details>

  <summary>Spoiler warning: Answer</summary>

    8.8.1

</details>

*Q3: What is the command used to check the status of Elasticsearch service?*

> This is actually given in the task info.

<details>

  <summary>Spoiler warning: Answer</summary>

  systemctl status elasticsearch.service

</details>

---

*Q4: What is the default value of the network.host variable found in the elasticsearch.yml file?*

> This is the value that was in the elasticsearch.yml file before you replaced the IPv4 address with 127.0.0.1

<details>

  <summary>Spoiler warning: Answer</summary>

  192.168.0.1

</details>

---

### Task 4 - Logstash: Installation and Configuration

We're told to follow these steps.

```bash
cd /home/tools/logstash
dpkg -i logstash.deb
systemctl daemon-reload
systemctl enable logstash.service
systemctl start logstash.service
nano /etc/logstash/logstash.service
# Edit IP Address & uncomment port
systemctl restart logstash.service
```

*Q1: What is the configuration reload interval set by default in logstash.yml?*

> Take note of this number when editing the logstash.yml file.

<details>

  <summary>Spoiler warning: Answer</summary>

  3s

</details>

---

*Q2: What is the Logstash version we just installed?*

>> This is revealed in the installation phase.

<details>

  <summary>Spoiler warning: Answer</summary>

    8.8.1

</details>

---

### Task 5 - Kibana: Installation and Configuration

The installation instructions for kibana from the downloaded .deb package.

```bash
cd /home/tools/kibana
dpkg -i kibana.deb
systemctl daemon-reload
systemctl enable kibana.service
systemctl start kibana.service
nano /etc/kibana/kibana.yml
# Edit IP Address & uncomment port
systemctl restart kibana.service
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
# Note the enrollment token for login
```

Navigate to the given address to log in to the system with a browser connected to the THM network.

*Q1: What is the default port Kibana runs on?*

> This is noted in the kibana.yml file.

<details>

  <summary>Spoiler warning: Answer</summary>

  5601

</details>

---

*Q2: How many files are found in /etc/kibana/ directory?*

> ls /etc/kibana/ # count 'em!

<details>

  <summary>Spoiler warning: Answer</summary>

  3

</details>

*Q3: Use the command systemctl stop kibana.service to stop Kibana.*

> No answer needed.

---

That's it for the VM. Hopefully you were able to get through it.

### Task 6 - Logstash Overview

We're basically be lead through an overview of Logstash.

*Q1: Which plugin is used to perform mutation of event fields?*

> The bare infinitive verb form of the noun mutation...

<details>

  <summary>Spoiler warning: Answer</summary>

  mutate

</details>

---

*Q2: Which plugin can be used to drop events based on certain conditions?*

> What do you do to the events?

<details>

  <summary>Spoiler warning: Answer</summary>

  drop

</details>

---

*Q3: Which plugin parses unstructured log data using custom patterns?*

> This is a matter of matching description to plugin name

<details>

  <summary>Spoiler warning: Answer</summary>

  Grok

</details>

---

### Task 7 - Writing Configurations

We are lead around the Logstash manual for this one.

*Q1: Is index a mandatory option in Elasticsearch plugin? (yay / nay)*

> The mandatory options are stated on the right hand side.

<details>

  <summary>Spoiler warning: Answer</summary>

  nay

</details>

---

*Q2: Look at the file input plugin documentation; which field is required in the configuration?*

> First find the input plugins and then look for the file entry. Only one field is required.

<details>

  <summary>Spoiler warning: Answer</summary>

  path

</details>

---

*Q3: Look at the filter documentation for CSV; what is the third field option mentioned?*

> Go to filter documentation and find CSV.

<details>

  <summary>Spoiler warning: Answer</summary>

  columns

</details>

---

*Q4: Which output plugin is used in the above example?*

> This is referring to the example in the task materials. The output plugin details are written in the configuration file

<details>

  <summary>Spoiler warning: Answer</summary>

  elasticsearch

</details>

### Task 8 - Logstash: Input Configurations

Here the focus is on the input section of the configuration file.

*Q1: If we want to read a constant stream from a TCP port 5678, which input plugin will be used?*

> The protocol being read is in the question.

<details>

  <summary>Spoiler warning: Answer</summary>

  TCP

</details>

---

*Q2: According to the Logstash documentation, the codec plugins are used to change the data representation. Which codec plugin is used for the CSV based data representation?*

> Again the format is in the question.

<details>

  <summary>Spoiler warning: Answer</summary>

  CSV

</details>

---

### Task 9 - Logstash: Filter Configurations

Next we look at filtering.

*Q1: Which filter plugin is used to remove empty fields from the events?*

> Another word to cut something especially something unnecessary or unwanted.

<details>

  <summary>Spoiler warning: Answer</summary>

  prune

</details>

---

*Q2: Which filter plugin is used to rename, replace, and modify event fields?*

> Another word to change something is to...

<details>

  <summary>Spoiler warning: Answer</summary>

 mutate

</details>

---

*Q2: Add plugin Name to rename the field 'src_ip' to 'source_ip.' And the fixed third line.*

```yaml
filter 
{ 

    mutate 
     { 
             plugin_name = { "field_1" = "field_2" } 
      }
}
```

*Answer only the corrected third line.*

> we need to rename "src_ip" to "source_ip".

<details>

  <summary>Spoiler warning: Answer</summary>

  rename { "src_ip" => "source_ip" }

</details>

---

### Task 10 - Logstash: Output Configurations

Let's look at output plugins and configuration.

*Q1: Can we use multiple output plugins at the same time in order to send data to multiple destinations? (yay / nay):*

<details>

  <summary>Spoiler warning: Answer</summary>

  yay

</details>

---

*Which Logstash output plugin is used to print events to the console?*

<details>

  <summary>Spoiler warning: Answer</summary>

  stdout

</details>

---

*Update the following configuration to send the event to the Syslog server.*

```yaml
output {
  plugin {
    field1 => "my_host.com"
    field2 => 514   
  }
}
```

> I wasn't very confident that this answer was correct, but it seems to work.

<details>

  <summary>Spoiler warning: Answer</summary>

  syslog,host,port

</details>

---

### Task 11 - Logstash: Running Configurations

Finally we look at how to run configurations.

*Q1: Which command is used to run the logstash.conf configuration from the command line?*

> This command is stated in the task materials

<details>

  <summary>Spoiler warning: Answer</summary>

  logstash -f logstash.conf

</details>

*Q2: What will be the input, filter, and output plugins in the below-mentioned configuration if we want to get the comma-separated values from the command line, and apply the filter and output back to the terminal to test the result?*

```yaml
input{
  input_plugin{}
} # Note THM materials omit this closing bracket.
filter{
  filter_plugin{}
}
output{
  output_plugin{}
}
```

*Answer format: input_plugin,filter_plugin,output_plugin*

> Console in is often referred to as stdin and console output is stdout as we saw earlier. In the middle of the pipeline we need a plugin that handles comma separated values.

<details>

  <summary>Spoiler warning: Answer</summary>

  stdin,csv,stdout

</details>

### Task 12 - Exercise and Conclusion

That looked like quite a lot of work at the start but the latter tasks were quite light and didn't involve using the VM at all.

We're given a small exercise to do using logstash configuration which is useful to solidify learning but not required to answer any questions.

I'd like to have a bit more practice using logstash - but there's a lot to take in and perhaps we don't need to be intimately familiar at this stage.

Next up Custom Alert Rules in Wazuh!