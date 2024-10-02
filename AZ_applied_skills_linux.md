# Applied Skills: Deploy and administer Linux virtual machines on Microsoft Azure  
[Link](https://learn.microsoft.com/en-us/credentials/applied-skills/deploy-and-administer-linux-virtual-machines-on-microsoft-azure/)  

## Configure virtual machines 
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


## Add and size disks in Azure virtual machines  
[Link](https://learn.microsoft.com/en-us/training/modules/add-and-size-disks-in-azure-virtual-machines/)  

