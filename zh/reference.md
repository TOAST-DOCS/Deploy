## Dev Tool > Deploy > Detail Guide

이 문서에서는 다음과 같은 내용을 다룹니다.

* [메뉴 설명](/Dev%20Tool/Deploy/ko/reference/#_1)
* [기능 별 설명](/Dev%20Tool/Deploy/ko/reference/#_15)

## 메뉴 설명

![deploy_ref_01_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_01_2018.png)

### 1. 아티팩트

배포를 관리하는 Deploy 구성의 기본 단위 입니다.
생성된 아티팩트는 화면 상단에 리스트 형태로 노출됩니다.

### 2. 배포

배포를 진행하는 페이지로 시나리오 생성 및 설정을 할 수 있습니다.
시나리오에는 Jenkins-cli 템플릿, 바이너리 배포, 파일 배포가 마련되어 있으며, 사용자가 원하는 스크립트를 실행할 수 있도록 직접 커맨드를 입력할 수 있습니다.

* 배포 시나리오 관리
    * 복사
    * 설정
    * 새로 만들기
    * 올리기/ 내려받기
        * 시나리오 단위의 Import/Export
* 서버 그룹과 시나리오를 선택하여 배포하기
* 서버 그룹 내 서버 동시/개별 배포하기
* 배포 노트에 배포에 대한 상세 내용 입력 시, 배포 이력 탭의 배포 별 결과보기 팝업에서 확인 가능합니다. 

#### 배포 옵션

##### 서버 그룹 지정

* 배포할 서버 그룹을 선택할 수 있습니다.
    * 서버 그룹의 생성 : 탭 메뉴 중 [서버 그룹]에서 가능합니다.

##### 실행 서버 지정

* 모든 서버
    * 지정된 서버 그룹의 모든 서버로 배포합니다.
* 서버 선택
    * 지정된 서버 그룹의 일부 서버를 선택하여 배포합니다.

##### 동시 실행 수

지정된 서버 그룹 내 서버들에 동시 또는 개별 배포합니다.

* 0 : 동시 실행
* 1 : 한대씩 배포
* N : 동시 배포 수 지정

##### 시나리오 실행 실패 시

* 실행 중단
    * 시나리오 실행 중 오류 발생 시 시나리오 실행이 중단됩니다.
* 무중단
    * 오류 여부와 관계 없이 시나리오가 계속 실행됩니다.

#### 진행 중인 배포 상태 보기 

* 배포 진행 중 시나리오의 진행상황을 확인 가능합니다.

![deploy_ref_02_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_02_2018.png)

* 배포 진행 중 Header(배포중인 Artifact 클릭시 나타남) 에서 해당하는 배포 건을 클릭합니다.
* 배포 이력 화면내 'Deploying' 상태를 클릭합니다.

### 배포 이력

배포 이력 및 배포 설정, 배포 노트에 대한 상세 내용을 확인할 수 있습니다.

![deploy_ref_03_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_03_2018.png)

* 배포 이력 및 이력 별 상세 내역 확인
    * 배포 결과
    * [결과 보기]로 이력 별 시나리오 설정 내용 및 태스크 별 실행 결과, 배포 노트 확인이 가능합니다.
    * 'Deploying' 상태인 경우 클릭시 배포 중인 상태 보기 화면으로 이동합니다.
    
### 바이너리 그룹

바이너리를 그룹으로 관리할 수 있습니다.
(Develop, Staging, Product 등의 서버 장비에 배포되는 바이너리를 구분하기 위해서 활용할 수 있습니다.)

![deploy_ref_04_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_04_2018.png)

* 그룹을 생성/수정하고, 바이너리 파일을 업로드/다운로드할 수 있습니다.

![deploy_ref_05_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_05_2018.png)

* Client 타입의 경우 바이너리 그룹의 비밀번호를 설정할수 있으며, 공유된 클라이언트 다운로드 페이지 접근제어를 할 수 있습니다.
    * 바이너리 그룹 생성시 [바이너리 그룹 비밀번호 사용] 체크를 하면 [비밀번호]를 입력 할 수 있습니다.
    * 해당 그룹 다운로드 페이지는 토스트 로그인 없이 설정된 비밀번호만으로 접근이 가능합니다.
* **Auto Remove Policy** 버튼을 클릭하여 해당 바이너리 그룹의 자동 삭제 정책을 설정할 수 있습니다.

![deploy_ref_06_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_06_2018.png)

### 서버 그룹

배포 대상 서버를 그룹으로 관리할 수 있습니다.
(Phase 속성으로 Develop, Staging, Product 등의 서버 장비를 구분하여 활용할 수 있습니다.)

![deploy_ref_07_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_07_2018.png)

* 그룹/서버 정보를 수정할 수 있습니다.
* 그룹에서 사용할 시나리오를 설정할 수 있습니다.

#### 서버 그룹 추가

![deploy_ref_08_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_08_2018.png)

* [Deploy] > 하단 탭 중 **배포** > **서버 그룹 생성** 클릭 또는, [Deploy] > 하단 탭 중 **서버 그룹** > **새로 만들기** 클릭
    * 이름(필수), 설명(선택) 항목을 입력합니다.
    * OS 선택 후 Shell Type를 지정합니다. (항목 선택 또는 직접 입력)
    * Phase를 선택합니다. (서버 장비 구분. 지정하지 않을 경우 NONE 선택)
    * **생성** 버튼을 클릭합니다.

#### 서버 정보 추가/삭제

서버 그룹 생성/수정 팝업에서 서버 정보를 추가/삭제할 수 있습니다.

##### 서버 정보 추가

1. 개별 추가

![deploy_ref_08_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_08_2018.png)

* 호스트 이름(필수), IP 주소(필수), OS(선택) 입력 후 **추가** 버튼을 클릭합니다.
* 하단 서버 리스트에 추가된 내용을 확인합니다. (왼쪽 체크 박스에 체크된 서버만 등록됩니다)
* **생성** 버튼을 클릭합니다. (수정 시 **수정** 버튼을 클릭합니다)

2. 대량 추가

![deploy_ref_09_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_09_2018.png)

* [대량입력] 체크박스를 체크합니다.
* 아래 형태로 서버 정보를 입력합니다.
    ```
    test.host.name1;1.1.1.1;CentOS6.8;
    test.host.name2;2.2.2.2;;
    ```

* **추가** 버튼을 클릭합니다.
* 2개의 서버 정보가 추가된 내용을 확인합니다. (왼쪽 체크 박스에 체크된 서버만 등록됩니다.)
* **생성** 버튼을 클릭합니다. (수정 시 **수정** 버튼을 클릭합니다)

##### 서버정보 삭제

* 왼쪽 체크 박스를 해제하거나 또는 오른쪽 **삭제** 버튼을 클릭합니다.
* **생성** 버튼을 클릭합니다. (수정 시 **수정** 버튼을 클릭합니다)

### 리소스

리소스를 관리할 수 있는 페이지로 파일 생성 및 업로드, 다운로드, 수정을 할 수 있으며 변경이력을 확인할 수 있습니다.

![deploy_ref_10_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_10_2018.png)

## 기능 별 설명

이 영역에서는 Getting Started에서 다루지 않은 기능의 상세 설명 및 추가 설정을 제공합니다.

### 바이너리

바이너리는 사용자로부터 업로드 된 배포 대상 파일 입니다.

#### 업로드

바이너리 업로드를 위한 두 가지 방법을 제공합니다.

* API 업로드
    * API 업로드에 대한 상세 내역은 [API 가이드의 Binary Upload API](/Dev%20Tool/Deploy/ko/api-guide/#binary-upload-api) 에서 확인하실 수 있습니다.
* UI 업로드

![deploy_ref_11_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_11_2018.png)

1. [Deploy] > 하단 탭 중 **바이너리 그룹** > **업로드** 버튼을 클릭합니다.
2. **파일선택** 버튼 클릭 후 바이너리 파일을 선택합니다.
3. 버전(선택), 설명(선택) 정보를 입력합니다.
4. **업로드** 버튼을 클릭합니다.

![deploy_ref_12_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_12_2018.png)

#### 1. 다운로드

바이너리 리스트 오른쪽 다운로드 버튼을 클릭합니다.

#### 2. 버전 픽스

Client OS별 1개씩 버전 픽스를 할 수 있습니다.

* 상태 구분
    * <span style="color:red">FIXED</span> (on)
        * 클릭 시 해당 버전을 unfix 합니다.
    * <span style="color:gray">FIXED</span> (off)
        * 클릭 시 해당 버전을 fix 합니다.

#### 3. 배포

##### 버전별 배포

Client 바이너리의 All / Fixed / Recent 버전을 원하는 방식으로 배포할 수 있습니다.

* 바이너리 다운로드 링크 제공
* 다운로드 링크 알림 기능 제공
    * 해당 프로젝트에 등록된 멤버에 SMS / E-Mail 전송

* 제공 타입
    * all Version List
        * 모든 버전 다운로드 가능한 페이지 제공
    * Fixed Version
        * iOS / Android / etc
            * fixed 버전 다운로드 페이지 제공 (fixed 버전이 있을 때)
    * Recent Version
        * iOS / Android / etc
            * 최신 버전 다운로드 페이지 제공
* 제공 방법
    * SMS/ E-Mail

##### 선택 배포

특정 바이너리 다운로드 페이지를 SMS / E-Mail로 전달할 수 있습니다.

![deploy_ref_13_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_13_2018.png)

1. [Deploy] > 하단 탭 중 [바이너리 그룹] > 배포 할 바이너리 파일을 선택합니다.
2. 우측 **전송** 버튼을 클릭합니다.
3. 다운로드 링크 전송 팝업에서 전송 유형과 수신자를 선택합니다
    * SMS / E-mail로 일부 또는 모두 지정 가능
4. **전송** 버튼을 클릭합니다.
* 지정한 전송 유형으로 수신자에게 바이너리 설치를 위한 내용이 전달됩니다.

### 태스크

태스크는 개별 기능 수행 및 순서 제어가 가능한 시나리오 구성 요소 입니다.

* Pre-run Task : 배포 전 처리 작업이 가능한 태스크
    * 대상 : 지정한 타겟 서버
        * 타겟 서버를 지정한 일회성 처리 가능
* Normal Task : 배포 과정 중 실행되는 태스크
    * 대상 : 서버 그룹 또는 서버 그룹 내 지정 서버

##### 태스크 이용 가이드

* 태스크 유형

![deploy_ref_14_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_14_2018.png)

1\. Pre-run Task form color : **<span style="color:#0052cc">blue</span>**
2\. Normal Task form color : <span style="color:#777777">**grey**</span>

* 태스크 순서
    * Pre-run 태스크는 영역 상단에 자리합니다.
    * Pre-run 태스크 / Normal 태스크 각 영역 내에서는 위치 이동이 자유롭습니다.

3\. 태스크 제어

* 각 태스크 우측 상단에 위치한 **∧** (위로 이동) **∨** (아래로 이동) **X** (Task 제거) 버튼으로 태스크를 제어 할 수 있습니다.
* 예약어 사용
    * 자세한 내용은 하단 Available Variables에서 확인하실 수 있습니다.

#### Pre-run Task

* Target Server : 별도로 Target Server를 지정한 일회성 처리가 가능합니다.

##### Jenkins-Cli Build

* ver. 2.46 이전 / ver. 2.46 이후 버전으로 구분됩니다.
* Jenkins 빌드 설정에 대한 상세 내역은 [플러그인 사용 가이드](/Dev%20Tool/Deploy/ko/plugin-guide/)에서 확인하실 수 있습니다.

![deploy_ref_15_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_15_2018.png)

**[ 사용자 입력 내용 ]**

* OS 선택
* Run As 입력
    * 실행 계정을 입력 합니다.
* Jenkins Server
    * 도메인 또는 IP를 입력 합니다.
* Server Charset
    * jenkins cli를 수행시킬 서버의 문자집합을 입력합니다. (아무값도 입력하지 않을 경우 기본값 utf-8)
* Jenkins Url
    * 빌드명령을 수행하고자 하는 Jenkins 서버의 주소를 입력합니다.
* Jenkins Charset
    * jenkins 서버의 문자집합을 입력합니다. (아무값도 입력하지 않을 경우 기본값 utf-8)
* Java Home
    * jenkins-cli를 수행하기 위한 JDK 또는 JRE의 Home 경로를 입력합니다.
* Job Name
    * 수행시킬 빌드 Job의 이름을 입력합니다.
* Private Key Path
    * SSH Public Key가 설정되어있는 Jenkins 유저로 인증을 위한 SSH Private Key 파일의 저장 경로를 입력합니다.
* User (ver. 2.46 이상 버전 필수 값)
	* Public Key가 설정된 Id를 입력합니다.
		* 참고 사항
			* 2.46 이전 버전
				* Public Key가 설정된 경우 Jenkins에서 자동 인증이 진행됩니다.
			* 2.46 이상 버전
				* User(Jenkins admin Id)가 추가 되었으며, 해당 Id에 Public Key가 설정된 경우만 인증이 통과됩니다.
* Build Parameter (선택)
    * 빌드에 필요한 파라미터를 입력 합니다.

##### User Command

* 사용자 정의 Command 태스크입니다.
* Available Variables(예약어) 사용이 가능합니다.
* 오류 발생 감지 및 오류 발생 시 배포가 중지됩니다.
    * 탭 메뉴 중 [배포] > 시나리오 실행 실패 시 [실행 중단] 설정이 되어있을 때

![deploy_ref_16_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_16_2018.png)

**[ 사용자 입력 내용 ]**

* OS 선택
* Run As 입력
    * 실행 계정을 입력 합니다.
* Target Server
    * 타켓 서버 정보를 입력 합니다. 서버 그룹에 포함되거나 포함되지 않은 서버 정보도 입력 가능합니다.
* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* Command
    * 실행할 Command를 입력 합니다.
    * 예약어를 사용할 수 있습니다. 아래 Available Variables 메뉴를 참고하세요.

#### Normal Task

* Target Server : 별도로 Target Server를 입력하지 않고, 서버 그룹에서 선택된 서버 정보를 사용합니다.

##### Binary Deploy

* 메뉴 탭 중 [바이너리 그룹]에서 관리하는 파일들을 배포할 수 있습니다.

![deploy_ref_17_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_17_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)

* Run As 입력
    * 실행 계정을 입력 합니다.

* 바이너리
    * 배포할 바이너리 파일을 세가지 타입으로 선택할 수 있습니다.
        * 최신 버전 사용
            * 최신 버전 사용에 체크하면 최신 버전이 자동 선택 됩니다. (최신 버전 파일이 있을 경우)
            * 바이너리 그룹을 선택할 수 있습니다. (선택된 그룹의 최신 버전이 사용됨)
        * 선택 버전 사용
            * ![deploy_ref_18_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_18_2018.png)
                1. [최신 버전 사용] 체크박스를 해체합니다.
                2.  **업로드** 버튼을 클릭합니다.
                3.  **파일선택** 버튼을 클릭하여 바이너리 파일을 선택합니다.
                4.  입력 완료 후 **업로드** 버튼을 클릭합니다.
                5.  업로드 완료 후 **바이너리 선택** 버튼을 클릭합니다.
                6.  배포 할 바이너리 버전을 검색/선택합니다.
                7.  **선택** 버튼을 클릭합니다.

* Variable As
    * 해당 바이너리의 Variable명을 지정합니다. Command에서 Available Variables를 사용할 수 있습니다.

* 타겟 디렉토리
    * 바이너리를 배포할 디렉토리를 지정합니다.

##### File Deploy

* 메뉴 탭 중 [Resource]에서 관리하는 파일을 최신 버전으로 배포할 수 있습니다.

![deploy_ref_19_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_19_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)

* RunAs 입력
    * 실행 계정을 입력 합니다.

*  파일
    * **파일 선택** 버튼을 클릭하여 파일을 선택합니다.
    
* 타겟 디렉토리
    * 파일을 배포할 디렉토리를 지정합니다.

##### User Command

* 사용자 정의 Command 태스크입니다.
* Available Variables(예약어) 사용이 가능합니다.
* script 오류 발생 감지 및 오류 발생 시 배포가 중지됩니다.
    * 탭 메뉴 중 [배포] > 시나리오 실행 실패 시 [실행 중단] 설정이 되어있을 때

![deploy_ref_20_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_20_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* Run As 입력
    * 실행 계정을 입력 합니다.
* 실행할 Command를 입력 합니다.
    * Available Variables을 사용할 수 있습니다.
        * `$${binary.지정한 Variables As 입력.binaryGroupName} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 그룹 이름`

#### Available Variables

태스크에서 아래와 같은 예약어를 사용할 수 있습니다.

```
[Available Variables]

* 기본 타입
$${artifact.name} : 아티팩트 이름
$${artifact.id} : 아티팩트 아이디
$${serverGroup.name} : 서버 그룹 이름
$${serverGroup.id} : 서버 그룹 아이디
$${executor.account} : 실행 유저의 아이디
$${executor.maskedAccount} : 실행 유저의 마스킹 된 아이디
$${timestamp} : 타임 스탬프
$${timestamp.date pattern} : 지정한 패턴의 타임 스탬프

* Binary Deploy의 Variables As을 사용할 수 있는 타입
$${binary.binary variable as value.version} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 버전
$${binary.binary variable as value.key} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 키
$${binary.binary variable as value.name} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 이름
$${binary.binary variable as value.targetDir} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 타겟 경로
$${binary.binary variable as value.binaryGroupKey} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 그룹 키
$${binary.binary variable as value.binaryGroupName} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 그룹 이름
```

* 사용 예시
    * 기본 타입
        * `$${timestamp} : 1514987008`
    * Binary Deploy의 Variables As을 사용할 수 있는 타입
        * `$${binary.지정한 Variables As 입력.binaryGroupName} : 해당 바이너리의 그룹 이름`

### 리소스

리소스는 선택적으로 사용할 수 있는 파일 관리 기능입니다.

* 파일 그룹 생성
* 파일 추가
* 파일 수정
* 파일 History
* 파일 이 관리

#### 파일 그룹 생성

![deploy_ref_21_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_21_2018.png)

1.**새로운 파일 그룹** 버튼을 클릭합니다.
2. 이름(필수), 설명(선택) 정보를 입력합니다.
3. **업로드** 버튼을 클릭합니다.

#### 파일 추가

##### 파일 업로드

![deploy_ref_22_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_22_2018.png)

1. **파일 추가** 버튼을 클릭합니다.
2. [파일 업로드] 생성 방식을 선택합니다.
3. 파일 선택 및 입력 완료 후 **업로드** 버튼을 클릭합니다.
* 파일 그룹 내 파일명 중복 시 업로드가 제한됩니다

##### 신규 생성

![deploy_ref_23_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_23_2018.png)

1. **파일 추가** 버튼을 클릭합니다.
2. [신규 생성] 생성 방식을 선택합니다.
3. **확인** 버튼을 클릭합니다.
4. 신규 생성 파일 내용을 입력합니다.
5. 입력 완료 후 **저장** 버튼을 클릭합니다.
6. 생성이 완료된 모습입니다.

#### 파일 수정

* 파일 설명 수정
* 파일 내용 수정
* 파일 History로 수정
* 파일 이력 관리

##### 파일 설명 수정

파일에 대한 설명을 수정할 수 있습니다.

![deploy_ref_24_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_24_2018.png)

1. 파일 설명란을 클릭하여 수정합니다.
2. **설명 수정** 버튼을 클릭합니다.

##### 파일 내용 수정

![deploy_ref_25_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_25_2018.png)

1. 파일 내용을 수정합니다.
2. **저장** 버튼을 클릭합니다.

##### 파일 History

파일 생성/수정 History를 확인할 수 있습니다.

![deploy_ref_26_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_26_2018.png)

1. **File History** 버튼을 클릭하여 리스트를 확장합니다.
    * 목록 확인
        * 첫번째 내역 : 현재 버전을 의미합니다.
        * 이후 내역 : 이전 버전으로, 클릭 시 상세 팝업으로 내용이 표시됩니다.

2. 내역 클릭 시 해당 버전의 상세 내용을 확인 가능합니다.

##### 파일 이력 관리

파일 History 상세 내역에서 아래 기능을 제공합니다.

* **최신 버전으로 저장** 버튼을 클릭하면 해당 버전이 최신 버전으로 저장됩니다.
