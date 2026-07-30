<!-- pre-align:aligned sig=a87ab717aea2 -->

<a id="dev-tools-deploy-setup-guide-before-use"></a>

## Dev Tools > Deploy > Setup Guide Before Use

This document contains the following.

* [Pre-requisites before using the service](/Dev%20Tools/Deploy/en/setup-guide/#pre-requisites-before-using-the-service)
* [Prepare to use NHN Cloud Agent](/Dev%20Tools/Deploy/en/setup-guide/#prepare-to-use-nhn-cloud-agent)
* [Prepare for an SSH connection](/Dev%20Tools/Deploy/en/setup-guide/#prepare-for-an-ssh-connection)

<a id="pre-requisites-before-using-the-service"></a>

## Pre-requisites before using the service { #pre-requisites-before-using-the-service }

<a id="nhn-cloud-vm-server"></a>

### NHN Cloud VM Server { #nhn-cloud-vm-server }
![SSH connectionRequired](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_19_202307.png)

> For NHN Cloud VM servers, you can use an SSH connection or NHN Cloud Agent to deliver the server's deployment command.
In case of SSH connection, you need to [prepare for SSH connection](/Dev%20Tools/Deploy/en/setup-guide/#prepare-for-an-ssh-connection), such as IP, port, and firewall exception handling of the target server.
For NHN Cloud Agent, you need to [prepare for using](/Dev%20Tools/Deploy/en/setup-guide/#prepare-to-use-nhn-cloud-agent)NHN Cloud Agent, such as installing and validating NHN Cloud Agent.

<a id="servers-other-than-nhn-cloud-vm"></a>

### Servers other than NHN Cloud VM { #servers-other-than-nhn-cloud-vm }
![SSH connectionRequired](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_20_202307.png)

> For servers other than NHN Cloud VMs, you can pass the server's deployment commands only through an SSH connection.
You need to connect to the deployment target server via SSH before deployment.
You need to [prepare for the SSH connection](/Dev%20Tools/Deploy/en/setup-guide/#prepare-for-an-ssh-connection), such as the target server's IP, port, and firewall exception handling.

<a id="prepare-to-use-nhn-cloud-agent"></a>

## Prepare to use NHN Cloud Agent { #prepare-to-use-nhn-cloud-agent }

<a id="install-nhn-cloud-agent-by-operating-system"></a>

### Install NHN Cloud Agent by operating system { #install-nhn-cloud-agent-by-operating-system }
* To pass deployment commands to NHN Cloud Agent, you need to install NHN Cloud Agent.
* When you create an instance in NHN Cloud Instance service, you can add the following installation script contents for Linux and Windows operating systems to **Additional Settings** > **User Script**to install it.
![User script](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_21_202307.png)
* If **Additional Settings** > **User script** is not available, connect to the instance directly and run the install script.

<a id="linux-installation-script"></a>

#### Linux installation script
```
#!/bin/bash
curl 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_linux_1.0.0.sh' | sudo bash
```

<a id="windows-installation-scripts"></a>

#### Windows installation scripts
```
#ps1_sysnative
Invoke-WebRequest -UseBasicParsing 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_windows_1.0.0.ps1' | Invoke-Expression
```

<a id="check-the-installation-of-nhn-cloud-agent"></a>

### Check the installation of NHN Cloud Agent { #check-the-installation-of-nhn-cloud-agent }
* Create a server group by adding the instances created by the NHN Cloud Deploy service.
    * When creating a server group, be sure to confirm the **OS** and **Shell Type**. The default values for **Shell Type**are /bin/bash (Linux), powershell (Windows).

![deploy_14_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_14_202307.png)
![deploy_15_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_15_202307.png)


* Go to the **Deployment** tab, select the server group you created above, and click **New**in the **Scenarios** section.

![deploy_16_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_16_202307.png)

* Enter a name for your scenario in the field on the left, and click **Add Task**to select **User Command**for **Normal Task**.

![deploy_22_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_22_202307.png)

* **In Command,**enter a command that has no effect, such as `pwd`, and click **Create**.

![deploy_23_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_23_202307.png)

* Click **Check Validity**.

![deploy_17_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_17_202307.png)

![deploy_18_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_18_202307.png)

The installation and validation of the NHN Cloud Agent service was successful.

<a id="prepare-for-an-ssh-connection"></a>

## Prepare for an SSH connection { #prepare-for-an-ssh-connection }

<a id="requirements-for-each-os"></a>

### Requirements for each OS { #requirements-for-each-os }
<a id="linux"></a>

#### Linux
* curl 7.19.7-43 or higher versions

<a id="windows"></a>

#### Windows
* Requires SSH installation
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3 or higher
        * When using Windows Server 2019, OpenSSH must be installed separately
    * SSH Shell: PowerShell specified

<a id="requirements-for-nhn-cloud-vm-deployment"></a>

### Requirements for NHN Cloud VM Deployment { #requirements-for-nhn-cloud-vm-deployment }
<a id="assign-public-ip"></a>

#### Assign Public IP
* To deploy to a VM instance in NHN Cloud, you need to create a VM instance [floating IP](https://docs.nhncloud.com/en/Compute/Instance/en/console-guide/#ip_1)and grant it a public IP.

<a id="add-security-exceptions"></a>

#### Add Security Exceptions
* Add the Deploy service IP (below) as an SSH Rule to the [security group](https://docs.nhncloud.com/en/Compute/Instance/en/console-guide/#_13)of the VM instance you want to deploy.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### Note) Adding Exceptions for Security

![deploy_01_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_01_202307.png)

1. In the NHN Cloud console, under **Network** services, select **Security Groups**.
2. Select a security group that is currently set on the VM or click **+ Create Security Group**to create a new security group.
3. Click the **+** button.
    * Direction: Select In.
    * IP protocol: Select Custom TCP.
    * Port: Enter 22. (SSH Port)
    * Remote: Enter an IP in the CIDR. You can also enter a band. (e.g. 133.186.185.112/28)

<a id="requirements-for-server-deployment-other-than-nhn-cloud-vm"></a>

### Requirements for Server Deployment Other than NHN Cloud VM { #requirements-for-server-deployment-other-than-nhn-cloud-vm }
<a id="requirements-for-server-deployment-other-than-nhn-cloud-vm-assign-public-ip"></a>

#### Assign Public IP
* To connect SSH, public IP must be assigned.

<a id="configure-firewalls-and-network-acl"></a>

#### Configure Firewalls and Network ACL
* Add exceptions on network and firewall, for the following IPs, so as to allow external access.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

- - -

If you have completed preparing the SSH connection or installing and validating the NHN Cloud Agent service, you can deploy using the Deploy service.
For more information, see [Deploy > Console User Guide](/Dev%20Tools/Deploy/en/console-guide/).
