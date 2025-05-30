## Dev Tools > Deploy > 릴리스 노트

### 2025. 04. 29.
#### 기능 추가
* 배포 실행 API Version 2.0 추가
    * Auto Scale 서버 그룹도 배포 실행 가능

#### 기능 개선
* Cloud Agent 아티팩트 Command Type에서도 Auto Scale 서버 그룹 생성 가능하도록 변경
* API로 바이너리 업로드 시 용량 제한을 2GB에서 4GB로 변경

### 2025. 03. 25.
#### 기능 개선
* 바이너리 그룹 목록 노출 시 이름을 기준으로 정렬하도록 변경

### 2025. 01. 21.
#### 버그 수정
* 시나리오 내용이 잘못된 경우 에러 페이지로 이동하는 오류 수정
* IAM 계정으로 배포 실행 API 호출 시 실행자에 email이 아닌 아이디를 표기하도록 수정

### 2024. 11. 26.
#### 기능 개선
* 최근 5년간의 배포 이력만 조회 가능하도록 변경

### 2024. 09. 10.
#### 기능 개선
* 미국(캘리포니아) 리전의 NHN Cloud Instance에 NHN Cloud Agent를 통한 배포 기능 지원
* Jenkins Plugin의 버전이 업데이트되었습니다(버전 1.1.3).
    * 바이너리 그룹 키가 빈 값인 경우 Default 바이너리 그룹에 업로드되도록 수정
    * 엔드포인트의 기본 값이 https://api-tcd.cloud.toast.com에서 https://api-tcd.nhncloudservice.com으로 변경

### 2024. 07. 09.
#### 기능 개선
* **바이너리 그룹** 탭 내 정렬 기능 추가
* 클라이언트 바이너리 배포 UI 개선
    * 알림 수신 그룹으로도 다운로드 경로 전송 가능
* **배포 이력** 탭에서 **실행자**를 클릭해 이름을 확인할 수 있는 기능 추가

### 2024. 06. 11.
#### 버그 수정
* 오토스케일 서버 그룹을 수정할 수 없는 오류 개선

### 2024. 03. 26.
#### 기능 개선
* NHN Cloud Instance 배포 시 SSH를 통한 배포 외 NHN Cloud Agent를 통한 배포 기능 추가
    * Instance에 Floating IP를 할당하지 않아도 배포 가능

### 2024. 02. 27.
#### 기능 개선
* 서버 그룹 수정 UI 개선
* 배포 실행 API 추가
* 알림 메일 수신 대상 설정 기능 추가
    * 조직/프로젝트 대시보드 > 알림 관리에서 수신 메일 주소명을 설정할 수 있도록 기능 추가

### 2024. 01. 23.
#### 기능 개선
* 바이너리 그룹 생성 및 수정 시 자동 삭제 정책을 설정하도록 변경

### 2023. 11. 28.
#### 기능 개선
* Jenkins Plugin의 버전이 업데이트되었습니다(버전 1.1.2).
  * Windows 환경에 설치된 Jenkins에서 Deploy로 업로드되지 않는 오류 수정
* API로 바이너리 업로드 시 용량 제한을 1GB에서 2GB로 변경
#### 버그 수정
* 리소스 파일 시간 정보가 노출되지 않는 오류 수정

### 2023. 06. 27.
#### 버그 수정
* Windows Server 2019 Standard에서 바이너리가 배포되지 않는 오류 수정

### 2023. 05. 30.
#### 기능 개선
* Jenkins Plugin의 버전이 업데이트되었습니다(버전 1.1.1).
    * Jenkins Agent 노드에서 Deploy로 업로드 안되는 오류 수정

### 2023. 04. 25. 
#### 버그 수정
* 배포 시나리오 수정 시 파일 선택 창에서 파일 목록이 10개 이상 로딩되지 않는 버그 수정
* 콘솔에서 리소스 파일 업로드 실패 시 파일 조회 안되는 문제 수정 및 **업로드 취소** 버튼 추가

### 2022. 12. 27.
#### 기능 개선
* **배포 이력** 탭에서 다운로드할 콘텐츠(Excel 파일)의 Row 수 표시

### 2022. 10. 25.
#### 기능 개선
* 실행 가능 확인 기능 개선
* Task가 없는 시나리오는 실행할 수 없도록 수정

### 2022. 10. 04.
#### 기능 개선
* Deploy 권한 세분화 기능 추가
* Deploy ADMIN 권한 사용자에게만 배포 실행 통지를 제공하도록 수정
* 시나리오 및 Task 이름의 글자 수 제한을 30자에서 50자로 변경
#### 버그 수정
* Task 및 아티팩트 생성 내 가이드 링크 오류 수정
* Deploy가 종료되지 않고 Deploying 상태가 지속되는 오류 수정

### 2022. 08. 23.
* API 엔드포인트의 도메인이 api-tcd.cloud.toast.com에서 api-tcd.nhncloudservice.com으로 변경되었습니다.

### 2022. 07. 26.
#### 기능 개선
* 바이너리 업로드 실패 시 안내 메시지 추가
#### 버그 수정
* 삭제된 바이너리 그룹에 업로드할 수 없도록 수정

### 2022. 04. 26.
#### 기능 개선
* Jenkins API Build로 Jenkins Pipeline Job 실행 시 실행 완료를 기다리도록 개선
* 시나리오 업로드 시 파일명 제한 삭제
#### 버그 수정
* Windows 서버 Log Monitoring 버그 수정

### 2022. 03. 29.
#### 기능 개선
* 아티팩트 목록 검색 시 사용 쿼리 개선

### 2022. 01. 25.
#### 기능 개선
* CloudTrail 서비스 연동
    * CloudTrail에서 Deploy 콘솔에서 발생한 **오토스케일 배포 실행** 사용자 이벤트 확인 가능

### 2021. 12. 28.
#### 버그 수정
* Task 내 가이드 링크 오류 수정

### 2021. 10. 26.
#### 버그 수정
* 요금 미납 사용자가 서비스 정상 이용 가능했던 버그 수정

### 2021. 07. 27.
#### 기능 개선
* 배포 이력 조회 추출 기능 개선
    * pre-run task만 존재하는 배포 이력도 포함되도록 수정
* Auto Scale 서비스 연계 배포 실패 시 인스턴스 폐기 기능 연동
    * Auto Scale 서비스에서 스케일 아웃을 진행하다 배포에 실패하면 스케일 아웃된 인스턴스 폐기
    * 3회 이상 스케일 아웃 배포 실패 시 스케일 아웃 기능 중지
* 보안 취약점 패치

### 2021. 06. 29.
#### 기능 개선
* **배포 이력** 탭 검색 조건 변경
    * 서버 그룹 및 실행일(시작일~종료일) 조건으로 배포 이력 검색
    * 실행일 총 검색 기간 1년 제한
* 배포 이력 결과 Excel 파일로 다운로드 기능 추가
    * 바이너리 파일 없는 경우 제외하여 다운로드 가능하게 선택 옵션 추가

### 2021. 03. 23.
#### 버그 수정
* 클라이언트 바이너리 전송 대상 목록에 프로젝트 전체 멤버가 노출되지 않는 현상 수정

### 2021. 02. 23.
#### 기능 개선
* 오토스케일 관련 기능 일본어 번역 추가
#### 버그 수정
* 간헐적 오토스케일 배포 실패 오류 수정

### 2021. 01. 26.
#### 버그 수정
* 배포 이력 탭의 2020년 데이터 확인 오류 수정

### 2020. 11. 24.
#### 기능 개선
* Auto Scale 서비스 연계 배포 기능 추가(US 리전 제외)
    * Auto Scale 타입의 서버 그룹 생성 및 시나리오 매핑 기능
    * Auto Scale 그룹에 대한 사용자 배포 기능
* 배포 실행 확인 기능 추가
    * 배포 실행 전에 배포 실행 여부 확인

### 2020. 08. 25.
#### 기능 개선
* 로그 모니터링 태스크 시작, 종료 스탬프(stamp)에 시간 정보 표시
* TOAST 서비스별로 권한에 따라 제어 가능

### 2020. 04. 28.
#### 기능 개선
* Https 페이지 내 http 리소스 참조 제거

### 2020. 03. 24.
#### 기능 개선
* TOAST Trail 서비스 연동
    * TOAST Trail에서 Deploy 콘솔에서 발생한 사용자 이벤트 확인 가능

### 2020. 02. 25.
#### 기능 개선
* 아티팩트 생성 시 기본 바이너리 그룹 리젼 설정 기능 추가
    * 아티팩트 생성 시 바이너리 그룹 리전 지정 가능(KR1/JP1)
* 아티팩트 설정 시 기본 바이너리 그룹 지정 기능 추가
    * 아티팩트 내 바이너리 그룹 중 선택
#### 버그 수정
* 시나리오 업로드 시 Binary Task의 잘못된 바이너리 그룹 키 설정 문제 수정

### 2019. 12. 24.
#### 기능 개선
* 바이너리 그룹의 리전 추가
    * 바이너리 생성 시 지정
    * 바이너리 다운로드 및 배포 시 연동되는 리전의 스토리지에서 다운로드
* 클라이언트 다운로드 페이지 만료 시간 적용

### 2019. 09. 24.
#### 기능 개선
* 네트워크 접근불가로 인한 바이너리 배포실패 메시지 추가

### 2019. 07. 23.
#### 기능 개선
* 바이너리 > iOS 파일 업로드 > plist 파싱 실패 오류에 대한 안내 개선
#### 기능 수정
* sms 발송 컨텐츠의 글자 수가 지정된 크기를 초과하지 않도록 조정

### 2019. 06. 25.
#### 오류 수정
* 바이너리 업로드 시 바이너리 버전이 100글자를 초과할 경우 발생하는 배포 오류 수정
* 바이너리 다운로드 페이지에서, 삭제된 바이너리 다운로드 링크에 접근할 수 있는 오류 수정

### 2019. 05. 28.
#### 기능 개선
* 아티팩트 검색 기능 추가

#### 오류 수정
* 배포 이력 > 결과 보기 창 > user command에 스크립트가 포함되어 있을 경우 발생하는 페이지 오류 수정

### 2019.04.23
#### 기능 개선
* 배포 이력 탭 > 결과 보기 팝업 > 배포 진행 상황이 실시간 업데이트 되도록 개선

#### 오류 수정
* 배포 시 태스크 진행 상황이 표시되지 않는 오류 수정

### 2019.03.26
#### 기능 변경
* 리소스 파일 업로드 용량 제한 (1GB)

### 2019.02.26
#### 기능 개선/변경
* 편집 가능한 리소스 크기 및 내용제한 
    * 기존 : 크기 무제한, 모든 편집가능 형식 파일
    * 변경 : 크기 1MB 제한, unicode로 편집 가능한 형식만 편집 가능
* 태스크 Timeout 제한 시간 증가
    * 기존 : 최대 30분
    * 변경 : 최대 2시간

#### 오류 수정
* 서버그룹 수정시 서버그룹 전체 선택 체크박스 오작동 현상 수정

### 2019.01.15
#### 기능 개선/변경
* User Console Org 인증 추가

### 2018.10.23
#### 기능 추가
* 배포 재 진입 이후 배포 종료 기능 활성화(배포자 한정)

#### 오류 수정
* 리소스 탭 > 파일 추가 > 파일 내용이 없어도 파일이 생성되는 현상 수정

### 2018.08.28
#### 기능 추가
* 배포 재 진입 기능
* 페이지 로딩을 안내하는 레이어 추가
* 바이너리 업로드 API Ver1.0 업데이트
    * 이전 버전과 동시 운영

#### 오류 수정
* 배포 시 인증 방법 항목 동작 오류 수정
    * pem 파일 필수값 체크 추가
    * 패스워드/pem 전환 라디오 버튼 클릭 시 input 타입이 전환되지 않는 문제

#### 문서 변경
* 바이너리 업로드 API Ver1.0 업데이트

### 2018.07.24
#### 기능 추가

* 시나리오 Import/Export 기능 추가
* Client Type의 메뉴 사용 제한 해제 (모든 메뉴 사용 가능)
* 서버 그룹 장비 구분을 위한 Phase 속성 추가
    * Phase Type이 Product인 서버의 배포 시 확인 단계 추가

### 2018.04.24
#### 기능 추가

* 비밀번호 입력을 통한 클라이언트 어플리케이션의 바이너리 설치 페이지 접근 제어 기능 추가

### 2018.02.22
#### 신규 상품 출시

* 배포 및 설치 편의를 제공하기 위한 서비스 입니다.
* 클릭 한 번으로 쉽고 빠르게 소프트웨어를 배포할 수 있습니다.
