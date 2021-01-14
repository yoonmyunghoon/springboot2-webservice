# 스프링부트 프로젝트

![Travis CI status](https://travis-ci.org/yoonmyunghoon/springboot2-webservice.svg?branch=master)

## 1. 목표

- Java와 Springboot를 사용한 웹서비스 개발 및 AWS를 이용한 배포

## 2. 환경

- springboot 2.1.7.RELEASE
- gradle 4.10.2
- Java 1.8
- IDE: IntelliJ
- 테스트프레임워크: JUnit4

## 3. 라이브러리

- Lombok

## 4. 사용된 기술 및 관련 개념

### mavenCentral

### jcenter

### JPA

- 자바 표준 ORM(Object Relational Mapping)
- 객체를 매핑
- 이 프로젝트에서는 JPA를 사용함

### MyBatis, iBatis

- SQL Mapper
- 쿼리를 매핑
- Node.js 프로젝트(너밥보)에서 사용해봄

### Spring Data JPA

### 더티 체킹

- JPA의 EntityManager가 활성화된 상태일 경우
  - Spring Data JPA를 사용하면 기본 옵션
- 트랜잭션 안에서 데이터베이스에서 데이터를 가져오면 영속성 컨텍스트가 유지됨
- 이 상태에서는 해당 데이터의 값을 변경하면 트랜잭션이 끝나는 시점에 해당 테이블의 변경을 반영하게 됨
- 별도의 쿼리 없이 Entity 객체의 값만 변경하면 Update가 됨

### H2

- 인메모리 데이터베이스
- 로컬, 테스트용 DB
- 어플리케이션 재시작 시, 초기화
- web console을 통해서 관리 가능

### Spring 웹 계층

- Web Layer
- Service Layer
- Repository Layer
- Dtos
- Domain Model

### JPA Auditing

- 차후 유지보수를 위해서는 Entity 생성 및 수정에 시간 데이터가 함께 저장될 필요가 있음
- 일일이 시간 정보를 등록하고 갱신하는 것을 자동화하는 것이 바람직
- JPA Auditing 이용

### 머스테치

- JSP와 같이 HTML을 만들어주는 템플릿 엔진
- 많은 언어를 지원해줌
- 심플함

### 프론트엔드 라이브러리: 부트스트랩, 제이쿼리

- 부트스트랩, 제이쿼리 등 프론트엔드 라이브러리를 사용하는 방법 2가지
  - 외부 CDN 사용 - 이 프로젝트에서는 이 방식 채택
    - 장: 쉽고 간단
    - 단: 해당 외부 서비스에 의존적이게 됨
      - 실제 서비스할 때는 직접 라이브러리를 받아서 사용하는 것이 좋음
  - 직접 라이브러리 받아서 사용
    - 장: 의존성 적음
    - 단: 직접 받아야하는 수고로움

### 스프링 시큐리티

- 인증 및 권한 기능을 가진 프레임워크
- 스프링 기반의 애플리케이션에서는 보안을 위한 표준급
- 인터셉터, 필터 기반의 보안 보다 권장됨

### OAuth 2.0

- 소셜 로그인
  - 로그인, 회원가입 시 인증 처리, 비밀번호 찾기 및 변경, 회원정보 변경 등을 직접 구현하지 않고 서비스 개발에 더 집중할 수 있음
- 구글 클라우드 플랫폼을 통해 해당 api 사용
- 네이버 개발자 api 사용

### Enum class

- 관련이 있는 상수들의 집합을 나타내기 위한 클래스
- 기존에는 인터페이스나 클래스 내에서 상수를 선언함으로써 상수를 관리 하였는데 클래스 내에서 선언하는 부분은 네이밍이 겹칠 수 있고 불필요하게 상수가 많아지는 단점이 있다
  - 이를 보완하기 위해 나온 것이 Enum

### 어노테이션 생성

- @interface를 통해 어노테이션 클래스 생성
  - @Target: 어노테이션이 생성될 위치 지정
  - @Retention: 어노테이션 유지 범위 지정
- HandlerMethodArgumentResolver를 구현할 클래스 생성
  - 조건 검사 후, 구현체가 지정한 값을 파라미터로 넘김
- 스프링에서 인식할 수 있도록 WebMvcConfigurer에 추가

### 세션 저장 방식 3가지

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

### 기존 테스트에 시큐리티 적용하기

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

### AWS EC2

- 외부에서 서비스에 접근하려면 24시간 작동하는 서버가 필요
  - PC, 호스팅 서비스, 클라우드 서비스 중에 선택
- 클라우드 서비스는 인터넷을 통해 서버, 스토리지, 데이터베이스, 네트워크, 소프트웨어, 모니터링 등의 컴퓨팅 서비스를 제공해주는 것
  - AWS, AZURE, GCP 등이 있음
- AWS EC2는 물리 장비인 서버뿐만 아니라 로그 관리, 모니터링, 하드웨어 교체, 네트워크 관리 등을 기본적으로 지원해줌
- 클라우드 종류
  - Infrastructure as a Service(IaaS)
    - 기존 물리 장비를 미들웨어와 함께 묶어둔 추상화 서비스
    - AWS의 EC2, S3 등
  - Platform as a Service(PaaS)
    - IaaS에서 많은 기능이 자동화된 추상화 서비스
    - AWS의 Beanstalk(빈스톡), Heroku(헤로쿠) 등
  - Software as a Service(SaaS)
    - 소프트웨어 서비스
    - 구글 드라이브, 드랍박스, 와탭 등
- 이 프로젝트에서는 AWS의 EC2를 선택
- 인스턴스
  - EC2 서비스에 생성된 가상머신
- AMI(Amazon Machine Image, 아마존 머신 이미지)
  - 인스턴스라는 가상머신에 운영체제 등 필요한 리소스를 설치할 수 있도록 이미지로 구워놓은 것
- EIP
  - 고정 IP(Elastic IP, 탄력적 IP)를 할당
- 아마존 리눅스 2에서 이 프로젝트를 돌리기 위한 설정
  - 톰캣과 스프링부트가 작동할 수 있도록 java 8 설치
  - 타임존 한국 시간대로 변경
  - 호스트네임 변경
    - 기존에 많이 사용했던 아마존 리눅스1과 아마존 리눅스2에는 약간의 차이가 존재함
    - 참고: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-hostname.html

### AWS RDS

- AWS에서 지원하는 클라우드 기반 관계형 데이터베이스
  - 직접 데이터베이스를 설치하게 되면 모니터링, 알람, 백업 등 모두 직접해야함, 많은 시간이 요구됨
  - 잦은 운영 작업들을 자동화해주기 때문에 개발자들이 개발에 집중할 수 있음
  - 조정 가능한 용량을 지원하여 예상치 못한 데이터 증가에도 정상 작동할 수 있도록 해줌
- MariaDB 선택
  - 가격
  - Amazon Aurora 교체 용이성
  - 동일 하드웨어 사양으로 MySQL보다 향상된 성능
  - 좀 더활성화된 커뮤니티
  - 다양한 기능
  - 다양한 스토리지 엔진

### 배포

- 배포 과정
  - github에서 프로젝트 clone해오기 or pull해서 프로젝트 갱신
  - gradle이나 maven을 통해 테스트 및 빌드
  - 서버에서 프로젝트 실행 및 재실행
- 배포 과정이 차례대로 한번에 실행될 수 있도록 쉘 스크립트 작성
- 외부 security파일 등록
- MariaDB 사용할 수 있도록 설정
  - 테이블 생성
    - H2에서 자동 생성해주던 테이블들을 MariaDB에선 직접 생성
  - 프로젝트 설정
    - DB 드라이버 추가
  - EC2 내부에 DB 접속 정보 보관
- error='Cannot allocate memory' (errno=12)에러
  - 메모리 부족 에러 발생
    - swap 메모리 사용 고려하기 전, 프로젝트 용량이 작기 때문에 필요없는 부분이 돌아가고 있는지 확인하기 위해서 재부팅
      - 해결
- 구글 및 네이버 등 소셜로그인으로 사용한 서비스들에 도메인 추가

### test, build, deploy 자동화 - Travis CI 배포 자동화

- 스크립트를 사용해서 빌드 및 배포할 때의 문제점
  - 수동으로 전체 test를 진행해야만 내가 짠 코드가 다른 개발자들의 코드에 영향을 주는지 알 수 있음
  - 브랜치가 병합될 때 발생하는 문제를 빌드 후, 실행해봐야만 확인할 수 있음
- 여러 개발자의 코드가 실시간으로 병합되고, 테스트가 수행되고, master 브랜치가 푸시되면 자동으로 배포되는 환경이 필요함
- CI & CD
  - CI(Continuous Integration - 지속적 통합)
    - 코드 버전 관리 시스템(Git, SVN 등)에 PUSH가 되면 자동으로 테스트와 빌드가 수행되어 안정적인 배포 파일을 만드는 과정
  - CD(Continuous Deployment - 지속적 배포)
    - 빌드 결과를 자동으로 운영 서버에 무중단 배포까지 진행되는 과정
- 테스팅 자동화의 중요성
  - 지속적 통합을 위해서는 이 프로젝트가 완전한 상태임을 보장하기 위한 테스트 코드가 구현되어 있어야함
- Travis CI
  - 깃허브에서 제공하는 무료 CI 서비스
  - 젠킨스
    - 설치형 CI 도구
    - 설치형이기 때문에 이를 위한 EC2 인스턴스가 하나 더 필요함
    - 이제 시작하는 서비스에서 배포용 EC2 인스턴스는 부담스러움
    - 따라서, 오픈소스 웹 서비스인 Travis CI를 사용하기로함
  - yml 확장자
    - yml 확장자를 YAML(야믈)이라고 함
    - JSON에서 괄호를 제거한 형태
    - 읽고 쓰기가 쉬움
    - Travis CI 설정에도 사용됨(.travis.yml 파일)

### Travis CI와 AWS S3 연동

- AWS S3
  - AWS에서 제공하는 일종의 파일 서버
  - 이미지와 같은 정적 파일 및 배포 파일 관리 기능
- AWS CodeDeploy
  - 배포 서비스
  - 저장 기능이 없음
  - 빌드와 배포가 분리되지 않음
    - 깃허브에서 코드를 가져오는 기능이 있음
    - 빌드 없이 배포만 필요할 때는 대응하기 어려움
    - 항상 빌드를 진행해야하기 때문에 확장성이 떨어짐 
- 빌드 및 배포 단계
  1. Github master 브랜치에 push
  2. Travis CI로 테스트 및 빌드
  3. 빌드 결과물을 S3에 전달
  4. CodeDeploy에 배포 요청
  5. CodeDeploy가 S3로부터 배포파일을 전달받음
  6. 배포
- IAM(Identity and Access Management)
  - 일반적으로 AWS서비스에 외부 서비스가 접근할 수 없음
  - 접근 가능한 권한을 가진 Key를 사용해야함
  - 이러한 인증과 관련된 기능을 제공하는 서비스
  - 이를 통해 Travis CI가 S3와 CodeDeploy에 접근할 수 있음
  - 사용자와 역할의 차이
    - 사용자: AWS 서비스 외에 사용할 수 있는 권한
    - 역할: AWS 서비스에만 할당할 수 있는 권한

### Travis CI와 AWS S3, CodeDeploy 연동

- EC2에 CodeDeploy 에이전트 설치
- CodeDeploy 생성
- Travis CI와 AWS S3, CodeDeploy 연동

### travis-ci.org shut down

- 기존에 사용하던 Travis CI 무료 버전 서버(travis-ci.org)가 2021년부터 없어지고, pro버전 서버(travis-ci.com)로 통합됨
- 해당 repository 마이그레이션





