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


# Installing and Configuring OpenVAS

## Introduction

In this lab, we'll be installing OpenVAS, an open-source vulnerability scanner. Then we'll configure it to scan `localhost` and export the scan task to our Downloads directory.

## Solution

We will connect to our lab server using VNC. The IP address and login credentials are provided on the lab instructions page.

VNC connections will be different for each operating system.

For Mac users:

1. Open Finder.
2. Press **Command+K** on your keyboard to bring up the _Connect to server_ window.
    - Alternatively, expand **Go** in the menu at the top of the screen and click **Connect to Server**.
3. In the _Connect to Server_ window, connect to `vnc://<IP_ADDRESS>:5901`, making sure to replace `<IP_ADDRESS>` with the IP address you were provided in the hands-on lab instructions.

### Install OpenVAS

**Note:** If you get a `dpkg lock` message, wait a few minutes, and then retry the command.

1. Add the repository to `apt`.
    
    `sudo add-apt-repository ppa:mrazavi/openvas`
    
2. Enter your password at the prompt.
3. Run an update.
    
    `sudo apt-get update`
    
4. Install SQLite 3.
    
    `sudo apt-get install -y sqlite3`
    
5. Install OpenVAS (**Note:** The step of rebuilding the NVT cache takes a few minutes).
    
    `sudo apt-get install -y openvas9`
    
    If you are installing for a production system, you will need to run the below commands. However, it will take about an additional hour for Greenbone to download all of the data. We are not going to do this as part of the lab since it's not necessary to complete the tasks.

    - `sudo greenbone-nvt-sync`
    - `greenbone-scapdata-sync`
    - `greenbone-certdata-sync`
    - `sudo openvasmd --rebuild --progress`
    - `sudo service openvas-manager restart`


6. Restart the OpenVAS manager service.
    
    `sudo service openvas-manager restart`
    
7. Check the services to verify that OpenVAS is running.
    
    `ps -ef | grep openvas`
    

### Create an OpenVAS Scan of `localhost` and Export the Task to Your Downloads Directory

1. Open your web browser, and navigate to [https://10.0.0.116:4000](https://10.0.0.116:4000/).
    
2. You will receive a warning message that the connection is unsecure. Click **Advanced** > **Add Exception** > **Confirm Security Exception**.
    
3. Log in to Greenbone with the username `admin` and the password `admin`.
    
4. Click **Scans** > **Tasks**.
    
5. Click the **`*`** icon, then **New Task**.
    
6. In the _New Task_ menu, fill out the following information:
    
    - **Name:** LabScan
    - **Scan Targets:**
        - Click the **`*`** icon next to _Scan Targets_ and configure the following settings:
            - **Name:** localhost
            - **Manual:** localhost
        - Click **Create**.
    - **Schedule:** Once
    - **Alterable Task:** Yes
7. Leave the rest of the options as their defaults, and click **Create**.
    
8. Under _Actions_, click the green arrow icon on the right to export the task.
    
9. In the dialog box that opens, choose **Save File**, then click **OK**.
    
10. Open your Downloads folder and verify that the exported file is there.
    

