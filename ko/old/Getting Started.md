## Upcoming Products > Deploy > Getting Started 

> ※ 본 문서는 alpha 개발 단계의 문서입니다.
> 사용에 관심이 있으신 분은 **support@cloud.toast.com**으로 문의해 주시기 바랍니다.

Deploy 상품의 이용은 아래와 같은 과정을 거칩니다.
아래 Client Application과 Server Application의 설정과정을 참고하여 배포를 진행할 수 있습니다.

## 서비스 활성화

Console에 [Upcoming Products] > [Deploy]를 선택한 후 [상품이용]버튼을 클릭하여 서비스를 활성화합니다.

![[그림 1] 서비스 활성화](http://images.toast.co.kr/toast/deploy/guide/01.png)
<center>[그림 1] 서비스 활성화</center>

## For Client Application

클라이언트 어플리케이션을 위한 설정은 크게 Artifact 설정과 Binary Upload 단계를 거칩니다.

### Artifact 설정 Deploy > Artifacts

Artifact는 Deploy 상품에서 배포의 기본 단위입니다.

- 아래 [그림 2]과 같이 필수 입력 사항을 기입하고 Artifact를 생성합니다.

![[그림 2] Artifact - Client Application 생성](http://images.toast.co.kr/toast/deploy/guide/02.png)
<center>[그림 2] Artifact - Client Application 생성</center>

1. New 버튼 클릭
2. Artifact Type을 "**Client Application**"으로 선택
3. Name(required), Description(optional) 입력
4. create 버튼 클릭

### Binary UploadDeploy > Artifacts

Binary를 업로드합니다.

- iOS의 경우는 ipa, plist 파일을 Android는 apk 파일을 각각 업로드합니다.
- etc의 경우는 windows 등의 기타 os의 설치 어플리케이션 용도로 사용합니다.

![[그림 3] iOS Application Upload](http://images.toast.co.kr/toast/deploy/guide/03.png)
<center>[그림 3] iOS Application Upload</center>

1. upload 버튼 클릭
2. Binary(iOS의 경우 ipa 파일, required), plist(plist파일, required) 파일 선택
3. Version(optional), Description(optional) 정보 입력
4. upload 버튼 클릭

## For Server Application

서버 어플리케이션의 배포는 설정(Artifact, Server Group, Scenario), Binary Upload, Deploy의 단계를 거칩니다.

### 설정

서버 어플리케이션의 배포를 위해서는 Artifact, Server Group, Scenario 설정이 필요합니다.

#### Artifact 설정Deploy > Artifacts

Artifact를 생성합니다.

- Artifact는 Deploy 상품에서 배포의 기본 단위입니다.
- 아래 [그림 4]과 같이 필수 입력사항을 기입하여 Artifact를 생성합니다.

![[그림 4] Artifact - Server Application 생성](http://images.toast.co.kr/toast/deploy/guide/04.png)
<center>[그림 4] Artifact - Server Application 생성</center>

1. New 버튼 클릭
2. Artifact Type을 "**Server Application**"으로 변경
3. Name(required), Description(optional) 입력
4. Command Type 선택 (Salt Stack과 SSH 중 선택하실 수 있습니다.)
	- **Salt Stack** : 대상 서버에 Salt Stack 연동을 위한 Minion이 설치되어 있어야 합니다.
	- **SSH** : 대상서버에 SSH를 사용 할 수 있어야 합니다.
5. 서버 접근 계정 입력 (대상서버로 Command를 실행하기 위한 Linux 계정)
6. create 버튼 클릭

#### Server Group 설정Deploy > Artifacts

Server Group을 생성합니다.

- 시나리오가 수행되는 기본 단위입니다.
- dev, alpha, beta, release 등과 같이 서버의 Stage를 구분하는 용도로 사용할 수 있습니다.

![[그림 5] Server Group 설정](http://images.toast.co.kr/toast/deploy/guide/05.png)
<center>[그림 5] Server Group 설정</center>

1. + 버튼 클릭
2. Name(required), Description(optional) 입력
3. host name(required), ip(required), os(optional) 정보 입력
4. 서버 추가 버튼 클릭
5. create 버튼 클릭

#### Scenario 설정Deploy > Deploy

Scenario를 생성합니다.

- 배포를 진행하기 위한 command 설정과정입니다.
- Task를 추가하거나, Task를 삭제하고 각각의 Task 순서를 조정하여 배포 프로세스를 설정합니다.

![[그림 6] Scenario 생성](http://images.toast.co.kr/toast/deploy/guide/06.png)
<center>[그림 6] Scenario 생성</center>

1. New 버튼 클릭
2. Name(required) 입력
3. create 버튼 클릭

![[그림 7] Scenario의 Task 추가](http://images.toast.co.kr/toast/deploy/guide/07.png)
<center>[그림 7] Scenario의 Task 추가</center>

1. add task 버튼 클릭
2. Task profile 선택
	- **Binary Deploy** : 배포 할 바이너리 선택을 위한 Task
	- **Log Monitoring** : 로그 확인을 위한 Task (배포시 성공과 실패에 대한 판단용으로 활용)
	- **User Command** : 배포 과정에서 실행되어야 하는 커맨드를 위한 Task (서버 재시작, 압축해제, 기존소스 삭제 등에 활용)
3. Task 작성 및 Task 실행 순서 설정
4. save 버튼 클릭

### Binary Upload

대상 서버의 배포는 우선 바이너리 파일의 업로드 과정을 거쳐야 합니다.
이후 Deploy 과정에서 업로드된 바이너리를 선택하여 진행하게 됩니다.
바이너리 업로드의 편의를 위해서 아래과 같은 방법을 지원하고 있습니다. (각 Plugin들의 사용법은 Developer's Guide에서 확인 가능합니다.)

- Web 업로드(Deploy 상품 웹페이지 직접 업로드)
- 업로드 API URL
- Jenkins Plugin
- Ant Plugin
- Maven Plugin

![[그림 8] Binary Upload - Deploy 상품 웹페이지의 Artifacts 관리 페이지에서의 업로드 화면](http://images.toast.co.kr/toast/deploy/guide/08.png)
<center>[그림 8] Binary Upload - Deploy 상품 웹페이지의 Artifacts 관리 페이지에서의 업로드 화면</center>

1. upload 버튼 클릭
2. Binary 파일 선택
3. Version(optional), Description(optional) 정보 입력
4. upload 버튼 클릭

Deploy의 과정은 위 "2.1 설정" - "Scenario 설정"에서 진행했던 시나리오를 수행하는 과정입니다.  
사전에 설정한 시나리오 실행을 통해서 선택된 바이너리를 대상 서버에 배포 하게됩니다.  
시나리오 실행 방법은 Salt Stack(default)과 SSH를 사용하는 방법이 지원됩니다.  
각각 아래와 같은 제약이 존재합니다.  

|시나리오|제약|
|---|---|
|Salt Stack| - 대상서버에 권한 있는 사용자만 시나리오 실행이 가능합니다.<br/>- 대상서버에 Salt Stack Minion이 설치되어 Salt Stack Master와의 통신이 가능해야 합니다.<br/> minion 설치는 아래 "Salt Stack Minion 설치 가이드"를 참고해주세요.|
|SSH|- Deploy 상품에서 SSH를 사용할 수 있도록 대상 서버에 작업이 되어 있어야 합니다.|

![[그림 9] Deploy - Salt Stack을 사용하여 시나리오를 실행하도록 설정된 Deploy화면](http://images.toast.co.kr/toast/deploy/guide/09.png)
<center>[그림 9] Deploy - Salt Stack을 사용하여 시나리오를 실행하도록 설정된 Deploy화면</center>


1. Artifact 선택
2. Server Group 선택
3. 실행서버 선택 (default 서버그룹에 설정된 모든 서버)
4. 동시실행수 입력
5. 시나리오 선택
6. deploy 버튼 클릭

#### [ 참고 ] Salt Stack Master 설치 가이드

1.salt-master, salt-api 패키지 설치

```
$> yum install salt-master salt-api
```

* Centos 4 버전에서는 python 2.6버전을 지원하지 않아 Salt Stack Master를 설치할 수 없습니다.
* salt-api는 Salt Stack Master에서 restful API를 지원하는 모듈입니다.

2.master 설정파일(/etc/salt/master) 변경

```
# 인증가능한 linux User 추가
external_auth:
   pam: #pam인증 방식
      user_id: # linux user 아이디 추가
         - .*
#salt-api 설정 추가
rest_cherrypy:
   port: 80 # rest API를 사용할 포트 추가
   disable_ssl: True
#Example#
external_auth:
   pam:
      irteam : # irteam 계정으로 인증 합니다.
         - .*
rest_cherrypy:
   port: 8080 # 웹콘솔의 Master Port를 설정과 같은 8080으로 설정해야 합니다.
   disable_ssl: True # SSL을 사용하지 않습니다.
```

* 설정값 중 external_auth의 user_id는 linux 환경에서 사용되는 사용자 계정이어야 합니다.

3.salt-master, salt-api 실행 (아래 각 command는 ROOT  권한으로 실행되어야 합니다.)

```
$> /etc/init.d/salt-master start
$> /etc/init.d/salt-api start
```

4.연결된 minion 확인 (ROOT 권한 필요)

```
$> /usr/bin/salt-key
결과
Accepted Keys:
minion_id1
minion_id2
Denied Keys:
Unaccepted Keys:
Rejected Keys:
```

* 정상적으로 Master와 연결된 Minion의 ID가 Accepted Keys에 출력됩니다.
* master는 4505,4506포트를 통해 minion과 통신합니다. 해당 포트로 외부에서의 접근이 가능하도록 방화벽 설정을 변경해야 합니다.

#### [ 참고 ] Salt Stack Minion 설치 가이드

1.salt-minion 패키지 설치

```
$> yum install salt-minion
```

* Centos 4 버전에서는 python 버전 으로 인해 설치가 불가능 합니다.

2.minion 설정파일(/etc/salt/minion) 변경

```
# Master 주소 설정 (Multi Master)
master:
	- master1.salt.toastoven.net
	- master2.salt.toastoven.net

# Minion ID 설정
id: Minion 서버의 Hostname
```

3.minion 실행 (ROOT 권한 필요)

```
$> /etc/init.d/salt-minion start
```

* minion은 master의 4505, 4506포트로 접근가능해야 합니다.
   불가능한 경우 네트워크 관리자에게 해당포트 ACL추가를 요청해야 합니다.
