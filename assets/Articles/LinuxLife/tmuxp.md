# TMUXP - 10x your Tmux

## A Tmux Workspace Manager

### What is Tmuxp?

Tmuxp is workspace management tool built in python that allows you to define tmux sessions, windows, panes and commands to run in either yaml or json files.

It's often said that if you have to do something more than once then you should automate it and I can't believe I've spent years using tmux without automating the layout. I almost always set my pane layout to the main-horizontal style with 4 panes. One large main pane and three smaller ones containing a vpn and a couple of panes for doing little tasks. 

I finally thought to myself that instead of creating 4 panes and pressing the prefix and space until the layout was just what I wanted, I could automate it.

I realise most of what I've done is probably do-able in the tmux.conf file. But when I tried tmuxp I loved the simplicity.

---

### Installation

Installation was a breeze. I'm using Ubuntu with a few extra repos. So apt worked fine for me.

```bash 
sudo apt install tmuxp
```

It's a Python based tool and is also available via pip. 

```bash
pip install --user tmuxp
```

Follow [their instructions](https://tmuxp.git-pull.com/quickstart.html) for yum or brew

---

### A few teething problems

I did run into a few minor hiccoughs though. Especially frustrating was trying to set the height of the main pane. The instructions suggested to do the following:

```yaml
> options:  
>   main-pane-height: 35  
```

For some reason this resulted in a tiny main pane and canging the value of 35 didn't make any difference. Adding a % to the number eg.:

```yaml
> options:  
>   main-pane-height: '65%'  
```

Did result in almost what I wanted, but again something strange was going on when I tried '50%' the sizing worked perfectly but any value larger ended up with about 95% of the area taken up. I tried many ways to resize the main pane including writing a small bash script for selecting and resizing the pane and running it on launch.

```bash
#! /bin/bash  
tmux select-pane -t 0.0  
tmux resize-pane -y 65
```

This didn't seem to do anything when run from the tmuxp yaml file. My guess is that another pane was automatically resizing after this script was run. I was connecting to a VPN in one of the non main panes. If anyone reading this does know what the issue here is, I would be very grateful if you would let me know!

### The Solution

While exploring the documentation for tmuxp I found the freeze command which really simplified my setup process. Just configure the tmux session windows and panes as you like them and then run:

```bash
tmuxp freeze [session_name] -o configuration_filename.yaml
```

Use the created file as a starting point for further configuration. It seems the freeze command sets the absolute size of the panes. Nice.

Here's my basic configuration.yaml file

```yaml
session_name: t  
windows:  
    - focus: 'true'  
    layout: 3359,156x39,0,0[156x34,0,0,1,156x4,0,35{51x4,0,35,2,51x4,52,35,3,52x4,104,35,4}]
    options: {}  
    panes:  
        - shell_command: ./panes.sh  # This is a bash script that runs a few commands I like to start with.  
        - pane  
        - pane  
        - pane  
    window_name: window
```

That's it for now. I realise this is a very brief look at tmuxp. Sorry if you wanted more. If I modify anything considerably in the future I will write more on it. If you have any suggestions or want to tell me where I have gone wrong, then do contact me - I always want to learn more. Happy multiplexing!
