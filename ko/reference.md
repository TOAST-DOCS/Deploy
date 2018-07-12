## Dev Tool > Deploy > 기능 상세 가이드

이 문서에서는 다음과 같은 내용을 다룹니다.

* [메뉴 설명](/Dev%20Tool/Deploy/ko/reference/#_1)
* [기능 별 설명](/Dev%20Tool/Deploy/ko/reference/#_15)
* [OS 별 요구사항](/Dev%20Tool/Deploy/ko/reference/#os)

## 메뉴 설명

### 아티팩트

배포를 관리하는 Deploy 구성의 기본 단위 입니다.
생성된 아티팩트는 화면 상단에 리스트 형태로 노출됩니다.

```
[Deploy] > 상단 리스트 영역
```

![[그림 1] 아티팩트 리스트](http://static.toastoven.net/prod_tcdeploy/reference/01.png)
<center>[그림 1] 아티팩트 리스트</center>

### 배포

배포를 진행하는 페이지로 시나리오 생성 및 설정을 할 수 있습니다.
시나리오에는 Jenkins-cli 템플릿, 바이너리 배포, 파일 배포가 마련되어 있으며, 사용자가 원하는 스크립트를 실행할 수 있도록 직접 커맨드를 입력할 수 있습니다.

![[그림 2] 탭 메뉴 - 배포](http://static.toastoven.net/prod_tcdeploy/reference/02.png)
<center>[그림 2] 탭 메뉴 - 배포</center>

* 배포 시나리오 관리
    * 복사
    * 설정
    * 새로 만들기
    * 올리기/ 내려받기
        * 시나리오 단위의 Import/Export
* 서버 그룹과 시나리오를 선택하여 배포
* 서버 그룹 내 서버 동시/개별 배포
* 배포 노트에 배포에 대한 상세 내용 입력 시, 배포 이력 탭의 배포 별 결과보기 팝업에서 확인 가능 

#### 배포 옵션

![[그림 3] 배포 옵션](http://static.toastoven.net/prod_tcdeploy/reference/reference_deploy_option.png)
<center>[그림 3] 배포 옵션</center>

##### 서버 그룹 지정

* 배포할 서버 그룹을 선택할 수 있음
    * 서버 그룹의 생성 : 탭 메뉴 중 [서버 그룹]에서 가능

##### 실행 서버 지정

* 모든 서버
    * 지정된 서버 그룹의 모든 서버에 배포
* 서버 선택
    * 지정된 서버 그룹의 일부 서버를 선택하여 배포

##### 동시 실행 수

지정된 서버 그룹 내 서버들에 동시 또는 개별 배포

* 0 : 동시 실행
* 1 : 한대씩 배포
* N : 동시 배포 수 지정

##### 시나리오 실행 실패 시

* 실행 중단
    * 시나리오 실행 중 오류 발생 시 시나리오 실행이 중단됨
* 무중단
    * 오류 여부와 관계 없이 시나리오 실행

### 배포 이력

배포 이력 및 배포 설정, 배포 노트에 대한 상세 내용을 확인할 수 있습니다.

![[그림 4] 탭 메뉴 - 배포 이력](http://static.toastoven.net/prod_tcdeploy/reference/04.png)
<center>[그림 4] 탭 메뉴 - 배포 이력</center>

![[그림 5] 배포 이력](http://static.toastoven.net/prod_tcdeploy/reference/05.png)
<center>[그림 5] 배포 이력</center>

* 배포 이력 및 이력 별 상세 내역 확인
    * 배포 결과
    * [결과 보기]로 이력 별 시나리오 설정 내용 및 태스크 별 실행 결과, 배포 노트 확인

### 바이너리 그룹

바이너리를 그룹으로 관리할 수 있습니다.
(Develop, Staging, Product 등의 서버 장비에 배포되는 바이너리를 구분하기 위해서 활용할 수 있습니다.)

![[그림 6] 탭 메뉴 - 바이너리 그룹](http://static.toastoven.net/prod_tcdeploy/reference/06.png)
<center>[그림 6] 탭 메뉴 - 바이너리 그룹</center>

* 그룹을 생성/수정하고, 바이너리 파일을 업로드/다운로드할 수 있습니다.

<img alt="binaryGroupPassword.png" src="http://static.toastoven.net/prod_tcdeploy/reference/06-1.png">

* Client 타입의 경우 바이너리 그룹의 비밀번호를 설정할수 있으며, 공유된 클라이언트 다운로드 페이지 접근제어를 할 수 있습니다.
    * 바이너리 그룹 생성시 [바이너리 그룹 비밀번호 사용] 체크를 하면 [비밀번호]를 입력 할 수 있습니다.
    * 해당 그룹 다운로드 페이지는 토스트 클라우드 로그인 없이 설정된 비밀번호만으로 접근이 가능합니다.
* <img class="img-inline" alt="autoremove-w.png" src="http://static.toastoven.net/prod_tcdeploy/btn/autoremove-w.png">를 클릭하여 해당 바이너리 그룹의 자동 삭제 정책을 설정할 수 있습니다.
    * ![[그림 7] 자동 삭제 설정 팝업](http://static.toastoven.net/prod_tcdeploy/reference/07.png)
      <center>[그림 7] 자동 삭제 설정 팝업</center>

### 서버 그룹

배포 대상 서버를 그룹으로 관리할 수 있습니다.
(Phase 속성으로 Develop, Staging, Product 등의 서버 장비를 구분하여 활용할 수 있습니다.)

![[그림 8] 탭 메뉴 - 서버 그룹](http://static.toastoven.net/prod_tcdeploy/reference/reference_servergroup_tab.png)
<center>[그림 8] 탭 메뉴 - 서버 그룹</center>

* 그룹/서버 정보를 수정할 수 있습니다.
* 그룹에서 사용할 시나리오를 설정할 수 있습니다.

#### 서버 그룹 추가

```
[Deploy] > 하단 탭 중 [배포] > [서버 그룹 생성] 클릭 또는,
[Deploy] > 하단 탭 중 [서버 그룹] > [새로 만들기] 클릭
```

![[그림 9] 서버 그룹 생성 팝업](http://static.toastoven.net/prod_tcdeploy/reference/reference_servergroup_create.png)
<center>[그림 9] 서버 그룹 생성 팝업</center>

1. <img class="img-inline" alt="servergroupcreate-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/servergroupcreate-g.png"> 클릭
2. 이름(필수), 설명(선택) 입력
3. OS 선택 후 Shell Type 지정 (항목 선택 또는 직접 입력)
4. Phase 선택 (서버 장비 구분. 지정하지 않을 경우 NONE 선택)
5. <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭

#### 서버 정보 추가/삭제

서버 그룹 생성/수정 팝업에서 서버 정보를 추가/삭제할 수 있습니다.

##### 서버 정보 추가

1. 개별 추가

    ![[그림 10] 서버 정보 개별 입력](http://static.toastoven.net/prod_tcdeploy/reference/10.png)
    <center>[그림 10] 서버 정보 개별 입력</center>

    * 호스트 이름(필수), IP 주소(필수), OS(선택) 입력 후 <img class="img-inline" alt="add-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/add-b.png"> 클릭

    ![[그림 11] 서버 정보 개별 입력 확인](http://static.toastoven.net/prod_tcdeploy/reference/11.png)
    <center>[그림 11] 서버 정보 개별 입력 확인</center>

    * 하단 서버 리스트에 추가된 내용 확인 (왼쪽 체크 박스에 체크된 서버만 등록됨)

    * 서버 그룹 생성/수정 팝업의 <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png">/<img class="img-inline" alt="edit-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/edit-b.png"> 클릭

2. 대량 추가
    * <img class="img-inline" alt="mtpinput.png" src="http://static.toastoven.net/prod_tcdeploy/btn/mtpinput.png">과 같이 체크
    * 아래 형태로 입력

    ![[그림 12] 서버 정보 대량 입력](http://static.toastoven.net/prod_tcdeploy/reference/12.png)
    <center>[그림 12] 서버 정보 대량 입력</center>

    * <img class="img-inline" alt="add-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/add-b.png"> 클릭

    ![[그림 13] 서버 정보 대량 입력 확인](http://static.toastoven.net/prod_tcdeploy/reference/13.png)
    <center>[그림 13] 서버 정보 대량 입력 확인</center>

    * 2개의 서버 정보가 추가된 내용 확인 (왼쪽 체크 박스에 체크된 서버만 등록됨)
    * 서버 그룹 생성/수정 팝업의 <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png">/<img class="img-inline" alt="edit-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/edit-b.png"> 클릭

##### 서버정보 삭제

![[그림 14] 서버 정보 삭제](http://static.toastoven.net/prod_tcdeploy/reference/14.png)
<center>[그림 14] 서버 정보 삭제</center>

* 삭제할 서버 정보
    * 왼쪽 체크 박스 해제 또는 오른쪽 <img class="img-inline" alt="delete-w.png" src="http://static.toastoven.net/prod_tcdeploy/btn/delete-w.png"> 클릭
    * 서버 그룹 생성/수정 팝업의 <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png">/<img class="img-inline" alt="edit-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/edit-b.png"> 클릭 후 삭제

### 리소스

리소스를 관리할 수 있는 페이지로 파일 생성 및 업로드, 다운로드, 수정을 할 수 있으며 변경이력을 확인할 수 있습니다.

![[그림 15] 탭 메뉴 - 리소스](http://static.toastoven.net/prod_tcdeploy/reference/15.png)
<center>[그림 15] 탭 메뉴 - 리소스</center>

## 기능 별 설명

이 영역에서는 Getting Started에서 다루지 않은 기능의 상세 설명 및 추가 설정을 제공합니다.

### 바이너리

바이너리는 사용자로부터 업로드 된 배포 대상 파일 입니다.

#### 업로드

바이너리 업로드를 위한 두 가지 방법을 제공합니다.

* API 업로드
    * API 업로드에 대한 상세 내역은 [API 가이드의 Binary Upload API](/Dev%20Tool/Deploy/ko/api-guide/#binary-upload-api) 에서 확인하실 수 있습니다.
* UI 업로드

```
[Deploy] > 하단 탭 중 [바이너리 그룹] > [업로드] 클릭
```

![[그림 16] 바이너리 업로드 팝업](http://static.toastoven.net/prod_tcdeploy/reference/16.png)
<center>[그림 16] 바이너리 업로드 팝업</center>

1. <img class="img-inline" alt="upload-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-g.png"> 클릭
2. <img class="img-inline" alt="fileselect-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-b.png"> 클릭 후 바이너리 파일 선택
3. 버전(선택), 설명(선택) 정보 입력
4. <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭

#### 다운로드

![[그림 17] 바이너리 다운로드](http://static.toastoven.net/prod_tcdeploy/reference/17.png)
<center>[그림 17] 바이너리 다운로드</center>

#### 버전 픽스

Client OS별 1개씩 버전 픽스를 할 수 있습니다.

* 상태 구분
    * fixed <img class="img-inline" alt="fixed.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fixed.png">
        * 클릭 시 해당 버전을 unfixed 합니다.
    * unfixed <img class="img-inline" alt="unfixed.png" src="http://static.toastoven.net/prod_tcdeploy/btn/unfixed.png">
        * 클릭 시 해당 버전을 fixed 합니다.

#### 배포

##### 버전별 배포

Client 바이너리의 all / fixed / recent 버전을 원하는 방식으로 배포할 수 있습니다.

* 바이너리 다운로드 링크 제공
* 다운로드 링크 알림 기능 제공
    * 해당 프로젝트에 등록된 멤버에 SMS / E-Mail 전송

```
[Deploy] > 하단 탭 중 [바이너리 그룹] > [버전별 배포] 클릭
```

![[그림 18] 바이너리 버전별 배포](http://static.toastoven.net/prod_tcdeploy/reference/18.png)
<center>[그림 18] 바이너리 버전별 배포</center>

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

특정 바이너리를 SMS / E-Mail로 배포할 수 있습니다.

```
[Deploy] > 하단 탭 중 [바이너리 그룹] > 바이너리 파일 선택 > [전송] > 다운로드 전송 타입 및 수신자 선택 > [전송] 클릭
```

![[그림 19] 바이너리 선택 배포](http://static.toastoven.net/prod_tcdeploy/reference/19.png)
<center>[그림 19] 바이너리 선택 배포</center>

1. 전송할 버전을 선택하고 <img class="img-inline" alt="send-w.png" src="http://static.toastoven.net/prod_tcdeploy/btn/send-w.png"> 클릭

    ![[그림 20] 바이너리 선택 배포 - 다운로드 경로 전송](http://static.toastoven.net/prod_tcdeploy/reference/20.png)
    <center>[그림 20] 바이너리 선택 배포 - 다운로드 경로 전송</center>
2. 다운로드 링크 전송 팝업에서 전송 유형과 수신자를 선택하고 <img class="img-inline" alt="send-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/send-b.png"> 클릭
    * SMS / E-mail로 일부 또는 모두 지정 가능
3. 지정한 전송 유형으로 수신자에게 바이너리 설치를 위한 내용이 전달됩니다.

### 태스크

태스크는 개별 기능 수행 및 순서 제어가 가능한 시나리오 구성 요소 입니다.

```
[Deploy] > 하단 탭 중 [배포] > 시나리오 필드의 [새로 만들기] >  [Task 추가] 클릭
```

* pre-run Task : 배포 전 처리 작업이 가능한 태스크
    * 대상 : 지정한 타겟 서버
        * 타겟 서버를 지정한 일회성 처리 가능
* Normal Task : 배포 과정 중 실행되는 태스크
    * 대상 : 서버 그룹 또는 서버 그룹 내 지정 서버

##### 태스크 이용 가이드

* 태스크 타입 구분
![[그림 21] 태스크 타입 구분](http://static.toastoven.net/prod_tcdeploy/reference/21.png)
<center>[그림 21] 태스크 타입 구분</center>
    * pre-run Task form color : **<span style="color:#0052cc">blue</span>**
    * Normal Task form color : <span style="color:#777777">**grey**</span>
* 태스크 순서
    * pre-run 태스크는 영역 상단에 자리합니다.
    pre-run 태스크 / Normal 태스크 각 영역 내에서는 위치 이동이 자유롭습니다.
* 태스크 제어
    * 순서 제어 및 삭제
    * <img class="img-inline" alt="task.png" src="http://static.toastoven.net/prod_tcdeploy/btn/task.png">
        * 각 태스크 우측 상단에 위치한 '∧' (위로 이동) '∨' (아래로 이동) 'X' (Task 제거) 버튼 사용
* 예약어 사용
    * 자세한 내용은 하단 Available Variables에서 확인하실 수 있습니다.

#### pre-run Task

* Target Server : 별도로 Target Server를 지정한 일회성 처리가 가능합니다.

##### Jenkins-Cli Build

* ver. 2.46 이전 / ver. 2.46 이후 버전으로 구분
* Jenkins 빌드 설정에 대한 상세 내역은 [플러그인 사용 가이드](/Dev%20Tool/Deploy/ko/plugin-guide/)에서 확인하실 수 있습니다.

![[그림 22] Jenkins-Cli Build](http://static.toastoven.net/prod_tcdeploy/reference/22.png)
<center>[그림 22] Jenkins-Cli Build</center>

**[ 사용자 입력 내용 ]**

* OS 선택
* RunAs 입력
    * 실행 계정을 입력 합니다.
* JenkinsServer
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
				* Public Key가 설정된 경우 Jenkins에서 자동 인증 진행
			* 2.46 이상 버전
				* User(Jenkins admin Id)가 추가 되었으며, 해당 Id에 Public Key가 설정된 경우만 인증 통과
* Build Parameter (선택)
    * 빌드에 필요한 파라미터를 입력 합니다.

##### User Command

* 사용자 정의 Command 태스크
* Available Variables(예약어) 사용
* 오류 발생 감지 및 오류 발생 시 배포 중지
    * 탭 메뉴 중 [배포] > 시나리오 실행 실패 시 [실행 중단] 설정이 되어있을 때

![[그림 23] pre-run Task - User Command](http://static.toastoven.net/prod_tcdeploy/reference/23.png)
<center>[그림 23] pre-run Task - User Command</center>

**[ 사용자 입력 내용 ]**

* OS 선택
* RunAs 입력
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

![[그림 24] Binary Deploy](http://static.toastoven.net/prod_tcdeploy/reference/24.png)
<center>[그림 24] Binary Deploy</center>

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* RunAs 입력
    * 실행 계정을 입력 합니다.
* 바이너리
    * 배포할 바이너리 파일을 세가지 타입으로 선택할 수 있습니다.

        * 최신 버전 사용

            ![[그림 25] Binary Deploy - 최신 버전 사용](http://static.toastoven.net/prod_tcdeploy/reference/25.png)
            <center>[그림 25] Binary Deploy - 최신 버전 사용</center>

            * 최신 버전 사용에 체크하면 최신 버전이 자동 선택 됩니다. (최신 버전 파일이 있을 경우)
            * 바이너리 그룹을 선택할 수 있습니다. (선택된 그룹의 최신 버전이 사용됨)

        * 선택 버전 사용
            * <img class="img-inline" alt="binaryselect-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/binaryselect-g.png"> 클릭

                ![[그림 26] Binary Deploy - 선택 팝업](http://static.toastoven.net/prod_tcdeploy/reference/26.png)
                <center>[그림 26] Binary Deploy - 선택 팝업</center>

                * 사용할 바이너리에 체크한 후 <img class="img-inline" alt="select-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/select-b.png"> 클릭

            ![[그림 27] Binary Deploy - 선택 확인](http://static.toastoven.net/prod_tcdeploy/reference/27.png)
            <center>[그림 27] Binary Deploy - 선택 확인</center>

            * 바이너리가 선택 됨.

        * 업로드 후 사용
            * <img class="img-inline" alt="upload-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-g.png"> 클릭

                ![[그림 28] Binary Deploy - 업로드 팝업](http://static.toastoven.net/prod_tcdeploy/reference/28.png)
                <center>[그림 28] Binary Deploy - 업로드 팝업</center>

                * <img class="img-inline" alt="fileselect-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-b.png"> 클릭 후 Binary 파일 선택
                * <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭

                ![[그림 29] Binary Deploy - 업로드 완료 팝업](http://static.toastoven.net/prod_tcdeploy/reference/29.png)
                <center>[그림 29] Binary Deploy - 업로드 팝업</center>

                * <img class="img-inline" alt="confirm-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/confirm-b.png"> 클릭

                ![[그림 30] Binary Deploy - 선택된 바이너리 확인](http://static.toastoven.net/prod_tcdeploy/reference/30.png)
                <center>[그림 30] Binary Deploy - 선택된 바이너리 확인</center>

                * 바이너리가 선택 됨.

* Variable As
    * 해당 바이너리의 Variable명을 지정합니다. Command에서 Available Variables를 사용할 수 있습니다.
* 타겟 디렉토리
    * 바이너리를 배포할 디렉토리를 지정합니다.

##### File Deploy

* 메뉴 탭 중 [Resource]에서 관리하는 파일을 최신 버전으로 배포할 수 있습니다.

![[그림 31] File Deploy](http://static.toastoven.net/prod_tcdeploy/reference/31.png)
<center>[그림 31] File Deploy</center>

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* RunAs 입력
    * 실행 계정을 입력 합니다.
*  파일
    * 배포할 파일을 선택 합니다.
        * <img class="img-inline" alt="fileselect-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-g.png"> 클릭

            ![[그림 32] File Deploy - 배포 파일 선택 팝업](http://static.toastoven.net/prod_tcdeploy/reference/32.png)
            <center>[그림 32] File Deploy - 배포 파일 선택 팝업</center>

            * 사용할 파일에 체크한 후 <img class="img-inline" alt="select-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/select-b.png"> 클릭

            ![[그림 33] File Deploy - 선택된 배포 파일 확인](http://static.toastoven.net/prod_tcdeploy/reference/33.png)
            <center>[그림 33] File Deploy - 선택된 배포 파일 확인</center>

            * 파일이 선택 됨
* 타겟 디렉토리
    * 파일을 배포할 디렉토리를 지정합니다.

##### User Command

* 사용자 정의 Command 태스크
* Available Variables(예약어) 사용
* script 오류 발생 감지 및 오류 발생 시 배포 중지
    * 탭 메뉴 중 [배포] > 시나리오 실행 실패 시 [실행 중단] 설정이 되어있을 때

![[그림 34] Normal Task - User Command](http://static.toastoven.net/prod_tcdeploy/reference/34.png)
<center>[그림 34] Normal Task - User Command</center>

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* RunAs 입력
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

![[그림 35] 파일 그룹 생성](http://static.toastoven.net/prod_tcdeploy/reference/35.png)
<center>[그림 35] 파일 그룹 생성</center>

1. <img class="img-inline" alt="newfilegroup.png" src="http://static.toastoven.net/prod_tcdeploy/btn/newfilegroup.png"> 클릭
2. 이름(필수), 설명(선택) 정보 입력
3. <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭

#### 파일 추가

![[그림 36] 파일 추가](http://static.toastoven.net/prod_tcdeploy/reference/36.png)
<center>[그림 36] 파일 추가</center>

1. <img class="img-inline" alt="fileadd-w.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileadd-w.png"> 클릭
2. 파일 그룹(필수) 지정에 따라 아래 두 가지 방법으로 파일을 추가할 수 있습니다.
*  파일 업로드
*  파일 생성

##### 파일 업로드

1. 생성 방식(필수) : 파일 업로드 선택
2. <img class="img-inline" alt="fileselect-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-b.png">(필수) 클릭 후 업로드할 파일 선택
* 파일 그룹 내 파일명 중복 시 업로드가 제한됩니다
3. 인코딩(선택), Description(선택), Comment(선택) 입력
4. <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭

##### 신규 생성

1. 생성 방식(선택) : 신규 생성 선택
2. <img class="img-inline" alt="confirm-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/confirm-b.png"> 클릭

    ![[그림 37] 파일 신규 생성](http://static.toastoven.net/prod_tcdeploy/reference/37.png)
    <center>[그림 37] 파일 신규 생성</center>

3. 파일 이름(필수) 입력
4. 파일 설명(선택), Comment(선택) 입력
5. 파일 내용(선택) 입력
6. 파일 에디터 view mode 선택 (파일 확장자에 따른 뷰모드 자동선택 / 단순 뷰 변경)
7. <img class="img-inline" alt="save-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/save-g.png"> 클릭
8. 생성 완료

    ![[그림 38] 생성된 신규 파일 확인](http://static.toastoven.net/prod_tcdeploy/reference/38.png)
    <center>[그림 38] 생성된 신규 파일 확인</center>

#### 파일 수정

* 파일 설명 수정
* 파일 내용 수정
* 파일 History로 수정
* 파일 이력 관리

##### 파일 설명 수정

* 파일에 대한 설명을 수정할 수 있습니다.

![[그림 39] 파일 설명 수정](http://static.toastoven.net/prod_tcdeploy/reference/39.png)
<center>[그림 39] 파일 설명 수정</center>

1. 파일 클릭
2. 파일 설명 수정
3. <img class="img-inline" alt="descedit-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/descedit-g.png"> 클릭

##### 파일 내용 수정

1. 파일 클릭
2. 파일 내용 수정
    ![[그림 40] 파일 내용 수정](http://static.toastoven.net/prod_tcdeploy/reference/40.png)
    <center>[그림 40] 파일 내용 수정</center>
3. <img class="img-inline" alt="save-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/save-g.png"> 클릭

##### 파일 History

파일 생성/수정 History를 확인할 수 있습니다.

1. <img class="img-inline" alt="filehistory.png" src="http://static.toastoven.net/prod_tcdeploy/btn/filehistory.png"> 클릭

    ![[그림 41] 파일 History 목록](http://static.toastoven.net/prod_tcdeploy/reference/41.png)
    <center>[그림 41] 파일 History 목록</center>

    * 목록 확인
        * 첫번째 내역 : 현재 버전을 의미
        * 이후 내역 : 이전 버전으로, 클릭 시 상세 팝업으로 내용 표시
    ![[그림 42] 파일 History 상세 내역](http://static.toastoven.net/prod_tcdeploy/reference/42.png)
    <center>[그림 42] 파일 History 상세 내역</center>

    * 내역 클릭 시 해당 버전의 상세 내용 확인 가능

##### 파일 이력 관리

파일 History 상세 내역에서 아래 기능을 제공합니다.

* 최신 버전으로 저장
    * <img class="img-inline" alt="savecurrent-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/savecurrent-g.png">
        * 해당 버전이 '최신 버전으로 저장' 됨

## OS 별 요구사항

### Linux

* curl 7.19.7-43 버전 이상

### Window

* SSH 설치 필요
    * SSH Shell : PowerShell 지정
