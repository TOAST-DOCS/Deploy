## Dev Tools > Deploy > 사용 전 설정 가이드

이 문서에서는 다음과 같은 내용을 다룹니다.

* [서비스 사용 전 필수 사항](/Dev%20Tools/Deploy/ko/setup-guide/#_1)
* [Cloud Agent 사용을 위한 준비](/Dev%20Tools/Deploy/ko/setup-guide/#cloud-agent)
* [SSH 연결을 위한 준비](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)

## 서비스 사용 전 필수 사항

### NHN Cloud VM 서버
![SSH연결필수](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_19_202307.png)

> NHN Cloud VM 서버의 경우 SSH 연결 혹은 Cloud Agent로 서버의 배포 명령을 전달합니다.
> SSH 연결의 경우 target server의 IP, 포트, 방화벽 예외 처리와 같은 [SSH 연결을 위한 준비](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)가 필요합니다.
> Cloud Agent의 경우 Cloud Agent 설치, 유효성 확인과 같은 [Cloud Agent 사용을 위한 준비](/Dev%20Tools/Deploy/ko/setup-guide/#cloud-agent)가 필요합니다.

### NHN Cloud VM 이외 서버
![SSH연결필수](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_20_202307.png)

> NHN Cloud VM 이외 서버의 경우 SSH 연결로만 서버의 배포 명령을 전달할 수 있습니다.
> 배포 전 배포 target server 와 SSH로 연결해야 하므로
> target server의 IP, 포트, 방화벽 예외 처리와 같은 [SSH 연결을 위한 준비](/Dev%20Tools/Deploy/ko/setup-guide/#ssh)가 필요합니다.

## Cloud Agent 사용을 위한 준비

### 운영체제별 Cloud Agent 설치
* Cloud Agent 설치 및 설정 파일 수정을 위해 NHN Cloud Instance 상품에서 인스턴스를 생성 시 추가 설정 > 사용자 스크립트에 Linux, Windows 운영체제에 맞는 스크립트 내용을 추가합니다.
  ![사용자 스크립트](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_21_202307.png)
* 만약 사용자 스크립트를 사용할 수 없는 경우나 이미 생성한 상태라면 아래의 스크립트를 인스턴스에 접속하여 실행해줍니다.

#### Linux 설치 스크립트
```
#!/bin/bash
curl 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_linux_1.0.0.sh' | sudo bash
```

#### Windows 설치 스크립트
```
#ps1_sysnative
Invoke-WebRequest -UseBasicParsing 'https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/qemu/cloud_agent_install_windows_1.0.0.ps1' | Invoke-Expression
```

### Cloud Agent 설치 확인
* NHN Cloud Deploy 상품에서 생성한 인스턴스를 추가하여 서버 그룹을 생성합니다.
    * 생성 전 OS 및 Shell Type 반드시 확인해 주세요. Shell Type의 기본값은 /bin/bash(Linux), powershell(Windows) 입니다.

![deploy_14_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_14_202307.png)
![deploy_15_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_15_202307.png)


* 배포 탭으로 이동 후 위 과정에서 생성한 서버 그룹을 선택하여 새로운 시나리오를 만들어줍니다.

![deploy_16_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_16_202307.png)

* 좌측에 시나리오 명을 입력하고, 우측의 [Task 추가] 버튼을 눌러서 Normal Task의 User Command를 선택합니다.

![deploy_22_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_22_202307.png)

* Command에 pwd와 같이 아무런 영향을 주지 않는 명령어 등을 입력하고 [생성] 버튼을 눌러 줍니다.

![deploy_23_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_23_202307.png)

* 유효성 확인 버튼을 눌러 줍니다.

![deploy_17_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_17_202307.png)

![deploy_18_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_18_202307.png)

Cloud Agent 서비스 설치 및 유효성 확인이 성공 하였습니다!

## SSH 연결을 위한 준비

### OS별 요구 사항
#### Linux
* curl 7.19.7-43 버전 이상

#### Windows
* SSH 설치 필요
    * OpenSSH_for_Windows_8.6p1, LibreSSL 3.3.3 버전 이상
        * Windows Server 2019 사용 시 OpenSSH 별도 설치 필요
    * SSH Shell: PowerShell 지정

### NHN Cloud VM 배포 요구 사항
#### 공인 IP 부여
* NHN Cloud의 VM 인스턴스에 배포하려면 VM 인스턴스 [플로팅 IP](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#ip_1)를 생성하여 공인 IP를 부여해야 합니다.

#### 보안 예외 추가
* 배포할 VM 인스턴스의 [보안 그룹](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#_13)에 Deploy 서비스 IP(아래)를 SSH Rule로 추가합니다.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```
##### 참고) 보안 예외 추가 방법

![deploy_01_202307](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_tcdeploy/deploy_01_202307.png)

1. NHN Cloud 콘솔의 **Network** 서비스 중 **Security Groups**를 선택합니다.
2. 현재 VM에 설정된 보안 그룹을 선택하거나 **+ 보안 그룹 생성**을 클릭해 신규 보안 그룹을 생성합니다.
3. **+** 버튼을 클릭합니다.
    * 방향: 수신을 선택합니다.
    * IP 프로토콜: 사용자 정의 TCP를 선택합니다.
    * 포트: 22를 입력합니다. (SSH Port)
    * 원격: CIDR에 IP를 입력합니다. 대역을 입력할 수도 있습니다. (예: 133.186.185.112/28)

### NHN Cloud VM 이외 서버 배포 요구 사항
#### 공인 IP 부여
* SSH 연결을 위해 공인 IP를 부여해야 합니다.

#### 방화벽 및 Network ACL 설정
* 외부에서 접근할 수 있게 아래 IP에 대해 네트워크와 방화벽 예외 설정을 추가해 주세요.
```
133.186.185.112/28
117.52.123.201/32
117.52.123.202/32
```

- - -

SSH 연결 준비 혹은 Cloud Agent 서비스 설치 및 유효성 확인이 완료 되었을 경우 Deploy 서비스를 사용하여 배포가 가능합니다. 
자세한 사항은 [Deploy > 콘솔 사용 가이드](/Dev%20Tools/Deploy/ko/console-guide/)에서 확인할 수 있습니다.