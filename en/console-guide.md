## Dev Tool > Deploy > Console Guide

이 문서에서는 예제로서 다음과 같은 내용을 다룹니다.

* [상품사용 요구사항](/Dev%20Tool/Deploy/ko/console-guide/#_1)
* [상품 활성화](/Dev%20Tool/Deploy/ko/console-guide/#_4)
* [Appkey와 URL 확인](/Dev%20Tool/Deploy/ko/console-guide/#appkey-url)
* [Client Application](/Dev%20Tool/Deploy/ko/console-guide/#client-application)
* [Server Application](/Dev%20Tool/Deploy/ko/console-guide/#server-application)

(본 예제에서 다루지 않은 기능은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)에서 확인하실 수 있습니다.)

## 상품사용 전 필수사항

![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)


> TOAST Deploy 는 SSH 연결을 통해 서버의 배포 명령을 전달합니다. 
> 배포전 배포대상 서버의 SSH 연결이 요구되므로
> 대상서버의 IP/포트/방화벽예외처리와 같은 SSH 연결을 위한 준비가 필요합니다.


### OS 별 요구사항
#### Linux
* curl 7.19.7-43 버전 이상

#### Window
* SSH 설치 필요
    * SSH Shell : PowerShell 지정

### TOAST Cloud VM배포 요구사항
#### 공인아이피 부여
* TOAST Cloud의 VM 인스탄스에 배포하기 위해 VM 인스턴스 [플로팅 IP](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#ip_1)를 생성하여 공인 아이피 부여가 필요합니다.

#### 보안예외 추가
* 배포하고자 하는 VM 인스턴스의 [보안그룹](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#_13)에 Deploy상품 IP(아래)를 SSH Rule로 추가해주시기 바랍니다.
```
133.186.185.112/28
133.186.188.208/28
```
##### 참고) 보안예외 추가방법

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. TOAST Cloud 콘솔의  'Compute' 상품 중 'Instance' 상품을 선택합니다.
2. 현재 VM에 설정된 보안그룹선택 또는 신규 보안그룹(Security Group)을 생성합니다.
3. **+ Rule 추가** 버튼을 클릭합니다. 
    * Rule: SSH 로 선택합니다.
    * CIDR에 IP를 입력합니다.
    * 대역인 경우 대역으로 입력가능합니다. (ex: 133.186.188.208/28)

### TOAST Cloud VM 이외 서버 배포 요구사항
#### 공인아이피 부여
* SSH 연결을 위해 공인아이피가 부여되어야 합니다.

#### 방화벽 및 Network ACL 설정
* 외부에서 접근 가능하도록 아래 IP에 대해  Nework와 방화벽 예외설정을 추가해 주세요
```
133.186.185.112/28
133.186.188.208/28
```

## Deploy 콘솔 화면

![deploy_02_201812](https://static.toastoven.net/prod_tcdeploy/deploy_02_201812.png)

Deploy 상품 콘솔 화면입니다.

## Client Application

클라이언트 어플리케이션을 위한 설정은 크게 아티팩트 설정과 바이너리 업로드 단계를 거칩니다.

### 아티팩트 설정

![deploy_03_201812](https://static.toastoven.net/prod_tcdeploy/deploy_03_201812.png)

1. 리스트 상단의 **생성** 버튼을 클릭합니다.
2. 아티팩트 유형을 "Client Application"으로 선택합니다.
    - 이름(필수), 설명(선택), port(필수) 항목들을 입력합니다.
3. 모달 창 **생성** 버튼을 클릭합니다.

### 바이너리 설정

#### 업로드

* iOS는 ipa, plist 파일을, Android는 apk 파일을 각각 업로드합니다.
* etc의 경우는 Windows 등의 기타 OS의 설치 어플리케이션 용도로 사용합니다.

![deploy_04_201812](https://static.toastoven.net/prod_tcdeploy/deploy_04_201812.png)

1. 하단 탭 중 [바이너리 그룹] > **Default** 클릭 합니다.
    - **새로 만들기**  버튼을 클릭하면 새로운 바이너리 그룹 생성이 가능합니다.
2. 우측 **업로드** 버튼을 클릭합니다.
3. 모달창, **파일선택** 버튼 클릭 후 바이너리 파일을 선택합니다.
    * iOS : ipa 파일(필수), plist 파일(필수)
        * plist : 다운로드 페이지에서 설치를 위해 사용합니다. 파일 내의 다운로드 URL은 선택 입력입니다.
    * Android : apk 파일(필수)
    * 버전(선택), 설명(선택) 정보 입력
4. 입력 완료 후 **업로드** 버튼을 클릭합니다.

#### 배포

특정 바이너리 다운로드 페이지를 SMS / E-mail로 전달할 수 있습니다.

![deploy_05_201812](https://static.toastoven.net/prod_tcdeploy/deploy_05_201812.png)

1. 우측 **전송** 버튼을 클릭합니다.
2. 다운로드 경로 전송 팝업에서 전송 유형과 수신자를 선택하고 **전송** 버튼을 클릭합니다.
    * SMS / E-mail로 일부 또는 모두 둘 다 지정 가능합니다.
3. 지정한 전송 유형으로 수신자에게 다운로드 페이지가 전달 됩니다.

## Server Application

서버 어플리케이션의 배포는 설정(아티팩트, 서버 그룹, 시나리오), 바이너리 업로드, 배포의 단계를 거칩니다.

### 아티팩트 설정

![deploy_06_201812](https://static.toastoven.net/prod_tcdeploy/deploy_06_201812.png)

1. 리스트 상단의 **생성** 버튼을 클릭합니다.
2. 아티팩트 유형을 "Server Application"으로 선택합니다.
    - 이름(필수), 설명(선택), port(필수) 항목들을 입력합니다.
3. 모달 창 **생성** 버튼을 클릭합니다.

### 서버 그룹 설정

배포할 서버를 관리할 수 있는 기능입니다.

![deploy_07_201812](https://static.toastoven.net/prod_tcdeploy/deploy_07_201812.png)

1. 하단 탭 중 [서버 그룹] > **새로 만들기** 버튼을 클릭합니다.
2. 새로 만들 서버 그룹을 설정합니다.
    * 이름(필수), 설명(선택)
    * OS 선택 후 Shell Type를 지정합니다. (항목 선택 또는 직접 입력)
    * Phase 선택 (서버 장비 구분. 지정하지 않을 경우 NONE을 선택합니다.)
    * 서버 추가
        * 서버 추가에는 아래 두가지 방법이 있으며 자세한 내용은 [기능 상세 가이드의 서버 그룹 메뉴](/Dev%20Tool/Deploy/ko/reference/#_11)에서 확인하실 수 있습니다.
            * 대량 추가
            * 개별 추가
         * 호스트 이름(필수), IP 주소(필수), OS(선택) 입력 후 **추가** 버튼을 클릭합니다.
         * 하단 서버 리스트에 추가된 내용 확인 (왼쪽 체크 박스에 체크된 서버만 등록됨)
    
3. 입력 완료 후 **생성** 버튼을 클릭합니다.

### 시나리오 생성

![deploy_08_201812](https://static.toastoven.net/prod_tcdeploy/deploy_08_201812.png)

1. 하단 탭 중 [배포] > 서버 그룹 영역에서 **새로만들기** 버튼을 클릭합니다.
2. 하단에 추가된 시나리오 영역에 시나리오명(선택)을 입력합니다.
3. 입력 완료 후 **생성** 버튼을 클릭합니다.

### 태스크 추가

태스크는 개별 기능 수행 및 순서 제어가 가능한 시나리오 구성 요소 입니다.
아래와 같이 두 가지 종류가 있습니다.

* pre-run Task : 배포 전 실행 기능
* Normal Task : 배포 시 실행 기능

필요에 따라 선택적으로 사용할 수 있으며, 본 문서에서는 기본적인 배포 시 필요한 태스크를 다룹니다.
더 많은 태스크는 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/ko/reference/#_25)에서 확인하실 수 있습니다.

배포를 위해 아래 세 개의 태스크를 추가합니다.

#### 1. User Command 추가

* 배포 시 실행되는 사용자 정의 Command 태스크입니다.
* Available Variables를 사용할 수 있습니다.
    * Available Variables : 예약어. 자세한 내용은 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/ko/reference/#_25) 하단에서 확인하실 수 있습니다.

![deploy_09_201812](https://static.toastoven.net/prod_tcdeploy/deploy_09_201812.png)

1. 하단 탭 중 [배포] > 시나리오 영역 우측 **Task 추가** 버튼을 클릭합니다.
2. Normal Task 하위에 있는 **User Command**를 클릭합니다.
3. 새로운 태스크 내용을 입력합니다.
    * Timeout (min)
        * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
    * Run As
        * 실행 계정을 입력합니다.
    * Command
        * 실행할 명령문을 입력합니다.

4. 입력/변경 완료 후 **적용** 버튼을 클릭합니다. 

#### 2. Binary Deploy 추가

* 업로드한 바이너리 파일의 배포 내용을 설정할 수 있는 태스크입니다.

![deploy_10_201812](https://static.toastoven.net/prod_tcdeploy/deploy_10_201812.png)

1. **Task 추가** 버튼을 클릭하여 Normal Task 하위에 있는 **Binary Deploy**를 클릭합니다.
2. 새로운 태스크 내용을 입력합니다.
    * Timeout (min)
        * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
    * Run As
        * 실행 계정을 입력 합니다.
3. 바이너리 파일을 업로드 하기 위해서 우측 **업로드** 버튼을 클릭합니다.
4. 바이너리 파일 정보를 입력합니다.
    * **파일 선택** 버튼을 클릭하여 바이너리 파일을 선택합니다.
    * 버전 (선택), 설명 (선택) 항목을 입력합니다.
5. 바이너리 업로드 모달창 **업로드** 버튼을 클릭합니다. 
6. 업로드 완료 후 **바이너리 선택** 버튼을 클릭합니다.
7. 원하는 바이너리 버전을 선택합니다.
    * 다수의 버전 존재 시 검색 기능을 활용하실 수 있습니다.
8. **선택** 버튼을 클릭합니다.
<br/>
    * Variable As
        * 해당 바이너리의 Variable명을 지정해 User Command에서 바이너리 정보를 사용할 수 있으며 자세한 내용은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)의 태스크 메뉴 하단에서 확인하실 수 있습니다.
    * 타겟 디렉토리
        * 바이너리를 배포할 디렉토리를 지정합니다.

#### 3. User Command 추가

![deploy_11_201812](https://static.toastoven.net/prod_tcdeploy/deploy_11_201812.png)

1. **Task 추가** 버튼을 클릭하여 Normal Task 하위에 있는 **User Command**를 클릭합니다.
2. 새로운 태스크 내용을 입력합니다.
    * Timeout (min)
        * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
    * Run As
        * 실행 계정을 입력합니다.
    * Command
        * 실행할 명령문을 입력합니다.
3. 입력/변경 완료 후 **적용** 버튼을 클릭합니다. 

### 실행

![deploy_12_201812](https://static.toastoven.net/prod_tcdeploy/deploy_12_201812.png)

1. 우측 **실행** 버튼을 클릭하여 배포 요청을 합니다.
2. 배포 실행 정보를 입력합니다.
    * 배포 노트(선택) 및 인증 방법을 지정합니다.
    * Password 선택 시 비밀번호를 입력하거나 또는 .pem 파일을 선택 후 업로드합니다.
3. 입력 완료 후 **확인** 버튼을 클릭합니다.

![deploy_13_201812](https://static.toastoven.net/prod_tcdeploy/deploy_13_201812.png)

1. 배포 진행 상황을 확인 할 수 있습니다.
2. 배포 완료를 확인합니다.
    * 각 Task 정상 실행 여부는 exit code로 판별됩니다.
3. **결과보기** 버튼을 클릭하여 상세결과를 확인합니다.
4. 배포 상세결과를 확인합니다.
    * 각 태스크 실행에 대한 상세 내용을 (리턴 값, exit code, 오류 내용 등) 확인 가능합니다.

- - -

서버에 파일 배포를 성공하였습니다!
TOAST Deploy는 더 많은 기능을 지원하며, 자세한 사항은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)에서 확인하실 수 있습니다!