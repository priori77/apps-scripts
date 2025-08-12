
# 테스트 자동화 리포트
Google Apps Script를 이용하여 QA 테스트 리포트를 자동 생성&배포하고, Slash Command(/qa-status)로 실시간 조회할 수 있도록 구현했습니다.

Webapp.gs
1. Slack Slash Command(/qa-status) 요청 처리
2. Google Sheets에서 테스트 데이터 조회 (URL: https://docs.google.com/spreadsheets/d/1tL6D3hy-4OPpmewULNZnccOle_u0fKf7KbZUTg1wGsI/edit?usp=sharing)
3. 응답을 Slack 메시지 형식(JSON)으로 출력

Code.gs
1. 매일 오전 8시 자동 실행되는 Daily Report 생성 및 Slack 전송
2. Google Sheets로부터 테스트 현황 데이터 파싱
3. 전일 대비 변화량 비교 및 시각화
4. 카테고리별 테스트 현황 블록 메시지 생성

Setup.gs
1. 스크립트 실행 트리거 설정 - `sendDailyReport` 매일 8시 실행 - AtHour()
2. 저장된 이전 데이터 초기화 및 조회 유틸리티 제공

# 함수 설명

Daily Report 자동 발송
1. 스프레드시트에서 데이터 수집 → Slack Webhook 채팅 전송
2. Pass Rate에 따른 상태 이모지(95>=Pass Rate 녹색, 80>= Pass Rate 노라색) 표시
3. 전일 대비 변화량 요약

Slash Command 지원
1. /qa-status → 기본 테스트 현황
2. /qa-status detail` → 카테고리별 상세 현황

데이터 관리
1. PropertiesService로 전일 데이터 저장/비교
2. 초기화(clearPreviousData) 및 데이터 확인(checkStoredData)가능


사용 방법
- 구글 스프레드시트 & AppsScripts 세팅
1. Google Spreadsheet 생성 및 스프레드 시트 설정
2. [스프레드 시트 > 확장 프로그램 > AppsScripts] 메뉴로 이동
2. `Webapp.gs`, `Code.gs`, `Setup.gs` 파일 업로드
3. `SPREADSHEET_ID`, `SHEET_NAME`, `SLACK_WEBHOOK_URL`, `SLACK_BOT_TOKEN` 값 및 데이터 범위 설정
4. `setupTriggers()` 실행하여 매일 8시 실행 트리거 생성
5. Web App 배포 후 Slack Slash Command에 연결

- Slack 설정 (/qa-status)
1. https://api.slack.com/apps 접속
2. Create New APP > From scratch 선택 후 App 생성
3. OAuth & Permissions 진입
4. Scopes > Add an OAuth Scope 클릭 후 chat:write, chat:write.public, commands, incoming-webhook 등록 및 토큰 복사 ('SLACK_BOT_TOKEN)