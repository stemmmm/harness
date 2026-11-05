---
name: deployment
description: 과제가 Dockerfile, docker-compose, 환경변수, 헬스체크, DB 초기화 등 배포를 고려한 구성을 요구할 때, 기본 구조 생성이나 배포 파일 작성 시 사용
---

# 배포 구성

- 참고: deepple-dev/deepple-api (커밋 58e401f32fee332d3e232f04624529306e0f2a74)의 Dockerfile, .dockerignore, compose.yml, application*.yml, build.gradle
- 아래는 기본값이며, 과제의 배포 요구사항에 없는 것은 넣지 않음
- 기본 구조를 만들 때 설정, 헬스체크, 스키마 초기화를 먼저 갖추고, Dockerfile과 compose는 기능 구현 전에 한 번 빌드해 확인

## 실행 방식

- Dockerfile은 앱만 빌드하고 실행, MySQL은 Dockerfile에 넣지 않음
- 앱과 MySQL을 함께 띄우는 것은 compose.yml이 담당
- 과제가 실행 방법을 적을 문서를 지정하면 그 문서에, 없으면 README에 `docker compose up --build`로 실행한다고 명시하고, Dockerfile만 단독 실행하면 DB가 없어 기동하지 않는다는 점도 적음

## Dockerfile

- 멀티 스테이지: 빌드는 `eclipse-temurin:21-jdk`, 실행은 `eclipse-temurin:21-jre`
- 빌드 스테이지
  - gradlew, settings.gradle, build.gradle, gradle/ 를 먼저 복사하고 의존성을 받은 뒤 src를 복사 (소스만 바뀌면 의존성 레이어 캐시 재사용)
  - `bootJar` 빌드, 테스트는 이미지 빌드에서 실행하지 않음
  - 계층형 jar로 추출 (`java -Djarmode=tools -jar <jar> extract --layers --launcher`, Spring Boot 3.3 이상)
- 실행 스테이지
  - 레이어를 자주 안 바뀌는 순서로 복사: dependencies → spring-boot-loader → snapshot-dependencies → application
  - root가 아닌 시스템 사용자를 만들어 `USER`로 실행
  - `EXPOSE`는 앱 포트, 실행은 `JarLauncher`
  - JVM 메모리는 `-XX:MaxRAMPercentage=75`로 컨테이너 메모리에 맞춤
- build.gradle에서 plain jar 생성을 끔 (`jar { enabled = false }`), build/libs에 jar가 하나만 생겨 와일드카드 복사가 안전함
- 이미지에 비밀값, .env, 로컬 설정 파일이 들어가지 않게 함 (로컬 전용 설정 파일이 있으면 bootJar에서 제외)

## .dockerignore

- 빌드 산출물(.gradle/, build/), IDE 설정, 로그, OS 파일, 임시 파일
- 비밀값: .env, .env.*, 키 파일
- 앱 빌드와 무관한 것: 문서(*.md, docs/), compose.yml, Dockerfile, .git, 하네스 파일(.agents/ 등)
- gradle-wrapper.jar는 제외하지 않음 (`!gradle/wrapper/gradle-wrapper.jar`)

## compose.yml

- mysql 서비스
  - 이미지는 버전을 고정 (예: `mysql:8.4.x`, latest 금지)
  - DB 이름, 계정, 비밀번호는 .env에서 주입
  - `TZ: Asia/Seoul`
  - 이름 있는 볼륨으로 데이터 보존 (재시작 후에도 데이터 유지 요구 대응)
  - 헬스체크: `mysqladmin ping`
- app 서비스
  - `build: .`, `env_file: .env`, 앱 포트 노출
  - DB 주소는 서비스 이름(`mysql`)으로 지정
  - `depends_on`에 `condition: service_healthy`로 MySQL 준비 후 기동
- 과제에 필요 없는 서비스(캐시, 스토리지, 모니터링 등)는 넣지 않음

## 설정과 환경변수

- application.yml에서 설정값과 비밀값은 `${ENV}` 형태로 주입, 코드와 설정 파일에 실제 값 금지
  - 예: `jdbc:mysql://${MYSQL_HOST}:${MYSQL_PORT}/${MYSQL_DATABASE}`, `${MYSQL_USER}`, `${MYSQL_PASSWORD}`, `server.port: ${SERVER_PORT:8080}`
  - 기본값은 포트처럼 비밀이 아닌 값에만 둠
- 환경변수 예시 파일(과제가 이름을 지정하면 그 이름, 없으면 `.env.example`)에 필요한 환경변수를 모두 나열하고 값은 예시로만 채움, 실제 값을 담은 .env는 .gitignore에 추가
- 시간대: Jackson과 Hibernate JDBC 시간대를 Asia/Seoul로 맞춤

## 헬스체크

- spring-boot-starter-actuator 사용, `/actuator/health` 노출
- 필요하면 `management.endpoint.health.probes.enabled: true`로 liveness, readiness 제공
- 보안 설정이 있으면 헬스체크 경로는 인증 없이 허용

## 스키마와 초기 데이터

- Flyway 사용 (`spring-boot-starter-flyway`, `flyway-mysql`), 마이그레이션 작성 규칙은 spring-conventions Skill의 DB 규칙
- 스키마는 `V1__create_tables.sql`, 비밀값이 없는 초기 데이터는 `V2__insert_default_data.sql`로 시작
- 빈 DB에서 앱을 띄우면 스키마와 초기 데이터가 자동으로 준비되는지 확인
- 초기 계정처럼 비밀번호가 필요한 데이터는 SQL에 넣지 않고, 앱 시작 시 환경변수 값으로 생성 (이미 있으면 건너뜀)

## 확인

- `docker compose up --build`로 빈 볼륨에서 기동
- 헬스체크 응답, 스키마와 초기 데이터 생성, 재시작 후 데이터 유지 확인
- 확인 결과와 명령을 보고하고, 실행 문서의 실행 방법과 일치하는지 대조
