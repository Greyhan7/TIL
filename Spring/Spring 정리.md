# [Spring & mybatis & JPA]

# 1. Spring이란

## 1) Spring의 개념

- 웹 사이트를 개발하기 위한 자바 기반의 프레임워크.
- 공공기관에서 Spring과 mybatis를 활용하기 때문에 전자정부 표준프레임워크 기술로 쓰이고 있다.

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled.png)

### 프레임워크란

- 개발에 필요한 기능이 미리 구현되어 있는 틀.
- Spring 에서는 MVC 라는 구조 하에서 프로그램을 개발할 수 있다.

### Spring의 MVC 패턴

- Model, View, Controller로 구성되어 있는 틀을 의미한다.
    - Model : 데이터를 가진 객체. 백그라운드에서 동작하는 데이터를 처리하는 영역.
    - View : 사용자에게 보여지는 화면을 의미한다. Spring에서는 JSP를 통해 화면을 구성하고, COntroller를 통해 백엔드 서버와 연결한다.
    - Controller : View와 Service 사이를 연결하는 기능. 클라이언트에서 입력한 URL에 맞는 View를 보여주고, View에서 처리하는 데이터를 Service로 전달해준다.
    - Service : Controller를 통해 화면과 연결되고, DAO를 통해 데이터베이스와 연결된다.

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%201.png)

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%202.png)

## 2) Spring의 속성

### Ioc (Inversion of Control) 제어의 역전

- 개발자가 직접 Service나 DAO를 생성하는 게 아니라 @Autowired를 통해 객체를 받아 사용할 수 있는 기능을 의미한다.

### DI(Dependency Injection) 의존성 주입

- Spring에서 IoC 구조를 만드는 방식.
- 클래스와 클래스 사이에 인터페이스를 넣어 객체 간 의존관계를 줄일 수 있는 기술.
- 의존관계를 줄여 수정과 관리를 용이하게 할 수 있다.

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%203.png)

### Pojo(Plain Old Java Object) 기반의 구성

- 특정 기술에 종속되지 않는 순수한 자바 객체.
- 대표적으로 getter, setter 등 기본적인 자바 기능만을 갖고 있는 객체가 있음.

### AOP (Aspect Oriented Programming) 관점 지향 프로그래밍

- 각 클래스, 메소드에서 공통적으로 갖고 있는 로직을 공통 관심사항으로 정의하여, 이를 쉽게 관리할 수 있다.

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%204.png)

- Advice : 공통 관심사항에 대한 정보. 어떤 것을 공통 사랑으로 해아하는 지에 대한 정보가 담겨있다.
    - Before Advice : 타깃 메소드가 호출되기 전에 기능하는 advice.
    - After Returing Advice : 타깃 메소드가 정상작동 되었을 때 기능하는 advice. 메소드의 반환값을 호출할 수 있다.
    - After Throwing Advice : 타깃 메소드가 예외를 발생시켰을 때 기능하는 advice.
    - After Advice : 타깃 메소드가 정상작동하든 예외가 나오든 기능하는 advice.
    - Around Advice : 메소드 실행 전 혹은 후에 공통 기능을 실행하는 것.
- JoinPoint : Advice가 적용 가능한 지점.
- Pointcut : JoinPoint의 부분 집합으로서 실제로 Advice가 적용되는 Join point를 나타낸다. 스프링에서는 정규 표현식아나 AspectJ의 문법을 이용하여 Pointcut을 정의할 수 있다. 즉, joint point는 적용 가능한 스팩에 가깝고 point cut은 실제 해당 advice가 적용되는 위치라 할 수 있다.
- Weaving : Advice를 핵심 로직 코드에 끼워 넣는 것을 weaving 이라고 한다.
- Aspect : 흩어진 관심사(Crosscutting Concerns)를 묶어서 모듈화 한 것. 여러 객체에 공통으로 적용되는 기능. 하나의 모듈. Advice와 Point Cut이 들어간다. AOP가 설정된 클래스 자체를 의미한다.
- 예시
    
    ```java
    package com.example.demo.common;
    
    import org.aopalliance.intercept.Joinpoint;
    import org.aspectj.lang.JoinPoint;
    import org.aspectj.lang.ProceedingJoinPoint;
    import org.aspectj.lang.annotation.Around;
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.aspectj.lang.annotation.Pointcut;
    import org.springframework.stereotype.Component;
    
    @Component
    @Aspect
    public class DaoCommon {
    	
    	@Pointcut("execution(public * com.example.demo.dao..*(..))")
    	public void daoCommon() {
    		
    	}
    	
    	@Around("daoCommon()")
    	public Object around(ProceedingJoinPoint joinPoint) {
    			Object obj = null;
    			String methodName1 = joinPoint.getSignature().getName();
    			System.out.println(methodName1+"가 동작하기 전");
    			long start = System.currentTimeMillis();
    			try {
    				obj = joinPoint.proceed();
    			}catch(Exception e) {
    				System.out.println("예외" + e.getMessage());} catch (Throwable e) {
    				// TODO Auto-generated catch block
    				e.printStackTrace();
    			}
    			long end = System.currentTimeMillis();
    			System.out.println((start-end)+ "만에 " +methodName1+"가 완료!");
    			return obj;
    	}
    	
    	/*
    	@Before("daoCommon()")
    	public void pro(JoinPoint joinpoint) {
    		String methodName1 = joinpoint.getSignature().toLongString();
    		String methodName2 = joinpoint.getSignature().toShortString();
    		Object target = joinpoint.getTarget();
    		System.out.println("target :"+target);
    		Object []args = joinpoint.getArgs();
    		if(args != null) {
    			for(Object obj : args) {
    				System.out.println("매개변수 :" + obj);
    			}
    		}
    		
    		System.out.println("longString : "+methodName1);
    		System.out.println("shortString :" + methodName2);
    		System.out.println("DAO 처리 전에 동작하는 공통기능");
    		System.out.println("-----------------------------------");
    	}*/
    }
    ```
    

## 3) MyBatis

- 개발자가 지정한 SQL, 프로시저 등을 대신 매핑해주는 프레임워크.
- SQL Mapper를 지원하여 복잡한 쿼리문을 작성하면서 데이터 매핑을 비교적 쉽게 할 수 있다.
- 객체와 쿼리문, CRUD 메소드를 직접 구현해야하기 때문에 번거로울 수 있다.

## 4) JPA

- Object와 DB테이블을 매핑하여 데이터를 객체화하는 ORM 기술 중 하나.
- CRUD 메소드를 기본으로 제공하기 때문에 기초적인 쿼리를 작성할 필요가 없다.
- 복잡한 쿼리문을 작성하기 어렵다.

# 2. Spring 활용하기

## 1) Spring&Mybatis로 간단한 게시판 만들기

### 1__프로젝트 생성

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%205.png)

- Name : 프로젝트의 이름을 말한다.
- Type : 어떤 프로젝트를 만들 것인지에 대한 설정
    - Gradle : 안드로이드 기반 앱의 공식 배포 도구. Maven보다 늦게 나온 만큼 속도, 안정성 모두 뛰어나다.
    - Maven : Pom.xml을 통해 여러 라이브러리를 관리할 수 있게 만든 아파치의 배포 도구.
    - Gradle의 성능이 좋지만 일반적으로 스프링 프로젝트는 Maven을 자주 사용함.
    
    [메이븐(Maven)과 그래들(Gradle)의 개념 및 비교](https://dev-coco.tistory.com/65)
    
    [Maven과 Gradle의 차이](https://hyojun123.github.io/2019/04/18/gradleAndMaven/)
    
- Packaging : Java 어플리케이션을 쉽게 동작하도록 하는 작동 방식. 일반적으로 스프링 프로젝트에서는 Jar를 사용한다.
    
    [JAR vs WAR 배포의 차이](https://velog.io/@mooh2jj/JAR-vs-WAR-%EB%B0%B0%ED%8F%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4)
    
- 사용하는 Dependency를 체크한다 (lombok, spring web 등)
- Pom.xml에서 dependency를 추가한다

### 2__데이터베이스 연동에 필요한 VO와 DAO, application.properties를 작성한다.

- VO는 lombok을 활용하기 때문에 @Data 어노테이션을 붙인다.
- DAO는 DBmanager를 통해 값을 전달하므로 @repository 어노테이션을 붙인다.

### 3__mybatis 홈페이지를 참조하여 sqlMapConfig, Mapper, DBmanager 등을 작성한다.

[mybatis – MyBatis 3 | Getting started](https://mybatis.org/mybatis-3/getting-started.html)

 

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%206.png)

- MapConfig를 통해 db의 정보와 mapper 파일의 경로를 입력한다.
- <typeAliases> 를 통해 Mapper에서 활용하는 vo의 애칭을 설정할 수 있다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

- id는 sql문의 애칭.
- resultType은 sql문이 적용되는 vo. (config에서 vo의 이름을 지정할 수 있음)
- namespace는 이 sql문이 저장되는 장소 자체의 이름을 뜻한다.

```xml
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="rehabillity">
    <!--전체 글 보기(페이징 처리 전)-->
    <select id="findAll" resultType="RehabillityVO">
        select * from rehabillity
    </select>
    <!--글 상세보기-->
    <select id="findByNo" resultType="RehabillityVO">
        select * from rehabillity where no=#{no}
    </select>
    <!--글 작성-->
    <insert id="insert" parameterType="RehabillityVO">
        insert into rehabillity(no, writer, pwd, title, content, regdate, fname, b_ref, b_step, b_level)
        values(#{no}, #{writer}, #{pwd}, #{title}, #{content}, #{regdate}, #{fname}, #{b_ref}, #{b_step}, #{b_level})
    </insert>
    <!--글 수정-->
    <update id="update" parameterType="RehabillityVO">
        update rehabillity set writer=#{writer}, title=#{title}, content=#{content}, fname=#{fname}
        where no=#{no} and pwd=#{pwd}
    </update>
    <!--글 삭제-->
    <delete id="delete" parameterType="RehabillityVO">
        delete rehabillity where no=#{no} and pwd=#{pwd}
    </delete>
</mapper>
```

- sqlMapConfig에서 설정한 애칭을 통해 type을 설정한다.
- 기능에 알맞는 sql문을 작성한다.
    - 변수는 #{변수명}을 통해 표현한다.

- Mapper에서 설정한 sql문을 실행하는 메소드들을 담는 DBmanager 작성

```java
static {
		try {
			String resource = "mapConfig의 경로";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		}catch(Exception e) {
			System.out.println("예외" + e.getMessage());}
	}
```

![Untitled](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/Untitled%207.png)

- sql문들을 담는 컨테이너인 sqlSessionFactory를 생성하고, 이 sql문들을 활용할 수 있는 메소드들을 작성한다.

### 4__DBmanager를 통해 만든 메소드를 통해 DAO를 완성하고, 이를 Controller에 전달하여 View를 만든다.

[페이징 처리하기 (thymeleaf 활용)](%5BSpring%20&%20mybatis%20&%20JPA%5D%20d2b49b3e0271488b93fbc9951dffa9de/%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%B5%E1%86%BC%20%E1%84%8E%E1%85%A5%E1%84%85%E1%85%B5%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20(thymeleaf%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC)%20eeff5d817d284e7984e0c4d93d80d7fa.md)

## 2) Spring & JPA로 간단한 게시판 만들기

### Mybatis에서 VO와 DAO를 만든 것처럼 Entity와 DAO를 만든다

- Entity란 DB에 존재하는 테이블과 1대1로 대응되는 객체. @Entity와 @Table 어노테이션을 통해 어떤 테이블에 대한 Entity인지를 명시할 수 있다.
- 어노테이션 정리
    - @

## 기타 사항들

### RequestMapping과 PostMapping, GetMapping

### 인텔리제이에서 Sprin하기

Spring & Mybatis 기본 개념

- Spring이란
- DI 의존성 주입
- AOP 관점 지향 프로그래밍
- Pojo 방식
- 어노테이션
- 스프링 MVC의 기본 구조
- Spring 시큐리티

- Spring & Mybatis 활용하여 홈페이지 만들기
- 데이터 JPA를 활용하여 홈페이지 만들기
