# Manage virtual machines with the Azure CLI  

## What is the Azure CLI  

The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources. It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/overview).  

Azure CLI can help you manage Azure resources such as virtual machines and disks from the command line or in scripts. Let's get started and see what it can do with Azure Virtual Machines.  

This module focuses on using the Azure CLI to create and manage virtual machines hosted in Azure. If you'd like to get an overview of the Azure CLI, how to install it, and how to work with your Azure subscriptions, make sure to check out the [Control Azure services with the CLI](https://learn.microsoft.com/en-us/training/modules/control-azure-services-with-cli/) training module.  

Prerequisites:  
- Basic understanding of the Azure CLI tool from the [Control Azure services with the CLI](https://learn.microsoft.com/en-us/training/modules/control-azure-services-with-cli/) module.  


## Logins, subscriptions, and resource groups  

Create VM  
```bash
az vm create --resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--location westus \
--name SampleVM \
--image Ubuntu2204 \
--admin-username azureuser \
--generate-ssh-keys \
--verbose
```
```json
{
  "fqdns": "",
  "id": "/subscriptions/c8e91e7a-1ac8-436f-a03c-d3569abbd0f9/resourceGroups/learn-bf9e1797-599d-4977-8171-6c8c74b44c8d/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-5A-D0-D3",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "23.101.201.253",
  "resourceGroup": "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d",
  "zones": ""
}
```

## Test new virtual machine  

connect with ssh  
```bash
ssh azureuser@$public_ip
ssh azureuser@23.101.201.253
```
```bash
azureuser@SampleVM:~$ ps
    PID TTY          TIME CMD
   1601 pts/0    00:00:00 bash
   1623 pts/0    00:00:00 ps
azureuser@SampleVM:~$ ls
azureuser@SampleVM:~$ la
.bash_logout  .bashrc  .cache  .profile  .ssh
azureuser@SampleVM:~$ exit
logout
Connection to 23.101.201.253 closed.
```

## Explore other VM images  

Listing images  

This outputs the most popular images that are part of an offline list built into the Azure CLI. However, there are hundreds of image options available in the Azure Marketplace.  
```bash
az vm image list --output table
```

Getting all images  

command to see all Wordpress images available:  
```bash
az vm image list --sku Wordpress --output table --all
```
Or this command to see all images provided by Microsoft:  
```bash
az vm image list --publisher Microsoft --output table --all
```

Location-specific images  
```bash
az vm image list --location eastus --output table
```
Try checking some of the images in the other Azure sandbox available locations:  
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

> 💡 Tip  
>These are the standard images that are provided by Azure. Keep in mind that you can also [create and upload your own custom images](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-custom-images) to create VMs based on unique configurations or less common versions or distributions of an operating system.

## Sizing VMs properly  

Predefined VM sizes  

<table>
<thead>
<tr>
<th>Type</th>
<th>Sizes</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>General purpose</td>
<td>Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</td>
<td>Balanced CPU-to-memory. Ideal for dev/test and small to medium applications and data solutions.</td>
</tr>
<tr>
<td>Compute optimized</td>
<td>Fs, F</td>
<td>High CPU-to-memory. Good for medium-traffic applications, network appliances, and batch processes.</td>
</tr>
<tr>
<td>Memory optimized</td>
<td>Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</td>
<td>High memory-to-core. Great for relational databases, medium to large caches, and in-memory analytics.</td>
</tr>
<tr>
<td>Storage optimized</td>
<td>Ls</td>
<td>High disk throughput and IO. Ideal for big data, SQL, and NoSQL databases.</td>
</tr>
<tr>
<td>GPU optimized</td>
<td>NV, NC</td>
<td>Specialized VMs targeted for heavy graphic rendering and video editing.</td>
</tr>
<tr>
<td>High performance</td>
<td>H, A8-11</td>
<td>Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</td>
</tr>
</tbody>
</table>


### Available sizes change based on the region  
```bash
az vm list-sizes --location eastus --output table
```

### Specify a size during VM creation  

```bash
az vm create \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM2 \
--image Ubuntu2204 \
--admin-username azureuser \
--generate-ssh-keys \
--verbose \
--size "Standard_DS2_v2"
```

>⚠️ Warning  
>Your subscription tier [enforces limits](https://learn.microsoft.com/en-us/azure/azure-subscription-service-limits) on how many resources you can create, as well as the total size of those resources. Quota limits depend upon your subscription type and region. The Azure CLI lets you know when you exceed this limit with a **Quota Exceeded** error. If you hit this error in your own paid subscription, you can request to raise the limits associated with your paid subscription (up to 10,000 vCPUs) through a [free online request](https://learn.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-quota-errors).  

### Resize an existing VM  

Resize an existing VM if the workload changes or if it was incorrectly sized at creation.  
```bash
az vm list-vm-resize-options \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--output table
```
That command returns a list of all the possible size configurations available in the resource group. If the size we want isn't available in our cluster but _is_ available in the region, we can [deallocate the VM](https://learn.microsoft.com/en-us/cli/azure/vm#az-vm-deallocate). This command stops the running VM and removes it from the current cluster without losing any resources. We can then resize it, which re-creates the VM in a new cluster where the size configuration is available.  

```bash
az vm resize \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--size Standard_D2s_v3
```
This command takes a few minutes to reduce the resources of the VM, and once it's done, it returns a new JSON configuration.  

## Query system and runtime information about the VM  

```bash
az vm list
```
That command will return _all_ virtual machines defined in this subscription. You can filter the output to a specific resource group through the `--resource-group` parameter.  

### Output types  
Notice that the default response type for all the commands we've done so far is JSON. This is great for scripting, but most people find it harder to read. You can change the output style for any response through the `--output` flag. For example, run the following command in Azure Cloud Shell to see the different output style.  
```bash
az vm list --output table
```
Along with `table`, you can specify `json` (the default), `jsonc` (colorized JSON), or `tsv` (Tab-Separated Values). Try a few variations with the preceding command to see the difference.  

### Get the IP address  
Another useful command is `vm list-ip-addresses`, which lists the public and private IP addresses for a VM. If they change, or you didn't capture them during creation, you can retrieve them at any time.  
```bash
az vm list-ip-addresses -n SampleVM -o table
```
>💡 Tip  
>Notice that we're using a shorthand syntax for the `--output` flag as `-o`. You can shorten most parameters to Azure CLI commands to a single dash and letter. For example, you can shorten `--name` to `-n` and `--resource-group` to `-g`. This is handy for entering keyboard characters, but we recommend using the full option name in scripts for clarity. Check the documentation for details about each command.  

### Get VM details  
We can get more detailed information about a specific virtual machine by name or ID running the `vm show` command.  
```bash
az vm show --resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" --name SampleVM
```
This returns a fairly large JSON block with all sorts of information about the VM, including attached storage devices, network interfaces, and all of the object IDs for resources that the VM is connected to. Again, we could change to a table format, but that omits almost all of the interesting data. Instead, we can turn to a built-in query language for JSON called [JMESPath](http://jmespath.org/).  

### Add filters to queries with JMESPath  

JMESPath is an industry-standard query language built around JSON objects. The simplest query is to specify an _identifier_ that selects a key in the JSON object.  

For example, given the object:  
```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```
We can use the query `people` to return the array of values for the `people` array. If we just want _one_ of the people, we can use an indexer. For example, `people[1]` would return:  
```json
{
    "name": "Barney",
    "age": 25
}
```
We can also add specific qualifiers that would return a subset of the objects based on some criteria. For example, adding the qualifier `people[?age > '25']` would return:  
```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```
Finally, we can constrain the results by adding a select: `people[?age > '25'].[name]` that returns just the names:  
```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```
JMESQuery has several other interesting query features. When you have time, check out the [online tutorial](http://jmespath.org/tutorial.html) available on the [JMESPath.org](http://jmespath.org/) site.  

### Filter Azure CLI queries  

With a basic understanding of JMES queries, we can add filters to the data returned by queries like the `vm show` command. For example, we can retrieve the admin username:  
```bash
az vm show \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--query "osProfile.adminUsername"
```
We can get the size assigned to our VM:  
```bash
az vm show \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--query hardwareProfile.vmSize
```
Or, to retrieve all the IDs for your network interfaces, we can run the query:  
```bash
az vm show \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--query "networkProfile.networkInterfaces[].id"
```
This query technique works with any Azure CLI command, and you can use it to pull specific bits of data out on the command line. It's useful for scripting, as well. For example, you can pull a value out of your Azure account and store it in an environment or script variable. If you decide to use it this way, it's useful to add the `--output tsv` parameter (which you can shorten to `-o tsv`). This will return results that only include the actual data values with tab separators.  

For example:  
```bash
az vm show \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM \
--query "networkProfile.networkInterfaces[].id" -o tsv
```

## Start and stop your VM with the Azure CLI  

### Stop a VM

We can stop a running VM with the `vm stop` command. You must pass the name and resource group or the unique ID for the VM:  
```bash
az vm stop \
--name SampleVM \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d"
```
You can verify the VM has stopped by attempting to ping the public IP address, using `ssh`, or through the `vm get-instance-view` command. This final approach returns the same basic data as `vm show`, but includes details about the instance itself. Try entering the following command into Azure Cloud Shell to see the current running state of your VM:  
```bash
az vm get-instance-view \
--name SampleVM \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```
That command should return `VM stopped` as the result.  

### Start a VM  
We can do the reverse through the `vm start` command.  
```bash
az vm start \
--name SampleVM \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d"
```
That command starts a stopped VM. You can verify it through the `vm get-instance-view` query you used in the last section, which should now return `VM running`.  

### Restart a VM  
Finally, we can restart a VM if we've made changes that require a reboot by running the `vm restart` command. You can add the `--no-wait` flag if you want the Azure CLI to return immediately without waiting for the VM to reboot.  

## Install software on VM  

The last thing we want to try on our VM is to install a web server. One of the easiest packages to install is `nginx`.  

### Install NGINX web server  

1. Locate the public IP address of your _SampleVM_ Linux virtual machine.  
```bash
az vm list-ip-addresses --name SampleVM --output table
```
2. Next, open an `ssh` connection to _SampleVM_ using the Public IP address from the preceding step.  
```bash
ssh azureuser@$PublicIPAddress
ssh azureuser@23.101.201.253
```
3. After you're logged in to the virtual machine, run the following command to install the `nginx` web server. The command takes a few moments to complete.  
```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```
4. Exit the Secure Shell:  
```bash
exit
```

### Retrieve your default page  
1. In Azure Cloud Shell, use `curl` to read the default page from your Linux web server by running the following command, replacing `<PublicIPAddress>` with the public IP you found previously. You can also open a new browser tab and try to browse to the public IP address.  
```bash
curl -m 80 $PublicIPAddress
curl -m 80 23.101.201.253
```
That command will fail, because the Linux virtual machine doesn't expose port 80 (`http`) through the network security group that secures the network connectivity to the virtual machine. We can fix the failure by running the Azure CLI command `vm open-port`.  
    
2. Enter the following command into Cloud Shell to open port 80:  
```bash
az vm open-port \
--port 80 \
--resource-group "learn-bf9e1797-599d-4977-8171-6c8c74b44c8d" \
--name SampleVM
```
It takes a moment to add the network rule and open the port through the firewall.  
    
3. Run the `curl` command again.  
```bash
curl -m 80 $PublicIPAddress
curl -m 80 23.101.201.253
```
This time, it should return data like the following. You can see the page in a browser as well.  
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
body {
    width: 35em;
    margin: 0 auto;
    font-family: Tahoma, Verdana, Arial, sans-serif;
}
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support, refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

## Summary and cleanup  
You've created a new Linux virtual machine, changed its size, stopped and started it, and updated the configuration with the Azure CLI.  

### Clean up  

The sandbox automatically cleans up your resources when you're finished with this module.  

When you're working in your own subscription, it's a good idea at the end of a project to identify whether you still need the resources you created. Resources that you leave running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.  

### Additional resources  

- [Azure CLI overview](https://learn.microsoft.com/en-us/cli/azure/)  
- [Azure CLI command reference](https://learn.microsoft.com/en-us/cli/azure/reference-index)  

