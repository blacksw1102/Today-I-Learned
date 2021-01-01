# Spring 공부 2일차

MVC 패턴
-개발 로직을 Model, View, Controller 3가지의 역할로 나누어서 구현하기 위한 디자인 패턴이다.
-애플리케이션의 시각적 부분과 동작 및 제어(비즈니스 로직)을 분리해서 서로에게 영향을 미치지
않고 애플리케이션을 개발해 나갈 수 있다.
-입출력 페이지, 데이터처리, 흐름제어 3가지로 역할을 분리되서 각 역할에 집중하도록 로직을
개발할 수 있으며, 추후 애플리케이션의 유지보수, 확장성 및 코드의 유연함을 잡을 수 있는 패턴이다.
-사용자 요청부터 응답처리 순서 : URL 호출 -> Controller -> Service ->ServiceImpl -> DAO ->
DAOImpl -> Service -> View

Model 
-애플리케이션의 데이터를 나타낸다. 데이터베이스, 상수, 변수 등을 가지며 이러한 데이터들의 가공을
책임지는 컴포넌트이다.
-모델은 뷰나 컨트롤러에 대해서 어떤 정보도 알면 안된다.
-Model은 DAO, DTO, Service 객체로 나눌 수 있다.

DAO
-Data Access Object라고 한다.
-DB를 사용해 데이터를 조작하는 기능을 전담하도록 만든 객체이다.
-DAO는 도메인 로직(비즈니스 로직과 DB와 관련없는 코드)를 persistence 메커니즘과 분리를 위해
사용한다.
-HTTP Request를 애플리케이션이 받게 되면, 쓰레드를 생성하게 되는데, 비즈니스 로직이 DB로부터
데이터를 얻어오기 위해 매번 Driver를 로드하고 Connection 객체를 생성하게되면 엄청 많은 커넥션
이 일어나므로 DAO를 하나 만들어 DB 전용 객체로만 사용하고 시스템의 부담을 줄일 수 있다. 
이 개념은 DBCP(Database Connection Pool)로부터 나왔다.
-WAS(Web Application Server)가 실행되면 일정량의 DB Connection 객체를 Pool에다가 저장해두고
HTTP Request가 들어올때마다 Pool에서 Connection 객체를 가져다 쓰고 반환하게 된다.

DTO
-DTO(Data Transfer Object) 또는 VO(Value Object)라고 한다.
-데이터를 각 레이어간 전달하는 목적을 가지는 가지며, 객체의 속성과 getter 및 setter만 가진다.
-계층간 데이터 교환을 위한 자바 빈즈다.
-데이터베이스 레코드의 데이터를 매핑하기 위해 사용되는 데이터 객체이다.
-일반적으로 로직을 가지지 않는다.

Service
-비즈니스 로직을 처리하는 객체다.
-Controller가 Request를 받으면 적절한 Service에 전달하고, 전달받은 Service는 비즈니스 로직을 처리.
-DAO로 데이터베이스를 접근하고 DTO로 데이터를 전달받은 다음, 적절한 처리 후 Controller에게
반환을 한다.

View
-사용자의 인터페이스 요소를 나타낸다. 데이터 입력 및 출력을 담당한다.
-데이터를 기반으로 사용자들이 볼 수 있는 화면이다.
-뷰는 모델이 가지고 있는 정보를 따로 저장해서는 안된다.
-모델이나 컨트롤러에 대해서 어떤 정보도 알면 안된다.

Controller
-데이터와 사용자 인터페이스 요소들을 잇는 다리역할을 한다.
-사용자가 데이터를 클릭하고, 수정하는 것에 대한 "이벤트"들을 처리하는 부분을 의미한다.
-컨트롤러는 흐름 중재를 위해 모델이나 뷰에 대해서 알고 있어야 한다.
-컨트롤러는 모델이나 뷰의 변경을 모니터링 해야 한다.
-애플리케이션의 메인 로직은 컨트롤러가 담당하게 된다.

Web Browser -> Front Controller ->(Model) Controller ->(Model) View -> Web Broweser

MVC Workflow
1.Client -> Dispatcher Servlet : URL로 접근하여 정보를 요청
2.Dispatcher Servlet -> HandlerMapping : 해당 요청을 매핑한 컨트롤러가 있는지 탐색
3.Handler Mapping -> Controller : 처리요청
4.Controller -> DispatcherServlet : 클라이언트의 요청을 처리하고 결과를 출력할 View의 이름을 리턴
5.DispatcherServlet -> ViewResolver : 컨틀롤러에서 보내온 View 이름을 토대로 처리 View를 검색
6.ViewResolver -> View : 처리결과를 View에 송신
7.View -> DispatcherServlet : 처리결과가 포함된 View를 DispatcherServlet에 송신
8.DispatcherServlet -> Client : 최종결과 출력

Dispatcher Servlet
-Servlet Contaier에서 HTTP 프로토콜을 통해 들어오는 모든 요청을 프레젠테이션 계층의 제일앞에
두어서 중앙집중식으로 처리해주는 Front Controller다.
-클라이언트로부터 어떠한 요청이 들어오면 톰캣과 같은 서블릿 컨테이너가 요청을 받게되는데,
제일 앞에서 서버로 들어오는 모든 요청을 처리는 프론트 컨트롤러를 스프링에선 디스패쳐 서블릿이라
부른다.
-공통처리 작업을 Dispatcher Servlet이 처리한 후, 적절한 세부 Controller로 작업을 위임한다.
-스프링 MVC에서 Dispatcher Servlet이 등장함에 따라 web.xml의 역할이 상당히 축소되었다. 기존에는
모든 서블릿에 대해 URL 매핑을 위해서 web.xml에 모두 등록을 해야 했지만, Dispatcher Servlet이 해당
애플리케이션으로 들어오는 모든 요청을 핸들링해주면서 작업을 상당히 편리하게 할 수 있게 되었다.

Front Controller
-서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는
컨트롤러인데, MVC 구조에서 함께 사용되는 패턴이다.

Handler Mapping
-Client로부터 http request가 들어오면 요청처리를 담당할 Controller를 mapping해준다. 

View Resolver
-사용자가 요청한 것에 대한 응답 view를 렌더링하는 역할을 한다.
-view 이름을 이용해서 응답할 view 객체를 매핑해준다.

Dependency Injection
-아래의 코드와 같이 B 클래스 내부에서 A 클래스를 변수로 사용하는 경우, "B 클래스는 A 클래스에 의존한다." 라고 말할 수 있다.
![Image for post](https://miro.medium.com/max/2178/1*3IXT2pbfJSG8zIJAFLRcdA.png)
-여기서 B 클래스가 의존하고있는 A 오브젝트를 내부가 아닌 외부에서 생성하여 B 클래스에서 사용한다면 "A 오브젝트를 B 클래스에 주입한다."라고 말할 수 있다.
-DI는 의존성을 분리시켜 사용하는 특성을 가진다. 의존성 분리에는 SOLID 중 하나인 의존관계 역전 원칙이 적용되어 클래스간의 의존관계를 분리시키는데, 아래 코드를 보면 A 클래스를 인터페이스로 구현하고, B 클래스 내부에서도 인터페이스 변수를 사용함으로써 B, A 클래스간의 의존 관계를 없애고 추상화에만 의존하도록 코드가 수정된걸 확인가능하다.
-코드에 DI를 적용하면 의존관계 역전 원칙에 의해서 1. 코드 재사용률 증가 2. 코드 테스트 용이 3. 코드 단순화 4. 코드간 종속성 감소 5. 결합도를 낮추고 확장성 증가 6. 객체간 의존관계 감소와 같은 많은 장점을 취할 수 있다.
![Image for post](https://miro.medium.com/max/2523/1*68042wsClK1dvElKTaT8ag.png) 
-의존관계 역전 원칙을 적용할 경우 구조적 설계에서는 의존의 방향이 역전 되었다고 하여, 이를 Inversion Of Control 즉 IOC라고 부른다.

Denpendency Injection 방법 3가지
생성자에 주입 : 생성자에 의존성 주입을 받고자 하는 field를 나열하는 방법. (권장)
Setter 메소드에 주입 : setter 메서드에 @Autowired annotation을 선언하여 주입받는 방법.
멤버변수에 주입 : member field에 @Autowired annotation을 선언하여 주입받는 방법.

스프링에서 사용중인 IOC와 DI에 대해서 아래 링크를 통해 자세하게 읽어볼 수 있다.
https://biggwang.github.io/2019/08/31/Spring/IoC,%20DI%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C/

의존관계 역전 원칙
-SOLID 원칙 중 하나이다.
-상위 계층이 하위계층(세부사항)에 의존하는 전통적인 의존관계를 역전시킴으로써 상위 계층이 하위 계층의 구현으로부터 독립되게 만들 수 있음을 지칭한다.
-첫째, 상위 모듈은 하위 모듈에 의존해선 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야만 한다.
-둘째, 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야만 한다.

Inversion of Control
-IOC는 간단히 말하면 "제어의 역전" 현상을 의미하며, 메소드나 객체의 호출 작업을 개발자가 아닌 외부에서 결정하는 것을 뜻한다.
-일반적인 자바 프로그램은 main() 메소드에서 부터 프로그램이 시작되어서, 개발자가 작성한 코드대로 객체가 생성되고 실행이 되어진다. 하지만 서블릿은 개발자가 개발 후 서버에 배포를 하고 나면 서블릿 제어권을 가지고 있는 컨테이너가 직접 특정시점에 서블릿 오브젝트를 생성하고, 오브젝트의 메소드를 호출하게 된다.
-대부분의 프레임워크에는 IOC가 적용되어 있는데, 때문에 개발자들은 프레임워크를 이용하면, 필요한 모듈을 개발해서 프레임워크에 끼워넣는 방식으로 작업을 하게된다.
-스프링에도 마찬가지로 IOC가 적용되어있지만, 제어 역행이 일어날 때 스프링 내부의 객체들관의 관계를 최대한 유연하도록 만들기 위해서 스프링에서는 DI를 사용하고 있다. 그래서 개발자는 각 클래스들을 추상화시켜 놓은 인터페이스를 이용해 개발을 진행하고, 프로그램이 시작되면 프레임워크에서 상황에 따라 인터페이스에 맞는 객체들을 생성해서 기능들을 호출하게 된다.