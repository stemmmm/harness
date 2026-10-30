---
name: handoff
description: 세션을 전환하기 전에 HANDOFF.md를 작성하거나, 새 세션을 시작할 때 HANDOFF.md로 맥락을 복원할 때 사용
---

# 세션 전환

## 전환 전 (작성)

- templates/HANDOFF.md 형식으로 docs/HANDOFF.md 갱신 (현재 단계 포함)
- 템플릿의 `<!-- 작성 시 삭제: ... -->` 안내는 내용을 채운 뒤 삭제, 해당 내용이 없는 섹션은 "없음"으로 기록
- 다음 작업은 바로 실행할 수 있을 만큼 구체적으로
- 커밋되지 않은 변경이 있으면 커밋할지 사용자에게 물음
- 실행 중인 서버나 백그라운드 프로세스가 있으면 HANDOFF.md에 기록하고, 종료할지 사용자에게 물음
- 갱신이 끝나면 새 세션을 시작하면 된다고 사용자에게 안내

## 새 세션 시작 (복원)

- docs/HANDOFF.md, docs/agreement.md, docs/PLAN.md 순서로 읽음 (없는 파일은 건너뜀)
- git log와 git status로 최근 커밋과 작업 상태 확인
- HANDOFF.md의 현재 단계로 WORKFLOW.md의 해당 단계 확인
- 현재 단계, 현재 상태, 다음 작업을 짧게 요약해 확인받은 뒤 진행
