## Spring과 Springboot 차이

### Spring의 정의
자바를 위한 오픈 소스 애플리케이션 프레임 워크.
동적 웹 사이트를 개발하기 위한 여러가지 서비스를 제공하는 전자 정부 표준 프레임워크 기반 기술.

### Spring의 특징
#### POJO(Plain Old Java Object) 프로그래밍 지향
- 외부의 라이브러리나 모듈을 사용하지 않고 순수한 Java를 통해서 객체를 만드는 방식.
- 만약 외부 기술을 사용한다면 해당 기술이 업데이트 될 경우, 그 기술이 쓰이는 객체의 코드를 모두 변경해야함.
- 반면 POJO 방식으로 만든 객체는 순수 Java로 이루어졌기 때문에 기술 변화에 얽매이지 않는다.
- POJO 방식을 위해 IOC, AOP, DI 등을 활용한다.
##### 1. IOC (Invertion Of Control) 제어의 역전 / DI 의존성 주입
- 메소드나 객체의 호출 작업을 개발자가 결정하는 것이 아니라 외부에서 결정되는 것.
- 개발자가 아닌 스프링 자체에서 특정 객체들간의 의존 관계를 맺어주는 것을 뜻한다.

##### 2. AOP (Aspect Oriented Programming) 관점 지향 프로그래밍
- 프로그램 기능은핵심 기능과 관련된 핵심 관심 사항과, 모든 핵심 관심 사항에 공통적으로 적용되는 공통 관심 사항으로 나뉜다.
- 공통 관심 사항은 중복되는 경우가 많기 때문에 이를 별도의 객체로 분리하여 실행시킬 수 있도록 해야한다.
- 이처럼 프로그램 전반에 걸쳐 적용되는 공통 기능을 비즈니스 로직으로부터 분리하는 것을 AOP라 한다.

##### 3. PSA (Portable Service Abstraction) 일관된 서비스 추상화
- 다양한 데이터베이스 툴을 활용하더라도 동일한 사용방법을 유지한 채 데이터베이스를 바꿀 수 있는 기능.
- 스프링이 데이터베이스를 추상화한 인터페이스를 제공하기 때문에 가능하다. 이를 JDBC (Java DataBase Connectivity)라 한다.
- 특정 기술과 관련된 서비스를 추상화하여 일관된 방식으로 사용될 수 있도록 한 것을 PSA라 한다.

### Springboot의 특징
Spring으로 프로그램을 만들 때에 필요한 설정을 간편하게 처리해주는 별도의 프레임워크.

#### Spring과 Springboot의 차이
- Springboot에선 임베드 톰캣 (Embed Tomcat) 을 사용하기 때문에 톰캣을 따로 설치할 필요가 없다.
- dependency 자동화가 이루어진다.
- XML 설정을 하지 않아도 된다.

### 참고
1. https://msyu1207.tistory.com/entry/Spring-VS-Spring-Boot-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90
2. https://www.codestates.com/blog/content/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8
