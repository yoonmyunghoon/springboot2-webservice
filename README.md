# 스프링부트 프로젝트



## 목표

- Java와 Springboot를 사용한 웹서비스 개발 및 AWS를 이용한 배포

## 환경

- springboot 2.1.7.RELEASE
- gradle 4.10.2
- Java 1.8
- IDE: IntelliJ
- 테스트프레임워크: JUnit4

## 라이브러리

- Lombok

## 사용된 기술 및 개념

- mavenCentral
- jcenter
- JPA
  - 자바 표준 ORM(Object Relational Mapping)
  - 객체를 매핑
  - 이 프로젝트에서는 JPA를 사용함
- MyBatis, iBatis
  - SQL Mapper
  - 쿼리를 매핑
  - Node.js 프로젝트(너밥보)에서 사용해봄
- Spring Data JPA
- 더티 체킹
  - JPA의 EntityManager가 활성화된 상태일 경우
    - Spring Data JPA를 사용하면 기본 옵션
  - 트랜잭션 안에서 데이터베이스에서 데이터를 가져오면 영속성 컨텍스트가 유지됨
  - 이 상태에서는 해당 데이터의 값을 변경하면 트랜잭션이 끝나는 시점에 해당 테이블의 변경을 반영하게 됨
  - 별도의 쿼리 없이 Entity 객체의 값만 변경하면 Update가 됨
- H2
  - 인메모리 데이터베이스
  - 로컬, 테스트용 DB
  - 어플리케이션 재시작 시, 초기화
  - web console을 통해서 관리 가능
- Spring 웹 계층
  - Web Layer
  - Service Layer
  - Repository Layer
  - Dtos
  - Domain Model
- JPA Auditing
  - 차후 유지보수를 위해서는 Entity 생성 및 수정에 시간 데이터가 함께 저장될 필요가 있음
  - 일일이 시간 정보를 등록하고 갱신하는 것을 자동화하는 것이 바람직
  - JPA Auditing 이용
- 머스테치
  - JSP와 같이 HTML을 만들어주는 템플릿 엔진
  - 많은 언어를 지원해줌
  - 심플함
- 프론트엔드 라이브러리: 부트스트랩, 제이쿼리
  - 부트스트랩, 제이쿼리 등 프론트엔드 라이브러리를 사용하는 방법 2가지
    - 외부 CDN 사용 - 이 프로젝트에서는 이 방식 채택
      - 장: 쉽고 간단
      - 단: 해당 외부 서비스에 의존적이게 됨
        - 실제 서비스할 때는 직접 라이브러리를 받아서 사용하는 것이 좋음
    - 직접 라이브러리 받아서 사용
      - 장: 의존성 적음
      - 단: 직접 받아야하는 수고로움
- 스프링 시큐리티
  - 인증 및 권한 기능을 가진 프레임워크
  - 스프링 기반의 애플리케이션에서는 보안을 위한 표준급
  - 인터셉터, 필터 기반의 보안 보다 권장됨
- OAuth 2.0
  - 소셜 로그인
    - 로그인, 회원가입 시 인증 처리, 비밀번호 찾기 및 변경, 회원정보 변경 등을 직접 구현하지 않고 서비스 개발에 더 집중할 수 있음
  - 구글 클라우드 플랫폼을 통해 해당 api 사용

