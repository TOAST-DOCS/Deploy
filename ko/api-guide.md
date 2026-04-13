## Dev Tools > Deploy > API 가이드

Deploy에서는 바이너리 업로드, 배포 실행을 위한 API를 제공합니다. 사용자가 HTTP 요청을 직접 구성하여 사용할 수 있습니다.  
이 문서는 API 버전별로 분리되어 있습니다. 아래 링크에서 원하는 버전의 문서를 확인하세요.

- [Version 1.0 API 가이드](/Dev%20Tools/Deploy/ko/api-guide-v1.0/)
- [Version 2.0 API 가이드](/Dev%20Tools/Deploy/ko/api-guide-v2.0/)
- [Version 2.1 API 가이드](/Dev%20Tools/Deploy/ko/api-guide-v2.1/)

### 공통 정보
#### 변수별 값 확인 방법

##### appKey 확인 방법
`URL & Appkey` 버튼을 눌러 확인할 수 있습니다.

![deploy_api_01_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_01_202402.png)

##### artifactId 확인 방법
아티팩트 리스트의 아이디 칼럼에서 확인할 수 있습니다.

![deploy_api_02_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_02_202402.png)

##### binaryGroupKey 확인 방법
바이너리 그룹을 선택한 후 Key 값을 확인할 수 있습니다.

![deploy_api_03_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_03_202402.png)
바이너리 그룹에 마우스 포인터를 올려도 Key 값을 확인할 수 있습니다(괄호 번호).

![deploy_api_04_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_04_202402.png)

##### serverGroupId 확인 방법
서버 그룹에 마우스 포인터를 올려서 ID를 확인할 수 있습니다(괄호 번호).

![deploy_api_05_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_05_202402.png)

##### scenarioId 확인 방법
시나리오를 선택하면 scenario ID를 확인할 수 있습니다.

![deploy_api_06_202402.png](https://static.toastoven.net/prod_tcdeploy/deploy_api_06_202402.png)
