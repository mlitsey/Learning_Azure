# Applied Skills: Deploy and administer Linux virtual machines on Microsoft Azure  
[Link](https://learn.microsoft.com/en-us/credentials/applied-skills/deploy-and-administer-linux-virtual-machines-on-microsoft-azure/)  

# Configure virtual machines 
[Link](https://learn.microsoft.com/en-us/training/modules/configure-virtual-machines/)  

### Plan virtual machines  
| Name element | Examples | Description |
| --- | --- | --- |
| **Environment or purpose** | `dev` (development), `prod` (production), `QA` (testing) | A portion of the name should identify the environment or purpose for the machine. |
| **Location** | `uw` (US West), `je` (Japan East), `ne` (North Europe) | Another portion of the name should specify the region where the machine is deployed. |
| **Instance** | `1`, `02`, `005` | For multiple machines that have similar names, include an instance number in the name to differentiate the machines in the same category. |
| **Product or service** | `Outlook`, `SQL`, `AzureAD` | A portion of the name can specify the product, application, or service that the machine supports. |
| **Role** | `security`, `web`, `messaging` | A portion of the name can specify what role the machine supports within the organization. |

### Determine virtual machine sizing  
| Classification | Description | Scenarios |
| --- | --- | --- |
| **General purpose** | General-purpose virtual machines are designed to have a balanced CPU-to-memory ratio. |  <ul><li>Testing and development</li>  <li>Small to medium databases</li>  <li>Low to medium traffic web servers</li> |
| **Compute optimized** | Compute optimized virtual machines are designed to have a high CPU-to-memory ratio. | <ul><li>Medium traffic web servers</li> <li>Network appliances</li> <li>Batch processes</li> <li>Application servers</li> |
| **Memory optimized** | Memory optimized virtual machines are designed to have a high memory-to-CPU ratio. | <ul><li>Relational database servers</li> <li>Medium to large caches</li> <li>In-memory analytics</li> |
| **Storage optimized** | Storage optimized virtual machines are designed to have high disk throughput and I/O. | <ul><li>Big Data</li> <li>SQL and NoSQL Databases</li> <li>Data warehousing</li> <li>Large transactional databases</li> |
| **GPU** | GPU virtual machines are specialized virtual machines targeted for heavy graphics rendering and video editing. Available with single or multiple GPUs. | <ul><li>Model training</li> <li>Inferencing with deep learning</li> |
| **High performance computes** | High performance compute offers the fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA). | <ul><li>Workloads that require fast performance</li> <li>High traffic networks</li> |



### Interactive lab simulation

#### **Lab scenario**

Your organization is planning on using virtual machines in Azure. As the Azure Administrator you need to:

- Be able to use virtual machine Quickstart templates.
- Use templates to create and configure virtual machines.
- Be able to monitor virtual machine activity.

<ins>**Objectives**</ins>

- **Task 1**: Use the Azure Quickstart Template gallery to deploy a virtual machine.
    - Browse to the [Azure Quickstart Template gallery](https://azure.microsoft.com/resources/templates/).
    - Search for a template that deploys a simple Windows Server virtual machine.
    - Edit the template and customize the parameters and variables.
    - Deploy the template to create the virtual machine.
- **Task 2**: Verify and monitor your virtual machine.
    - In the portal, locate your new virtual machine.
    - View monitoring data for CPU, network, and data usage.
    - View activity log information.

### Summary and resources  
#### Learn more with documentation

- [Azure Virtual Machines documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/). This article is your starting point for all things Azure virtual machines.
    
- [Virtual machine selector](https://azure.microsoft.com/pricing/vm-selector/). This tool helps you find the virtual machines for your needs and budget.
    

#### Learn more with self-paced training

- [Introduction to Azure Virtual Machines (sandbox)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-virtual-machines). Learn about the decisions you make before creating a virtual machine.
    
- [Plan and deploy Windows Server IaaS Virtual Machines (demonstration videos)](https://learn.microsoft.com/en-us/training/modules/plan-deploy-windows-server-iaas-virtual-machines). Understand Azure compute and storage in relation to Azure VMs. Deploy Azure VMs by using the Azure portal, Azure CLI, or templates.
    
- [Connect to virtual machines through the Azure portal by using Azure Bastion](https://learn.microsoft.com/en-us/training/modules/connect-vm-with-azure-bastion/). Learn how to deploy Azure Bastion to securely connect to Azure virtual machines.
    
- [Create a Windows virtual machines in Azure (sandbox)](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/).
    
- [Provision a Linux virtual machine in Microsoft Azure](https://learn.microsoft.com/en-us/training/modules/provision-linux-virtual-machine-in-azure/). Learn how to deploy a Linux virtual machine with Bicep, the Azure portal, and the Azure CLI.
    
- [Choose the right disk storage for your virtual machine workload](https://learn.microsoft.com/en-us/training/modules/choose-the-right-disk-storage-for-vm-workload/). Learn about the variety of disk storage options for virtual machine workloads.


# Add and size disks in Azure virtual machines  
[Link](https://learn.microsoft.com/en-us/training/modules/add-and-size-disks-in-azure-virtual-machines/)  

There are three main disk roles in Azure: the data disk, the OS disk, and the temporary disk. These roles map to disks that are attached to your virtual machine.

- **OS Disk**. Every VM includes one disk that stores the operating system. This drive is registered as a SATA drive and labeled as the C: drive in Windows or mounted at "/" in Unix-like operating systems. This disk has a maximum capacity of 4,095 GiB, however, many operating systems are partitioned with [master boot record (MBR)](https://wikipedia.org/wiki/Master_boot_record) by default. MBR limits the usable size to 2 TiB. If you need more than 2 TiB, create and attach data disks and use them for data storage. If you need to store data on the OS disk and require the extra space, [convert it to GUID Partition Table](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/change-an-mbr-disk-into-a-gpt-disk) (GPT). To learn about the differences between MBR and GPT on Windows deployments, see [Windows and GPT FAQ](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-and-gpt-faq).
- **Data storage**. A data disk is a managed disk attached to a virtual machine to store application data, or other data you need to persist across reboots. Some VM images include data disks by default. You can also add more data disks up to the maximum number specified by the VM's size. Each data disk is registered as a SCSI drive and has a max capacity of 32,767 GiB. You can choose drive letters or mount points for your data drives.
- **Temporary storage**. Most VMs contain a temporary disk, which isn't a managed disk. The temporary disk provides short-term storage for applications and processes, and is intended to only store data such as page files, swap files, or SQL Server tempdb. Data on this drive can be lost during a maintenance event or redeployment. The drive is labeled as D: on a Windows VM by default. Don't use this drive to store important data that you don't want to lose.

### Attach an existing data disk to an Azure VM

You might already have a VHD that stores the data you want to use in your Azure VM. In our law firm scenario, for example, perhaps you already converted your physical disks to VHDs locally. In this case, you can upload your VHD directly to a managed disk. Generally, you should upload a disk by using the PowerShell `Add-AzVhd` cmdlet. This cmdlet is optimized for transferring VHD files, and can complete the upload faster than other methods. The transfer is completed by using multiple threads for best performance. Once the VHD is uploaded, attach it to an existing VM as a data disk. This approach is an excellent way to deploy data of all types to Azure VMs. The data is automatically present in the VM, and there's no need to partition or format the new disk.

# Exercise - Add a data disk to a VM

Your law firm is expanding its case load, and you're tasked with creating a new Linux web server to store critical documents from various sources: clients, other law firms, and law-enforcement offices. The web server lets you upload documents and store them on disk.

Tip

This exercise uses Linux as the example, but the basic process of creating VMs and adding disks is the same for Windows. The primary difference would be in partitioning and formatting the disk. On Windows, you can connect to your VM over Remote Desktop and use the built-in Disk Management tools or deploy a PowerShell script that's similar to the Bash script you'll use here.

Your goal is to create a Linux VM and attach a new virtual hard disk (VHD) named **uploadDataDisk1** to store the `/uploads` directory.

## Set Azure CLI default values

The Azure CLI lets you set default values so you don't have to repeat them each time you run a command.

You specify the default Azure location, or region. This location is where your Azure VM is placed.

Ideally, this location is close to your clients. In this case, select the closest region to you from the locations available to the Azure sandbox.

The free sandbox allows you to create resources in a subset of the Azure global regions. Select a region from this list when you create resources:

- westus2
- southcentralus
- centralus
- eastus
- westeurope

- southeastasia
- japaneast
- brazilsouth
- australiasoutheast
- centralindia

1. Run `az configure` to set the default location you want to use. Replace **eastus** with the location chosen in the previous step.
    
    Azure CLI
    
    ```
    az configure --defaults location=eastus
    ```
    
> 💡 **Tip:**  
> You can use the **Copy** button to copy commands to the clipboard. To paste, right-click on a new line in the Cloud Shell terminal and select **Paste**, or use the Shift+Insert keyboard shortcut (⌘+V on macOS).
    
2. Set the default resource group name to the preconfigured resource group created for you through the Azure sandbox: **learn-93d1ac41-2467-4491-a93b-79b12cf183a0**
    
    Azure CLI
    
    ```
    az configure --defaults group="learn-93d1ac41-2467-4491-a93b-79b12cf183a0"
    ```
    

## Create a Linux VM

Here, you create a Linux VM to host your web server.

1. Run this `az vm create` command to create an Ubuntu Linux VM.
    
    Azure CLI
    
    ```bash
    az vm create \
      --name support-web-vm01 \
      --image Canonical:UbuntuServer:16.04-LTS:latest \
      --size Standard_DS1_v2 \
      --admin-username azureuser \
      --generate-ssh-keys
    ```
    
    - The VM's name is **support-web-vm01**.
    - Its size is **Standard\_DS1\_v2**.
    - The admin username is **azureuser**. In practice, this name can be whatever you like.
    - The `--generate-ssh-keys` argument generates an SSH keypair for you, allowing you to connect to your VM over SSH.
    
    The VM takes a few minutes to deploy. When the VM is ready, you see information about it in JSON format. Here's an example:
    
    JSON Copy
    
    ```json
    {
      "fqdns": "",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/680469d8-edB7-42ec-b118-cd80d51741e7/providers/Microsoft.Compute/virtualMachines/support-web-vm01",
      "location": "eastus",
      "macAddress": "00-0D-3A-10-63-0A",
      "powerState": "VM running",
      "privateIpAddress": "10.0.0.4",
      "publicIpAddress": "104.211.38.211",
      "resourceGroup": "680469d8-edB7-42ec-b118-cd80d51741e7",
      "zones": ""
    }
    ```
    
> 📝 **Note:**  
> In this lesson, you're using this VM to learn how to manage disks. In practice, you might also install web server and other software and then run `az vm open-port` to make the ports you need available to the outside world.
    

## Add an empty data disk to your VM

Here, you create an empty data disk and attach it to your VM. Initially, Your data disk is 64 GB in size. Later, you mount this disk to the `/uploads` directory on your VM.

> 💡 **Tip:**  
> For learning purposes, you're creating the VM and data disk as separate steps. In practice, you can specify the `--data-disk-sizes-gb` argument to the `az vm create` command to add data disks when the VM is created.

1. Run the following `az vm disk attach` command to add a new empty disk to the VM.
    
    Azure CLI
    
    ```
    az vm disk attach \
      --vm-name support-web-vm01 \
      --name uploadDataDisk1 \
      --size-gb 64 \
      --sku Premium_LRS \
      --new
    ```
    
    This command:
    
    - Names the disk **uploadDataDisk1**.
    - Sets its size to be 64 GB.
    - Specifies the use of premium storage with local redundancy.

To use the disk, you need to partition and format it. Let's do that next.

## Initialize and format your data disk

Your empty data drive needs to be initialized and formatted. The process to do that is the same as for a physical disk.

For one-time tasks, you might manually connect to your VM over SSH and run the commands you need. However, to make the process more repeatable and less error-prone, you can specify your commands in a Bash script or a PowerShell script (if it's available).

Using a script to automate the process has an added benefit: your script serves as documentation for how the process is performed. Others can read your script to understand how the system is configured. If you need to change the process, you can just modify your script and test it on a temporary scratch VM before you deploy your change to production.

To automate the process in this lesson, you use the _Custom Script Extension_. The Custom Script Extension is an easy way to download and run scripts on your Azure VMs. It's just one of the many ways you can configure the system after your VM is up and running.

You can store your scripts in Azure storage, or in a public location such as GitHub. You can run scripts manually or as part of a more automated deployment. Here, you run an Azure CLI command to download a premade Bash script from GitHub and execute it on your VM.

For learning purposes, let's also run a few commands on your VM to verify that the VM is configured as you expect.

1. Run `az vm show` to get your VM's public IP address and save the IP address as a Bash variable.
    
    Azure CLI
    
    ```
    ipaddress=$(az vm show \
      --name support-web-vm01 \
      --show-details \
      --query [publicIps] \
      --output tsv)
    ```
    
2. Run the following `ssh` command to run the `lsblk` command on your VM over an SSH connection using the `ipaddress` variable data you created in the previous step. Recall that `azureuser` was the admin username we used when we created the VM. If you chose a different name, use that instead. Enter _yes_ when prompted.
    
    Bash Copy
    
    ```
    ssh azureuser@$ipaddress lsblk
    ```
    
    The output of this command should look like the following.
    
    Output Copy
    
    ```
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb       8:16   0    7G  0 disk 
    └─sdb1    8:17   0    7G  0 part /mnt
    sr0      11:0    1  628K  0 rom  
    sdc       8:32   0   64G  0 disk 
    sda       8:0    0   30G  0 disk 
    ├─sda14   8:14   0    4M  0 part 
    ├─sda15   8:15   0  106M  0 part /boot/efi
    └─sda1    8:1    0 29.9G  0 part /
    ```
    
    Notice that the 64-GB drive, `sdc`, you created isn't mounted. The drive is listed this way because it isn't initialized yet.
    
3. Run the following `az vm extension set` command to run the premade Bash script on your VM.
    
> ⚠️ **Warning:**  
> The script modifies `/etc/fstab`. Improperly modifying the `/etc/fstab` file could result in an unbootable system. Always test configuration changes on a temporary scratch system before you deploy to production. Refer to your distribution's documentation to learn how to properly modify this file. In production, we also recommend that you create a backup of this file so you can restore the configuration if needed.
    
Azure CLI
    
```
az vm extension set \
    --vm-name support-web-vm01 \
    --name customScript \
    --publisher Microsoft.Azure.Extensions \
    --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/add-data-disk.sh"]}' \
    --protected-settings '{"commandToExecute": "./add-data-disk.sh"}'
```
    
While the command runs, you can [examine the Bash script](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/add-data-disk.sh) from a separate browser tab, if you'd like.
    
To summarize, the script:
- Partitions the drive `/dev/sdc`.
- Creates an ext4 filesystem on the drive.
- Creates the `/uploads` directory, which we use as our mount point.
- Attaches the disk to the mount point.
- Updates `/etc/fstab` so that the drive is mounted automatically after the system reboots.

4. To verify the configuration, run the same `ssh` command as you did previously to run the `lsblk` command on your VM over an SSH connection.
    
    Bash Copy
    
    ```
    ssh azureuser@$ipaddress lsblk
    ```
    
    You see that `sdc/sdc1` is partitioned and mounted to the `/uploads` directory as you expect.
    
    Output Copy
    
    ```
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb       8:16   0    7G  0 disk 
    └─sdb1    8:17   0    7G  0 part /mnt
    sr0      11:0    1  628K  0 rom  
    sdc       8:32   0   64G  0 disk 
    └─sdc1    8:33   0   64G  0 part /uploads
    sda       8:0    0   30G  0 disk 
    ├─sda14   8:14   0    4M  0 part 
    ├─sda15   8:15   0  106M  0 part /boot/efi
    └─sda1    8:1    0 29.9G  0 part /
    ```
    

> 💡 **Tip:**  
> Some Linux kernels support TRIM to discard unused blocks on disks. This feature is available on Azure disks and can save you money if you create large files and then delete them. Learn how to [turn this feature on](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in the Azure documentation.

## Summary

Here, you created a data disk and attached it to your VM. You used the Custom Script Extension to run a premade Bash script on your VM to make the process more repeatable. The Bash script partitions, formats, and mounts your disk so that your web server can write to it.

Now that the data disk on your VM is prepared, let's explore a bit more about the various types of disks you can create. Your primary decision is whether to choose Standard or Premium storage.

# Determine whether to use premium storage

Some applications place greater demands on storage than others. Apps such as Dynamics CRM, Exchange Server, SAP Business Suite, SQL Server, Oracle, and SharePoint require constant high performance and low latency to run at their best.

When creating your VMs or adding new disks, you have a few choices that make a dramatic impact on disk performance, starting with the _type_ of storage you choose.

|  | Ultra disk | Premium SSD v2 | Premium SSD | Standard SSD | Standard HDD |
| --- | --- | --- | --- | --- | --- |
| Disk type | SSD | SSD | SSD | SSD | HDD |
| Scenario | IO-intensive workloads such as [SAP HANA](https://learn.microsoft.com/en-us/azure/sap/workloads/hana-vm-operations-storage), top tier databases (for example, SQL, Oracle), and other transaction-heavy workloads. | Production and performance-sensitive workloads that consistently require low latency and high IOPS and throughput. | Production and performance sensitive workloads. | Web servers, lightly used enterprise applications and dev/test. | Backup data, noncritical, infrequent access. |
| Max disk size | 65,536 GiB | 65,536 GiB | 32,767 GiB | 32,767 GiB | 32,767 GiB |
| Max throughput | 4,000 MB/s | 1,200 MB/s | 900 MB/s | 750 MB/s | 500 MB/s |
| Max IOPS | 160,000 | 80,000 | 20,000 | 6,000 | 2,000, 3,000\* |
| Usable as OS Disk? | No | No | Yes | Yes | Yes |

## Data replication

The data in your managed disk is automatically replicated to ensure durability and high availability. Azure Storage replication copies your data to protect it from planned and unplanned events like transient hardware failures, network or power outages, natural disasters, and so on. You can choose to replicate your data within the same data center, across zonal data centers within the same region, and even across regions.

There are several types of replication:

- **Locally redundant storage (LRS)**: Azure replicates the data within the same Azure data center. The data remains available if a node fails. However, if an entire data center fails, data might be unavailable.
- **Zone-redundant storage (ZRS)**: Azure replicates your data synchronously across three storage clusters in a single region. Each storage cluster is physically separated from the others and resides in its own availability zone (AZ). With this type of replication, you can still access and manage your data if a zone becomes unavailable.

## Expand a disk using the Azure CLI

> ⚠️ **Warning:**  
> Always make sure that you back up your data before performing disk resize operations!

You can't perform operations on VHDs with the VM running. The first step is to stop and deallocate the VM with `az vm deallocate`, supplying the VM name and resource group name.

When you deallocate a VM, instead of just _stopping_ a VM, it releases the associated computing resources and allows Azure to make configuration changes to the virtualized hardware.

> 📝 **Note:**  
> Don't run these commands just yet. You practice this process in the next unit.

Azure CLI

```
az vm deallocate \
  --resource-group <resource-group-name> \
  --name <vm-name>
```

Next, to resize a disk, you use `az disk update`, passing the disk name, resource group name, and newly requested size. When you expand a managed disk, the specified size is mapped to the nearest managed disk size.

Azure CLI

```
az disk update \
  --resource-group <resource-group-name> \
  --name <disk-name> \
  --size-gb 200
```

Finally, you run `az vm start` to restart the VM.

Azure CLI

```
az vm start \
  --resource-group <resource-group-name> \
  --name <vm-name>
```

## Expanding a disk using the Azure portal

You can also expand a disk through the Azure portal:

1. To stop the VM, on the **Overview** page for the VM, select **Stop** in the toolbar.
    
2. In the left menu pane, under **Settings**, select **Disks**.
    
3. Select the data disk you want to resize.
    
4. Select **Size + performance** under **Settings**. In the list, select a size _larger_ than the current size. You can also change from Premium to Standard (or vice-versa) here. These settings adjust your performance as shown in the predicted IOPS section.
    
5. Select **Resize** to save the changes.
    
6. Restart the VM.
    

### Expand the partition

Just like adding a new data disk, an expanded disk doesn't add any usable space until you expand the partition and filesystem. You must do this using the OS tools available to the VM.

On Windows, you might use the Disk Manager tool or the `diskpart` command line tool.

On Linux, you might use `parted` and `resize2fs`. You'll do that in the next unit.

# Exercise - Resize a VM disk
Let's say you underestimated how large some of the uploaded files are and that your upload disk is running out of space. You decide to double the space from 64 GB to 128 GB.

Here, you practice the process you learned about in the previous units.

## Resize the data disk

To resize a disk, you need the ID or name of the disk. In this case, you already know the name, **uploadDataDisk1**. But in case you didn't, or someone else created the disk, you can run `az disk list` to find the name.

1. Run the `az disk list` command to print the list of the managed disks in the resource group. This list might include other disks if you have multiple VMs in the same resource group.
    
    Azure CLI
    
    ```
    az disk list \
      --query '[*].{Name:name,Gb:diskSizeGb,Tier:sku.tier}' \
      --output table
    ```
    
    You see the disk named **uploadDataDisk1**.
    
    Output Copy
    
    ```bash
    Name                                                        Tier
    ----------------------------------------------------------  -------
    support-web-vm01_OsDisk_1_a7c59897dfda42dfab2edf4933e713a6  Premium
    uploadDataDisk1                                             Premium
    ```
    
2. Run the following `az vm deallocate` command to stop and deallocate your VM. This command doesn't delete your VM, but puts it in a state where you can modify the virtual disks.
    
    Azure CLI
    ```bash
    az vm deallocate --name support-web-vm01
    ```
    
3. Run the `az disk update` command to resize the disk to 128 GB.
    
    Azure CLI
    ```bash
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
4. Run the `az vm start` command to restart the VM.
    
    Azure CLI
    ```bash
    az vm start --name support-web-vm01
    ```
    
    But we aren't finished yet. The operating system on the VM can't use the extra space yet. We handle this situation in the next section.
    

## Expand the disk partition

The final step is to tell the OS about the available space. Just like the partitioning and format steps you did earlier, this process is identical to the one you follow to expand a physical on-premises disk.

1. Although you can reserve a fixed public IP address for your VM, by default your VM receives a new public IP address when the VM is deallocated and restarted. Run the following `az vm show` command to update your Bash variable with your VM's new public IP address.
    
    Azure CLI
    ```bash
    ipaddress=$(az vm show --name support-web-vm01 -d --query [publicIps] -o tsv)
    ```
    
2. As you did earlier, run `lsblk` on your VM over SSH to understand its current state.
    
    Bash
    ```bash
    ssh azureuser@$ipaddress lsblk
    ```
    
    You can see that disk `sdc/sdc1` still has a size of 64 GB.
    
    Output
    ```bash
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb       8:16   0    7G  0 disk 
    └─sdb1    8:17   0    7G  0 part /mnt
    sdc       8:32   0  128G  0 disk 
    └─sdc1    8:33   0   64G  0 part /uploads
    sda       8:0    0   30G  0 disk 
    ├─sda14   8:14   0    4M  0 part 
    ├─sda15   8:15   0  106M  0 part /boot/efi
    └─sda1    8:1    0 29.9G  0 part /
    ```
    
3. Similar to what you did previously to initialize your disk, run the following `az vm extension set` command to tell the OS on the VM about the newly available space by executing a premade Bash script we create to help you along.
    
    Azure CLI
    ```bash
    az vm extension set \
      --vm-name support-web-vm01 \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/resize-data-disk.sh"]}' \
      --protected-settings '{"commandToExecute": "./resize-data-disk.sh"}'
    ```
    
    While the command runs, you can [examine the Bash script](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-add-and-size-disks-in-azure-virtual-machines/master/resize-data-disk.sh) from a separate browser tab if you'd like.
    
    To summarize, the script:
    - Unmounts the disk `/dev/sdc1`.
    - Resizes partition 1 to be 128 GB.
    - Verifies partition consistency.
    - Resizes the filesystem.
    - Remounts the drive `/dev/sdc1` back to the mount point `/uploads`.

4. To verify the configuration, run `lsblk` on your VM over SSH a second time.
    
    Bash
    ```bash
    ssh azureuser@$ipaddress lsblk
    ```
    
    This time, you see that disk `sdc/sdc1` is expanded to accommodate the increased size of your disk.
    
    Output
    ```bash
    NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sdb       8:16   0     7G  0 disk 
    └─sdb1    8:17   0     7G  0 part /mnt
    sdc       8:32   0   128G  0 disk 
    └─sdc1    8:33   0 119.2G  0 part /uploads
    sda       8:0    0    30G  0 disk 
    ├─sda14   8:14   0     4M  0 part 
    ├─sda15   8:15   0   106M  0 part /boot/efi
    └─sda1    8:1    0  29.9G  0 part /
    ```
    
5. As a final verification step, run the operating system's `df` utility on your VM over SSH to prove that the OS can see it correctly.
    
    Bash
    ```bash
    ssh azureuser@$ipaddress df -h
    ```
    
    You see that the drive's size is 128 GB.
    
    Output
    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           345M  9.3M  335M   3% /run
    /dev/sda1        29G  1.9G   28G   7% /
    tmpfs           1.7G     0  1.7G   0% /dev/shm
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs           1.7G     0  1.7G   0% /sys/fs/cgroup
    /dev/sda15      105M  3.2M  102M   3% /boot/efi
    /dev/sdb1       6.8G   16M  6.4G   1% /mnt
    /dev/sdc1       118G   60M  112G   1% /uploads
    tmpfs           345M     0  345M   0% /run/user/1000
    ```

## Summary
In this module, you learned how to add new disks to your VMs to increase their storage. In addition, you explored the different disk types available, features of Standard versus Premium storage, and how to resize existing disks.

### Clean up

The sandbox automatically cleans up your resources when you're finished with this module.

When you're working in your own subscription, it's a good idea at the end of a project to identify whether you still need the resources you created. Resources that you leave running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.

### Learn more

- [Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/faq-for-disks)
- [Convert Azure managed disks storage from standard to premium, and vice versa](https://learn.microsoft.com/en-us/azure/virtual-machines/disks-convert-types?tabs=azure-powershell)
- [Full list of VM sizes that support premium storage for Windows and Linux](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes)


# **Monitor your Azure virtual machines with Azure Monitor**

## VM monitoring layers

Azure VMs have several layers that require monitoring. Each of the following layers has a distinct set of telemetry and monitoring requirements.

- Host VM
- Guest operating system (OS)
- Client workloads
- Applications that run on the VM

## Summary

Azure Monitor helps you collect, analyze, and alert on various types of host and client monitoring data from your Azure VMs.

- Azure Monitor provides a set of VM host logs and performance and usage metrics for all Azure VMs.
- You can enable recommended alert rules when you create VMs or afterwards to alert on important VM host metrics.
- Azure Monitor Metrics Explorer lets you graph and analyze metrics for Azure VMs and other resources.
- VM insights provides a simple way to monitor important VM client performance counters and processes running on your VM.
- You can create data collection rules to collect other metrics and logs from your VM client.
- You can use Log Analytics to query and analyze log data.

Now that you understand these tools, you're confident that Azure Monitor can effectively monitor your Azure VMs and help you keep your website running effectively.

### Clean up resources

In this module, you created a VM in your Azure subscription. To prevent further charges for this VM, you can delete it or the resource group that contains it.

To delete the resource group that contains the VM and its resources:

1. Select the **Resource group** link at the top of the **Essentials** section on the VM's **Overview** page.
2. At the top of the resource group page, select **Delete resource group**.
3. On the delete screen, select the checkbox next to **Apply force delete for selected virtual machines and virtual machine scale sets**. Enter the resource group name in the field, and then select **Delete**.

### Learn more

To learn more about monitoring your VMs with Azure Monitor, see the following resources:

- [Azure Monitor documentation](https://learn.microsoft.com/en-us/azure/azure-monitor)
- [Monitor virtual machines with Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/monitor-virtual-machine)
- [Supported metrics with Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/supported-metrics/metrics-index)
- [Send Azure Monitor Activity log data](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/activity-log)
- [Supported metrics for Microsoft.Compute/virtualMachines](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/supported-metrics/microsoft-compute-virtualmachines-metrics)
- [Overview of VM insights](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-overview)
- [Create interactive reports with VM insights workbooks](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-workbooks)
- [Use the Map feature of VM insights to understand application components](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-maps)
- [Azure Monitor Agent overview](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-overview)
- [Collect data with Azure Monitor Agent](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-data-collection)
- [Tutorial: Collect guest logs and metrics from an Azure virtual machine](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/tutorial-monitor-vm-guest)


# **Protect your virtual machines by using Azure Backup**  

