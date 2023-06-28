## 서비스 사용 전 필수 사항

### NHN Cloud VM 이외 서버
![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)

> NHN Cloud VM 이외 서버의 경우 SSH 연결로만 서버의 배포 명령을 전달할 수 있습니다.
> 배포 전 배포 target server 와 SSH로 연결해야 하므로
> target server의 IP, 포트, 방화벽 예외 처리와 같은 SSH 연결을 위한 준비가 필요합니다.

### NHN Cloud VM 서버
![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)

> NHN Cloud VM 서버의 경우 SSH 연결 혹은 Cloud-Agent로 서버의 배포 명령을 전달합니다.
> SSH 연결의 경우 위와 동일하게 SSH 연결을 위한 준비가 필요합니다.
> Cloud-Agent 연결의 경우 Cloud-Agent 연결을 위한 준비가 필요합니다.

## SSH 연결을 위한 준비

### Requirements for Each OS
#### Linux
* curl 7.19.7-43 or higher versions

#### Window
* Requires SSH Installation
    * SSH Shell: PowerShell specified

### Requirements for NHN Cloud VM Deployment
#### Assign Public IP
* For the deployment of NHN Cloud VM instances, create a [Floating IP](https://docs.toast.com/zh/Compute/Instance/zh/console-guide/#ip_1) for VM instance and assign public IP.

#### Add Security Exceptions
* Add IP for Deploy (as below) to [Security Group](https://docs.toast.com/zh/Compute/Instance/zh/console-guide/#_13) of a VM instance to deploy, as part of the SSH rule.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### Note) Adding Exceptions for Security

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. Select **Instance** from **Compute** on the NHN Cloud console.
2. Select the security group set for VM, or click **+ Create Security Group** to create a new security group.
3. Click **+ Add Rules**.
    * Rule: Choose SSH.
    * Enter IP at CIDR.
    * Bandwidth may be required. (e.g. 133.186.185.112/28).

### Requirements for Server Deployment Other than NHN Cloud VM
#### Assign Public IP
* To connect SSH, public IP must be assigned.

#### Configure Firewalls and Network ACL
* Add exceptions on network and firewall, for the following IPs, so as to allow external access.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

## Cloud-Agent 연결을 위한 준비

### 운영체제별 Cloud-Agent 설치
#### Linux(CentOS)

* Cloud-Agent 작업 디렉토리 생성 및 설정 파일 수정을 위해 NHN Cloud Instance 상품에서 인스턴스를 생성 시 사용자스크립트에 아래의 내용을 추가합니다.
    * 이미 생성한 상태라면 아래의 스크립트를 인스턴스에 접속하여 실행해줍니다.
```
#!/bin/bash

function download_qga_config() {
    sudo wget http://static.toastoven.net/prod_tcdeploy/qemu/qemu-ga -O /etc/sysconfig/qemu-ga
}

function create_qga_directory() {
    if [ ! -d "/var/log/qemu-ga" ]; then
        sudo mkdir /var/log/qemu-ga
    fi
}

function write_qga_rotate_file() {
    sudo sh -c 'echo "/var/log/qemu-ga {
    daily
    rotate 50
    missingok
    notifempty
    nocompress
}" >> /etc/logrotate.d/qemu_ga'
}

### Main ###
download_qga_config
create_qga_directory
write_qga_rotate_file
```

#### Windows
* NHN Cloud Instance 상품에서 인스턴스를 생성 시 사용자스크립트에 아래의 내용을 추가합니다.
    * 이미 생성한 상태라면 아래의 스크립트를 인스턴스에 접속하여 실행해줍니다.
```
#ps1_sysnative

if (!(Test-Path -Path C:\temp\qemu-ga)){
    New-Item -ItemType directory -Path C:\temp\qemu-ga | out-null;
}

if (!(Test-Path -Path C:\.qemu-download)){
    New-Item -ItemType directory -Path C:\.qemu-download | out-null;
}

$isoFilePath = 'C:\.qemu-download\virtio-win-0.1.229.iso';

if ( -NOT (Test-Path isoFilePath) ) {
    $wc = New-Object System.Net.WebClient;
    $wc.DownloadFile('http://static.toastoven.net/prod_tcdeploy/qemu/virtio-win-0.1.229.iso', $isoFilePath);
}


Mount-DiskImage -ImagePath $isoFilePath

$msiPath = 'D:\guest-agent\qemu-ga-x86_64.msi';

Start-Process -FilePath msiexec.exe -ArgumentList "/i $msiPath /qn" -Wait

Dismount-DiskImage -ImagePath $isoFilePath

Remove-Item -Path $isoFilePath
```

### 유효성 확인을 통한 Cloud-Agent 활성화
* NHN Cloud Deploy 상품에서 생성한 인스턴스를 추가하여 서버 그룹을 생성합니다.
    * 생성 전 OS 및 Shell Type 반드시 확인해 주세요.

![deploy_14_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_14_202307.png)
![deploy_15_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_15_202307.png)


* 배포 탭으로 이동 후 위 과정에서 생성한 서버 그룹을 선택하여 새로운 시나리오를 만들어줍니다.

![deploy_16_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_16_202307.png)

* 유효성 확인 버튼을 눌러 줍니다.

![deploy_17_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_17_202307.png)

![deploy_18_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_18_202307.png)
- - -

QGA 서비스 설치 및 유효성 확인이 성공 하였습니다!
유효성 확인이 성공 하였을 경우 Deploy 서비스를 사용하여 배포가 가능합니다. 자세한 사항은 [Deploy > 콘솔 사용 가이드](/Dev%20Tools/Deploy/en/console-guide/)에서 확인할 수 있습니다.