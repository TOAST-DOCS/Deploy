## Dev Tool > Deploy > 플러그인 가이드

## Jenkins Plugin Guide

Toast Cloud Deploy(이하 TCD) Jenkins 업로드 플러그인은 Jenkins의 빌드 결과물을 TCD 서버로 업로드 할 수 있게 해줍니다.

## Jenkins 설치

Jenkins 설치 및 자세한 사항은 [https://jenkins.io/](https://jenkins.io/)에서 확인하실 수 있습니다.

#### Jenkins 최소 요구사항 버전

**Jenkins 1580.1** 이후 버전을 요구합니다.

#### 플러그인 설치

1.Jenkins 관리 ▷ 플러그인 관리 ▷ 고급 탭 ▷ 플러그인 올리기 메뉴에서 **tcdeploy-upload-jenkins.hpi** 파일을 올립니다.
([tcdeploy-upload-jenkins.hpi](http://images.hangame.co.kr/tcdeploy/plugins/upload/jenkins/tcdeploy-upload-jenkins-ext.hpi) 다운로드 링크)
![[그림 1] 플러그인 올리기 ](http://static.toastoven.net/prod_tcdeploy/devguide/01.png)
<center>[그림 1] 플러그인 올리기</center>

2.설치가 완료되면 "설치된 플러그인 목록" 탭 메뉴에서 아래 [그림 2]와 같이 설치된 내역을 확인할 수 있습니다.
![[그림 2] 설치된 플러그인 목록](http://static.toastoven.net/prod_tcdeploy/devguide/02.png)
<center>[그림 2] 설치된 플러그인 목록</center>

3.해당 빌드 설정의 "빌드 후 조치 추가" 버튼을 눌러 서버 또는 클라이언트 타입 어플리케이션 업로드 테스크를 추가할 수 있습니다.
![[그림 3] 설치된 플러그인 목록](http://static.toastoven.net/prod_tcdeploy/devguide/03.png)
<center>[그림 3] 설치된 플러그인 목록</center>

#### 서버(server) 타입 어플리케이션 업로드 플러그인 설정

서버 타입 어플리케이션 업로드 플러그인은 성공적으로 빌드된 결과물을 ZIP으로 압축해서 TCD 서버로 업로드 해줍니다.

![[그림 4] 서버 어플리케이션 업로드 플러그인 설정](http://static.toastoven.net/prod_tcdeploy/devguide/04.png)
<center>[그림 4] 서버 어플리케이션 업로드 플러그인 설정</center>

* enable upload
    * 플러그인의 동작의 활성화/비활성화 여부를 결정하는 옵션 입니다. 플러그인 설정이 저장된 상태를 유지하면서 플러그인의 동작은 비활성화하고 싶을때 유용합니다.
* publish path
    * 빌드가 완료된 워크스페이스 상에서 배포대상이 될 상대경로 입니다. 예제에서는 /로 입력되어있는데 이 경우\, JENKINS\_HOME/jobs/프로젝트명/workspace 이하의 내용들이 업로드 대상 경로가 되고\, target이라고 입력한다면 JENKINS\_HOME/jobs/프로젝트명/workspace/target 이하의 내용들이 업로드 대상 경로가 됩니다\.
* artifact id
    * 아티펙트 ID입니다. 숫자만 입력될 수 있습니다.
* app key
    * 인증을 위한 application key 입니다.
* archive file name
    * 생성될 zip파일의 이름입니다. 이름을 지정하지 않으면 기본 이름으로 자동등록됩니다.
        * 예) 기본 파일명 tcdeploy-artifactid-[artifactId]-[yyyyMMddHHmmss].zip
* artifact version
    * 아티펙트의 버전입니다. Jenkins의 빌드번호, 빌드 시작일시등의 표현식을 지정할 수 있습니다. 버전을 지정하지 않으면 기본 버전으로 자동등록됩니다.
        * 표현식사용 버전형식 예\): TCD\_SVR\_$\{BUILD\_NUMBER\}**$\{JENKINS\_PROJECT\_NAME\}**$\{BUILD\_START\_DATE\}
        * 표현식사용 버전결과 예\): TCD\_SVR\_95\_\_Maven\_Project\_Test\_\_2015\-09\-14\_16\-46\-38
        * 주의) 버전에는 금칙문자가 지정되어있어, 공백은 밑줄문자(_)로 치환되고 ( ) - _ ~ . , 외의 ASCII특수문자들은 제거됩니다.
* description
    * 현재 업로드에 대한 간단한 설명을 첨부합니다.
* include filter
    * ant-style glob 패턴을 사용해서 포함규칙을 정의할 수 있습니다.
        * 예) **/*.java,**/*.xml - 모든 하위 디렉토리의 java, xml 파일을 포함합니다.
* exclude filter
    * ant-style glob 패턴을 사용해서 제외규칙을 정의할 수 있습니다.
        * 예) **/.svn/** \- 모든 하위 디렉토리의 경로 이름 중\, \.svn이 들어간 파일 및 디렉토리를 제외합니다\.
* compression level
    * 압축 수준을 3단계로 설정 할 수 있습니다.
        * Store: 무압축 / Normal: 보통압축 / Maximum: 최대압축

**[ 서버타입 어플리케이션 업로드 빌드 콘솔출력 내용 예시 ]**

```
[TCDeploy] Artifact id: [artifact id 값 출력 부분]
[TCDeploy] App key: [application key 값 출력 부분]
[TCDeploy] Application type: server
[TCDeploy] Description: 테스트 배포
[TCDeploy] Upload address: [업로드 주소 출력 부분]
[TCDeploy] Artifact version: TCD_SVR_95__Maven_Project_Test__2015-09-14_16-46-38
[TCDeploy] Deploy relative path: /
[TCDeploy] Ant include pattern: **/*.java,**/*.xml
[TCDeploy] Ant exclude pattern: **/.svn/**
[TCDeploy] Deploy relative path: file:/var/lib/jenkins/jobs/Maven%20Project%20Test/workspace/
[TCDeploy] ------------------------------- Zip Contents Start -------------------------------
	1:	pom.xml
	2:	src/main/java/a_dir/a_sub_dir/a.java
    ..........
	11:	src/main/resources/applicationContext.xml
	12:	src/main/resources/b_dir/b.xml
    ..........
	19:	src/test/resources/logback-test.xml
	20:	target/classes/applicationContext.xml
	..........
	27:	target/test-classes/logback-access.xml
	28:	target/test-classes/logback-test.xml
[TCDeploy] -------------------------------- Zip Contents End --------------------------------
[TCDeploy] Archive Compression Level: Normal
[TCDeploy] Archive temp file path: /tmp/1442216811482-0/Maven_Project_Test.zip
[TCDeploy] Archive temp file size: 33796
[TCDeploy] Archived content count: 28
[TCDeploy] ------------------------------- Upload Start -------------------------------
[TCDeploy] -------------------------------- Upload End --------------------------------
Finished: SUCCESS
```

artifact id, app key 등 사용자 입력정보가 출력되고, 압축파일에 포함된 파일 내용을 출력합니다.

#### 클라이언트(client) 타입 어플리케이션 업로드 플러그인 설정

클라이언트 타입 어플리케이션 업로드 플러그인은 성공적으로 빌드된 결과물 중 사용자가 지정한 특정 바이너리를 TCD 서버로 업로드 해줍니다.

![[그림 5] 클라이언트 어플리케이션 업로드 플러그인 설정](http://static.toastoven.net/prod_tcdeploy/devguide/05.png)
<center>[그림 5] 클라이언트 어플리케이션 업로드 플러그인 설정</center>

* enable upload
    * 플러그인의 동작의 활성화/비활성화 여부를 결정하는 옵션 입니다. 플러그인 설정이 저장된 상태를 유지하면서 플러그인의 동작은 비활성화하고 싶을때 유용합니다.
* artifact id
    * 아티펙트 ID입니다. 숫자만 입력될 수 있습니다.
* app key
    * 인증을 위한 application key 입니다.
* binary kind
    * 업로드할 바이너리가 안드로이드(Android) 바이너리인지 아이폰(iOS) 바이너리인지, 기타(etc) 바이너리인지 구분합니다.
* artifact version
    * 아티펙트의 버전입니다. 서버 어플리케이션의 버전 폼과 동일하게 처리됩니다.
* (ipa/apk/etc/plist) file path
    * 업로드할 바이너리 파일의 경로를 포함한 파일명입니다.
        * 기타(etc)바이너리는 확장자 제한이 없지만, Android, iOS는 확장자제한이 있습니다.
        * iOS일 때는 ipa바이너리 파일 및, plist 메타파일을 같이 업로드 해야합니다.
        * 서버타입 업로드 테스크와는 다르게 빌드 워크스페이스 외부의 파일 경로도 입력가능합니다.
        * 워크스페이스로 바로 접근하는 경로 표현식으로 ${WORKSPACE}/target/a.ipa와 같이 사용할 수 있습니다.
* description
    * 현재 업로드에 대한 간단한 설명을 첨부합니다.

**[ iOS 클라이언트 타입 어플리케이션 업로드 빌드 콘솔출력 내용 예시 ]**

```
[TCDeploy] Artifact id: [artifact id 값 출력 부분]
[TCDeploy] App key: [application key 값 출력 부분]
[TCDeploy] Application type: client
[TCDeploy] Description: 클라이언트 프로그램 테스트 업로드
[TCDeploy] Upload address: [업로드 주소 출력 부분]
[TCDeploy] Binary type: iOS
[TCDeploy] Binary version: IOS_59__build_iOS_Project_Test__2015-09-16_11-13-12
[TCDeploy] iOS binary file path: /tcdUploadTest/a.ipa
[TCDeploy] iOS meta file path: /tcdUploadTest/a.plist
[TCDeploy] ------------------------------- Upload Start -------------------------------
[TCDeploy] -------------------------------- Upload End --------------------------------
Finished: SUCCESS
```

artifact id, app key 등 사용자 입력정보가 출력되고, 업로드할 바이너리 및 메타파일이 출력됩니다.

## Jenkins-cli Build Profile

사용자가 jenkins-cli를 사용하여 빌드 명령 수행에 도움을 주는 사전에 정의된 Profile입니다.

#### 준비사항

1. SSH Pubilc/Private Key Pair 생성
ssh-keygen 또는 PuTTY Key Generator등으로 SSH Pubilc/Private Key Pair를 생성하고, Jenkins의 빌드 수행 계정에 SSH Public Key를 저장, SSH Private Key 파일은 Task 수행서버에 저장.
[주의] 암호문(passphrase)이 설정되지 않은 SSH Private Key만 지원합니다.
* 예) http://[JENKINS_URL]/user/[사용자명]/configure 페이지에 접근하여 SSH Public Keys 항목에 생성한 SSH Public Key의 내용을 저장하고, 생성한 SSH Private Key 파일은 Task를 수행하는 서버의 사용자 지정 경로에 저장.

2. http keep alive timeout 조정
Jenkins 서버의 http keep alive timeout 값 확인 후, 값 조정.
* 예) jenkins를 RPM으로 설치했을 경우,
    * /etc/sysconfig/jenkins 의 JENKINS_ARGS에 httpKeepAliveTimeout=[적당한 밀리초값] 옵션 추가.

3. Stream 예외 발생 관련
빌드 콘솔 출력중 java.io.StreamCorruptedException이 발생할 경우 Jenkins를 수행하는 JVM옵션에 -Dhudson.diyChunking=false 옵션 추가.
* 예) jenkins를 RPM으로 설치했을 경우.
    * /etc/sysconfig/jenkins의 JENKINS\_JAVA\_OPTIONS에 -Dhudson.diyChunking=false 옵션 추가.

#### Profile 설정

![[그림 6] Jenkins-cli Build Profile 설정](http://static.toastoven.net/prod_tcdeploy/devguide/06.png)
<center>[그림 6] Jenkins-cli Build Profile 설정</center>

**[ 사용자 입력 내용 ]**

* OS 선택
* RunAs 입력
    * 실행 계정을 입력 합니다.
* JenkinsServer
    * 도메인 또는 IP를 입력 합니다.
* Server Charset
    * jenkins cli를 수행시킬 서버의 문자집합을 입력합니다.
    아무값도 입력하지 않을 경우 기본값 utf-8
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
* Build Parameter (선택)
    * 빌드에 필요한 파라미터를 입력 합니다.

#### 연동 예시

Jenkins build -> Deploy로 바이너리 업로드(by plugin) ->배포 시나리오 실행(배포/종료/재시작/기타 사전작업 및 후처리)
이를 위해 다음의 순서를 통해 배포환경을 구축해야 합니다.

1.tcDeploy에 Artifact를 생성
2.jenkins build job 설정
3.서버그룹 생성
4.배포 시나리오 작성
