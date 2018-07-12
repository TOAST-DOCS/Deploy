## Dev Tool > Deploy > Console Guide

이 문서에서는 예제로서 다음과 같은 내용을 다룹니다.

* [상품 활성화](/Dev%20Tool/Deploy/en/console-guide/#_1)
* [Appkey와 URL 확인](/Dev%20Tool/Deploy/en/console-guide/#appkey-url)
* [Client Application](/Dev%20Tool/Deploy/en/console-guide/#client-application)
* [Server Application](/Dev%20Tool/Deploy/en/console-guide/#server-application)

(본 예제에서 다루지 않은 기능은 [기능 상세 가이드](/Dev%20Tool/Deploy/en/reference/)에서 확인하실 수 있습니다.)

[참고사항]
![[그림 1] 참고 사항 - 입력 값](http://static.toastoven.net/prod_tcdeploy/getstarted/01.png)
<center>[그림 1] 참고 사항 - 입력 값</center>

입력 값에 대한 설명은 아래와 같은 형태로 이미지에 표시됩니다.
- 필드 색상 : 옅은 파란색
- 입력 값 설명 : 설명 + (필수 또는 선택 값 여부)

## 상품 활성화

Console에 접속하여 Deploy를 활성화합니다.

```
[서비스 선택] > [Deploy] > 서비스 활성화 팝업에서 [확인] 클릭
```

> [주의] 
> 서비스 비활성화 문의 팝업에서 확인을 누를 경우 서비스 이용이 종료되며, 발급된 Appkey는 복구되지 않으니 주의해주시기 바랍니다.

## Appkey와 URL 확인

상품을 활성화한 후, Console 페이지 상단의 [URL & Appkey] 를 클릭하여 발급된 Appkey를 확인합니다.

```
[Deploy] > [URL * AppKey] 클릭
```

![[그림 2] URL & Appkey](http://static.toastoven.net/prod_tcdeploy/getstarted/02-1.png)
<center>[그림 2] URL & Appkey</center>

## Client Application

클라이언트 어플리케이션을 위한 설정은 크게 아티팩트 설정과 바이너리 업로드 단계를 거칩니다.

### 아티팩트 설정

![[그림 3] Client - 아티팩트 설정](http://static.toastoven.net/prod_tcdeploy/getstarted/getstarted_client_artifact.png)
<center>[그림 3] Client - 아티팩트 설정</center>

1. 리스트 상단의 <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭
2. 유형을 "Client Application"으로 선택
3. 이름(필수), 설명(선택), port(필수) 입력
4. <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭

### 바이너리 설정

#### 업로드

* iOS는 ipa, plist 파일을, Android는 apk 파일을 각각 업로드합니다.
* etc의 경우는 Windows 등의 기타 OS의 설치 어플리케이션 용도로 사용합니다.

```
[Deploy] > 하단 탭 중 [바이너리 그룹] > [업로드] 클릭
```

![[그림 4] Client - 바이너리 업로드](http://static.toastoven.net/prod_tcdeploy/getstarted/04.png)
<center>[그림 4] Client - 바이너리 업로드</center>

1. <img class="img-inline" alt="upload-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-g.png"> 클릭
2. <img class="img-inline" alt="fileselect-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-b.png"> 클릭 후 바이너리 파일 선택
    * iOS : ipa 파일(필수), plist 파일(필수) 선택
        * plist : 다운로드 페이지에서 설치를 위해 사용. 파일 내의 다운로드 URL은 선택 입력
    * Android : apk 파일(필수) 선택
3. 버전(선택), 설명(선택) 정보 입력
4. <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭

#### 배포

특정 바이너리를 SMS / E-mail로 배포할 수 있습니다.

```
[Deploy] > 하단 탭 중 [바이너리 그룹] > 바이너리 파일 선택 > [전송] > 다운로드 전송 타입 및 수신자 선택 > [전송] 클릭
```

![[그림 5] Client - 바이너리 배포](http://static.toastoven.net/prod_tcdeploy/getstarted/05.png)
<center>[그림 5] Client - 바이너리 배포</center>

1. 전송할 버전을 선택하고 <img class="img-inline" alt="send-w.png" src="http://static.toastoven.net/prod_tcdeploy/btn/send-w.png"> 클릭

    ![[그림 6] Client - 다운로드 경로 전송](http://static.toastoven.net/prod_tcdeploy/getstarted/06.png)
    <center>[그림 6] Client - 다운로드 경로 전송</center>

2. 다운로드 경로 전송 팝업에서 전송 유형과 수신자를 선택하고 <img class="img-inline" alt="send-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/send-b.png"> 클릭
    * SMS / E-mail로 일부 또는 모두 지정 가능
3. 지정한 전송 유형으로 수신자에게 다운로드 페이지가 전달 됩니다.

## Server Application

서버 어플리케이션의 배포는 설정(아티팩트, 서버 그룹, 시나리오), 바이너리 업로드, 배포의 단계를 거칩니다.

### 아티팩트 설정

![[그림 7] Server - 아티팩트 설정](http://static.toastoven.net/prod_tcdeploy/getstarted/07.png)
<center>[그림 7] Server - 아티팩트 설정</center>

1. 리스트 상단의 <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭
2. 유형을 "Server Application"으로 선택
3. 이름(필수), 설명(선택), port(필수) 입력
4. <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭

### 서버 그룹 설정

배포할 서버를 관리할 수 있는 기능입니다.

```
[Deploy] > 하단 탭 중 [배포] > [서버 그룹 생성] 클릭
```

![[그림 8] Server - 서버 그룹 생성 팝업](http://static.toastoven.net/prod_tcdeploy/getstarted/getstarted_servergroup_create.png)
<center>[그림 8] Server - 서버 그룹 생성 팝업</center>

1. <img class="img-inline" alt="servergroupcreate-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/servergroupcreate-g.png"> 클릭
2. 이름(필수), 설명(선택) 입력
3. OS 선택 후 Shell Type 지정 (항목 선택 또는 직접 입력)
4. Phase 선택 (서버 장비 구분. 지정하지 않을 경우 NONE 선택)
5. 서버 추가
    * 서버 추가에는 아래 두가지 방법이 있으며 자세한 내용은 [기능 상세 가이드의 서버 그룹 메뉴](/Dev%20Tool/Deploy/en/reference/#_11)에서 확인하실 수 있습니다.
        * 대량 추가
        * 개별 추가
            
            ![[그림 9] Server - 서버 개별 추가](http://static.toastoven.net/prod_tcdeploy/getstarted/09.png)
            <center>[그림 9] Server - 서버 개별 추가</center>
            
            * 호스트 이름(필수), IP 주소(필수), OS(선택) 입력 후 <img class="img-inline" alt="add-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/add-b.png"> 클릭
            
            ![[그림 10] Server - 추가된 서버 확인](http://static.toastoven.net/prod_tcdeploy/getstarted/10.png)
            <center> [그림 10]Server - 추가된 서버 확인 </center>
            
            * 하단 서버 리스트에 추가된 내용 확인 (왼쪽 체크 박스에 체크된 서버만 등록됨)
            
6. <img class="img-inline" alt="create-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-b.png"> 클릭

### 시나리오 생성

```
[Deploy] > 하단 탭 중 [배포] > 서버 그룹 영역에서 [새로 만들기] 클릭
```

![[그림 11] Server - 시나리오 생성](http://static.toastoven.net/prod_tcdeploy/getstarted/11.png)
<center>[그림 11] Server - 시나리오 생성</center>

1. <img class="img-inline" alt="newcreate-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/newcreate-g.png"> 클릭
2. 하단에 추가된 시나리오 영역에 시나리오명(선택) 입력
3. <img class="img-inline" alt="create-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/create-g.png"> 클릭

### 태스크 추가

태스크는 개별 기능 수행 및 순서 제어가 가능한 시나리오 구성 요소 입니다.
아래와 같이 두 가지 종류가 있습니다.

* pre-run Task : 배포 전 실행 기능
* Normal Task : 배포 시 실행 기능

필요에 따라 선택적으로 사용할 수 있으며, 본 문서에서는 기본적인 배포 시 필요한 태스크를 다룹니다.
더 많은 태스크는 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/en/reference/#_25)에서 확인하실 수 있습니다.

```
[Deploy] > 하단 탭 중 [배포] > 시나리오 영역에서 [Task 추가] 클릭
```

배포를 위해 아래 세 개의 태스크를 추가합니다.

#### 1. User Command 추가

* 배포 시 실행되는 사용자 정의 Command 태스크
* Available Variables를 사용할 수 있음
    * Available Variables : 예약어. 자세한 내용은 [기능 상세 가이드의 태스크 메뉴](/Dev%20Tool/Deploy/en/reference/#_25) 하단에서 확인하실 수 있습니다.

![[그림 12] Server - User Command 추가](http://static.toastoven.net/prod_tcdeploy/getstarted/12.png)
<center>[그림 12] Server - User Command 추가</center>

1. <img class="img-inline" alt="taskadd-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/taskadd-g.png"> 클릭
2. Normal Task > User Command 클릭

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* RunAs 입력
    * 실행 계정을 입력합니다.
* 실행할 Command 입력
* <img class="img-inline" alt="apply-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/apply-g.png"> 클릭

#### 2. Binary Deploy 추가

* 업로드한 바이너리 파일의 배포 내용을 설정할 수 있는 태스크

![[그림 13] Server - Binary Deploy 추가](http://static.toastoven.net/prod_tcdeploy/getstarted/13.png)
<center>[그림 13] Server - Binary Deploy 추가</center>

1. <img class="img-inline" alt="taskadd-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/taskadd-g.png"> 클릭
2. Normal Task > Binary Deploy 클릭

**[ 사용자 입력 내용 ]**

* Timeout(min)
    * 해당 태스크가 실행 완료 대기 시간으로 인식할 시간 값을 지정합니다. (최소 1분, 최대 30분)
* RunAs 입력
    * 실행 계정을 입력 합니다.
* 바이너리
    * 업로드 후 사용할 수 있습니다.
        * 업로드 할 테스트 파일을 준비합니다.
            * deploy.zip
        * <img class="img-inline" alt="upload-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-g.png"> 클릭
        
            ![[그림 14] Server - Binary 업로드 팝업](http://static.toastoven.net/prod_tcdeploy/getstarted/14.png)
            <center>[그림 14] Server - Binary 업로드 팝업</center>
            
            * <img class="img-inline" alt="fileselect-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/fileselect-b.png"> 클릭 후 바이너리 파일 선택
            * 버전(선택), 설명(선택) 입력
            * <img class="img-inline" alt="upload-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/upload-b.png"> 클릭
        
            ![[그림 15] Server - Binary 업로드 팝업](http://static.toastoven.net/prod_tcdeploy/getstarted/15.png)
            <center>[그림 15] Server - Binary 업로드 팝업</center>
        
            * <img class="img-inline" alt="confirm-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/confirm-b.png"> 클릭
        
            ![[그림 16] Server - 선택된 Binary](http://static.toastoven.net/prod_tcdeploy/getstarted/16.png)
            <center>[그림 16] Server - 선택된 Binary</center>
            
            * 바이너리가 선택 됨.
            
* Variable As
    * 해당 바이너리의 Variable명을 지정해 User Command에서 바이너리 정보를 사용할 수 있으며 자세한 내용은 [기능 상세 가이드](/Dev%20Tool/Deploy/en/reference/)의 태스크 메뉴 하단에서 확인하실 수 있습니다.
* 타겟 디렉토리
    * 바이너리를 배포할 디렉토리를 지정합니다.
* <img class="img-inline" alt="apply-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/apply-g.png"> 클릭

#### 3. User Command 추가

![[그림 17] Server - User Command 추가 2](http://static.toastoven.net/prod_tcdeploy/getstarted/17.png)
<center>[그림 17] Server - User Command 추가 2</center>
       
* 필요 값 입력 후 <img class="img-inline" alt="apply-g.png" src="http://static.toastoven.net/prod_tcdeploy/btn/apply-g.png"> 클릭

### 실행

1. <img class="img-inline" alt="deploy-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/deploy-b.png"> 클릭으로 배포 요청

    ![[그림 18] 배포 내용 확인 팝업](http://static.toastoven.net/prod_tcdeploy/getstarted/18.png)
    <center>[그림 18] 배포 내용 확인 팝업</center>

2. 배포 노트(선택) 및 인증 방법 지정 후 인증 값 또는 파일(필수) 입력
    * Password 선택 후 비밀번호 입력 또는 pemFile 선택 후 파일 업로드
    * <img class="img-inline" alt="confirm-b.png" src="http://static.toastoven.net/prod_tcdeploy/btn/confirm-b.png"> 클릭

3. 배포 진행 상황 확인

    ![[그림 19] Server - 배포 진행상황 확인](http://static.toastoven.net/prod_tcdeploy/getstarted/19.png)
    <center>[그림 19] Server - 배포 진행상황 확인</center>

4. 배포 완료 확인

    ![[그림 20] Server - 배포 완료 확인](http://static.toastoven.net/prod_tcdeploy/getstarted/20.png)
    <center>[그림 20] Server - 배포 완료 확인</center>

    * 각 Task 정상 실행 여부는 exit code로 판별됩니다.

5. 배포 결과 확인
    * '결과 보기' 클릭 시 각 태스크 실행에 대한 상세 내용 (리턴 값, exit code, 오류 내용 등) 확인 가능

    ![[그림 21] Server - 배포 결과 보기 팝업](http://static.toastoven.net/prod_tcdeploy/getstarted/21.png)
    <center>[그림 21] Server - 배포 결과 보기 팝업</center>

- - -

서버에 파일 배포를 성공하였습니다!
Toast Cloud Deploy는 더 많은 기능을 지원하며, 자세한 사항은 [기능 상세 가이드](/Dev%20Tool/Deploy/en/reference/)에서 확인하실 수 있습니다!