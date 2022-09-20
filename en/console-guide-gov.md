## Dev Tools > Deploy > Console User Guide 

This document contains the following: 

* [Service Pre-requisites](/Dev%20Tools/Deploy/en/console-guide-gov/#service-pre-requisites)
* [Deploy Console Page](/Dev%20Tools/Deploy/en/console-guide-gov/#deploy-console-page)
* [Client Application](/Dev%20Tools/Deploy/en/console-guide-gov/#client-application)
* [Server Application](/Dev%20Tools/Deploy/en/console-guide-gov/#server-application)

(Any other functions are available in [Detail Functional Guide](/Dev%20Tools/Deploy/en/reference-gov/).)

## Service Pre-requisites 

![Requires SSH Connection](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)


> NHN Cloud Deploy delivers deployment commands via SSH connection. 
> Before deployment, it must be connected with a target server via SSH, so preparation is required, such as target server IP, port, and exceptions for firewall.  
>


### Requirements for Each OS 
#### Linux 
* curl 7.19.7-43 or higher versions  

#### Windows
* Requires SSH installation 
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3 or higher
         * When using Windows Server 2019, OpenSSH must be installed separately
    * SSH Shell: PowerShell specified  

### Requirements for NHN Cloud VM Deployment 
#### Assign Public IP  
* For the deployment of NHN Cloud VM instances, create a [Floating IP](https://gov-docs.toast.com/en/Compute/Instance/en/console-guide/#ip_1) for VM instance and assign public IP. 

#### Add Security Exceptions 
* Add IP for Deploy (as below) to [Security Group](https://gov-docs.toast.com/en/Compute/Instance/en/console-guide/#_13) of a VM instance to deploy, as part of the SSH rule. 
```
133.186.185.112/28
133.186.188.208/28
```
##### Note) Adding Exceptions for Security  

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. Select **Instance** from **Compute** on the NHN Cloud console.
2. Select the security group set for VM, or click **+ Create Security Group** to create a new security group. 
3. Click **+ Add Rules**. 
    * Rule: Choose SSH.
    * Enter IP at CIDR.
    * Bandwidth may be required. (e.g. 133.186.188.208/28).

### Requirements for Server Deployment Other than NHN Cloud VM 
#### Assign Public IP 
* To connect SSH, public IP must be assigned. 

#### Configure Firewalls and Network ACL 
* Add exceptions on network and firewall, for the following IPs, so as to allow external access. 
```
211.56.2.51/32
211.56.2.52/32
```

## Deploy Console Page  

Below is the console page for the Deploy service. 

![deploy_02_201812](https://static.toastoven.net/prod_tcdeploy/deploy_02_201812.png)

## Client Application

Client application deployment requires setting artifacts and uploading binaries.  

### Setting Artifacts  

![deploy_03_201812](https://static.toastoven.net/prod_tcdeploy/deploy_03_201812.png)

1. Click **Create** on the top left of the **Deploy** page.   
2. Choose **Client Application** for the type of an artifact. 
    - Enter name (required), description (optional), and port (required).
3. Click **Create**.

### Setting Binaries

#### Upload 

* Upload .ipa and .plist files for iOS, and .apk files for Android. 
* etc. is applied as an installation application for other OS, like Windows. 

![deploy_04_201812](https://static.toastoven.net/prod_tcdeploy/deploy_04_201812.png)

1. Go to **Binary Group > Default** on the tab below the **Deploy** page. 
    Click **Create** to create a new binary group. 
2. Click **Upload** on the right. 
3. Choose **Select Files** from **Upload Binaries**, and select a binary file.
    * iOS: .ipa file (required), .plist file (required)
        * .plist: Applied to install on the download page. Download URL in the file is optional.
    * Android: .apk file (required)
    * Enter version (optional) and description (optional).
4. When it is completed, click **Upload**. 

#### Deploy

Download pages can be selectively delivered via SMS or email. 

![deploy_05_201812](https://static.toastoven.net/prod_tcdeploy/deploy_05_201812.png)

1. Click **Send** on the right. 
2. On the **Send Download Path** window, select Type and Recipient, and click **Send**.  
    * You may choose either SMS or email, or both. 

Then, the binary download page is delivered to the recipient in the specified type of delivery. 

## Server Application

Server application requires setting deployment (artifact, server, server group, and scenario), uploading binaries, and deployment. 

### Setting Artifacts 

![deploy_06_201812](https://static.toastoven.net/prod_tcdeploy/deploy_06_201812.png)

1. Click **Create** on the list. 
2. Select **Server Application** for the type of an artifact. 
    - Enter name (required), description (optional), and port (required). 
3. Click **Create** from the **Create Artifacts** window.  

### Setting Server Groups 

Deployment servers are managed by this setting. 

![deploy_07_201812](https://static.toastoven.net/prod_tcdeploy/deploy_07_201812.png)

1. Go to **Deploy** and click **Server Group > Create** at the bottom of the page.
2. Set a server group to create on the **Create Server Group** window.  
    * Enter name (required) and description (optional).
    * Select OS and specify the **Shell Type**: enter one or select from the list. 
    * Select Phase: Choose a server tool. Otherwise, select NONE. 
    * Add Servers 
        * Servers can be added in the following two methods, and find more details from [Detail Functional Guide on Server Groups](/Dev%20Tools/Deploy/en/reference-gov/#_11).
            * Add in Mass
            * Add Individually 
         * Enter host name (required), IP address (required), and OS (optional), and click **Add**. 
         * See what is added on the server list: only the servers checked on the left box can be registered. 

3.  When it is completed, click **Create**. 

### Create Scenarios 

![deploy_08_201812](https://static.toastoven.net/prod_tcdeploy/deploy_08_201812.png)

1. Go to **Deploy** and click **Deploy > Create**.
2. Enter scenario name (optional) in Scenario which is added below. 
3. Click **Create**.  

### Add Tasks 

A task is an element of a scenario which can execute individual functions and control order, and it is categorized into two as below: 

* pre-run Task: Execute before deployment 
* Normal Task: Execute while deployed 

Choose one as you need. This document describes tasks that are basically required for deployment. 
Find more tasks on [Detail Functional Guide on Tasks](/Dev%20Tools/Deploy/en/reference-gov/#_25).

To test deployment, the following three tasks are added: 

#### 1. Add User Commands 

* It is a user-defined command task which is executed for deployment. 
* You may use Available Variables.
    * Available Variables: Reserved words. Find more details on [Detail Functional Guide on Tasks](/Dev%20Tools/Deploy/en/reference-gov/#_25).

![deploy_09_201812](https://static.toastoven.net/prod_tcdeploy/deploy_09_201812.png)

1. Click **Add Tasks** on the right of Scenario from the **Deploy** tab.
2. Click **User Command** below **Normal Task**.  
3. Fill out information for the new task. 
    * Timeout (min)
        * Specify timeout to complete task execution: min 1, and max 30 minutes. 
    * Run As
        * Enter execution account. 
    * Command
        * Enter commands to run. 

4. When it is completed, click **Apply**. 

#### 2. Add Binary Deploy 

Deployment for uploaded binary files can be set. 

![deploy_10_201812](https://static.toastoven.net/prod_tcdeploy/deploy_10_201812.png)

1. Click **Add Tasks** and **Binary Deploy** below **Normal Task**.
2. Fill out information for the new task. 
    * Timeout (min)
        * Specify timeout to complete task execution: min 1, and max 30 minutes.
    * Run As
        * Enter execution account. 
3. To upload binary files, click **Upload** on the right. 
4. Enter binary file information. 
    * Click **Select Files** to choose a binary file. 
    * Enter Version (optional) and Description (optional).
5. Click **Upload**. 
6. After it is uploaded, click **Select Binaries**. 
7. Select a binary version you need. 
    * Use Search to choose from many versions. 
8. Click **Select**.  
    <br/>
   * Variable As
       * You can specify the name of variables of binary and use binary information at User Command. Find moe details at the bottom of the task menu of [Detail Functional Guide](/Dev%20Tools/Deploy/en/reference-gov/).
   * Target Directory
       * Specify a target directory to deploy binaries. 

#### 3. Add User Commands

![deploy_11_201812](https://static.toastoven.net/prod_tcdeploy/deploy_11_201812.png)

1. Click **Add Tasks** and select **User Command** below **Normal Tasks**.
2. Fill in information for the new task. 
    * Timeout (min)
        * Specify timeout to complete task execution: min 1, and max 30 minutes. 
    * Run As
        * Enter account for execution. 
    * Command
        * Enter commands to execute. 
3. When it is completed, click **Apply**.  

### Execute 

![deploy_12_201812](https://static.toastoven.net/prod_tcdeploy/deploy_12_201812.png)

1. Click **Execute** on the right to request for deployment. 
2. Fill in execution information for deployment. 
    * Specify deployment note (optional) and an authentication method. 
    * Enter password for **Password**, or select and upload .pem file.  
3. When it is completed, click **Confirm**. 

![deploy_13_201812](https://static.toastoven.net/prod_tcdeploy/deploy_13_201812.png)

1. You can find the progress of deployment. 
2. Check deployment is completed. 
    * Use exit code to see if each task is normally executed. 
3. To find detail results, click **See Results**. 
4. Check deployment result on the window of **See Results**.
    * You can find details on the execution of each task (return value, exit code, error, and etc.). 

- - -

File has been deployed to the server. 
NHN Cloud Deploy supports more functions, and find more details in [Detail Functional Guide](/Dev%20Tools/Deploy/en/reference-gov/).

