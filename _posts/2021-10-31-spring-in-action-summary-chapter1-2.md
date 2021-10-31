---
layout: post-detail
title: "Spring은 어떤 문제를 해결해줄까? #2 빈을 담는 그릇, 컨테이너"
date: 2021-10-31 21:54:00 +0900
category: blog
tags: spring di aop springinaction
---


# 목표
* Spring Container에 대한 이해 
* 객체는 어떻게 관리되는가

# Bean을 담는 그릇, Container
Spring application에서는 Spring container에서 객체가 태어나고, 자라고, 소멸한다. Spring Container는 객체 생성, wiring, 그리고 이들의 전체 생명주기를 관리한다. (어떤 객체를 생성, 구성, 서로 엮어줘야 하는지 알 수 있게 설정하는 방법은 2장) 즉, Spring Container는 객체들의 삶의 터전이다.

Spring container는 Spring framework의 <b>핵심부</b>에 위치한다. Spring Container의 역할은 아래와 같다. 
* <b>DI(종속 객체 주입)</b>을 이용해 application을 구성하는 <u>컴포넌트 관리</u>
* 협력 컴포넌트 간 연관관계의 형성 (wiring)

이러한 짐들을 container에 덜어버린(=역할을 위임했다고 이해했다.) 객체들은 더 명확하고 이해하기 쉬우며, 재사용을 촉진하고, 단위 테스트가 용이해진다. 
그렇다면 Container에는 어떤 것들이 있을까? 

## 1. Spring Containers
Spring Container는 여러 가지가 있다. 여러 컨테이너 구현체가 존재하며, 크게 두 가지로 분류된다. 

 * 빈팩토리
[Bean Factory (org.springframework.beans.factory.BeanFactory)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html) interface에 의해 정의된다. 
<u>DI에 대한 기본적인 지원을 제공</u>하는 가장 단순한 Container다. 

*  어플리케이션 컨텍스트
[ApplicationContext (org.springframework.context.ApplicationContext)(https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html) interface에 의해 정의된다.
빈팩토리를 확장해서 property file에 텍스트 메시지를 읽고, 해당 이벤트 리스너에 대한 애플리케이션 이벤트 발행 같은 애플리케이션 프레임워크 서비스를 제공하는 컨테이너다.

Spring으로 작업할 때 빈팩토리나 어플리케이션 컨텍스트 중 아무거나 사용해도 상관없지만 빈팩토리는 지나치게 저수준의 기능을 제공하므로 일반적으로 어플리케이션 컨텍스트를 더 선호한다. 


### 1-1. Application Context
Spring에는 다양한 종류의 application context가 존재한다. 가장 많이 접하게 될 세 가지는 아래와 같다. (2021년 기준으로 5가지라 5가지를 소개한다.)
application context를 얻은 다음 context의 `getBean()` 메소드를 호출해 Spring container에서 bean을 조회할 수 있다.

* AnnotationConfigApplicationContext
Spring 3.0에서 도입. `@Configuration`, `@Component`로 정의된 내용을 가져올 수 있다. 

* AnnotationConfigWebApplicationContext
`AnnotationConfigApplicationContext`의 웹 기반.  이 클래스는 Spring의 `ContextLoaderListener` servlet listener 혹은 `web.xml` 파일의 Spring MVC `DispatcherServlet`을 구성할 때 사용할 수 있다.
Spring 3.0 부터는 `WebApplicationInitializer` interface를 구현해 programming 방식으로도 구현할 수 있다. 

* XmlWebApplicationContext
웹 어플리케이션에서 xml 기반 configuration을 사용할 경우 사용 가능하다. 웹 어플리케이션에서 포함된 XML 파일에서 context 정의 내용을 load한다. 

* FileSystemXmlApplicationContext
파일 시스템에서, 즉 파일 경로로 지정된 xml파일에서 context 정의 내용을 load한다.

```
ApplicationContext context = new FileSystemXmlApplicationContext("c:/foo.xml");
```

* ClassPathXmlApplicationContext
ClassPath에 위치한 xml 파일에서 context 정의 내용을 load한다.

```
ApplicationContext context = new ClassPathXmlApplicationContext("foo.xml");
```

이 경우 클래스패스에 포함된 모든 경로(JAR 파일 포함)에서 찾으려 한다. 
 
 
 ### 1-2. Bean Life Cycle
일반적인 Java application에서 bean의 생명주기는 매우 단순하다. new keyword를 이용하거나 역직렬화(deserialization)를 통해 bean을 인스턴스화하고, 이를 바로 사용한다.
빈이 더 이상 사용되지 않으면 가비지 컬렉션 후보가 되어 언젠가 메모리 덩어리가 됐다가 사라지게 될 것이다. 

반면 Spring Container 내에서 빈의 생명주기는 좀 더 정교하다. 
앞서 얘기한 것처럼 Bean life cycle은 Spring container에 의해 관리되며, container는 프로그램 실행 시 시작된다. container는 요청에 따라 bean의 인스턴스를 생성하고, 종속 객체를 주입한다. 이후 container가 닫히면 bean은 파괴된다. 

도식화된 모습을 살펴보자. 

![Spring bean life cycle process](https://user-images.githubusercontent.com/62458327/139584602-3ef84eea-89ef-476f-ad3b-81456517bf79.png)


bean instance는 Java 또는 XML bean 정의를 기반으로 인스턴스화 된다. 이를 사용 가능한 상태로 만들기 위해 일부 전후 초기화 단계를 수행해야 할 수도 있고, 더 이상 bean이 필요하지 않게 되면 IoC Container에서 제거되는데 시스템 리소스를 해제하기 위해 전후 destory 단계를 수행해야 할 수도 있다.

bean이 생성될 때 Spring이 제공하는 <u>커스터마이징 기회(life cycle callback method)</u>를 이용하려면 Life Cycle을 이해해두는 것이 좋다. 

<details>
	<summary>상세히 보기</summary>
	<div>
		1. Spring이 bean을 인스턴스화 한다. 
		2. Spring이 값, bean reference를 bean의 property에 주입한다. 
		3. 빈이 `BeanNameAware`를 구현하면, Spring이 Bean의 ID를 `setBeanName()` 메소드에 넘긴다. 
		4. 빈이 `BeanFactoryAware`를 구현하면, `setBeanFactory()` 메소드를 호출하여 빈팩토리 자체를 넘긴다.
		5. 빈이 `ApplicationContextAware`를 구현하면, Spring이 `setApplicationContext()` 메소드를 호출하고, 둘러싼(enclosing) 어플리케이션 컨텍스트에 대한 참조를 넘긴다. 
		6. 빈이 BeanPostProcessor 인터페이스를 구현하면, Spring은 postProcessBeforeInitialization() 메소드를 호출한다.
		7. 빈이 InitializingBean 인터페이스를 구현하면, 스프링은 afterPropertiesSet() 메소드를 호출한다. custom init method가 존재한다면 해당 메소드가 호출된다.
		8. 빈이 BeanPostProcessor를 구현하면, 스프링은 postProcessAfterInitialization() 메소드를 호출한다.
		9. 이 상태가 되면 빈은 어플리케이션에서 사용할 준비가 된 것이며, 어플리케이션 컨텍스트가 소멸될 때까지 어플리케이션 컨텍스트에 남아 있다. 
		10. 빈이 `DisposableBean` 인터페이스를 구현하면, Spring은 `destroy()` 메소드를 호출한다. custom destroy method가 존재한다면 해당 메소드가 호출된다. 
	</div>
</details>

Spring이 제공하는 Bean의 Life cycle callback 구현 방법은 아래와 같다. 
1. Annotation 
2. 프로그래밍 
3. XML 

3가지 방법 중 Annotation을 이용해 구현해보았다. 

* HelloWorld.java

```
...
@Component
public class HelloWorld {

  @PostConstruct
  public void init() {
    System.out.println(
        "HelloWorld Bean has been instantiated "
        + "and I am the init() method!");
  }

  @PreDestroy
  public void destroy() {
    System.out.println(
        "HelloWorld Bean has been closed "
            + "and I am the destroy() method!");
  }
  
}
```

* HelloWorldTests.java

```
@SpringBootTest
public class HelloWorldTests {

  @Autowired
  ApplicationContext context;

  @Test
  public void test_that_successfully_created_bean() {
    HelloWorld helloWorld = context.getBean(HelloWorld.class);
    
    assert (Objects.nonNull(helloWorld));
  }
}
```

* Result

![Result in console](https://user-images.githubusercontent.com/62458327/139584717-566ff9c9-4038-4d42-b339-d423068f7aef.png)


Spring container가 어떻게 생성되고 로드되는지는 확인하였으나 아직 container에 아무것도 넣지 않았기 때문에 쓸모가 없다.
Spring DI를 활용하려면, container에 어플리케이션 객체를 넣고 wiring 해주어야 한다. 


# 글을 끝내면서...
이해를 위해 도식화된 그림을 2가지 정도로 그려보았었는데, 하나는 요약 하나는 상세 버전이다. 요약 버전은 아래와 같다. 

![Spring bean life cycle process summary](https://user-images.githubusercontent.com/62458327/139584621-c1acbdad-de30-4eae-bbb0-1d8fea1faeef.png)


여기서 궁금한 점이 있었는데, Properties 할당부터 afterPropertiesSet() 전까지를 DI를 볼 수 있을까였다. 아마 아직 종속객체 주입 과정을 잘 이해하지 못한 부분이 있는 것 같고, 해당 process가 잘 흡수되진 않았기 때문에 그런 듯하다. wiring까지 세부적으로 더 공부한 뒤에 다시 한 번 살펴본다면 좋을 것이라 생각한다. 


# References
* https://www.baeldung.com/spring-application-context
* https://howtodoinjava.com/spring-core/spring-bean-life-cycle/
* https://www.geeksforgeeks.org/bean-life-cycle-in-java-spring/
