# harness

AI 코딩 에이전트와 협업할 때 사용하는 개인 작업 하네스.

## 구성

- AGENTS.md
- SETUP.md
- WORKFLOW.md
- skills/
- templates/

## 사용법

1. 이 레포는 작업 폴더 밖에 clone한다. 작업 폴더 안에 clone하면 Git 저장소가 중첩되므로, 이미 안에 clone했다면 밖으로 옮긴다.
2. README.md와 .git을 제외한 파일을 작업 폴더 루트로 복사한다. 단, skills/ 안의 각 Skill 폴더는 루트가 아니라 Codex의 프로젝트 범위 Skill 경로인 `.agents/skills/`로 복사한다 (예: `.agents/skills/handoff/SKILL.md`).
   - 근거: Codex 저장소의 Skill 테스트가 프로젝트 Skill을 `<작업 폴더>/.agents/skills/<이름>/SKILL.md`에 만든다 (openai/codex `codex-rs/core/tests/suite/skills.rs`, 커밋 f2ac9983552d187aa1c9a53c4707cf3b2c4afde6)
3. 복사할 파일이나 폴더와 같은 이름이 작업 폴더에 이미 있으면 덮어쓰지 않는다. 겹치는 목록을 보여주고 어떻게 처리할지 사용자에게 묻는다.
4. 작업 폴더의 SETUP.md를 읽고 한 단계씩 진행한다. 단계마다 결과를 보고하고 사용자 확인을 받은 뒤 다음으로 넘어간다.
5. 완료 후 사용자에게 Codex 재시작이 필요하다고 안내한다. Skill과 AGENTS.md는 재시작 후 적용된다.
