## Dev Tools > Deploy > 기능 상세 가이드

이 문서에서는 다음과 같은 내용을 다룹니다.

* [메뉴 설명](/Dev%20Tools/Deploy/ko/reference-gov/#_1)
* [기능별 설명](/Dev%20Tools/Deploy/ko/reference-gov/#_18)

## 메뉴 설명

![deploy_ref_01_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_01_2018.png)

### 1. 아티팩트 메뉴 영역

배포를 관리하는 Deploy 구성의 기본 단위입니다.
생성된 아티팩트는 화면 위쪽에 목록 형태로 표시됩니다.

### 2. 배포 메뉴 영역

배포를 진행하는 페이지로 시나리오 생성과 설정을 할 수 있습니다.
시나리오에는 Jenkins-CLI 템플릿, 바이너리 배포, 파일 배포가 있으며, 사용자가 원하는 스크립트를 실행할 수 있도록 직접 명령어를 입력할 수 있습니다.

* 배포 시나리오 관리
    * 복사
    * 설정
    * 새로 만들기
    * 업로드/다운로드
        * 시나리오 단위의 가져오기/내보내기
* 서버 그룹과 시나리오를 선택해 배포
* 서버 그룹 내 서버 동시/개별 배포
* 배포 노트에 배포 상세 내용을 입력하면, **배포 이력** 탭의 배포별 결과 보기 창에서 확인할 수 있습니다. 

#### 배포 옵션

##### 서버 그룹

* 배포할 서버 그룹을 선택할 수 있습니다. 서버 그룹은 **서버 그룹** 탭에서 생성할 수 있습니다.

##### 실행 서버

* **모든 서버**
    * 지정된 서버 그룹의 모든 서버로 배포합니다.
* **서버 선택**
    * 지정된 서버 그룹의 일부 서버를 선택해 배포합니다.

##### 동시 실행 수

지정된 서버 그룹 내 서버들에 동시 또는 개별 배포합니다.

* 0: 동시 실행
* 1: 한대씩 배포
* N: 동시 배포 수 지정

##### 시나리오 실행 실패 시

* **실행 중단**
    * 시나리오 실행 중 오류가 발생하면 시나리오 실행이 중단됩니다.
* **무중단**
    * 오류 여부와 관계없이 시나리오가 계속 실행됩니다.

#### 진행 중인 배포 상태 보기 

배포 중인 시나리오의 진행 상황을 확인할 수 있습니다.

![deploy_ref_02_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_02_2018.png)

* 배포 중인 아티팩트를 클릭하면 나타나는 헤더에서 확인하고자 하는 배포 항목을 클릭합니다.
* 배포 이력 화면에서 'deploying' 상태를 클릭합니다.

### 배포 이력
배포 이력과 배포 설정, 배포 노트의 자세한 내용을 확인할 수 있습니다.
![deploy_ref_01_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_01_2021.png)
![deploy_ref_02_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_02_2021.png)
* 배포 이력과 이력별 자세한 내용 확인
    * 배포 결과
    * **결과 보기**에서 이력별 시나리오 설정 내용과 태스크별 실행 결과, 배포 노트를 확인할 수 있습니다.
    * 'deploying' 상태일 때 클릭하면 배포 중인 상태 보기 화면으로 이동합니다.

배포 이력을 실행일 및 서버 그룹을 기준으로 검색할 수 있습니다.
![deploy_ref_03_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_03_2021.png)
![deploy_ref_04_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_04_2021.png)
* 배포 이력을 실행일 및 서버 그룹을 기준으로 검색
    * 서버 그룹 선택 창 및 실행일 시작일~종료일로 검색할 수 있습니다.
    * 실행일 기간이 1년을 초과하여 선택할 수 없습니다(예: 2020-06-07~2021-06-17).
     ![deploy_ref_08_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_08_2021.png)

검색된 배포 이력을 Excel 파일로 다운로드할 수 있습니다.
![deploy_ref_05_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_05_2021.png)
* 검색된 배포 이력을 Excel 파일로 다운로드
    * 원하는 서버 그룹 및 실행일을 선택한 후 **다운로드** 버튼을 클릭합니다.
    * **바이너리 파일 없는 이력 제외** 옵션을 선택하여 다운로드할 수 있습니다.
    * **바이너리 파일 없는 이력 제외** 선택 없이 **다운로드** 클릭 시
    ![deploy_ref_07_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_07_2021.png)
    * **바이너리 파일 없는 이력 제외** 선택 후 **다운로드** 클릭 시
    ![deploy_ref_06_2021.png](https://static.toastoven.net/prod_tcdeploy/reference/deploy_ref_06_2021.png)
    
### 바이너리 그룹

**바이너리 그룹** 탭에서는 바이너리를 그룹으로 관리할 수 있습니다.
Develop, Staging, Product 등의 서버 장비에 배포되는 바이너리를 구분할 때 활용할 수 있습니다.

![deploy_ref_04_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_04_2018.png)

* 그룹을 생성, 수정하고, 바이너리 파일을 업로드, 다운로드할 수 있습니다.

![deploy_ref_05_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_05_2018.png)

* Client 타입일 때는 바이너리 그룹의 비밀번호를 설정할수 있으며, 공유된 클라이언트 다운로드 페이지의 접근을 제어할 수 있습니다.
    * 바이너리 그룹을 생성할 때 **바이너리 그룹 비밀번호 사용**을 선택하면 비밀번호를 입력할 수 있습니다.
    * 해당 그룹 다운로드 페이지는 NHN Cloud에 로그인하지 않고도 설정된 비밀번호로만 접근할 수 있습니다.
* **Auto Remove Policy** 버튼을 클릭하여 해당 바이너리 그룹의 자동 삭제 정책을 설정할 수 있습니다.

![deploy_ref_06_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_06_2018.png)

### 서버 그룹

배포 대상 서버를 그룹으로 관리할 수 있습니다.
Phase 속성으로 Develop, Staging, Product 등의 서버 장비를 구분하여 활용할 수 있습니다. 
![deploy_ref_07_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_07_2018.png)

* 그룹, 서버 정보를 수정할 수 있습니다.
* 그룹에서 사용할 시나리오를 설정할 수 있습니다.

#### 서버 그룹 추가

![deploy_ref_08_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_08_2018.png)

* **Deploy** 아래의 탭 중 **배포** > **서버 그룹 생성**을 클릭하거나, **서버 그룹** > **새로 만들기**를 클릭합니다.
    * 이름(필수), 설명(선택) 항목을 입력합니다.
    * OS를 선택한 후 Shell Type를 지정합니다. 목록에서 항목을 선택하거나 직접 입력할 수 있습니다.
    * Phase를 선택합니다. 서버 장비를 구분합니다. 지정하지 않으려면 NONE을 선택합니다.
    * **생성** 버튼을 클릭합니다.

#### 서버 정보 추가/삭제

**서버 그룹 생성(수정)** 창에서 서버 정보를 추가하거나 삭제할 수 있습니다.

##### 서버 정보 추가

1. 개별 추가

![deploy_ref_08_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_08_2018.png)

* 호스트 이름(필수), IP 주소(필수), OS(선택) 입력 후 **추가** 버튼을 클릭합니다.
* 아래 서버 목록에 추가된 내용을 확인합니다. 왼쪽 체크 박스에 선택된 서버만 등록됩니다.
* **생성** 버튼을 클릭합니다. 수정할 때는 **수정** 버튼을 클릭합니다.

2. 대량 추가

![deploy_ref_09_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_09_2018.png)

* **대량 입력** 체크 박스를 선택합니다.
* 아래와 같은 형식으로 서버 정보를 입력합니다.
    ```
    test.host.name1;1.1.1.1;CentOS6.8;
    test.host.name2;2.2.2.2;;
    ```

* **추가** 버튼을 클릭합니다.
* 서버 정보가 2개 추가된 것을 확인합니다.  왼쪽 체크 박스를 선택한 서버만 등록됩니다.
* **생성** 버튼을 클릭합니다. 수정할 때는 **수정** 버튼을 클릭합니다.

##### 서버 정보 삭제

* 왼쪽 체크 박스를 선택 해제하거나 오른쪽 **삭제** 버튼을 클릭합니다.
* **생성** 버튼을 클릭합니다. 수정할 때는 **수정** 버튼을 클릭합니다.

### 리소스

리소스를 관리할 수 있는 페이지로, 파일 생성, 업로드, 다운로드, 수정을 할 수 있으며 변경 이력을 확인할 수 있습니다.

![deploy_ref_10_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_10_2018.png)

## 기능별 설명

여기에서는 Getting Started에서 다루지 않은 기능과 추가 설정을 자세히 설명합니다.

### 바이너리

바이너리는 업로드된 배포 대상 파일입니다.

#### 업로드

바이너리를 업로드할 수 있는 방법은 두 가지입니다.

* API 업로드
    * API 업로드에 대한 자세한 설명은 [API 가이드의 Binary Upload API](/Dev%20Tools/Deploy/ko/api-guide-gov/#binary-upload-api)에서 확인하실 수 있습니다.
* 콘솔에서 업로드

![deploy_ref_11_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_11_2018.png)

1. **Deploy** 아래 탭 중 **바이너리 그룹**을 클릭하고 **업로드** 버튼을 클릭합니다.
2. **파일 선택** 버튼을 클릭하고 바이너리 파일을 선택합니다.
3. 버전(선택), 설명(선택) 정보를 입력합니다.
4. **업로드** 버튼을 클릭합니다.

![deploy_ref_12_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_12_2018.png)

#### 1. 다운로드

바이너리 목록 오른쪽의 다운로드 버튼을 클릭합니다.

#### 2. 버전 픽스

Client OS별 1개씩 버전 픽스를 할 수 있습니다.

* 상태 구분
    * <span style="color:red">FIXED</span>(on)
        * 클릭 시 해당 버전을 unfix합니다.
    * <span style="color:gray">FIXED</span>(off)
        * 클릭 시 해당 버전을 fix합니다.

#### 3. 배포

##### 버전별 배포

Client 바이너리의 All, Fixed, Recent 버전을 원하는 방식으로 배포할 수 있습니다.

* 바이너리 다운로드 링크 제공
* 다운로드 링크 알림 기능 제공
    * 해당 프로젝트에 등록된 멤버에 SMS, E-Mail 전송

* 제공 타입
    * All Version List
        * 모든 버전을 다운로드할 수 있는 페이지 제공
    * Fixed Version
        * iOS, Android, etc
            * Fixed 버전 다운로드 페이지 제공(Fixed 버전이 있을 때)
    * Recent Version
        * iOS, Android, etc
            * 최신 버전 다운로드 페이지 제공
* 제공 방법
    * SMS, E-Mail

##### 선택 배포

특정 바이너리 다운로드 페이지를 SMS나 E-Mail로 전달할 수 있습니다.

![deploy_ref_13_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_13_2018.png)

1. **Deploy** 아래 **바이너리 그룹** 탭에서 배포할 바이너리 파일을 선택합니다.
2. 오른쪽 **전송** 버튼을 클릭합니다.
3. **다운로드 경로 전송** 창에서 전송 유형과 수신자를 선택합니다
    * SMS 또는 E-mail 중 하나를 선택할 수도 있고 모두 선택할 수도 있습니다.
4. **전송** 버튼을 클릭합니다.

지정한 전송 유형으로 수신자에게 바이너리 다운로드 페이지가 전달됩니다.

### 태스크

태스크는 개별 기능 수행 및 순서 제어가 가능한 시나리오 구성 요소 입니다.

* Pre-run Task : 배포 전 처리 작업이 가능한 태스크
    * 대상: 서버 그룹에 저장하지 않은 별도의 서버 지정 가능
* Normal Task: 배포 과정 중 실행되는 태스크
    * 대상: 서버 그룹 또는 서버 그룹 내 지정 서버

##### 태스크 이용 가이드

* 태스크 유형

![deploy_ref_14_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_14_2018.png)

1\. Pre-run Task
2\. Normal Task

* 태스크 순서
    * Pre-run 태스크는 영역 위쪽에 있습니다.
    * Pre-run 태스크 또는 Normal 태스크 각 영역 내에서는 위치를 자유롭게 옮길 수 있습니다.

3\. 태스크 제어

* 각 태스크 오른쪽 위에 있는 **∧** (위로 이동) **∨** (아래로 이동) **X** (Task 제거) 버튼으로 태스크를 제어할 수 있습니다.
* 예약어 사용
    * 자세한 내용은 아래 Available Variables를 참고하시기 바랍니다.

#### Pre-run Task

* Target Server: 별도로 Target Server를 지정한 일회성 처리가 가능합니다.

##### Jenkins-CLI Build

* ver. 2.46 이전/ver. 2.46 이후 버전으로 구분됩니다.
* Jenkins 빌드 설정에 대한 자세한 설명은 [플러그인 사용 가이드](/Dev%20Tools/Deploy/ko/plugin-guide/)를 참고하시기 바랍니다.

![deploy_ref_15_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_15_2018.png)

**[ 사용자 입력 내용 ]**

* OS 선택
* Run As 입력
    * 실행 계정을 입력합니다.
* Jenkins Server
    * 도메인 또는 IP를 입력합니다.
* Server Charset
    * Jenkins-CLI를 수행할 서버의 문자집합을 입력합니다. 아무값도 입력하지 않으면 기본값은 UTF-8입니다.
* Jenkins Url
    * 빌드 명령을 수행할 Jenkins 서버 주소를 입력합니다.
* Jenkins Charset
    * Jenkins 서버의 문자집합을 입력합니다. 아무값도 입력하지 않으면 기본값은 UTF-8입니다.
* Java Home
    * Jenkins-CLI를 수행하려면 JDK 또는 JRE의 Home 경로를 입력합니다.
* Job Name
    * 수행할 빌드 Job의 이름을 입력합니다.
* Private Key Path
    * SSH Public Key가 설정되어 있는 Jenkins 유저로 인증하기 위해 SSH Private Key 파일의 저장 경로를 입력합니다.
* User(ver. 2.46 이상 버전 필숫값)
	* Public Key가 설정된 ID를 입력합니다.
		* 참고 사항
			* 2.46 이전 버전
				* Public Key가 설정된 경우 Jenkins에서 자동 인증이 진행됩니다.
			* 2.46 이상 버전
				* User(Jenkins admin ID)가 추가되었으며, 해당 ID에 Public Key가 설정된 경우만 인증이 통과됩니다.
* Build Parameter(선택)
    * 빌드에 필요한 파라미터를 입력합니다.

##### User Command

* 사용자 정의 Command 태스크입니다.
* Available Variables(예약어)를 사용할 수 있습니다.
* 오류 발생 감지 및 오류 발생 시 배포가 중지됩니다.
    * **배포** 탭의 **시나리오 실행 실패 시**에서 **실행 중단**이 설정되어 있을 때

![deploy_ref_16_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_16_2018.png)

**[ 사용자 입력 내용 ]**

* OS 선택
* Run As 입력
    * 실행 계정을 입력합니다.
* Target Server
    * target server 정보를 입력합니다. 서버 그룹에 포함되거나 포함되지 않은 서버 정보도 입력 가능합니다.
* Timeout(min)
    * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.
* Command
    * 실행할 Command를 입력합니다.
    * 예약어를 사용할 수 있습니다. 아래 Available Variables 메뉴를 참고하세요.

#### Normal Task

* Target Server: 별도로 Target Server를 입력하지 않고, 서버 그룹에서 선택된 서버 정보를 사용합니다.

##### Binary Deploy

* **바이너리 그룹** 탭에서 관리하는 파일을 배포할 수 있습니다.

![deploy_ref_17_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_17_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.

* Run As 입력
    * 실행 계정을 입력합니다.

* 바이너리
    * 배포할 바이너리 파일을 세 가지 타입으로 선택할 수 있습니다.
        * 최신 버전 사용
            * 최신 버전 사용을 선택하면 최신 버전이 자동으로 선택됩니다.(최신 버전 파일이 있을 경우)
            * 바이너리 그룹을 선택할 수 있습니다.(선택된 그룹의 최신 버전이 사용됨)
        * 선택 버전 사용
            * ![deploy_ref_18_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_18_2018.png)
                1. **최신 버전 사용**  체크 박스를 선택 해제합니다.
                2.  **업로드** 버튼을 클릭합니다.
                3.  **파일 선택** 버튼을 클릭하여 바이너리 파일을 선택합니다.
                4.  입력 완료 후 **업로드** 버튼을 클릭합니다.
                5.  업로드 완료 후 **바이너리 선택** 버튼을 클릭합니다.
                6.  배포할 바이너리 버전을 검색해 선택합니다.
                7.  **선택** 버튼을 클릭합니다.

* Variable As
    * 해당 바이너리의 Variable 이름을 지정합니다. Command에서 Available Variables를 사용할 수 있습니다.

* 타겟 디렉터리
    * 바이너리를 배포할 타겟 디렉터리를 지정합니다.

##### File Deploy

* **Resource** 탭에서 관리하는 파일을 최신 버전으로 배포할 수 있습니다.

![deploy_ref_19_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_19_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.

* RunAs 입력
    * 실행 계정을 입력합니다.

*  파일
    * **파일 선택** 버튼을 클릭하여 파일을 선택합니다.
    
* 타겟 디렉토리
    * 파일을 배포할 타겟 디렉토리를 지정합니다.

##### User Command

* 사용자 정의 Command 태스크입니다.
* Available Variables(예약어)를 사용할 수 있습니다.
* 스크립트 오류 발생 감지 및 오류 발생 시 배포가 중지됩니다.
    * 탭 메뉴 중 **배포 > 시나리오 실행 실패 시 실행 중단**이 설정되어 있을 때

![deploy_ref_20_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_20_2018.png)

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크의 실행 완료 대기 시간을 지정합니다. 최소 1분, 최대 30분.
* Run As 입력
    * 실행 계정을 입력합니다.
* 실행할 Command를 입력합니다.
    * Available Variables를 사용할 수 있습니다.
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
$${executor.maskedAccount} : 실행 유저의 마스킹된 아이디
$${timestamp} : 타임 스탬프
$${timestamp.date pattern} : 지정한 패턴의 타임 스탬프

* Binary Deploy의 Variables As을 사용할 수 있는 타입
$${binary.binary variable as value.version} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 버전
$${binary.binary variable as value.key} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 키
$${binary.binary variable as value.name} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 이름
$${binary.binary variable as value.targetDir} : 바이너리에 설정한 변수 이름으로 선택된 바이너리의 대상 경로
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
* 파일 히스토리
* 파일 이력 관리

#### 파일 그룹 생성

![deploy_ref_21_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_21_2018.png)

1. **새로운 파일 그룹** 버튼을 클릭합니다.

2. 이름(필수), 설명(선택) 정보를 입력합니다.
3. **확인** 버튼을 클릭합니다.

#### 파일 추가

##### 파일 업로드

![deploy_ref_22_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_22_2018.png)

1. **파일 추가** 버튼을 클릭합니다.
2. **파일 업로드** 생성 방식을 선택합니다.
3. 파일 선택 및 입력 완료 후 **업로드** 버튼을 클릭합니다.

파일 그룹 내 파일 이름이 중복되면 업로드가 제한됩니다.

##### 신규 생성

![deploy_ref_23_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_23_2018.png)

1. **파일 추가** 버튼을 클릭합니다.
2. **신규 생성** 생성 방식을 선택합니다.
3. **확인** 버튼을 클릭합니다.
4. 신규 생성 파일 내용을 입력합니다.
5. 입력 완료 후 **저장** 버튼을 클릭합니다.
6. 생성이 완료된 모습입니다.

#### 파일 수정

* 파일 설명 수정
* 파일 내용 수정
* 파일 히스토리로 수정
* 파일 히스토리 관리

##### 파일 설명 수정

파일 설명을 수정할 수 있습니다.

![deploy_ref_24_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_24_2018.png)

1. 파일 설명란을 클릭하여 수정합니다.
2. **설명 수정** 버튼을 클릭합니다.

##### 파일 내용 수정

![deploy_ref_25_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_25_2018.png)

1. 파일 내용을 수정합니다.
2. **저장** 버튼을 클릭합니다.

##### 파일 히스토리

파일 생성, 수정 히스토리를 확인할 수 있습니다.

![deploy_ref_26_2018.png](https://static.toastoven.net/prod_tcdeploy/deploy_ref_26_2018.png)

1. **File History** 버튼을 클릭하여 목록을 확장합니다.
    * 목록 확인
        * 첫 번째 내역: 현재 버전을 의미합니다.
        * 이후 내역: 이전 버전으로, 클릭하면 상세 창에 내용이 표시됩니다.

2. 확인하고자 하는 항목을 클릭하면 해당 버전의 자세한 내용을 확인할 수 있습니다.

##### 파일 히스토리 관리

파일 히스토리 상세 내역에서 아래 기능을 제공합니다.

* **최신 버전으로 저장** 버튼을 클릭하면 해당 버전이 최신 버전으로 저장됩니다.
