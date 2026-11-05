# SETUP

README에 따라 파일 복사가 끝난 뒤, 작업 폴더에서 이 문서를 읽고 진행한다.

## 진행 방식

- 단계가 끝나면 결과를 성공 / 실패로 보고하고, 사용자 확인을 받은 뒤 다음 단계로 이동
- 실패하면 원인과 대안을 함께 보고하고 사용자 결정을 기다림
- 불필요해 보이는 단계도 건너뛰지 않고, 이유를 말하고 사용자에게 물음

## 공통 규칙

- 설치 단계가 3분을 넘거나 출력 없이 멈춰 있으면 중단하고 실패로 처리
- 다운로드가 차단되면 미러, 프록시 등 다른 경로는 시도하지도, 대안으로 제시하지도 않음
- 과제의 기존 파일은 수정하지 않음

## 1. 과제 확인과 하네스 맞춤 조정

- 과제 README와 안내 문서에서 요구사항, 지정된 기술 스택, 폴더 구조, 문서명, 제출 형식, 화면 유무 확인
- Git 저장소인지 확인
- 확인한 내용과 하네스(AGENTS.md, WORKFLOW.md, SETUP.md, Skill, templates/)를 대조해 맞지 않는 부분의 조정안 작성
  - Skill: 과제에 필요 없는 Skill 제외, 필요한 Skill 수정이나 추가
  - WORKFLOW: 단계, 검증 방법, 리뷰 대상 조정
  - SETUP: 과제가 지정한 기술 스택이 있으면 5단계를 그 스택으로 대체
  - 템플릿과 문서 형식: 과제가 지정한 문서명과 형식에 맞춤
- 조정안은 항목별 이유와 함께 한 번에 제시하고, 사용자 확인을 받은 뒤 적용 (확인 전에는 읽기만)
- 조정할 것이 없으면 없다고 보고하고 다음 단계로
- 외부 Skill을 수정하면 해당 Skill 폴더의 SOURCE.md에 수정 내역 기록
- Skill을 추가하거나 제외하면 재시작 후 적용되므로, 재시작은 SETUP 완료 후 한 번에 진행
- 적용한 조정은 2단계에서 docs/DECISIONS.md를 만든 뒤 기록

## 2. 기록 폴더 준비

- 작업 폴더에 docs/ 생성
- templates/HANDOFF.md로 docs/HANDOFF.md 생성
- templates/DECISIONS.md로 docs/DECISIONS.md 생성
- 현재 단계를 1단계로 기록

## 3. Git 정리

- Git 저장소가 아니면 초기화 여부를 사용자에게 묻기
- `git config user.name`, `git config user.email`이 비어 있으면 저장소 로컬에만 설정 (`--global` 사용 금지)
  - `git config user.name "Hoyun Jung"`
  - `git config user.email "hoyunjung1201@gmail.com"`
- 하네스 파일(AGENTS.md, SETUP.md, WORKFLOW.md, templates/, Skill 경로의 skills, docs/)만 별도로 커밋
- 커밋 메시지: chore: 작업 하네스 추가

## 4. Playwright CLI 설치

- 설치 후 동작 확인
- Chrome 없이 Chromium만 있는 환경에서는 기본 설정(Chrome 채널)으로 브라우저가 열리지 않으므로, 작업 폴더에 `.playwright/cli.config.json`을 만들어 Chromium을 지정
  - 예: `{"browser":{"browserName":"chromium","launchOptions":{"executablePath":"<Chromium 실행 파일 경로>"}}}`
- `file://` 주소는 열리지 않으므로 화면 확인은 서버를 띄운 뒤 http 주소로 진행
- 실패하면 대안으로 Playwright MCP 설정 JSON을 제시 (사용자가 MCP 탭에 붙여넣고 저장)
- MCP는 Codex 재시작 후 적용되므로, 재시작은 완료 후 한 번에 진행

## 5. 개발 환경

- AGENTS.md의 기본 스택 기준으로 확인
- JDK와 Gradle이 있는지, 버전은 무엇인지 확인
- 있으면 그대로 쓰고, 없으면 설치할지 사용자에게 물음 (JDK는 21)
- Spring Boot와 Gradle 버전은 JDK에 맞춰 선택
- Docker가 있는지 확인하고, MySQL 컨테이너를 띄울 수 있는지 확인 (없으면 설치할지 사용자에게 물음)
- 작업 폴더 밖 임시 위치에 빈 Spring Boot 프로젝트를 만들어 MySQL에 접속한 상태로 0.0.0.0에 띄우고, 내장 브라우저에서 확인할 포트와 경로를 사용자에게 알려줌
- 사용자 확인 후 테스트 서버와 MySQL 컨테이너 종료
- 확인한 환경, 스택과 버전을 docs/DECISIONS.md에 기록 (Java/Spring과 MySQL은 사용자가 익숙해 결과물을 직접 검증하고 설명할 수 있어 기본으로 사용)

## 완료

- 모든 단계가 끝나면 docs/HANDOFF.md에 SETUP 완료와 확인한 환경(JDK, Gradle, Spring Boot, MySQL 버전, Docker, Playwright 방식)을 기록
