---
name: spring-conventions
description: Java/Spring Boot 코드(엔티티, 서비스, 컨트롤러, Flyway 마이그레이션, 테스트, build.gradle)를 작성하거나 수정하기 전에 읽는 코드 작성 규칙
---

# Spring 코드 규칙

- 참고: deepple-dev/deepple-api (커밋 58e401f32fee332d3e232f04624529306e0f2a74)의 CLAUDE.md 코딩 규칙과 .claude/rules/(java, db, test, build)
- 과제 안내에 지정된 규칙이 있으면 그것을 우선
- 서식(들여쓰기, 줄 길이)은 포맷터에 맡기고, 아래는 포맷터가 잡지 못하는 규칙

## 구조

- 패키지는 도메인 단위로 나누고, 도메인 안에서 controller, service, domain(엔티티, 리포지토리), dto로 나눔, 공통 코드는 common
- 컨트롤러에는 비즈니스 로직을 두지 않고 서비스에 위임
- 요청·응답 DTO는 record로 만들고 엔티티를 그대로 응답하지 않음
- 예외는 도메인 예외로 던지고 전역 예외 처리기(`@RestControllerAdvice`)에서 응답으로 변환
- 주석과 예외 메시지는 한국어

## Java

- null이 될 수 없는 값은 primitive 타입(`long`, `int`, `boolean`, `double`)
- wrapper 타입은 null이 의미 있을 때만 사용
  - 저장 전 null인 엔티티 PK(`Long id`), nullable 컬럼
  - 요청 DTO에서 값이 없을 수 있는 필드 (`@NotNull` 검증 대상 포함)
  - 제네릭 타입 인자 (`List<Long>`)
- 한글 발음을 영어로 옮긴 이름 금지 (나쁜 예: `moohyungJasan`, 좋은 예: `intangibleAssets`), 한국어 고유명사는 예외
- 약어도 카멜 표기 (나쁜 예: `HTTPAPIURL`, 좋은 예: `HttpApiUrl`)
- 메서드 이름은 동사로 시작, 변환 메서드(`toString`)와 빌더 메서드(`withUserId`)는 예외
- 메서드 범위를 넘는 변수에 한 글자 이름 금지, 반복문 인덱스와 람다 파라미터는 허용
- 조건문과 반복문은 한 줄이어도 중괄호 사용
- `var`는 오른쪽 식에서 타입이 바로 보일 때만
- 스트림과 메서드 체이닝이 2단계 이상이면 메서드마다 줄바꿈
- `long` 리터럴에는 대문자 `L`
- 생성자, getter, 로거처럼 Lombok으로 대신할 수 있는 코드는 Lombok 사용 (테스트 코드 포함), 유틸 클래스의 private 생성자는 `@NoArgsConstructor(access = AccessLevel.PRIVATE)`

## DB

- 스키마 변경은 Flyway 마이그레이션(`src/main/resources/db/migration/V{다음번호}__설명.sql`)으로만
- 이미 커밋한 마이그레이션 파일은 수정하지 않고 새 파일 추가
- `ddl-auto: validate`, 엔티티와 마이그레이션이 일치해야 함 (앱 기동과 테스트에서 바로 확인됨)
- PK와 `*_id` 컬럼은 `bigint`

## 테스트

- 서비스와 도메인: JUnit + Mockito (`@ExtendWith(MockitoExtension.class)`)
- 리포지토리와 통합 테스트: Testcontainers MySQL (Docker가 없으면 사용자에게 보고)
- 테스트 패키지는 main과 같은 구조
- 테스트를 끄거나 지워서 빌드를 통과시키지 않음

## 빌드

- Spring Boot가 관리하는 라이브러리는 버전을 적지 않음
