# Logstash: Data Processing Unit

## Learn how to collect, process and transform data with Logstash.

## SOC Level 2 Learning Pathway

### Introduction

Wow, this looks like a long room - better get started!

Start the room VM and either use the Attack box or ssh in over the VPN. I chose the latter as credentials are provided and I like my terminal.

We'll want to be root user so:

```bash
sudo su
```

We're going to start by installing the ELK stack. This consists of:

- Elastic Search
- Logstash
- Kibana

Elasticsearch is the underlying search and analytics engine.

Logstash is a server-side data processing pipeline that.

Kirbana is a data visualisation tool and dashboard that provides the user interface for the elk stack.

### Task 2 - Installing Elasticsearch

We start off installing Elasticsearch.

If you're not root already then switch to root user. All the install files are in the /home/tools/ directory.

Let's start the installer.

```bash
cd /home/tools/elasticsearch
dpkg -i elasticsearch.deb
```

Take a note of the important information displayed after the package has installed:

In my case I noted the password. Of course, your password will be different.

The generated password for the elastic built-in superuser is : wQIjyJxn3ArG9BfE6=0a

We're also instructed to note a couple of useful commands.

Reset the password of the elastic built-in superuser with:

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

*Q1: What is the default port Elasticsearch runs on?*

> It's right there in the elasticsearch.yml file

<details>

  <summary>Spoiler warning: Answer</summary>

    9200

</details>

*Q2: What version of Elasticsearch we have installed in this task?*

> This is shown at installation. Once your elasticsearch is up and running on localhost you can use the following command to get the front page info.

```bash
curl -XGET http://127.0.0.1:9200
```

<details>

  <summary>Spoiler warning: Answer</summary>

    8.8.1

</details>

> This is actually given in the task info.

<details>

  <summary>Spoiler warning: Answer</summary>

    systemctl status elasticsearch.service

</details>

> This is the value that was in the elasticsearch.yml file before you replaced the IPv4 address with 127.0.0.1

<details>

  <summary>Spoiler warning: Answer</summary>

    192.168.0.1

</details>


### Task 3 - Installing Logstash


*What is the configuration reload interval set by default in logstash.yml?
3s
What is the Logstash version we just installed?

8.8.1
Task 5  Kibana:*

### Task 4 - Installing Kibana