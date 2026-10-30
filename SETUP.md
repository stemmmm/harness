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
- 과제 안내에 지정된 구조나 문서 형식이 있으면 그것을 우선

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
- 이 시점은 기본값이며, 이후 합의나 진행 중에 맞지 않는 부분이 드러나면 언제든 같은 절차로 다시 조정

## 2. 기록 폴더 준비

- 작업 폴더에 docs/ 생성
- templates/HANDOFF.md로 docs/HANDOFF.md 생성
- templates/DECISIONS.md로 docs/DECISIONS.md 생성
- 현재 단계를 1단계로 기록

## 3. Git 정리

- Git 저장소가 아니면 초기화 여부를 사용자에게 묻기
- 하네스 파일(AGENTS.md, SETUP.md, WORKFLOW.md, templates/, Skill 경로의 skills, docs/)만 별도로 커밋
- 커밋 메시지: chore: 작업 하네스 추가
- 커밋 전 대상 파일과 메시지를 보여주고 사용자 확인을 받은 뒤 커밋

## 4. Playwright CLI 설치

- 설치 후 동작 확인
- Chrome 없이 Chromium만 있는 환경에서는 기본 설정(Chrome 채널)으로 브라우저가 열리지 않으므로, 작업 폴더에 `.playwright/cli.config.json`을 만들어 Chromium을 지정
  - 예: `{"browser":{"browserName":"chromium","launchOptions":{"executablePath":"<Chromium 실행 파일 경로>"}}}`
- `file://` 주소는 열리지 않으므로 화면 확인은 서버를 띄운 뒤 http 주소로 진행
- 실패하면 대안으로 Playwright MCP 설정 JSON을 제시 (사용자가 MCP 탭에 붙여넣고 저장)
- MCP는 Codex 재시작 후 적용되므로, 재시작은 완료 후 한 번에 진행

## 5. Java 환경

- JDK와 Gradle이 있는지, 버전은 무엇인지 확인
- 있으면 그대로 쓰고, 없으면 설치할지 사용자에게 물음 (JDK는 21)
- Spring Boot와 Gradle 버전은 JDK에 맞춰 선택
- 작업 폴더 밖 임시 위치에 빈 Spring Boot 프로젝트를 만들어 0.0.0.0으로 띄우고, 내장 브라우저에서 확인할 포트와 경로를 사용자에게 알려줌
- 사용자 확인 후 테스트 서버 종료
- 안 되면 과제에 적합한 대안 스택을 근거와 함께 제시하고 사용자 결정을 기다림
- 확인한 환경, 선택한 스택과 버전, 선택 근거, 대안 전환 여부를 docs/DECISIONS.md에 기록 (Java/Spring은 사용자가 익숙해 결과물을 직접 검증하고 설명할 수 있어 기본으로 사용)

## 완료

- 모든 단계가 끝나면 docs/HANDOFF.md에 SETUP 완료와 확인한 환경(JDK, Gradle, Spring Boot 버전, Playwright 방식)을 기록
