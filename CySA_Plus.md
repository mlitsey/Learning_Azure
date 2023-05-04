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


# Analyzing Possible Malware

## Introduction

In this lab, we will learn how to analyze a file that is suspected of being malware. We will do this in two ways:

- Upload the file directly to VirusTotal to check if it is malicious or not.
- Create an MD5 hash of the file and then search the VirusTotal website for the hash.

## Setting Up the Environment

1. Using VNC, connect to the public IP address of the instance on port 5901 (`x.x.x.x:5901`).
2. Log in using the credentials provided on the lab instructions page.

`ssh cloud_user@<PUBLIC_IP_ADDRESS>`

## Method 1: Download the File and Upload it to the VirusTotal Website

### Download the Suspicious File

1. Open your web browser.
2. Navigate to the provided [GitHub](https://github.com/linuxacademy/content-cysa-wiresharkanalysis/blob/master/eicar_com.zip) URL.
3. Click **Download**.
4. Save the file to your Downloads folder.

### Upload the File to VirusTotal

1. In your web browser, navigate to [http://www.virustotal.com](http://www.virustotal.com/).
2. Click **Choose file**.
3. Select the file we downloaded earlier, and click **Open** to upload it for analysis.

## Method 2: Generate the MD5 Hash of the Downloaded File and Run it through the VirusTotal Website

### Generate the MD5 Hash

1. Open your terminal application.
2. Change to your `/downloads` directory.

`cd Downloads`

1. List the contents of the directory.

`ls`

1. Generate the MD5 hash of the file.

`md5sum eicar_com.zip`

1. Copy the hash to your clipboard.

### Run the Hash through the VirusTotal Website

1. In your web browser, go to the VirusTotal home page.
2. Click the **Search** tab.
3. Paste the file hash we copied to the clipboard, and click the search icon to run the analysis.

### Save the Hash to Your Desktop

1. Open gedit and copy/paste the hash into the text editor.
2. Right click in the document, then select **Paste**.
3. Click **File** > **Save As**.
4. Choose **Desktop**.
5. For _Name_, type "hash.txt".
6. Click **Save**.

## Conclusion

Congratulations, you've successfully completed this hands-on lab!