## Dev Tool > Deploy > 콘솔 사용 가이드

이 문서에서는 다음과 같은 내용을 다룹니다.

* [서비스 사용 전 필수사항](/Dev%20Tool/Deploy/ko/console-guide/#1)
* [Deploy 콘솔 화면](/Dev%20Tool/Deploy/ko/console-guide/#deploy)
* [Client Application](/Dev%20Tool/Deploy/ko/console-guide/#client-application)
* [Server Application](/Dev%20Tool/Deploy/ko/console-guide/#server-application)

(여기에서 다루지 않는 기능은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)에서 확인하실 수 있습니다.)

## 애플리케이션 배포하기
<iframe width="560" height="315" src="https://www.youtube.com/embed/oVyGlPTOyNg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 리소스 관리하기
<iframe width="560" height="315" src="https://www.youtube.com/embed/Fcc0g0E6-r0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 서비스 사용 전 필수 사항

![SSH연결필수](http://static.toastoven.net/prod_tcdeploy/getstarted/console_ssh_required.png)


> TOAST Deploy는 SSH 연결로 서버의 배포 명령을 전달합니다. 
> 배포 전 배포 target server 와 SSH로 연결해야 하므로
> target server의 IP, 포트, 방화벽 예외 처리와 같은 SSH 연결을 위한 준비가 필요합니다.


### OS별 요구 사항
#### Linux
* curl 7.19.7-43 버전 이상

#### Window
* SSH 설치 필요
    * SSH Shell: PowerShell 지정

### TOAST VM 배포 요구 사항
#### 공인 IP 부여
* TOAST의 VM 인스턴스에 배포하려면 VM 인스턴스 [플로팅 IP](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#ip_1)를 생성하여 공인 IP를 부여해야 합니다.

#### 보안 예외 추가
* 배포할 VM 인스턴스의 [보안 그룹](https://docs.toast.com/ko/Compute/Instance/ko/console-guide/#_13)에 Deploy 서비스 IP(아래)를 SSH Rule로 추가합니다.
```
133.186.185.112/28
133.186.188.208/28
```
##### 참고) 보안 예외 추가 방법

![deploy_01_201812](https://static.toastoven.net/prod_tcdeploy/deploy_01_201812.png)

1. TOAST 콘솔의 **Compute** 서비스 중 **Instance**를 선택합니다.
2. 현재 VM에 설정된 보안 그룹을 선택하거나 **+ Security Group 생성**을 클릭해 신규 보안 그룹(Security Group)을 생성합니다.
3. **+ Rule 추가** 버튼을 클릭합니다. 
    * Rule: SSH로 선택합니다.
    * CIDR에 IP를 입력합니다.
    * 대역을 입력할 수도 있습니다(예​:​ 133.186.188.208/28).

### TOAST VM 이외 서버 배포 요구 사항
#### 공인 IP 부여
* SSH 연결을 위해 공인 IP를 부여해야 합니다.

#### 방화벽 및 Network ACL 설정
* 외부에서 접근할 수 있게 아래 IP에 대해 네트워크와 방화벽 예외 설정을 추가해 주세요.
```
133.186.185.112/28
133.186.188.208/28
```

## Deploy 콘솔 화면

다음은 Deploy 서비스 콘솔 화면입니다.

![deploy_02_201812](https://static.toastoven.net/prod_tcdeploy/deploy_02_201812.png)

## Client Application

클라이언트 애플리케이션 배포 설정은 크게 아티팩트 설정과 바이너리 업로드 단계를 거칩니다.

### 아티팩트 설정

![deploy_03_201812](https://static.toastoven.net/prod_tcdeploy/deploy_03_201812.png)

1. **Deploy** 화면 왼쪽 위에서 **생성** 버튼을 클릭합니다.
2. 아티팩트 유형을 **Client Application**으로 선택합니다.
    - 이름(필수), 설명(선택), port(필수)를 입력합니다.
3. **생성** 버튼을 클릭합니다.

### 바이너리 설정

#### 업로드

* iOS는 .ipa, .plist 파일을, Android는 .apk 파일을 각각 업로드합니다.
* etc의 경우는 Windows 등의 기타 OS의 설치 애플리케이션 용도로 사용합니다.

![deploy_04_201812](https://static.toastoven.net/prod_tcdeploy/deploy_04_201812.png)

1. **Deploy** 화면 아래 탭에서 **바이너리 그룹 > Default**를 클릭합니다.
    새 바이너리 그룹을 만들려면 **새로 만들기**  버튼을 클릭합니다.
2. 오른쪽에서 **업로드** 버튼을 클릭합니다.
3. **바이너리 업로드** 창에서 **파일 선택** 버튼을 클릭하고 바이너리 파일을 선택합니다.
    * iOS: .ipa 파일(필수), .plist 파일(필수)
        * .plist: 다운로드 페이지에서 설치를 위해 사용합니다. 파일 내의 다운로드 URL은 선택 입력입니다.
    * Android: .apk 파일(필수)
    * 버전(선택), 설명(선택) 정보 입력
4. 입력을 완료하고 **업로드** 버튼을 클릭합니다.

#### 배포

특정 바이너리 다운로드 페이지를 SMS나 E-mail로 전달할 수 있습니다.

![deploy_05_201812](https://static.toastoven.net/prod_tcdeploy/deploy_05_201812.png)

1. 오른쪽 **전송** 버튼을 클릭합니다.
2. **다운로드 경로 전송** 창에서 전송 유형과 수신자를 선택하고 **전송** 버튼을 클릭합니다.
    * SMS 또는 E-mail 중 하나를 선택할 수도 있고 모두 선택할 수도 있습니다.

지정한 전송 유형으로 수신자에게 바이너리 다운로드 페이지가 전달됩니다.

## Server Application

서버 애플리케이션 배포 설정(아티팩트, 서버 그룹, 시나리오), 바이너리 업로드, 배포 단계를 거칩니다.

### 아티팩트 설정

![deploy_06_201812](https://static.toastoven.net/prod_tcdeploy/deploy_06_201812.png)

1. 목록 위의 **생성** 버튼을 클릭합니다.
2. 아티팩트 유형을 **Server Application**으로 선택합니다.
    - 이름(필수), 설명(선택), port(필수) 항목을 입력합니다.
3. **아티팩트 생성** 창에서 **생성** 버튼을 클릭합니다.

### 서버 그룹 설정

배포할 서버를 관리할 수 있는 기능입니다.

![deploy_07_201812](https://static.toastoven.net/prod_tcdeploy/deploy_07_201812.png)

1. **Deploy** 화면 아래 탭에서 **서버 그룹 > 새로 만들기**를 클릭합니다.
2. **서버 그룹 생성** 창에서 새로 만들 서버 그룹을 설정합니다.
    * 이름(필수), 설명(선택)을 입력합니다.
    * OS를 선택하고 Shell Type을 지정합니다. Shell Type은 **Shell Type** 목록에서 선택하거나 직접 입력할 수 있습니다.
    * Phase를 선택합니다. 서버 장비를 구분합니다. 지정하지 않으려면 NONE을 선택합니다.
    * 서버 추가
        * 서버를 추가하는 방법은 아래 두 가지이며 자세한 내용은 [기능 상세 가이드 서버 그룹 메뉴](/Dev%20Tool/Deploy/ko/reference/#_11)에서 확인할 수 있습니다.
            * 대량 추가
            * 개별 추가
         * 호스트 이름(필수), IP 주소(필수), OS(선택)를 입력하고 **추가** 버튼을 클릭합니다.
         * 아래 서버 목록에 추가된 내용을 확인합니다. 왼쪽 체크 박스가 선택된 서버만 등록됩니다.

3. 입력을 완료하고 **생성** 버튼을 클릭합니다.

### 시나리오 생성

![deploy_08_201812](https://static.toastoven.net/prod_tcdeploy/deploy_08_201812.png)

1. **Deploy** 화면 아래 탭에서 **배포 > 새로 만들기** 버튼을 클릭합니다.
2. 아래 추가된 시나리오 영역에 시나리오 이름(선택)을 입력합니다.
3. **생성** 버튼을 클릭합니다.

### 태스크 추가

태스크는 개별 기능을 수행하고 순서를 제어할 수 있는 시나리오 구성 요소입니다.
태스크 종류는 아래 두 가지입니다.

* pre-run Task: 배포 전 실행 기능
* Normal Task: 배포 시 실행 기능

원하는 것을 선택해서 사용할 수 있습니다. 여기에서는 기본적인 배포 시 필요한 태스크를 다룹니다.
더 많은 태스크는 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/ko/reference/#_25)에서 확인할 수 있습니다.

배포 테스트를 위해 아래 세 개의 태스크를 추가합니다.

#### 1. User Command 추가

* 배포 시 실행되는 사용자 정의 Command 태스크입니다.
* Available Variables를 사용할 수 있습니다.
    * Available Variables: 예약어. 자세한 내용은 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/ko/reference/#_25)에서 확인하실 수 있습니다.

![deploy_09_201812](https://static.toastoven.net/prod_tcdeploy/deploy_09_201812.png)

1. **배포** 탭 시나리오 영역 오른쪽에서 **Task 추가** 버튼을 클릭합니다.
2. **Normal Task** 아래에 있는 **User Command**를 클릭합니다.
3. 새 태스크 내용을 입력합니다.
    * Timeout(min)
        * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.
    * Run As
        * 실행 계정을 입력합니다.
    * Command
        * 실행할 명령문을 입력합니다.

4. 입력이나 변경을 완료한 후 **적용** 버튼을 클릭합니다. 

#### 2. Binary Deploy 추가

업로드한 바이너리 파일의 배포 내용을 설정할 수 있는 태스크입니다.

![deploy_10_201812](https://static.toastoven.net/prod_tcdeploy/deploy_10_201812.png)

1. **Task 추가** 버튼을 클릭하고 **Normal Task** 아래에 있는 **Binary Deploy**를 클릭합니다.
2. 새 태스크 내용을 입력합니다.
    * Timeout(min)
        * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.
    * Run As
        * 실행 계정을 입력합니다.
3. 바이너리 파일을 업로드하려면 오른쪽 **업로드** 버튼을 클릭합니다.
4. 바이너리 파일 정보를 입력합니다.
    * **파일 선택** 버튼을 클릭하여 바이너리 파일을 선택합니다.
    * 버전(선택), 설명(선택) 항목을 입력합니다.
5. **업로드** 버튼을 클릭합니다. 
6. 업로드 완료 후 **바이너리 선택** 버튼을 클릭합니다.
7. 원하는 바이너리 버전을 선택합니다.
    * 여러 버전이 있을 때는 검색 기능을 활용합니다.
8. **선택** 버튼을 클릭합니다.
  <br/>
   * Variable As
       * 해당 바이너리의 Variable 이름을 지정해 User Command에서 바이너리 정보를 사용할 수 있으며 자세한 내용은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)의 태스크 메뉴 하단에서 확인하실 수 있습니다.
   * 타겟 디렉토리
       * 바이너리를 배포할 타겟 디렉토리를 지정합니다.

#### 3. User Command 추가

![deploy_11_201812](https://static.toastoven.net/prod_tcdeploy/deploy_11_201812.png)

1. **Task 추가** 버튼을 클릭하고 **Normal Task** 아래 **User Command**를 클릭합니다.
2. 새로운 태스크 내용을 입력합니다.
    * Timeout(min)
        * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.
    * Run As
        * 실행 계정을 입력합니다.
    * Command
        * 실행할 명령문을 입력합니다.
3. 입력이나 변경을 완료한 후 **적용** 버튼을 클릭합니다. 

### 실행

![deploy_12_201812](https://static.toastoven.net/prod_tcdeploy/deploy_12_201812.png)

1. 오른쪽 **실행** 버튼을 클릭해 배포를 요청합니다.
2. 배포 실행 정보를 입력합니다.
    * 배포 노트(선택) 및 인증 방법을 지정합니다.
    * **Password** 선택 시 비밀번호를 입력하거나 .pem 파일을 선택한 후 업로드합니다.
3. 입력을 완료한 후 **확인** 버튼을 클릭합니다.

![deploy_13_201812](https://static.toastoven.net/prod_tcdeploy/deploy_13_201812.png)

1. 배포 진행 상황을 확인할 수 있습니다.
2. 배포 완료를 확인합니다.
    * 각 태스크 정상 실행 여부는 exit code로 확인할 수 있습니다.
3. 상세 결과를 확인하려면 **결과 보기** 버튼을 클릭합니다.
4. **결과 보기** 창에서 배포 결과를 확인합니다.
    * 각 태스크 실행에 대한 자세한 내용(리턴값, exit code, 오류 내용 등)을 확인할 수 있습니다.

- - -

서버에 파일을 배포했습니다!
TOAST Deploy는 더 많은 기능을 지원하며, 자세한 사항은 [기능 상세 가이드](/Dev%20Tool/Deploy/ko/reference/)에서 확인하실 수 있습니다.
