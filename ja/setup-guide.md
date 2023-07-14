## 서비스 사용 전 필수 사항

### NHN Cloud VM 이외 서버
![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)

> NHN Cloud VM 이외 서버의 경우 SSH 연결로만 서버의 배포 명령을 전달할 수 있습니다.
> 배포 전 배포 target server 와 SSH로 연결해야 하므로
> target server의 IP, 포트, 방화벽 예외 처리와 같은 SSH 연결을 위한 준비가 필요합니다.

### NHN Cloud VM 서버
![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)

> NHN Cloud VM 서버의 경우 SSH 연결 혹은 Cloud Agent로 서버의 배포 명령을 전달합니다.
> SSH 연결의 경우 위와 동일하게 SSH 연결을 위한 준비가 필요합니다.
> Cloud Agent 연결의 경우 Cloud Agent 연결을 위한 준비가 필요합니다.

## SSH 연결을 위한 준비

### OS別要件
#### Linux
* curl 7.19.7-43バージョン以上

#### Windows
* SSHインストール必要
    * OpenSSH_for_Windows_8.6p1、LibreSSL 3.3.3バージョン以上
        * Windows Server 2019使用時、OpenSSHを別途インストール必要
    * SSH Shell: PowerShell指定

### NHN Cloudインスタンスへ配布するのための要件
#### グローバルIPの付与
* NHN Cloudのインスタンスに配布するには、インスタンスに[Floating IP](https://docs.toast.com/ja/Compute/Instance/ja/console-guide/#ip_1)接続して、グローバルIPを付与する必要があります。

#### セキュリティー例外の追加
* 配布するインスタンスの[セキュリティーグループ](https://docs.toast.com/ja/Compute/Instance/ja/console-guide/#_13)に、DeployサービスのIP(下記)をSSH のアクセスルール行に追加します。
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### 参考)セキュリティー例外追加方法

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/ja/deploy_01_ja_20200519.png)

1. NHN Cloudコンソールの**Compute**サービスの中から**Instance**を選択します。
2. 対象のインスタンスに設定されているセキュリティグループを選択するか、**+ セキュリティグループの作成**　をクリックして新規セキュリティグループを作成します。
3. **+ セキュリティポリシー作成**　をクリックします。
    * IPプロトコル　SSH を選択します。
    * CIDRにIPを入力します。
    * 帯域を入力することもできます(例：133.186.185.112/28)。

### NHN Cloudインスタンス以外のサーバー配布要求事項
#### グローバルIP付与
* SSH接続のためにグローバルIPを付与する必要があります。

#### ファイアウォールおよびNetwork ACL設定
* 外部からアクセスできるように、下記IPに対してネットワークとファイアウォール例外設定を追加してください。
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

## Cloud Agent 연결을 위한 준비

### 운영체제별 Cloud Agent 설치
#### Linux(CentOS)

* Cloud Agent 작업 디렉토리 생성 및 설정 파일 수정을 위해 NHN Cloud Instance 상품에서 인스턴스를 생성 시 사용자스크립트에 아래의 내용을 추가합니다.
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

### Cloud Agent 설치 확인
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

Cloud Agent 서비스 설치 및 유효성 확인이 성공 하였습니다!
유효성 확인이 성공 하였을 경우 Deploy 서비스를 사용하여 배포가 가능합니다. 자세한 사항은 [Deploy > 콘솔 사용 가이드](/Dev%20Tools/Deploy/ja/console-guide/)에서 확인할 수 있습니다.