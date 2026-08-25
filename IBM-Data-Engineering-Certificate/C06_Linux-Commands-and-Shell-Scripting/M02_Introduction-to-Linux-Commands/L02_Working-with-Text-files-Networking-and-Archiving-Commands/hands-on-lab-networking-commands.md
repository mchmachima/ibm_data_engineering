::page{title="Hands-on Lab: Working with Networking Commands"}


<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-LX0117EN-SkillsNetwork/images/IDSN-logo.png" width="300" alt="cognitiveclass.ai logo">


Estimated time needed: **30** minutes

## Learning Objectives

After completing this lab, you will be able to:

- View your network configuration using the `hostname` and `ip` commands
- Test a network connection using the `ping` command
- Transfer data using the `curl` and `wget` commands

::page{title="About Skills Network Cloud IDE"}

Skills Network Cloud IDE (based on Theia and Docker) provides an environment for hands on labs for course and project related labs. Theia is an open source IDE (Integrated Development Environment), that can be run on desktop or on the cloud. To complete this lab, you will be using the Cloud IDE based on Theia.

## Important notice about this lab environment

Please be aware that sessions for this lab environment are not persisted. Thus, every time you connect to this lab, a new environment is created for you and any data or files you may have saved in a previous session will be lost. To avoid losing your data, plan to complete these labs in a single session.

::page{title="Exercise 1 - View configuration info about your network"}

### 1.1. Display your system\'s hostname and IP address

**`hostname`**

A **hostname** is a name that is assigned to a computer or device on a network, and it is used to identify and communicate with that device.

To view the current hostname, run the command below:

```
hostname
```
An **IP address** (Internet Protocol address) is a numerical label assigned to each device connected to a computer network that uses the Internet Protocol for communication.

You can use the `-i` option to view the IP address of the host:

```
hostname -i
```

### 1.2. Display network interface configuration

Please execute the below commands to install the iproute2 package:

```
sudo apt update
sudo apt install iproute2
```
## **iproute2**

The `ip` command is used to configure or display network interface parameters for a network.

To display the configuration of all network interfaces of your system, enter:

```
ip a
```

To display the configuration of a particular device, such as the ethernet adapter `eth0`, enter:

```
ip addr show eth0
```

`eth0` is usually the primary network interface that connects your server to the network.

You can see your server\'s IP address in line 2 after the word `inet`.

::page{title="Exercise 2 - Test network connectivity"}

### 2.1. Test connectivity to a host

**`ping`**

Use the `ping` command to check if `www.google.com` is reachable. The command keeps pinging data packets to server at `www.google.com` and prints the response it gets back. (Press `Ctrl`+`c` to stop pinging.)

```
ping www.google.com
```

If you want to ping only a limited number of times, use `-c` option.

```
ping -c 5 www.google.com
```

::page{title="Exercise 3 - View or download data from a server"}

### 3.1. Transfer data from a server

**`curl`**

You can use `curl` to access the file at the following URL and display the file\'s contents on your screen:

```
curl https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt
```

To access the file at the given URL and also save it in your current working directory, use the `-O` option:

```
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt
```

You can also use `curl` to view the HTML code for any web page if you know its URL.

### 3.2. Download file(s) from a URL

**`wget`**

The `wget` command is similar to `curl`, however its primary use is for file downloading. One unique feature of `wget` is that it can recursively download files at a URL.

To see `wget` in action, first remove `usdoi.txt` from your current directory: 

```
rm usdoi.txt
```

then download it again using `wget` as follows:

```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt
```

::page{title="Practice exercises"}

Before you begin, ensure you\'re in your `/home/project` directory by entering:

```
cd `/home/project`
pwd
```

##### 1. Display your host\'s IP address.

<details>
    <summary>Click here for Hint</summary>

> Use the `hostname` command with the correct option.
	
>**Note:** There are many other ways to get your IP address, for example using `ping` or `ip`. Both will display your IP address, but they will also include a lot of extra information.

</details>

<details>
    <summary>Click here for Solution</summary>

```
hostname -i
```

</details>

##### 2. Get connectivity stats on your connection to `www.google.com`.

<details>
    <summary>Click here for Hint</summary>

> Use the `ping` command.

</details>

<details>
    <summary>Click here for Solution</summary>

```
ping www.google.com
```

</details>

##### 3. View info about your ethernet adapter `eth0`.

<details>
    <summary>Click here for Hint</summary>

> Use the `ip` command with the correct argument.

</details>

<details>
    <summary>Click here for Solution</summary>

```
ip addr show eth0
```

</details>

##### 4. View the HTML code for `www.google.com`\'s landing page.

<details>
    <summary>Click here for Hint</summary>

> Use the `curl` command with the correct argument.

</details>

<details>
    <summary>Click here for Solution</summary>

```
curl www.google.com
```

</details>

##### 5. Download the HTML code for `www.google.com`\'s landing page.

<details>
    <summary>Click here for Hint</summary>

> Use the `wget` command with the correct argument.

</details>

<details>
    <summary>Click here for Solution</summary>

```
wget www.google.com
```

>**Note:** `wget` saves the HTML code as `index.html`. You can check this with:

```
ls -l
```

</details>

::page{title="Summary"}

In this lab, you learned how to:
- View your network configuration using the `hostname` and `ip` commands
- Test a network connection using the `ping` command
- Transfer data using the `curl` and `wget` commands

## Authors

Jeff Grossman  
Ramesh Sannareddy  
Sam Prokopchuk  

### Other contributors

Rav Ahuja
<!--
## Change Log

| Date (YYYY-MM-DD) | Version | Changed By        | Change Description                 |
| ----------------- | ------- | ----------------- |----------------------------------- |
| 2025-01-24        | 3.4     | Nikesh Kumar 	| Updated ifconfig to iproute2 in Excercise 1.2 |
| 2023-05-23        | 3.3     | Benny Li           | Review lab 			   		|
| 2023-04-27        | 3.2     | Nick Yi           | QA Pass                            | 
| 2023-04-13        | 3.1     | Nick Yi           | ID Review                          | 
| 2023-01-10        | 3.0     | Jeff Grossman     | Split lab and add new content      |  
| 2021-12-02        | 2.1     | Jeff Grossman     | Review and Update lab              |  
| 2021-11-29        | 2.0     | Sam Prokopchuk    | Update lab contents and split      |  
| 2021-05-30        | 1.0     | Ramesh Sannareddy | Created initial version of the lab | 

-->
<h3 align="center"> &#169; IBM Corporation. All rights reserved. <h3/>
