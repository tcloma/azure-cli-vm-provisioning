![azure logo](/images/azure.png)

# Creating VMs on Azure using the Azure CLI
This walkthrough outlines the step-by-step process of **Creating and Provisioning** Virtual Machines on Microsoft Azure using the official Azure CLI tool, and observing SSH traffic using PowerShell

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines)
- Azure CLI
- PowerShell

## Operating Systems Used
- Windows 11
- Ubuntu 24.04

## High-Level Steps
- Login to Azure CLI with your Microsoft Account using `az login`
- Create a Resource Group with `az group create`
- Provision a Virtual Machine utilizing `az vm create`
- Connect to the Virtual Machine through `ssh`
- Delete all provisioned resources by running `az group delete`

## Deployment and Configuration Steps

### Prerequisites
- **Azure CLI installed**
    - If you don’t have it, install from https://aka.ms/installazurecliwindows.
    - or in PowerShell run:
   `winget install --exact --id Microsoft.AzureCLI`
- **Have an account on Microsoft Azure with an active Azure Subscription**
  - You can create one on https://signup.azure.com
-  **OpenSSH installed**
   -  Download the installer from https://github.com/PowerShell/Win32-OpenSSH/releases/download/v9.8.3.0p2-Preview/OpenSSH-Win64-v9.8.3.0.msi
   -  or in an ***ADMINISTRATOR*** PowerShell run:
  `Get-WindowsCapability -Online | Where-Object Name -like ‘OpenSSH.Server*’ | Add-WindowsCapability –Online`

### 1. Login to tha Azure CLI
> Command to  run: `az login`

![logging in to azure](/images/step1.png)

- This opens a browser window that displays your logged in Microsoft Account, or a prompt to login.
- After signing in, you will see a list of your active Azure Subscriptions. Follow the prompt and enter the number of the subscription you wish to use or simply press enter if you only have one.

### 2. Create the Resource Group
> Command to run: `az group create` `--name` `--location`

![creating the resource group](/images/step2.png)
- Supply the appropriate parameters of the `--name` and `--location` fields.
- If the creation was succesful, you will receive a response in JSON confirming the new group.

> For reference:  `az group create --name az-cli-test --location eastus2`

### 3. Create the Virtual Machine
For this example, we'll be making a basic Ubuntu VM with SSH key auto-generation
> Command to run `az vm create` `--resource-group` `--name` `--image` `--admin-username` `--size` `--generate-ssh-keys`
> 
![creating the virtual machine](/images/step3.png)
- Ensure that you enter the exact name of the previously created Resource Group for the `--resource-group` parameter
- For our purposes, ensure the `--image` parameter is Ubuntu2404
- Take note of the value you supply `--admin-username` as we will need it to connect to the VM
- `--generate-ssh-keys` generates public & private keys and automatically opens port 22 to SSH traffic
- If VM creation is succesful, the command returns useful info as JSON
- Note down the **"publicIpAddress"** to use to connect to the VM

> For reference: `az vm create --resource-group az-cli-test --name az-cli-vm --image Ubuntu2404 --admin-username labuser --size Standard_B1s --generate-ssh-keys`


### 4. Connect to the Virtual Machine using SSH
> Command to run `ssh USERNAME@PUBLIC IP ADDRESS`

![SSHing into the VM](/images/step4.png)

- SSH into the Virtual Machine using the `--admin-username` you supplied in the last step, and the Public IP Address
- If you've succesfully connected, the Ubuntu 24.04 welcome message should appear and your terminal prompt will change from PowerShell to Bash
- Try running linux commands to observe traffic
  - `echo "Hello, World!`
  - `pwd`
  - `htop` (ctrl+c to exit)

> For reference: `ssh labuser@172.175.200.69`

### 7. Resource clean up
After observing traffic, to avoid charges, delete the entire resource group
> Command to run: `az group delete` `--resource-group`

![deleting all provisioend resources](/images/step5.png)

- Supply the exact name of the resource group, and enter `y` to confirm the operation
- Allow it to finish running, and confirm on https://portal.azure.com/?icid=get-started#browse/resourcegroups that the resource group has been succesfully deleted
> For reference: `az group delete --resource-group az-cli-test` `--yes`