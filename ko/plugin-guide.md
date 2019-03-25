## Dev Tool > Deploy > 플러그인 사용 가이드

## Jenkins Plugin Guide

TOAST Deploy Jenkins 업로드 플러그인을 사용하면 Jenkins의 빌드 결과물을 TOAST Deploy 서버로 업로드할 수 있습니다.

## Deploy <-> Jenkins 연동 절차

Jenkins build -> Deploy로 바이너리 업로드(by plugin) -> 배포 시나리오 실행(배포/종료/재시작/기타 사전 작업 및 후처리)
이를 위해 다음 순서대로 배포 환경을 구축해야 합니다.

1. Deploy에 아티팩트를 생성
2. Jenkins build job 설정
3. 서버 그룹 생성
4. 배포 시나리오 작성

## Jenkins 설치

Jenkins 설치 및 자세한 사항은 [https://jenkins.io/](https://jenkins.io/)에서 확인할 수 있습니다.

#### Jenkins 최소 요구 사항 버전

**Jenkins 1580.1** 이후 버전을 요구합니다.

#### 플러그인 설치

1. **Jenkins 관리 > 플러그인 관리 > 고급 탭 > 플러그인 올리기** 메뉴에서 **tcdeploy-upload-jenkins.hpi** 파일을 업로드합니다.
  ([tcdeploy-upload-jenkins.hpi](http://images.hangame.co.kr/tcdeploy/plugins/upload/jenkins/tcdeploy-upload-jenkins-ext.hpi) 다운로드 링크)

    ![01.png](http://static.toastoven.net/prod_tcdeploy/devguide/01.png)

2. 설치가 완료되면 **설치된 플러그인 목록** 탭 메뉴에서 아래 그림과 같이 설치된 내역을 확인할 수 있습니다.

    ![02.png](http://static.toastoven.net/prod_tcdeploy/devguide/02.png)

3. 해당 빌드 설정의 **빌드 후 조치 추가** 버튼을 클릭해서 서버 또는 클라이언트 타입 애플리케이션 업로드 태스크를 추가할 수 있습니다.

    ![03.png](http://static.toastoven.net/prod_tcdeploy/devguide/03.png)

#### 서버(server) 타입 애플리케이션 업로드 플러그인 설정

서버 타입 애플리케이션 업로드 플러그인으로 빌드 결과물을 ZIP 형식으로 압축해서 TOAST Cloud Deploy(TCD) 서버로 업로드할 수 있습니다.

![04.png](http://static.toastoven.net/prod_tcdeploy/devguide/04.png)

* enable upload
    * 플러그인 동작의 활성화/비활성화 여부를 결정하는 옵션입니다. 플러그인 설정이 저장된 상태에서 플러그인의 동작은 비활성화하고 싶을 때 유용합니다.
* publish path
    * 빌드가 완료된 워크스페이스에서 배포 대상이 될 상대 경로입니다. 예제에서는 /로 입력되어 있는데 이 경우, JENKINS\_HOME/jobs/프로젝트명/workspace 이하의 내용이 업로드 대상 경로가 되고, target이라고 입력하면 JENKINS\_HOME/jobs/프로젝트명/workspace/target 이하의 내용이 업로드 대상 경로가 됩니다.
* artifact id
    * 아티팩트 ID입니다. 숫자만 입력할 수 있습니다.
* app key
    * 인증을 위한 application key입니다.
* archive file name
    * 생성될 .zip 파일의 이름입니다. 이름을 지정하지 않으면 기본 이름으로 자동 등록됩니다.
        * 예) 기본 파일 이름 tcdeploy-artifactid-[artifactId]-[yyyyMMddHHmmss].zip
* artifact version
    * 아티팩트 버전입니다. Jenkins의 빌드 번호, 빌드 시작 일시 등의 표현식을 지정할 수 있습니다. 버전을 지정하지 않으면 기본 버전으로 자동 등록됩니다.
        * 표현식 사용 버전 형식 예\) TCD\_SVR\_$\{BUILD\_NUMBER\}**$\{JENKINS\_PROJECT\_NAME\}**$\{BUILD\_START\_DATE\}
        * 표현식 사용 버전 결과 예\) TCD\_SVR\_95\_\_Maven\_Project\_Test\_\_2015\-09\-14\_16\-46\-38
        * 주의) 버전에는 금칙문자가 지정되어 있어, 공백은 밑줄(_)로 치환되고 ( ) - _ ~ . , 외의 ASCII 특수문자들은 제거됩니다.
* description
    * 업로드에 대한 설명을 추가합니다.
* include filter
    * ant-style glob 패턴을 사용해 포함 규칙을 정의할 수 있습니다.
        * 예) **/*.java,**/*.xml - 모든 하위 디렉터리의 java, xml 파일을 포함합니다.
* exclude filter
    * ant-style glob 패턴을 사용해 제외 규칙을 정의할 수 있습니다.
        * 예) **/.svn/** \- 모든 하위 디렉터리의 경로 이름 중 \, \.svn이 들어간 파일 및 디렉터리를 제외합니다\.
* compression level
    * 압축 수준을 3단계로 설정할 수 있습니다.
        * Store: 무압축, Normal: 보통 압축, Maximum: 최대 압축

**[ 서버 타입 애플리케이션 업로드 빌드 콘솔 출력 내용 예시 ]**

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

artifact id, app key 등 사용자 입력 정보가 출력되고, 압축 파일에 포함된 파일 내용을 출력합니다.

#### 클라이언트(client) 타입 애플리케이션 업로드 플러그인 설정

클라이언트 타입 애플리케이션 업로드 플러그인으로 성공적으로 빌드된 결과물 중 사용자가 지정한 특정 바이너리를 TCD 서버로 업로드할 수 있습니다.

![05.png](http://static.toastoven.net/prod_tcdeploy/devguide/05.png)

* enable upload
    * 플러그인 동작의 활성화/비활성화 여부를 결정하는 옵션입니다. 플러그인 설정이 저장된 상태를 유지하면서 플러그인의 동작은 비활성화하고 싶을때 유용합니다.
* artifact id
    * 아티팩트 ID입니다. 숫자만 입력할 수 있습니다.
* app key
    * 인증을 위한 application key입니다.
* binary kind
    * 업로드할 바이너리가 Android 바이너리인지 iOS 바이너리인지, 기타(etc) 바이너리인지 구분합니다.
* artifact version
    * 아티팩트 버전입니다. 서버 애플리케이션의 버전 폼과 동일하게 처리됩니다.
* (ipa/apk/etc/plist) file path
    * 업로드할 바이너리 파일의 경로를 포함한 파일 이름입니다.
        * 기타(etc) 바이너리는 확장자 제한이 없지만, Android, iOS는 확장자 제한이 있습니다.
        * iOS일 때는 .ipa 바이너리 파일 및 .plist 메타파일을 같이 업로드해야 합니다.
        * 서버 타입 업로드 태스크와는 다르게 빌드 워크스페이스 외부의 파일 경로도 입력할 수 있습니다.
        * 워크스페이스로 바로 접근하는 경로 표현식으로 ${WORKSPACE}/target/a.ipa와 같이 사용할 수 있습니다.
* description
    * 업로드의 설명을 추가합니다.

**[ iOS 클라이언트 타입 애플리케이션 업로드 빌드 콘솔 출력 내용 예시 ]**

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

artifact id, app key 등 사용자 입력 정보가 출력되고, 업로드할 바이너리 및 메타파일이 출력됩니다.

## Jenkins-CLI Build Profile

사용자가 Jenkins-CLI를 사용하여 빌드 명령 수행에 도움을 주는 사전에 정의된 Profile입니다.

#### 준비 사항

1. SSH Pubilc/Private Key Pair 생성
    * ssh-keygen 또는 PuTTY Key Generator 등으로 SSH Pubilc/Private Key Pair를 생성하고, Jenkins의 빌드 수행 계정에 SSH Public Key를 저장, SSH Private Key 파일은 태스크 수행 서버에 저장.
        * 암호문(passphrase)이 설정되지 않은 SSH Private Key만 지원합니다.
    * 예) http://[JENKINS_URL]/user/[사용자명]/configure 페이지에 접근하여 SSH Public Keys 항목에 생성한 SSH Public Key의 내용을 저장하고, 생성한 SSH Private Key 파일은 태스크 수행 서버의 사용자 지정 경로에 저장.

2. http keep alive timeout 조정
    * Jenkins 서버의 http keep alive timeout 값 확인 후, 값 조정.
    * 예) Jenkins를 RPM으로 설치했을 경우,
        * /etc/sysconfig/jenkins 의 JENKINS_ARGS에 httpKeepAliveTimeout=[적당한 밀리초값] 옵션 추가.

3. Stream 예외 발생 관련
    * 빌드 콘솔 출력 중 java.io.StreamCorruptedException이 발생할 경우 Jenkins를 수행하는 JVM옵션에 -Dhudson.diyChunking=false 옵션 추가.
    * 예) Jenkins를 RPM으로 설치했을 경우.
        * /etc/sysconfig/jenkins의 JENKINS\_JAVA\_OPTIONS에 -Dhudson.diyChunking=false 옵션 추가.

#### Profile 설정

![06.png](http://static.toastoven.net/prod_tcdeploy/devguide/06.png)

**[ 사용자 입력 내용 ]**

* OS 선택
* RunAs 입력
    * 실행 계정을 입력합니다.
* JenkinsServer
    * 도메인 또는 IP를 입력합니다.
* Server Charset
    * Jenkins-CLI를 수행할 서버의 문자집합을 입력합니다.
    아무 값도 입력하지 않으면 기본값 UTF-8
* Jenkins Url
    * 빌드 명령을 수행할 Jenkins 서버 주소를 입력합니다.
* Jenkins Charset
    * Jenkins 서버의 문자집합을 입력합니다. 아무 값도 입력하지 않으면 기본값은 UTF-8입니다.
* Java Home
    * Jenkins-CLI를 수행하기 위한 JDK 또는 JRE의 Home 경로를 입력합니다.
* Job Name
    * 수행할 빌드 Job의 이름을 입력합니다.
* Private Key Path
    * SSH Public Key가 설정되어 있는 Jenkins 유저로 인증하기 위한 SSH Private Key 파일의 저장 경로를 입력합니다.
* User(ver. 2.46 이상 버전 필수)
	* Public Key가 설정된 ID를 입력합니다.
		* 참고 사항
			* 2.46 이전 버전
				* Public Key가 설정된 경우 Jenkins에서 자동 인증 진행
			* 2.46 이상 버전
				* User(Jenkins admin ID)가 추가되었으며, 해당 ID에 Public Key가 설정된 경우만 인증 통과
* Build Parameter(선택)
    * 빌드에 필요한 파라미터를 입력합니다.