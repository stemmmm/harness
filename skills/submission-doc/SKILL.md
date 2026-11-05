---
name: submission-doc
description: 작업 기록을 바탕으로 최종 제출 문서를 작성할 때 사용
---

# 제출 문서 작성

## 입력

- docs/의 기록: agreement.md, PLAN.md, DECISIONS.md, checklist.md, reviews/, screenshots/
- git log
- 테스트 실행 결과

## 절차

- 과제가 제공한 제출 문서 파일이 작업 폴더에 있으면 양식(제목, 질문, 항목 순서)을 그대로 두고 답변만 채움
- 파일은 없고 과제 안내에 양식만 있으면 그 양식을 그대로 옮겨 지정된 위치에 생성
- 둘 다 없으면 어떤 문서를 어디에 쓸지 사용자에게 물음
- 각 항목은 docs/DECISIONS.md에서 관련 구분(설계, 엣지케이스·장애, AI 출력 검증, AI 제안 수정 등)의 항목을 골라 핵심만 옮김
- 공개 자료나 AI 활용을 묻는 항목이 있으면 사용한 하네스 문서(AGENTS.md, WORKFLOW.md, SETUP.md, Skill)와 외부 Skill 출처(각 Skill 폴더의 SOURCE.md) 포함
- 문서의 모든 주장을 실제 코드, 커밋, 테스트 결과와 대조, 확인되지 않는 내용은 삭제하거나 미검증으로 표시
- 대조 결과 불일치 항목을 따로 보고
- 초안을 사용자에게 보여주고 확인받은 뒤 확정
