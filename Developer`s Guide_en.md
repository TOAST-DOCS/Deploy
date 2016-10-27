Upcoming Products &gt; Deploy &gt; Developer's Guide
----------------------------------------------------

> ※ This document contains information on alpha development in process. For those interested in use, please contact **support@cloud.toast.com**.
>
> Below plugins support both server and client application types.

Ant Plugin Guide
----------------

Upload to the distribution system through plugins in Ant build project.

#### Method of use

1.  [module\_tcd-0.0.2.jar](http://images.hangame.co.kr/tcdeploy/plugins/upload/ant/module_tcd-0.0.2.jar) download

2.  Create tcd-lib folder and add module\_tcd-0.0.2.jar

3.  Add tcdeploy target in build.xml

4.  Run tcdeploy target

#### tcdeploy target setting for server type application

build.xml

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

**\[ User input contents \]**

① : Project basic build target (optional)
② : Upload file name (required): File name including the path
③ : Distribution target directory (required)
④ : App key (required)
⑤ : Artifact ID (required)
⑥ : Application type (required): server
⑦ : Version name (optional)
⑧ : Upload description (optional)
⑨ : Time stamp (optional) : If the version is managed as a time stamp, enter the property name then use as prefix in ⑦ or postfix

#### tcdeploy target setting for client type application

build.xml

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

**\[ User input contents \]**

① : Project basic build target (optional)
② : Upload file name(required): File name including the path
③ : App key (required)
④ : Artifact ID (required)
⑤ : Application type (required): client
⑥ : Version name (optional)
⑦ : Upload description (optional)
⑧ : Time stamp (optional) : If the version is managed as a time stamp, enter the property name then use as prefix in ⑥ or postfix
⑨ : Binary type (required) : Select among Android, iPhone (iOS) and others (etc)
⑩ : Meta file name : Meta file name including the path, Required property in case of a binary type with a meta file, ex) iOS type binary.

Maven Plugin Guide
------------------

Upload to the distribution system via Maven Plugin.

#### 1. Add Maven Repository 

pom.xml

    ...

    <repositories>
        <repository>
            <id>hangameRepository</id>
            <url>http://nexus.hangame.com/content/group/all/</url>
        </repository>
    </repositories>

    ...

#### 2. Add Plugin settings

pom.xml setting for server type application

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

pom.xml setting for client type application

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

**\[ User input contents \]**

① : App key (required)
② : Application type (required)
Server : server / Client : client
③ : Artifact ID (required)
④ : Artifact upload file name (required): File name including the file path
⑤ : Artifact version (optional)
⑥ : Upload description (optional)
⑦ : Binary type (required) : Select among Android, iPhone (iOS) and others (etc). Required property in case of a client type application
⑧ : Meta file name : Meta file name including the path, Required property in case of a binary type with a meta file, ex) iOS type binary.

#### 3. Run Maven

    mvn module_tcd_mvn:binaryUpload

Jenkins Plugin Guide
--------------------

Toast Cloud Deploy (hereafter TCD) Jenkins upload plugin uploads Jenkins’ build output to TCD server.

#### Jenkins minimum required version

Require versions after **Jenkins 1580.1**.

#### Install plugin

Upload **tcdeploy-upload-jenkins.hpi** file in Jenkins Management ▷ Plugin Management ▷ Advanced Tap ▷ Upload Plugin menu. (tcdeploy-upload-jenkins.hpi download link)

<img src="media/image1.png" width="532" height="184" />

\[Figure 1\] Upload plugin

Once it is installed, the user can check installation history in the "Installed plugin list" tab as shown in \[Figure 2\] below.

<img src="media/image2.png" width="560" height="41" />

\[Figure 2\] Installed plugin list

Click on ‘Add an action after build’ in the applicable build settings to add a server or client type application upload task.

<img src="media/image3.png" width="425" height="290" />

\[Figure 3\] Installed plugin list

#### Server type application upload task setting

Server type application upload task uploads successfully built output to TCD server after compressing to ZIP.

<img src="media/image4.png" width="560" height="531" />

\[Figure 4\] Server application upload task setting

① : enable upload
Option which decides activation/deactivation of plugin’s operation.
Useful when the user wants to deactivate plugin’s operation but maintain the saved plugin settings.
② : publish path
It is a relative path which will become a distribution target on build completed work space.
In the example, it entered / but in this case, subordinate contents of JENKINS\_HOME/jobs/project name/workspace will be the upload target path, but if target is entered then subordinate contents of JENKINS\_HOME/jobs/project name/workspace/target will be the upload target path.
③ : artifact id
It is an artifact ID. Enter numbers only.
④ : app key
application key for authentication.
⑤ : archive file name
Name of Zip file to be created. If no name is designated then a default name will be registered automatically.
ex) Basic file name tcdeploy-artifactid-\[artifactId\]-\[yyyyMMddHHmmss\].zip
⑥ : artifact version
It’s an artifact version. The user can designate expressions such as Jenkins build number and build start date. If no version is designated then the basic version will be registered automatically.
- Expression used version format example): TCD\_SVR\_BUIL*D*<sub>*N*</sub>*U**M**B**E**R*\_\_{JENKINS\_PROJECT\_NAME}\_\_${BUILD\_START\_DATE}
- Expression used version result example): TCD\_SVR\_95\_\_Maven\_Project\_Test\_\_2015-09-14\_16-46-38
Notice) There are prohibited words in the version that a blank will be switched to underscore (\_) and other ASCII special characters besides ( ) - \_ ~ . , will be removed.
⑦ : description
Attach a simple description on the current upload.
⑧ : include filter
Define inclusion rules using ant-style glob pattern.
ex) \*\*/\*.java,\*\*/\*.xml – Include java and xml files in all subordinate directories.
⑨ : exclude filter
Define exclusion rules using ant-style glob pattern.
ex) \*\*/.svn/\*\* - Among the path names of all subordinate directories, exclude files and directories with .svn.
⑩ : compression level
You can set three levels of compression.
Store: No compression / Normal: Normal compression / Maximum: Maximum compression

**\[ Example of server type application upload build console output contents \]**

    [TCDeploy] Artifact id: [artifact id value output part]
    [TCDeploy] App key: [application key value output part]
    [TCDeploy] Application type: server
    [TCDeploy] Description: Test distribution
    [TCDeploy] Upload address: [Upload address output part]
    [TCDeploy] Artifact version: TCD_SVR_95__Maven_Project_Test__2015-09-14_16-46-38
    [TCDeploy] Deploy relative path: /
    [TCDeploy] Ant include pattern: **/*.java,**/*.xml
    [TCDeploy] Ant exclude pattern: **/.svn/**
    [TCDeploy] Deploy relative path: file:/var/lib/jenkins/jobs/Maven%20Project%20Test/workspace/
    [TCDeploy] ------------------------------- Zip Contents Start -------------------------------
        1:  pom.xml
        2:  src/main/java/a_dir/a_sub_dir/a.java
        ..........
        11: src/main/resources/applicationContext.xml
        12: src/main/resources/b_dir/b.xml
        ..........
        19: src/test/resources/logback-test.xml
        20: target/classes/applicationContext.xml
        ..........
        27: target/test-classes/logback-access.xml
        28: target/test-classes/logback-test.xml
    [TCDeploy] -------------------------------- Zip Contents End --------------------------------
    [TCDeploy] Archive Compression Level: Normal
    [TCDeploy] Archive temp file path: /tmp/1442216811482-0/Maven_Project_Test.zip
    [TCDeploy] Archive temp file size: 33796
    [TCDeploy] Archived content count: 28
    [TCDeploy] ------------------------------- Upload Start -------------------------------
    [TCDeploy] -------------------------------- Upload End --------------------------------
    Finished: SUCCESS

User’s input information such as artifact id, app key will be printed, and print file contents included in the compressed file.

#### Client type application plugin setting

Client type application upload task uploads a certain binary which the user selected among successfully built output to TCD server.

<img src="media/image5.png" width="560" height="465" />

\[Figure 5\] Client application upload task setting

① : enable upload
Option which decides activation/deactivation of plugin’s operation.
Useful when the user wants to deactivate plugin’s operation but maintain the saved plugin settings.
② : artifact id
It is an artifact ID. Enter numbers only.
③ : app key
It is anapplication key for verification.
④ : binary kind
Classify the binary for upload into Android binary, iPhone (iOS) binary and etc binary.
⑤ : artifact version
It is an artifact version. Processed same as server application’s version form.
⑥ : (ipa/apk/etc/plist) file path
It is a file name including the binary file path for upload.
- There is no extension restriction on etc binary but there is an extension restriction on Android and iOS.
- For iOS, ipa binary file and plist meta file must be uploaded together.
- Different from server type upload task, a file path of external build work space can be entered.
- It is a path expression to access work space directly that it can be used with ${WORKSPACE}/target/a.ipa.
⑦ : description
Attach a simple description on the current upload.

**\[ Example of iOS client type application upload build console output contents \]**

    [TCDeploy] Artifact id: [artifact id value output part]
    [TCDeploy] App key: [application key value output part]
    [TCDeploy] Application type: client
    [TCDeploy] Description: Upload client program test
    [TCDeploy] Upload address: [Upload address output part]
    [TCDeploy] Binary type: iOS
    [TCDeploy] Binary version: IOS_59__build_iOS_Project_Test__2015-09-16_11-13-12
    [TCDeploy] iOS binary file path: /tcdUploadTest/a.ipa
    [TCDeploy] iOS meta file path: /tcdUploadTest/a.plist
    [TCDeploy] ------------------------------- Upload Start -------------------------------
    [TCDeploy] -------------------------------- Upload End --------------------------------
    Finished: SUCCESS

User’s input information such as artifact id, app key will be printed, and binary and meta file for upload will be printed.

Jenkins-cli Build Profile
-------------------------

Profile which the user defined in advance to help conduct the build command using jenkins-cli.

#### Preparation

① : Create SSH Pubilc/Private Key Pair
Create SSH Pubilc/Private Key Pair using ssh-keygen or PuTTY Key Generator,
save SSH Public Key on the build conduct account in Jenkins, and save SSH Private Key file on Task conduct server.
\[Notice\] Only supports SSH Private Key that has not set passphrase yet.
ex) Access to [http://\[JENKINS\_URL\]/user/\[User](http://[JENKINS_URL]/user/%5bUser) name\]/configure page
and save the contents of SSH Public Key created in SSH Public Keys list,
Save created SSH Private Key file in the user designated path which conducts Task.
② : Modify http keep alive timeout
After checking the http keep alive timeout value in Jenkins server, modify the value.
ex) In case installed Jenkins as RPM,
add --httpKeepAliveTimeout=\[Adequate millisecond value\] option in JENKINS\_ARGS of /etc/sysconfig/jenkins.
③ : Stream exception occurrence related
If java.io.StreamCorruptedException occurred while printing out build console
add -Dhudson.diyChunking=false option in JVM option which conducts Jenkins.
ex) In case installed Jenkins as RPM.
Add -Dhudson.diyChunking=false option to JENKINS\_JAVA\_OPTIONS in /etc/sysconfig/jenkins.

#### Profile settings

<img src="media/image6.png" width="560" height="264" />

\[Figure 6\] Jenkins-cli Build Profile setting

**\[ User input contents \]**

① : Java Home
Enter JDK or Home path of JRE to perform jenkins-cli.
② : Jenkins Url
Enter Jenkins server address to perform the build command.
③ : Job Name
Enter the name of build Job to perform.
④ : Private Key Path
Enter the path of SSH Private Key file to verify as Jenkins user with SSH Public Key.
Enter SSH Private Key file’s saved path which is created in the preparations.
⑤ : Server Charset
Enter character set of a server to perform jenkins cli.
If no value is entered, the default value is utf-8
⑥ : Jenkins Charset
Enter character set of jenkins server.
If no value is entered, the default value is utf-8

Binary Upload API
-----------------

Provide API for users construct HTTP Request and upload a binary themselves.

#### Preparation

| Http Method | POST                                                                       |
|-------------|----------------------------------------------------------------------------|
| Request URL | https://api-deploy.cloud.toast.com/api/binary/upload/artifact/{artifactId} |

#### Parameter

| Name            | Type   | Description                                                          | Value                 | Required |
|-----------------|--------|----------------------------------------------------------------------|-----------------------|----------|
| artifactId      | Long   | Key value of Artifact, Can be checked on Artifacts page              | -                     | true     |
| appKey          | String | Can be checked on TOAST CLOUD APPKEY, Deploy product page            | -                     | true     |
| applicationType | String | Artifact Type                                                        | client or server      | true     |
| version         | String | Binary version for upload, If not entered, replaced with timestamp   | -                     | false    |
| description     | String | Binary discription                                                   | -                     | false    |
| osType          | String | In case applicationType is client, os information of the binary file | iOS or Android or etc | false    |
| binaryFile      | File   | Binary file object                                                   | -                     | true     |
| metaFile        | File   | In case of iOS, plist file object                                    | -                     | false    |

#### Sample Request Code for JAVA

Below code is an example of a code uploading a binary via API using HttpClient library (httpclient 4.3.6).

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

#### Response(json)

| Name      | Type    | Description           | Value                                                                                                                                                                                                                                            |
|-----------|---------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| isSuccess | boolean | Upload result         | true or false                                                                                                                                                                                                                                    |
| result    | String  | Upload result message | isSuccess : true - key information of uploaded binary isSuccess : false - INAVLID\_INFORMATION : wrong parameter information - BINARY\_UPLOAD\_ERROR : error occurred while uploading binary - ALREADY\_UPLOADED\_VERSION : binary version clash |

#### Response Sample

    {
        "isSuccess" : true,
        "result" : 111
    }
