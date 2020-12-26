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
- 