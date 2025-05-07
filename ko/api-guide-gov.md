## Dev Tools > Deploy > API 가이드

Deploy에서는 사용자가 HTTP Request를 직접 구성하여 바이너리 업로드, 배포 실행을 위한 API를 제공합니다.  
이 문서는 API 버전별로 분리되어 있습니다. 아래 링크를 통해 원하는 버전의 문서를 확인하세요.

- [Version 1.0 API 가이드](/Dev%20Tools/Deploy/ko/api-guide-v1.0-gov/)
- [Version 2.0 API 가이드](/Dev%20Tools/Deploy/ko/api-guide-v2.0-gov/)

### 공통 정보
#### 변수별 값 확인 방법

##### appKey 확인 방법
* `URL & Appkey` 버튼을 눌러서 확인 가능합니다.

![deploy_api_01_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_01_202402.png)

##### artifactId 확인 방법
* 아티팩트 리스트의 아이디 칼럼에서 확인 가능합니다.

![deploy_api_02_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_02_202402.png)

##### binaryGroupKey 확인 방법
* 바이너리 그룹에서 선택 후 들어가서 Key 값을 확인 가능합니다.

![deploy_api_03_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_03_202402.png)
* 바이너리 그룹에 마우스 오버를 통해서도 확인 가능합니다. (괄호 번호)

![deploy_api_04_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_04_202402.png)

##### serverGroupId 확인 방법
* 서버 그룹에서 마우스 오버를 통해서도 확인 가능합니다. (괄호 번호)

![deploy_api_05_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_05_202402.png)

##### scenarioId 확인 방법
* 시나리오 선택 시 scenario ID를 확인 가능합니다.

![deploy_api_06_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_06_202402.png)