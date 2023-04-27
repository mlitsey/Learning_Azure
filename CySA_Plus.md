# CySA+

[Link](https://interactive.linuxacademy.com/diagrams/SentinelShieldDiagram.html)

## Scanning a Host with NMAP

In this lab, we'll use a well-known network and host scanning tool named NMAP to scan a host for open ports and attempt to identify which services are running on those ports. We will then export that information to the `scan.txt` document and review it.

### Install NMAP

The first thing we need to do, once signed in, is to install NMAP. To do so, complete the following:

1. Switch from the `cloud_user` account to `root` by running `sudo su` and entering the `cloud_user`'s password.
2. Run the command:
    
    `apt-get update`
    
3. Install NMAP by running the command:
    
    `apt-get install -y nmap`
    

### Scan the LocalHost with NMAP

Once installed, we need to learn the scan abilities for NMAP:

1. To learn the different scan types for NMAP, run the following command:
    
    `nmap`
    
2. Multiple scans can be run with NAMP. To test it out, let's start by looking at the information on our localhost:
    
    `nmap localhost`
    
3. We see information over our localhost. But, we want to have more information; specifically, we want to know the service and version information of our open ports. We can do this by using the `-sV` command:
    
    `nmap -sV localhost`
    
4. Now, we want to make sure we know the exact operating system. To do so, run the following:
    
    `nmap -O localhost`
    
5. For this lab, we want to send the version information we found to the `scan.txt` document. To do so, enter the following:
    
    `nmap -sV localhost -oN scan.txt`
    
6. Make sure that the information west sent to the file with one, or both, of the following commands:
    
    `vi scan.txt`
    
    or
    
    `cat scan.txt`
    
    We see the same information that was provided when we ran the `nmap -sV localhost` command.


