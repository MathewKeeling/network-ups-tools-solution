# Network UPS Tools (NUT)


## Table of Contents

- [Overview](#overview)
- [Preparation](#preparation)
- [Helpful Documentation](#helpfuldocumentation)
- [Installation](#installation)
- [Version Control](#versioncontrol)


<a id="overview"></a>
## Overview

### What is Network UPS Tools (NUT)?

[Source: Read more](https://networkupstools.org/)

    A: The primary goal of the Network UPS Tools (NUT) project is to provide support for Power Devices, such as Uninterruptible Power Supplies, Power Distribution Units, Automatic Transfer Switches, Power Supply Units and Solar Controllers. NUT provides a common protocol and set of tools to monitor and manage such devices, and to consistently name equivalent features and data points, across a vast range of vendor-specific protocols and connection media types. 


<a id="preparation"></a>
## Preparation


### Test System Information
```
# NUT Host
hostname: hostname
ipaddr:   <0.0.0.0>

# Test UPS
nic model:    AP9631
ups model:    Smart-UPS 1500 RM
ipv4:         0.0.0.0/24
username:     <redacted>
password:     <redacted>
```


<a id="helpfuldocumentation"></a>
## Helpful Documentation 

- [**Install Guide (2021)**](https://gist.github.com/Jiab77/0778ef11a441f49df62e2b65f3daef76)
- [Install Guide (2018)](https://docs.deeztek.com/books/ubuntu/page/installing-nut-network-ups-tools-on-ubuntu-1804-lts)
- [Install Guide (2009)](https://blog.shadypixel.com/monitoring-a-ups-with-nut-on-debian-or-ubuntu-linux)
- [Configuration Guide](https://networkupstools.org/docs/user-manual.chunked/Configuration_notes.html)
- [Examples Guide](https://rogerprice.org/NUT/ConfigExamples.A5.pdf)


<a id="installation"></a>
## Installation

This installation guide reflects an installation on a virtual machine running **Ubuntu 22.04.4 LTS** (server).

```bash
# Install the binaries
sudo apt install nut -y
sudo apt install nut-snmp -y
sudo apt install snmp -y



# set up git repository
git_repo_url='http://hostname.contoso.com/contoso-services/nut'
git_repo_dir='/etc/nut'
cd $git_repo_dir

USERNAME="email@example.com"
NAME="Mathew Keeling"

# set up username
git config --global user.email "$USERNAME"
git config --global user.name "$NAME"

git init
git add .
git commit -m "Adding existing project files before sending to GitLab."
git remote add origin $git_repo_url
git push -u origin master
```

### nut-scanner

Generate the config bby running the following command:

```bash
nut-scanner -S -s <0.0.0.0>

# You should get an output like this:

[nutdev1]
        driver = "snmp-ups"
        port = "0.0.0.0"
        desc = "APC"
        mibs = "ietf"
        community = "public"
[nutdev2]
        driver = "snmp-ups"
        port = "0.0.0.0"
        desc = "Smart-UPS 1500 RM"
        mibs = "apcc"
        community = "public"
```

### Modify ups.conf

```bash
# using the output of the above commands:

vi /etc/nut/ups.conf

# Place this at the end of your .conf file
[<REDACTED>]
        driver = "snmp-ups"
        port = "0.0.0.0"
        desc = "Smart-UPS 1500 RM"
        mibs = "apcc"
        community = "public"
```

### Create a monitoring user

```bash
sudo su -
UPS_TO_MONITOR="<REDACTED>"
SUPER_SECURE_PASSWORD="<PASSWORD>"
echo -e "\n[monitor]\n\tpassword = ${SUPER_SECURE_PASSWORD}\n\tupsmon master\n" | tee -a /etc/nut/upsd.users

# Validate
echo -e "\nMONITOR $UPS_TO_MONITOR@localhost 1 monitor $SUPER_SECURE_PASSWORD master\n"

# It should look like:
MONITOR @localhost 1 monitor <your_password_here> master

# Take note of the output of that command.

# Add that line in between the examples section, and the MINSUPPLIES section.

vi /etc/nut/upsmon.conf
```


### Update the Scheduling Script

Read more about this [here](/install/upsd/linux/scripts/README.md)


### Update the Commands Scheduling

Now that we have improved the existing commands scheduling scripts, we have to edit the /etc/nut/upssched.conf file and change or set some values:

```bash
vi /etc/nut/upssched.conf

ups_name='apc-ups-apc001'

# Define PIPE file
sudo sed -e 's|# PIPEFN /run/|PIPEFN /run/|' -i /etc/nut/upssched.conf

# Define Lock file
sudo sed -e 's|# LOCKFN /run/|LOCKFN /run/|' -i /etc/nut/upssched.conf

# Add commands defined in the scripts
echo -e "\n# Custom commands" | sudo tee -a /etc/nut/upssched.conf
echo -e "AT ONLINE ${ups_name}@localhost EXECUTE online\nAT ONBATT ${ups_name}@localhost EXECUTE onbatt\nAT LOWBATT ${ups_name}@localhost EXECUTE lowbatt" | sudo tee -a /etc/nut/upssched.conf

# Check the result
sudo vi /etc/nut/upssched.conf
```

### Configuring nut.conf

```bash
# Change the running mode
sudo sed -e 's/MODE=none/MODE=netserver/' -i /etc/nut/nut.conf

# Check the result
sudo vi /etc/nut/nut.conf
```

### Configuring upsd.conf

Enabling Listening Server

This allows clients to poll the upsd for information relating to the status of a UPS.

```bash
vi /etc/nut/upsd.conf

# Add these lines to the end of the file.
LISTEN 0.0.0.0 3493
```


### Service Management

```bash
# Status

systemctl status nut-client.service
systemctl status nut-driver.service
systemctl status nut-monitor.service
systemctl status nut-server.service

# Stop
systemctl stop nut-client.service
systemctl stop nut-driver.service
systemctl stop nut-monitor.service
systemctl stop nut-server.service

# Start
systemctl start nut-client.service
systemctl start nut-driver.service
systemctl start nut-monitor.service
systemctl start nut-server.service

# Restart
systemctl restart nut-client.service
systemctl restart nut-driver.service
systemctl restart nut-monitor.service
systemctl restart nut-server.service

# Restart all services
for S in nut-client.service nut-driver.service nut-monitor.service nut-server.service ; do sudo systemctl restart $S ; done

# Status for all services
for S in nut-client.service nut-driver.service nut-monitor.service nut-server.service ; do systemctl status $S -l ; done
```


### Validating Results

```bash
# restart all services

snmpwalk -c public -v1 0.0.0.0 | less

name_of_ups="<REDACTED>"
upsc $name_of_ups
```


<a id="versioncontrol"></a>
## Version Control 

[General Guide on How to Setup a Remote Gitlab repository](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-add-and-push-an-existing-project-to-GitLab#:~:text=Add%20all%20of%20your%20project's,have%20been%20uploaded%20to%20GitLab.)


Go to Gitlab and create a new repository. 

```bash
# This variable should be set to equal the url of your git repository.
git_repo_url='http://git.example.contoso.com/example-group/example-repo'
git_repo_dir='/etc/nut'
cd $git_repo_dir

git init
git add .
git commit -m "Adding existing project files before sending to GitLab."
git remote add origin $git_repo_url
git push -u origin master
```