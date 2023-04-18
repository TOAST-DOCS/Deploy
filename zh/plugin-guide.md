## Dev Tools > Deploy > User Guide for Plugin 

## Jenkins Plugin Guide

With NHN Cloud Deploy Jenkins Plugin, build results of Jenkins can be uploaded to NHN Cloud Deploy servers.  

## Integrating Deploy <-> Jenkins 

Build Jenkins -> Upload Binary with Deploy (by plugin) -> Execute Deployment Scenario (Deploy/Close/Re-start/Prerequisites and follow-ups) 
To this end, the deployment environment must be built in the following order: 

1. Create artifacts at Deploy
2. Set Jenkins build job 
3. Create a server group
4. Write deployment scenario 

## Installing Jenkins  

Find out how to install Jenkins and details on [https://jenkins.io/](https://jenkins.io/).

#### Minimum Version Requirements for Jenkins 

Requires **Jenkins 1580.1** or later versions.  

1. Go to **Manage Jenkins > Manage Plugin > Advanced > Upload Plugin** and upload **tcdeploy-upload-jenkins.hpi**. 
     (Download from [tcdeploy-upload-jenkins.hpi](http://static.toastoven.net/prod_tcdeploy/plugins/tcdeploy-upload-jenkins-1.1.1.hpi))

     ![[Figure 1] Upload Plugin ](http://static.toastoven.net/prod_tcdeploy/devguide/01.png)

2. When installation is completed, find it from the **List of Plugin Installation** as below. ![[Figure 2] List of Installed Plugins](http://static.toastoven.net/prod_tcdeploy/devguide/02.png)

3. Click *Build and Add Measures** of the **Build Setting** to add tasks to upload server or client type applications.  

    ![[Figure 3] List of Installed Plugins](http://static.toastoven.net/prod_tcdeploy/devguide/03.png)

#### Plugin Setting for Server Type Application Uploads 

With server-type application upload plugin, build results can be compressed in zip and uploaded to a NHN Cloud Cloud Deploy (TCD) server. 

![[Figure 4] Plugin Setting for Server Application Uploads](http://static.toastoven.net/prod_tcdeploy/devguide/04_2.png)

* enable upload
    * Decide whether to enable/disable plugin operations. It is useful to disable plugin operations, while plugin setting is saved. 
* publish path
    * It is the path to be deployed to, from the workspace where build is completed. The example shows /, and in this case, what follows  JENKINS\_HOME/jobs/projectname/workspace becomes the path to upload, and if you enter target, then what follows JENKINS\_HOME/jobs/projectname/workspace/target becomes the path. 
* app key
    * Application key for authentication. 
* artifact id
    * Artifact ID: only numbers are allowed.
* binary group key
    * Key for the binary group to be uploaded: only numbers are allowed. 
* archive file name
    * Name of a .zip file to create: automatically registered in default name, if it is not specified.
        * e.g) Basic file name
           tcdeploy-artifactid-[artifactId]-[yyyyMMddHHmmss].zip
* binary version
    * Binary version. Expressions, including Jenkins build number, or start date of build, can be specified. If a version is not specified, default version is automatically registered.  
        * Format of Expression Version Use
          e.g.) TCD\_SVR\_$\{BUILD\_NUMBER\}**$\{JENKINS\_PROJECT\_NAME\}**$\{BUILD\_START\_DATE\}
        * Result of Expression Version Use 
          e.g.\) TCD\_SVR\_95\_\_Maven\_Project\_Test\_\_2015\-09\-14\_16\-46\-38
        * Caution) Prohibited characters are specified for a version: space is replaced by underscore (_ ), and ASCII special characters, except () - _ ~ . and , are deleted.
* description
    * Add description on uploads.
* include filter
    * Define include filter, by using ant-style glob pattern.
        * e.g.) **/*.java,**/*.xml - includes java, xml files of all lower directories.  
* exclude filter
    * Define exclue filter, by using ant-style glob pattern. 
        * e.g.) **/.svn/** \- excludes files and directories with  \, \.svn, of all path names in lower directories. 
* compression level
    * Set the compression level in three stages: 
        * Store: Non-compression, Normal: Normal Compression, Maximum: Maximum Compression 

**[ Example of Server Type Application Upload Build Output on Console ]**

```
[TCDeploy] Artifact id: [artifact id output]
[TCDeploy] App key: [application key output]
[TCDeploy] Application type: server
[TCDeploy] Description: Test deployment
[TCDeploy] Upload address: [Upload address output]
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

Result in user input information, such as artifact ID and appkey, as well as files included in compressed files. 

#### Plugin Setting for Client Type Application Uploads 

User-specified binaries of successful build results from client-type application upload plugin can be uploaded to TCD server. 

![05.png](http://static.toastoven.net/prod_tcdeploy/devguide/05.png)

* enable upload
    * Decide whether to enable/disable plugin operations. It is useful to disable plugin operations, while plugin setting is saved
* app key
    * Application key for authentication.
* artifact id
    * Artifact ID: only numbers are allowed. 
* binary group key
    * Key for the binary group to be uploaded: only numbers are allowed.
* publish path
    * It is the path to be deployed to, from the workspace where build is completed. The example shows /, and in this case, what follows  JENKINS\_HOME/jobs/projectname/workspace becomes the path to upload, and if you enter target, then what follows JENKINS\_HOME/jobs/projectname/workspace/target becomes the path.
* binary kind
    * Categories upload binaries into Android, iOS, and others (etc).
* (ipa/apk/etc/plist) file path
    * File name including path of the binary file to upload. 
        * Unlike Android and iOS, other (etc) binaries have restrictions on extension. 
        * For iOS, .ipa binary files and .plist metafiles must be uploaded altogether. 
        * Allows file paths outside of the build workspace as well, unlike server-type upload tasks.
        * An expression like ${WORKSPACE}/target/a.ipa is available, as direct access path expression to workspace.
* binary version
    * Binary version: processed the same as server application version. 
* description
    * Add description for upload. 

**[ Example of iOS Client Type Application Upload Build Output on Console]**

```
[TCDeploy] Artifact id: [artifact id output]
[TCDeploy] App key: [application key output]
[TCDeploy] Application type: client
[TCDeploy] Description: Upload of client program test 
[TCDeploy] Upload address: [Upload address output]
[TCDeploy] Binary type: iOS
[TCDeploy] Binary version: IOS_59__build_iOS_Project_Test__2015-09-16_11-13-12
[TCDeploy] iOS binary file path: /tcdUploadTest/a.ipa
[TCDeploy] iOS meta file path: /tcdUploadTest/a.plist
[TCDeploy] ------------------------------- Upload Start -------------------------------
[TCDeploy] -------------------------------- Upload End --------------------------------
Finished: SUCCESS
```

Result in user input information, such as artifact ID and appkey, as well as binaries and metafiles to upload. 

## Jenkins-CLI Build Profile

It is the pre-defined profile supporting users to execute build commands by using Jenkins-CLI. 

#### Preparations 

1. Create SSH Pubilc/Private Key Pair 
    * Create SSH public/private key pair with ssh-keygen or PuTTY Key Generator, and save SSH public key in the Jenkins build execution account,   and SSH private key on the task execution server. 
        * Only the SSH private keys which are not configured with passphrases are supported.  
    * e.g.) Access http://[JENKINS_URL]/user/[user name]/configure, and save SSH public key created on SSH Public Keys, and save SSH private key files that are created on a user-defined path on the task execution server.  

2. Adjust http keep alive timeout 
    * Check http keep alive timeout at a Jenkins server, and adjust the value.
    * e.g) If Jenkins is installed on RPM,
        * Add httpKeepAliveTimeout=[appropriate milliseconds] option to JENKINS_ARGS of /etc/sysconfig/jenkins.

3. Regarding Stream Exceptions 
    * In case java.io.StreamCorruptedException occurs during build output is displayed on console, add -Dhudson.diyChunking=false option to JVM option which is for Jenkins.   
    * e.g.) If Jenkins is installed on RPM,
        * Add Dhudson.diyChunking=false to JENKINS\_JAVA\_OPTIONS of /etc/sysconfig/jenkins.

#### Setting Profile 

![06.png](http://static.toastoven.net/prod_tcdeploy/devguide/06.png)

**[ User Inputs ]**

* Select OS 
* RunAs 
    * Enter execution account.
* JenkinsServer
    * Enter domain or IP. 
* Server Charset
    * Enter collection of characters of a server to run Jenkins-CLI: default is UTF-8, if it is left empty.  
* Jenkins Url
    * Enter Jenkin server address to run build commands. 
* Jenkins Charset
    * Enter collection of characters of a Jenkins server: default is UTF-8, if it is left empty.  
* Java Home
    * Enter home path of JDK or JRE to perform Jenkins-CLI.  
* Job Name
    * Enter name of a build job to perform.  
* Private Key Path
    * Enter saving path of the SSH private key file to authenticate Jenkins user with SSH public key setting.
* User (required value for 2.46 or higher versions)
  * Enter ID with public key setting.
  	* Reference
  		* 2.46 or Earlier Versions
  			* Automatically authenticated at Jenkin on the public key setting. 
  		* 2.46 or Later Versions
  			* Added with User (Jenkins admin ID), and authenticated only when public key is set for the ID. 
* Build Parameter (optional)
    * Enter parameters required for a build.
