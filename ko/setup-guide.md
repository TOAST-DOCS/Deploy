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

### 운영체제별 요구 사항
#### Linux
* curl 7.19.7-43 버전 이상

#### Windows
* SSH 설치 필요
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3 버전 이상
        * Windows Server 2019 사용 시 OpenSSH 별도 설치 필요
    * SSH Shell: PowerShell 지정

### NHN Cloud VM SSH 연결 요구 사항
#### 공인 IP 부여
* NHN Cloud의 VM 인스턴스에 배포하려면 VM 인스턴스 [플로팅 IP](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#ip_1)를 생성하여 공인 IP를 부여해야 합니다.

#### 보안 예외 추가
* 배포할 VM 인스턴스의 [보안 그룹](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#_13)에 Deploy 서비스 IP(아래)를 SSH Rule로 추가합니다.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### 참고) 보안 예외 추가 방법

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. NHN Cloud 콘솔의 **Compute** 서비스 중 **Instance**를 선택합니다.
2. 현재 VM에 설정된 보안 그룹을 선택하거나 **+ Security Group 생성**을 클릭해 신규 보안 그룹(Security Group)을 생성합니다.
3. **+ Rule 추가** 버튼을 클릭합니다.
    * Rule: SSH로 선택합니다.
    * CIDR에 IP를 입력합니다.
    * 대역을 입력할 수도 있습니다(예​:​ 133.186.185.112/28).

### NHN Cloud VM 이외 서버 SSH 연결 요구 사항
#### 공인 IP 부여
* SSH 연결을 위해 공인 IP를 부여해야 합니다.

#### 방화벽 및 Network ACL 설정
* 외부에서 접근할 수 있게 아래 IP에 대해 네트워크와 방화벽 예외 설정을 추가해 주세요.
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

### 유효성 확인을 통한 Cloud-Agent 활성화 (필수)
* NHN Cloud Deploy 상품에서 생성한 인스턴스를 추가하여 서버그룹을 생성합니다.
  * 생성 전 OS 및 Shell Type 반드시 확인 부탁 드립니다.

* 배포 탭으로 이동 후 위 과정에서 생성한 서버그룹을 선택하여 새로운 시나리오를 만들어줍니다.

* 유효성 확인 버튼을 눌러 줍니다.

- - -

QGA 서비스 설치 및 유효성 확인이 성공 하였습니다!
유효성 확인이 성공 하였을 경우 Deploy 서비스를 사용하여 배포가 가능합니다. 자세한 사항은 Deploy > 콘솔 사용 가이드에서 확인하실 수 있습니다.