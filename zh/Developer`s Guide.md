## Upcoming Products > Deploy > Developer's Guide

> ※ 본 문서는 alpha 개발 단계의 문서입니다.
> 사용에 관심이 있으신 분은 **support@cloud.toast.com**으로 문의해 주시기 바랍니다.

<br/>

> 아래 플러그인들은 서버, 클라이언트 어플리케이션 타입 모두 지원하고 있습니다.

## Ant Plugin Guide

Ant 빌드 프로젝트 내에서 플러그인을 통해 배포시스템으로 업로드를 할 수 있습니다.

#### 사용 방법

1. [module_tcd-0.0.2.jar](http://images.hangame.co.kr/tcdeploy/plugins/upload/ant/module_tcd-0.0.2.jar) 다운로드
2. tcd-lib 폴더 생성 및 module_tcd-0.0.2.jar 추가
3. build.xml에 tcdeploy 타겟 추가
4. tcdeploy 타겟 실행

#### 서버(server) 타입 어플리케이션을 위한 tcdeploy 타겟 설정

build.xml

```
<target name="tcdeploy" depends="①">
	<zip destfile="②.zip">
		<zipfileset dir="③" />
	</zip>
	<taskdef name="tcdTask" classname="com.nhnent.deploy.TcdTask">
		<classpath>
			<pathelement location="tcd-lib/module_tcd-0.0.2.jar" />
		</classpath>
	</taskdef>
	<tstamp>
		<format property="⑨" pattern="yyyy-MM-dd_HH-mm-ss"/>
	</tstamp>
	<tcdTask appKey="④"
			artifactId="⑤"
			applicationType="⑥"
			binaryFilePath="②"
			version="⑦"
			description="⑧" />
	<delete file="②.zip" />
</target>
```

**[ 사용자 입력 내용 ]**

① : 프로젝트 기본 빌드 타겟 (optional)  
② : 업로드 파일명 (required): 경로 포함한 파일명  
③ : 배포대상 디렉토리 (required)  
④ : 앱키 (required)  
⑤ : 아티팩트 아이디 (required)  
⑥ : 애플리케이션 타입 (required): server  
⑦ : 버전명 (optional)  
⑧ : 업로드 설명 (optional)  
⑨ : 타임스탬프 (optional) : 버전을 타임스탬프로 관리시, property명을 입력 후 ⑦의 prefix 혹은 postfix로 사용  

#### 클라이언트(client) 타입 어플리케이션을 위한 tcdeploy 타겟 설정

build.xml

```
<target name="tcdeploy" depends="①">

	<taskdef name="tcdTask" classname="com.nhnent.deploy.TcdTask">
		<classpath>
			<pathelement location="tcd-lib/module_tcd-0.0.2.jar" />
		</classpath>
	</taskdef>
	<tstamp>
		<format property="⑧" pattern="yyyy-MM-dd_HH-mm-ss"/>
	</tstamp>
	<tcdTask appKey="③"
			artifactId="④"
			applicationType="⑤"
			binaryType="⑨"
			binaryFilePath="②"
			metaFilePath="⑩"
			version="⑥"
			description="⑦" />
</target>
```

**[ 사용자 입력 내용 ]**

① : 프로젝트 기본 빌드 타겟 (optional)  
② : 업로드 파일명(required): 경로 포함한 파일명  
③ : 앱키 (required)  
④ : 아티팩트 아이디 (required)  
⑤ : 애플리케이션 타입 (required): client  
⑥ : 버전명 (optional)  
⑦ : 업로드 설명 (optional)  
⑧ : 타임스탬프 (optional) : 버전을 타임스탬프로 관리시, property명을 입력 후 ⑥의 prefix 혹은 postfix로 사용  
⑨ : 바이너리 타입 (required) : 안드로이드(Android), 아이폰(iOS), 기타(etc) 중 선택  
⑩ : 메타 파일명 : 경로를 포함한 메타파일명, 메타파일이 있는 바이너리 타입의 경우 필수속성, 예) iOS타입 바이너리.  

## Maven Plugin Guide

Maven Plugin을 통해 배포시스템으로 업로드 할 수 있습니다.

#### 1. Maven Repository 추가

pom.xml

```
...

<repositories>
	<repository>
		<id>hangameRepository</id>
		<url>http://nexus.hangame.com/content/group/all/</url>
	</repository>
</repositories>

...
```

#### 2. Plugin 설정 추가

서버(server)타입 어플리케이션을 위한 pom.xml 설정

```
...

<plugin>
	<groupId>com.nhnent</groupId>
	<artifactId>module_tcd_mvn</artifactId>
	<version>1.0.2</version>
	<executions>
		<execution>
			<goals>
				<goal>binaryUpload</goal>
			</goals>
		</execution>
	</executions>
	<configuration>
		<appKey>①</appKey>
		<artifactId>②</artifactId>
		<applicationType>③</applicationType>
		<binaryFilePath>④</binaryFilePath>
		<version>⑤</version>
		<description>⑥</description>

	</configuration>
</plugin>

...
```

클라이언트(client)타입 어플리케이션을 위한 pom.xml 설정

```
...

<plugin>
	<groupId>com.nhnent</groupId>
	<artifactId>module_tcd_mvn</artifactId>
	<version>1.0.2</version>
	<executions>
		<execution>
			<goals>
				<goal>binaryUpload</goal>
			</goals>
		</execution>
	</executions>
	<configuration>
		<appKey>①</appKey>
		<artifactId>②</artifactId>
		<applicationType>③</applicationType>
		<binaryType>⑦</binaryType>
		<binaryFilePath>④</binaryFilePath>
		<metaFilePath>⑧</metaFilePath>
		<version>⑤</version>
		<description>⑥</description>

	</configuration>
</plugin>

...
```

**[ 사용자 입력 내용 ]**

① : 앱키 (required)  
② : 애플리케이션 타입 (required)  
서버 : server / 클라이언트 : client  
③ : 아티팩트 아이디 (required)  
④ : 아티팩트 업로드 파일명 (required): 파일경로 포함한 파일명  
⑤ : 아티팩트 버전 (optional)  
⑥ : 업로드 설명 (optional)  
⑦ : 바이너리 타입 (required) : 안드로이드(Android), 아이폰(iOS), 기타(etc) 중 선택. 클라이언트 타입 어플리케이션의 경우 필수 속성  
⑧ : 메타 파일명 : 경로를 포함한 메타파일명, 메타파일이 있는 바이너리 타입의 경우 필수속성, 예) iOS타입 바이너리.  

#### 3. Maven 실행

```
mvn module_tcd_mvn:binaryUpload
```

## Jenkins Plugin Guide

Toast Cloud Deploy(이하 TCD) Jenkins 업로드 플러그인은 Jenkins의 빌드 결과물을 TCD 서버로 업로드 할 수 있게 해줍니다.

#### Jenkins 최소 요구사항 버전

**Jenkins 1580.1** 이후 버전을 요구합니다.

#### 플러그인 설치

Jenkins 관리 ▷ 플러그인 관리 ▷ 고급 탭 ▷ 플러그인 올리기 메뉴에서 **tcdeploy-upload-jenkins.hpi** 파일을 올립니다.
([tcdeploy-upload-jenkins.hpi](http://images.hangame.co.kr/tcdeploy/plugins/upload/jenkins/tcdeploy-upload-jenkins.hpi) 다운로드 링크)


![[그림 1] 플러그인 올리기 ](http://images.toast.co.kr/toast/deploy/guide/11.png)
<center>[그림 1] 플러그인 올리기 </center>

설치가 완료되면 "설치된 플러그인 목록" 탭 메뉴에서 아래 [그림 2]와 같이 설치된 내역을 확인할 수 있습니다.

![[그림 2] 설치된 플러그인 목록](http://images.toast.co.kr/toast/deploy/guide/12_0_0_3.png)
<center>[그림 2] 설치된 플러그인 목록</center>

해당 빌드 설정의 "빌드 후 조치 추가" 버튼을 눌러 서버 또는 클라이언트 타입 어플리케이션 업로드 테스크를 추가할 수 있습니다.

![[그림 3] 설치된 플러그인 목록](http://images.toast.co.kr/toast/deploy/guide/15_0_0_3.png)
<center>[그림 3] 설치된 플러그인 목록</center>

#### 서버(server) 타입 어플리케이션 업로드 테스크 설정

서버 타입 어플리케이션 업로드 테스크는 성공적으로 빌드된 결과물을 ZIP으로 압축해서 TCD 서버로 업로드 해줍니다.

![[그림 4] 서버 어플리케이션 업로드 테스크 설정](http://images.toast.co.kr/toast/deploy/guide/13_0_0_3.png)
<center>[그림 4] 서버 어플리케이션 업로드 테스크 설정</center>

① : enable upload  
    플러그인의 동작의 활성화/비활성화 여부를 결정하는 옵션 입니다.  
    플러그인 설정이 저장된 상태를 유지하면서 플러그인의 동작은 비활성화하고 싶을때 유용합니다.  
② : publish path  
    빌드가 완료된 워크스페이스 상에서 배포대상이 될 상대경로 입니다.  
    예제에서는 /로 입력되어있는데 이 경우, JENKINS_HOME/jobs/프로젝트명/workspace 이하의 내용들이 업로드 대상 경로가 되고, target이라고 입력한다면 JENKINS_HOME/jobs/프로젝트명/workspace/target 이하의 내용들이 업로드 대상 경로가 됩니다.  
③ : artifact id  
    아티펙트 ID입니다. 숫자만 입력될 수 있습니다.  
④ : app key  
    인증을 위한 application key 입니다.  
⑤ : archive file name  
    생성될 Zip파일의 이름입니다. 이름을 지정하지 않으면 기본 이름으로 자동등록됩니다.  
    예) 기본 파일명 tcdeploy-artifactid-[artifactId]-[yyyyMMddHHmmss].zip  
⑥ : artifact version  
    아티펙트의 버전입니다. Jenkins의 빌드번호, 빌드 시작일시등의 표현식을 지정할 수 있습니다. 버전을 지정하지 않으면 기본 버전으로 자동등록됩니다.  
    - 표현식사용 버전형식 예): TCD_SVR_${BUILD_NUMBER}\_\_${JENKINS_PROJECT_NAME}\_\_${BUILD_START_DATE}  
    - 표현식사용 버전결과 예): TCD_SVR_95__Maven_Project_Test__2015-09-14_16-46-38  
    주의) 버전에는 금칙문자가 지정되어있어, 공백은 밑줄문자(\_)로 치환되고 ( ) - _ ~ . , 외의 ASCII특수문자들은 제거됩니다.  
⑦ : description  
    현재 업로드에 대한 간단한 설명을 첨부합니다.  
⑧ : include filter  
    ant-style glob 패턴을 사용해서 포함규칙을 정의할 수 있습니다.  
    예) \*\*/\*.java,\*\*/\*.xml - 모든 하위 디렉토리의 java, xml 파일을 포함합니다.  
⑨ : exclude filter  
    ant-style glob 패턴을 사용해서 제외규칙을 정의할 수 있습니다.  
    예) \*\*/.svn/\*\* - 모든 하위 디렉토리의 경로 이름 중, .svn이 들어간 파일 및 디렉토리를 제외합니다.  
⑩ : compression level  
    압축 수준을 3단계로 설정 할 수 있습니다.  
    Store: 무압축 / Normal: 보통압축 / Maximum: 최대압축  

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

#### 클라이언트(client) 타입 어플리케이션 플러그인 설정

클라이언트 타입 어플리케이션 업로드 테스크는 성공적으로 빌드된 결과물 중 사용자가 지정한 특정 바이너리를 TCD 서버로 업로드 해줍니다.

![[그림 5] 클라이언트 어플리케이션 업로드 테스크 설정](http://images.toast.co.kr/toast/deploy/guide/16_0_0_3.png)
<center>[그림 5] 클라이언트 어플리케이션 업로드 테스크 설정</center>

① : enable upload  
    플러그인의 동작의 활성화/비활성화 여부를 결정하는 옵션 입니다.  
    플러그인 설정이 저장된 상태를 유지하면서 플러그인의 동작은 비활성화하고 싶을때 유용합니다.  
② : artifact id  
    아티펙트 ID입니다. 숫자만 입력될 수 있습니다.  
③ : app key  
    인증을 위한 application key 입니다.  
④ : binary kind  
    업로드할 바이너리가 안드로이드(Android) 바이너리인지 아이폰(iOS) 바이너리인지, 기타(etc) 바이너리인지 구분합니다.  
⑤ : artifact version  
    아티펙트의 버전입니다. 서버 어플리케이션의 버전 폼과 동일하게 처리됩니다.  
⑥ : (ipa/apk/etc/plist) file path  
    업로드할 바이너리 파일의 경로를 포함한 파일명입니다.  
    - 기타(etc)바이너리는 확장자 제한이 없지만, Android, iOS는 확장자제한이 있습니다.  
    - iOS일 때는 ipa바이너리 파일 및, plist 메타파일을 같이 업로드 해야합니다.  
    - 서버타입 업로드 테스크와는 다르게 빌드 워크스페이스 외부의 파일 경로도 입력가능합니다.  
    - 워크스페이스로 바로 접근하는 경로 표현식으로 ${WORKSPACE}/target/a.ipa와 같이 사용할 수 있습니다.  
⑦ : description  
    현재 업로드에 대한 간단한 설명을 첨부합니다.  

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

① : SSH Pubilc/Private Key Pair 생성  
    ssh-keygen 또는 PuTTY Key Generator등으로 SSH Pubilc/Private Key Pair를 생성하고,  
    Jenkins의 빌드 수행 계정에 SSH Public Key를 저장, SSH Private Key 파일은 Task 수행서버에 저장.  
    [주의] 암호문(passphrase)이 설정되지 않은 SSH Private Key만 지원합니다.  
    예) http://[JENKINS_URL]/user/[사용자명]/configure 페이지에 접근하여  
    SSH Public Keys 항목에 생성한 SSH Public Key의 내용을 저장하고,  
    생성한 SSH Private Key 파일은 Task를 수행하는 서버의 사용자 지정 경로에 저장.  
② : http keep alive timeout 조정  
    Jenkins 서버의 http keep alive timeout 값 확인 후, 값 조정.  
    예) jenkins를 RPM으로 설치했을 경우,  
    /etc/sysconfig/jenkins 의 JENKINS_ARGS에  
    --httpKeepAliveTimeout=[적당한 밀리초값] 옵션 추가.  
③ : Stream 예외 발생 관련  
    빌드 콘솔 출력중 java.io.StreamCorruptedException이 발생할 경우  
    Jenkins를 수행하는 JVM옵션에 -Dhudson.diyChunking=false 옵션 추가.  
    예) jenkins를 RPM으로 설치했을 경우.  
    /etc/sysconfig/jenkins의 JENKINS_JAVA_OPTIONS에  
    -Dhudson.diyChunking=false 옵션 추가.  

#### Profile 설정

![[그림 6] Jenkins-cli Build Profile 설정](http://images.hangame.co.kr/toast/deploy/guide/14.png)
<center>[그림 6] Jenkins-cli Build Profile 설정</center>

**[ 사용자 입력 내용 ]**

① : Java Home  
    jenkins-cli를 수행하기 위한 JDK 또는 JRE의 Home 경로를 입력합니다.  
② : Jenkins Url  
    빌드명령을 수행하고자 하는 Jenkins 서버의 주소를 입력합니다.  
③ : Job Name  
    수행시킬 빌드 Job의 이름을 입력합니다.  
④ : Private Key Path  
    SSH Public Key가 설정되어있는 Jenkins 유저로 인증을 위한 SSH Private Key 파일의 경로를 입력합니다.  
    준비사항 항목에서 생성해둔 SSH Private Key 파일이 저장된 경로를 입력함.  
⑤ : Server Charset  
    jenkins cli를 수행시킬 서버의 문자집합을 입력합니다.  
    아무값도 입력하지 않을 경우 기본값 utf-8  
⑥ : Jenkins Charset  
    jenkins 서버의 문자집합을 입력합니다.  
    아무값도 입력하지 않을 경우 기본값 utf-8  

## Binary Upload API

사용자가 HTTP Request를 직접 구성하여 바이너리를 업로드 할 수 있는 API를 제공합니다.

#### 준비사항.

|Http Method|	POST|
|---|---|
|Request URL| https://api-deploy.cloud.toast.com/api/binary/upload/artifact/{artifactId}|

#### Parameter

|Name|	Type|	Description|	Value|	Required|
|---|---|---|---|---|
|artifactId|	Long|	Artifact의 key 값, Artifacts 페이지에서 확인가능|-|		true|
|appKey|	String|	토스크 클라우드 APPKEY, Deploy 상품 페이지에서 확인가능|-|		true|
|applicationType|	String|	Artifact의 Type|	client 또는 server|	true|
|version|	String|	업로드하는 바이너리의 버전, 미입력시 timestamp로 대체|-|		false|
|description|	String|	바이너리의 설명|-|		false|
|osType|	String|	applicationType이 client인 경우 바이너리 파일의 os 정보|	iOS 또는 Android 또는 etc|	false|
|binaryFile|	File|	바이너리 파일 객체|-|		true|
|metaFile|	File|	iOS인 경우 plist 파일 객체|-|		false|

#### Sample Request Code For JAVA

아래 코드는 HttpClient 라이브러리(httpclient 4.3.6)를 사용하여 API를 통해 바이너리를 업로드하는 코드의 예시이다.

```
String artifactId = "1";
String binaryName = "ojdbc14.jar";
String filePath = "/xxx/xxxx" + binaryName;
FileBody binaryFile = new FileBody(new File(filePath));

StringBody appKey = new StringBody("xxxxxxxxx", ContentType.TEXT_PLAIN);
StringBody applicationType = new StringBody("server", ContentType.TEXT_PLAIN);
StringBody description = new StringBody("A binary file of some kind", ContentType.TEXT_PLAIN);

String requestUri = "https://api-deploy.cloud.toast.com/api/binary/upload/artifact/" + artifactId;

HttpPost method = new HttpPost(requestUri);

HttpEntity reqEntity = MultipartEntityBuilder.create()
		.addPart("binaryFile", binaryFile)
		.addPart("appKey", appKey)
		.addPart("applicationType", applicationType)
		.addPart("description", description)
        .build();

method.setEntity(reqEntity);

try {
	CloseableHttpClient httpclient = HttpClients.createDefault();
	ResponseHandler<String> responseHandler = new ResponseHandler<String>() {
		public String handleResponse(final HttpResponse response) throws IOException {
			int status = response.getStatusLine().getStatusCode();
			if (status == HttpStatus.SC_OK) {
				HttpEntity entity = response.getEntity();
				return entity != null ? EntityUtils.toString(entity) : null;
			} else {
				throw new ClientProtocolException("Unexpected response status: " + status);
			}
		}
	};

	String responseBody = httpclient.execute(method, responseHandler);
	if (responseBody == null) {
		throw new Exception("Empty Response Data Error");
	}

	if (responseBody.contains("false")) {
		throw new Exception("Upload Fail Result Code : " + responseBody);
	}
	logger.debug("Result : " + responseBody);
} catch (Exception e) {
	logger.error("Fail to request : " + e.getMessage(), e);
}
```

#### Response(json)

|Name|	Type|	Description|	Value|
|---|---|---|---|
|isSuccess|	boolean|	업로드 결과|	true 또는 false|
|result|	String|	업로드 결과 메세지|	isSuccess : true <br/> - 업로드된 바이너리의 키정보 <br/> isSuccess : false <br/> - INAVLID_INFORMATION : 잘못된 파라미터 정보 <br/> - BINARY_UPLOAD_ERROR : 바이너리 업로드 중 오류 발생  <br/> - ALREADY_UPLOADED_VERSION : 바이너리 버전충돌 <br/>|

#### Response Sample

```
{
	"isSuccess" : true,
	"result" : 111
}
```
