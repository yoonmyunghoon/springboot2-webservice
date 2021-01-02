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
  
- Enum class
  - 관련이 있는 상수들의 집합을 나타내기 위한 클래스
  - 기존에는 인터페이스나 클래스 내에서 상수를 선언함으로써 상수를 관리 하였는데 클래스 내에서 선언하는 부분은 네이밍이 겹칠 수 있고 불필요하게 상수가 많아지는 단점이 있다
    - 이를 보완하기 위해 나온 것이 Enum
  
- 어노테이션 생성
  - @interface를 통해 어노테이션 클래스 생성
    - @Target: 어노테이션이 생성될 위치 지정
    - @Retention: 어노테이션 유지 범위 지정
  - HandlerMethodArgumentResolver를 구현할 클래스 생성
    - 조건 검사 후, 구현체가 지정한 값을 파라미터로 넘김
  - 스프링에서 인식할 수 있도록 WebMvcConfigurer에 추가
  
- 세션 저장 방식 3가지
  - 톰캣 세션
    - 별다른 설정을 하지 않으면 WAS의 메모리에 저장되고 호출됨
    - 만약 2대 이상의 WAS가 구동되면 세션 공유를 위한 처리가 필요함
  - 데이터베이스에 저장
    - 설정은 간단하나, 로그인 요청마다 DB IO가 발생하기 때문에 성능상 이슈가 발생할 수 있음
    - 보통 로그인 요청이 적은 사내 시스템 용도로 사용
    - 이 프로젝트에서는 이 방법으로 진행함
  - Redis와 같은 메모리 DB에 저장
    - B2C 서비스에서 많이 사용하는 방식
    - AWS에 배포하고 운영할 때 별도의 비용이 발생함
  
- 기존 테스트에 시큐리티 적용하기

  - 1단계(교재에서는 문제가 있어보였지만, 실제로 진행할 때는 이와관련된 문제는 없었음)

    - src/main과 src/test의 환경 차이가 존재
      - application.properties가 각각 적용되는데 만약, test쪽에 따로 만들지 않았다면 main쪽 설정을 자동으로 따라감
      - 단, 자동으로 따라오는 옵션의 범위가 application.properties라고 함
      - 따라서, application-oauth.properties 부분은 사용할 수 없음
      - test 환경용으로 application.properties를 새로 만들었음
    - 교재의 질문게시판에서 spring.profiles.include 값은 @SpringBootTest를 통한 설정에도 적용된다는 사실을 알게됨
      - 따로 테스트용 application.properties를 만들지 않아도 문제되지 않았던 이유가 이거였음
      - 교재에 있는 문제 발생 원인은 2단계와 3단계만으로 해결가능

  - 2단계

    - Posts를 수정하고 등록하는 부분은 권한이 있는 사용자만 가능하기 때문에 기본 api test에서는 권한관련 문제가 발생함
    - 임의로 인증된 사용자를 추가하는 방식의 테스트로 리팩토링
      - 스프링 시큐리티에서 공식적으로 지원하는 방식
        - spring-security-test를 추가하고 진행
    - SpringBootTest에서 MockMvc를 사용해서 임의의 인증된 사용자를 만들어서 테스트 진행

  - 3단계

    - WebMvcTest에서 CustomOAuth2UserService를 찾을 수 없음

    - WebMvcTest는 Repository, Service, Component 등을 읽을 수 없음

    - SecurityConfig는 읽었지만, 이를 생성하기 위해서 필요한 CustomOAuth2UserService를 읽지 못하기 때문에 발생한 오류

    - 스킨대상에서 SecurityConfig 제거시켜줌

    - 여기서도 임의의 인증된 사용자를 생성해줌

    - 이제는 EnableJpaAuditing으로 인한 문제가 발생함

      - WebMvcTest에서 Entity를 사용하지 않기 때문에 발생하는 문제
      - Application에 달아줬던 EnableJpaAuditing과 SpringBootApplication를 분리
      - JpaConfig라는 클래스에 추가해줌

      

