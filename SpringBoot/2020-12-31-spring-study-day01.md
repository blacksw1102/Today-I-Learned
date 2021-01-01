# Spring 공부 1일차

@RestController

-@Controller + @ResonseBody를 합쳐놓은 개념

-@Controll는 View 기술을 사용하지만, @RestController는 객체를 반환할 때, 객체 데이터는 JSON

타입의 HTTP 응답을 리턴하게 된다.

-실행 흐름 : Client -> HTTP Request -> Dispatcher Servlet -> Handler Mapping -> 

RestController (자동 ResponseBody 추가)-> HTTP Response -> Client



@Controller

-웹 요청을 처리하는 컨트롤러로 사용한다.

-실행 흐름 : Client -> Request -> Dispatcher Servlet -> Handler Mapping -> 

Controller -> View -> Dispatcher Servlet -> Response -> Client 



@RequestBody, @ResponseBody

-비동기 통신을 하는 경우 사용한다.

-JSON 형식으로 데이터를 주고 받는다.

-@RequestBody의 경우, HttpMessageConverter가 요청 헤더의 컨텐츠 타입을 보고, 

어떤 메시지 컨버터를 사용할 것인지 판단하여 본문 값(JSON)을 자바 객체로 Conversion해준다.

-@ResponseBody의 경우,Jackson2ObjectMapperBuilder가 JSON 타입으로 변환하기 위한 

객체(DTO)에 getter와 setter 메서드가 존재해야 한다.

-@ResponseBody 실행 흐름 : Client -> Request -> Dispatcher Servlet -> 

Handler Mapping -> Controller (ResponseBody)-> Response -> Client 



@RequestMapping("/hello"),

-컨트롤러가 처리할 요청 URL을 명시하는데 사용. 클래스나 메소드에 적용.

-메소드에만 적용할 경우 각 메서드가 처리할 요청 URL을 명시한다.



@SpringBootApplication

-스프링 부트의 가장 기본적인 설정을 선언해준다.

-Sprint Boot Application을 실행한다.

-@SpringBootConfiguration, @ComponentScan, @EnableAutoConfiguration 3가지를 수행한다.



@SpringBootConfiguration

-빈에 대해서 Context에 추가하거나, 특정 클래스를 참조해 올 수 있다.

-스프링 부트의 설정을 나타내는 전용 어노테이션이다. 스프링의 @Configuration을 대체한다. 



@ComponentScan

-@Component, @Configuration, @Repository, @Service, @Controller, @RestController

등의 어노테이션을 스캔해서 Bean으로 등록해주는 어노테이션이다.



@EnableAutoConfiguration

-사전에 정의한 라이브러리들을 Bean으로 등록해주는 어노테이션이다.

-사전에 정의된 라이브러리들 모두가 등록되는 것은 아니고 특정 Condition(조건)이 만족될 경우에

Bean으로 등록을 한다.

-사전 정의 파일 위치 : Dependencies > spring-boot-autoconfigure > META-INF > spring.factories



SpringApplication.run(HelloWorldApplication.class, args);

-SpringApplication을 실행할 때 사용하는 메소드이다.



application.properties

-스프링 부트가 애플리케이션을 구동할 대 자동으로 로딩하는 파일이다.

-key-value 형식으로 값을 정의하면 애플리케이션에서 참조해서 사용 가능하다.

-모든 프로퍼티들은 Environment 객체를 통해서 getProperty() 메서드로 참조 가능하다.

-@Value 어노테이션으로도 프로퍼티 값을 참조 가능하다.

-application.properties의 우선순위는 1. 프로젝트 디렉토리 바로 아래에 config 디렉토리를 생성 후 그 아래,

\2. 프로젝트 디렉토리 루트 바로 아래, 3. :classpath 아래에 config 디렉토리 만들고 그 아래, 4. :classpath

아래에 생성하는 경우로 나뉜다.



po[m.xml](http://m.xml/)

-POM(project object model)을 설정하는 부분으로, 프로젝트 내 빌드 옵션을 설정하는 문서다.

-문서에는 크게 3가지로, 프로젝트 정보와 의존성 라이브러리 정보, build 정보를 명시한다.

-<modelVersion> : maven의 po[m.xml의](http://m.xn--xml-yh0o/) 모델 버전

-<groupId> : 프로젝트를 생성한 조직 또는 그룹, 보통 URL의 역순으로 지정한다.

-<artifactId> : 프로젝트에서 생성되는 고유 아티팩트 이름이다.

-<version> : 애플리케이션 버전, 접미사로 SNAPSHOT이 붙으면 아직 개발단계임을 의미한다.

-<packaging> : jar, war, ear, pom 등 패키지 유형을 의미한다.

-<name> : 프로젝트 명

-<description> : 프로젝트 설명

-<url> : 프로젝트를 찾을 수 있는 URL

-<properties> : po[m.xml에서](http://m.xn--xml-k94n91q/) 중복해서 사용하는 설정(상수)값을 지정해 놓는 부분. 다른 위치에서 

${...}로 표기해서 사용할 수 있다.

-<profiles> : dev, prod 이런식으로 개발할 때. 릴리즈 할 때 나눠야 할 필요가 있는 설정 값을 지정한다.

-의존성 라이브러리는 최소 groupId, artifactId, version 정보가 필요하다.

-spring-boot-starter-*의 경우는 부모 po[m.xml에](http://m.xn--xml-568n/) 이미 버전이 명시되어 있어 version 정보는 생략한다.

-A라는 라이브러리를 사용하는데 B, C, D가 의존성을 가진다면 A를 dependency에 추가 시, 자동으로

필요한 B, C, D도 가져오는 기능을 한다.



Maven

-자바 프로젝트의 빌드(build)를 자동화 해주는 빌드 툴(build tool)이다.



build 라이프 사이클

mvn process-resources : resources:resources의 실행으로 resource 디렉토리에 있는 내용을 target/classes로 복사한다.

mvn compile : compiler:compile의 실행으로 src/java 밑의 모든 자바 소스를 컴파일해서 target/classes로 복사

mvn process-testResources, mvn test-compile : 이것은 위의 두 개가 src/java였다면 test/java의 내용을 target/test-classes로 복사. (참고로 test만 mvn test 명령을 내리면 라이프사이클상 원본 소스로 컴파일된다.)

mvn test : surefire:test의 실행으로 target/test-classes에 있는 테스트케이스의 단위테스트를 진행한다. 결과를 target/surefire-reports에 생성한다.

mvn package : target디렉토리 하위에 jar, war, ear등 패키지파일을 생성하고 이름은 <build>의 <finalName>의 값을 사용한다 지정되지 않았을 때는 아까 설명한 "artifactId-version.extention" 이름으로 생성

mvn install : 로컬 저장소로 배포

mvn deploy : 원격 저장소로 배포

mvn clean : 빌드 과정에서 생긴 target 디렉토리 내용 삭제

mvn site : target/site에 문서 사이트 생성

mvn site-deploy : 문서 사이트를 서버로 배포