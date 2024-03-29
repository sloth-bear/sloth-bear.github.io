---
layout: post-detail
title: "Spring은 어떤 문제를 해결해줄까? #1 Java 개발 간소화(POJO, DI, AOP, Template)"
date: 2021-10-31 21:53:00 +0900
category: blog
tags: spring di aop springinaction
---


# 들어가기  전에...
스프링 프레임워크의 기능은 매우 다양하지만, 가장 핵심적인 두 가지 기능은 종속객체 주입(DI: Dependency Injection)과 관점 지향 프로그래밍(Aspect Oriented Programming)이다. 이 두 가지는 스프링의 기본 개념으로 잘 이해하고 있어야 한다.

1장에서는 DI와 AOP를 소개하고 있다. 이 장의 목표는 느슨하게 결합된 자바 애플리케이션을 개발하는 데 이들이 어떤 역할을 하는지 이해하는 것이다. 

이 포스팅은 예전 버전의 Spring을 설명한 책을 요약한 것이며 Spring framwork 이해에 초점을 두고 있다.

Spring은 open source framework로, enterprise application 개발의 복잡함을 겨냥해 만들어졌다. 
과연 어떤 종류의 문제를 해결해줄까?


# 목표
1. DI와 AOP 개념 이해
2. 느슨하게 결합된 자바 애플리케이션을 개발하는 데 DI와 AOP는 어떤 역할을 하는가?


# Java 개발 간소화
Spring은 EJB로만 할 수 있었던 작업을 평범한 JavaBean(=application component, 이 책에서는 느슨한 수준의 자바빈 정의)을 사용해 할 수 있게 해준다. 
이는 서버 측 개발에만 유용한 것은 아니다. 간소함, 테스트의 용이함, 낮은 결합도라는 견지에서 보면 모든 자바 애플리케이션이 스프링의 덕을 볼 수 있다. 

Spring은 Java 개발을 간소화하기 위해 네 가지 주요 전략을 채택했다. 
- POJO를 이용한 가볍고 비 침투적인 개발 
- DI, interface orientation(인터페이스 지향)을 통한 느슨한 결합도 (loose  coupling)
- aspect와 공통 규약을 통한 선언적 프로그래밍 
- aspect와 template을 통한 상투적인 코드 축소 

Java 개발을 간소화하는 방법에 대한 구체적인 예제들을 살펴보자.

## #1. POJO의 힘
```
package com.habuma.ejb.session;

import javax.ejb.SessionBean;
import javax.ejb.SessionContext;

public class HelloWorldBean implements SessionBean {

  public void ejbActivate() {}
  
  public void ejbPassivate() {}
  
  public void ejbRemove() {}
  
  public void setSessionContext(SessionContext ctx) {}
  
  public String sayHello() { // 핵심 비즈니스 로직
    return "Hello World";
  }

  public void ejbCreate() {}
}
```
`SessionBean` 인터페이스에 존재하는 여러 생명주기 콜백 메소드들은 매우 침투적이다. (침투적: 특정 기술을 사용할 때 관련 코드나 규약이 등장하는 경우. ex: interface 구현하는 데 관련 없는 메소드 다 오버라이딩)

`SessionBean` 인터페이스는 여러 생명주기 콜백 메소드 구현으로 EJB의 생명주기에 연결시킨다. 이처럼 '강제로' 연결시키게 되면서 `HelloWorldBean`의 대부분의 코드가 전적으로 프레임워크를 위한 용도로 이루어져 있다. EJB2 외에도 Struts, WebWork, Tapestry의 초기 버전 등도 자신만의 프레임워크를 강요했다. 이는 불필요한 코드가 흩어져 있는 클래스 작성을 강요하고, 프레임워크에 묶이게 되며, 테스트 수행도 어려워지게 한다. 

그러나 스프링은 API를 이용하여 애플리케이션 코드의 분산을 막는다. 특화된 인터페이스 구현이나 클래스 확장을 거의 요구하지 않는다. 클래스에 스프링 어노테이션이 붙기도 하지만, 그렇지 않은 경우 POJO다. 

```
package com.habuma.spring; 

public class HelloWorldBean {
  public String sayHello() {
    return "Hello World";
  }
}
```

불필요한 생명주기 메소드는 모두 제거되었다. POJO에 힘을 불어넣기 위해서는 DI를 활용해 조립할 수 있다. 


## #2. Dependency Injection
DI의 정의는 다음과 같다. 

* DI (Dependency Injection) - 종속객체 주입
한국어로 번역하면 '의존성 주입'이라고 해석된다. 이 표현은 없는 의존성을 만들어 주입한다는 오해를 일으킬 수 있고, 이해를 어렵게 한다. DI는 없는 의존성을 주입하는 것이 아니라 <b>의존성은 이미 존재하되, '실제 객체가 필요로 하는 종속객체를 주입하는 것'</b>을 의미한다.


일반적으로 실제 애플리케이션에서는 두 개 이상의 클래스가 서로 협력하여 비즈니스 로직을 수행한다. 
이 때 각 객체는 협력하는 객체에 대한 Reference(종속객체)를 얻을 책임을 진다. 그 결과, 결합도가 높아지고 테스트하기 힘든 코드가 만들어지기 쉽다. 

```
package com.springinaction.knights;

public class HostageRescuingKnight implements Knight {
  private RescueHostageQuest quest;

  public HostageRescuingKnight() {
    quest = new RescueHostageQuest();
  }

  public void embarkOnQuest() throws QuestException {
    quest.embark();
  }
}
```

위 코드는 `RescueHostageQuest`가 `RescueHostageQuest`에 강하게 결합돼있다. 만약 다른 Quest를 수행해야 한다면 해당 기사는 아무 것도 할 수 없다. 또한 `embarkOnQuest()`가 호출될 때 quest의 `embark()` 메소드가 호출되는지 테스트할 명확한 방법이 없다. 

* 결합도가 높을 경우 
* 테스트와 재활용이 어려워짐 
* 오류를 하나 수정하면 다른 오류가 발생하는 경향 

결합은 반드시 필요하지만, 주의해서 관리해야 한다. 

이때 DI를 사용하면 객체는 시스템에서 각 객체를 조율하는 제 3자에 의해 생성 시점에 종속 객체가 부여된다. 객체가 직접 생성하거나 얻지 않는다. 즉, 주입되는 것이다. 

* Constructor injection

```
package com.springinaction.knights;

public class BraveKnight implements Knight {
  private Quest quest; 

  public BraveKnight(final Quest quest) {
    this.quest = quest;
  }

  public void embarkOnQuest() throws QuestException {
    quest.embark();
  }
}
```

`BraveKnight`는 `Quest`를 직접 생성하지 않는다. 생성 시점에 생성자 인자로 `Quest`가 주입된다.  더불어 `Quest` 인터페이스 타입으로 제공됨으로써 주입되었던 `Quest` 구현체를 떠날 수 있게 된다. 즉, `BraveKnight`는 특정 `Quest` 구현체에 결합되지 않는다. 결합은 되었지만 느슨해진 것이다. 더불어 `Quest` mock 객체를 통해 `embarkOnQuest()` 실행 시 `Quest`의 `embark()`가 한 번 호출되었는지 테스트도 할 수 있게 되었다. 

### #2-1. Wiring
위에서 `BraveKnight`클래스가 어떤 `Quest`를 부여 받을지 지정할 수 있는 점을 확인하였다. 
이처럼 애플리케이션 컴포넌트 간의 관계를 정하는 것을 와이어링(wiring)이라고 한다. 

* Annotation 기반 (Spring 2.5~)

```
package com.springinaction.knights;

public class BraveKnight implements Knight {

  private Quest quest; 

  // @Autowired // Spring 4.3부터 생략 가능
  public BraveKnight(@Qualifier("rescueHostageQuest") final Quest quest) {
    this.quest = quest;
  }

  public void embarkOnQuest() throws QuestException {
    quest.embark();
  }
}
```

`@Primary`를 쓰는 방법도 있는데 여기서는 생략한다.


* xml 기반 (Spring 2.5 이전)

```
...
<bean id="knight" class="com.springinaction.knights.BraveKnight">
  <constructor-arg ref="quest" />
</bean>

<bean id="quest" class="com.springinaction.knights.SlayDragonQuest" />
...
```


### #2-2. Application context
bean에 관한 정의들을 바탕으로 bean을 엮어준다. application을 구성하는 객체의 생성과 와이어링을 전적으로 책임지고 있다. 스프링에는 application의 여러 구현체가 존재하며, 각각의 주요 차이점은 설정을 로드하는 방법에 있을 뿐이다. 

xml 파일에 선언되어있다면 application context로 `ClassPathXmlApplicationContext`를 사용하면 좋다. Application의 ClassPath에 있는 하나 이상의 XML파일에서 Spring context를 로드한다: 

```
...
public static void main(String[] args) {
  ApplicationContext context = new ClassPathXmlApplicationContext("knights.xml");

  Knight knight = (Knight) context.getBean("knight");
  knight.embarkOnQuest();
}
...
```

`main()` method는 `knights.xml` 파일을 기반으로 spring application context를 생성한다.
ID가 knight인 bean을 조회하기 위해 factory로 application context를 사용한다.
이후 `Knight` 객체에 대한 reference를 얻고, `embarkOnQuest()` 메소드를 호출한다.

`Knight` 클래스는 어떤 유형의 Quest를 사용할지 알지 못한다. BraveKnight를 다룬다는 사실도 알지 못한다.
`knights.xml` 파일만이 어떤 구현체인지 확실히 알고 있다.


또는 `AnnotationConfigApplicationContext`를 활용할 수도 있다:
```
...
public static void main(String[] args) {
  ApplicationContext context = new AnnotationConfigApplicationContext(KnightConfig.class);

  Knight knight = (Knight) context.getBean("knight");
  knight.embarkOnQuest();
}
...
```


## #3. Aspect 적용
관점 지향 프로그래밍은 애플리케이션 전체에 걸쳐 사용되는 기능을 재사용할 수 있는 컴포넌트에 담을 수 있게 해준다. 소프트웨어 시스템 내부의 관심사들을 서로 분리(separation of concerns)하는 기술이라고도 할 수 있다.
예를 들면, 시스템을 구성하는 여러 컴포넌트는 본연의 특정한 기능 외에 로깅, 트랜잭션 관리, 보안 등의 시스템 서비스도 수행해야 하는 경우가 많다. 이와 같은 시스템 서비스는 여러 컴포넌트에 관련되는 경향이 있어 <b>횡단관심사(cross-cutting concerns)</b>라고 한다.

이 경우 두 가지 차원에서 복잡해질 수 있다.:
* 시스템 전반에 걸친 관심사를 구현하는 코드가 여러 컴포넌트에서 중복, 변경해야 할 경우 모두 변경해야 됨 (추상화해서 공통 컴포넌트로 분리했다 하더라도 여러 컴포넌트에서 중복해서 호출되는 문제)
* 본연의 기능과 관련 없는 코드로 인해 지저분해짐

AOP는 시스템 서비스를 모듈화해서 컴포넌트에 선언적으로 적용할 수 있게 해준다. 즉, 응집도가 높고 본연의 관심사에 집중하는 컴포넌트를 만들 수 있다. (=POJO를 평범하게 해준다)

`Knight` 클래스에 quest 수행 전 후의 logging을 담고 싶다고 가정해보면, 아래와 같은 고민들이 생긴다.
* `Knight` 클래스는 logging과는 관련 없이 자신의 역할을 수행한다. 그렇다면 logging은 정말 필요한 것일까?
* logging의 역할을 `Knight` 클래스가 요청하는 것이 과연 적절할까?
* logging을 하지 않는 `Knight` 클래스를 구현해야 한다면? `Knight` 클래스에 종속되어있는 logging에 대한 `null`처리까지 필요한 것일까?

여기서 AOP를 사용하면 logging을 `Knight`가 직접 처리하는 일에서 벗어날 수 있다.

* `knight.xml`
`Minstrel` 클래스가 logging을 담당한다. 
다음은 AOP 설정 네임스페이스를 사용해 `Minstrel` bean이 aspect라고 선언한 예제이다.

```
...
<bean id="knight" class="com.springinaction.knights.BraveKnight">
  <constructor-arg ref="quest" />
</bean>

<bean id="quest" class="com.springinaction.knights.SlayDragonQuest" />
<bean id="minstrel" class="com.springinaction.knights.Minstrel" />

<aop:config>
  <aop:aspect ref="minstrel">
    <aop:pointcut id="embark" expression="execution(* *.embarkOnQuest(..))" />
    <aop:before pointcut-ref="embark" method="singBeforeQuest" />
    <aop:after pointcut-ref="embark" method="singAfterQuest" />
  </aop:aspect>
</aop:config>
...
```

1. `Minstrel` Bean 선언
2. `<aop:aspect>` 엘리먼트에서 `minstrel` bean 참조
3. `<aop:pointcut>` 정의
4. `before advice`, `after advice` 정의: `embarkOnQuest()` 메소드 실행 전후 호출될 메소드들 선언

`pointcut-ref` attribute는 emark라는 이름의 pointcut을 참조한다. `pointcut`엘리먼트에 `advice`가 가 적용될 위치를 선택하는 `expression` attribute와 함께 정의되어있다. `expression` 구문은 AspectJ의 포인트컷 표현식 언어다. 이 부분은 4장에서 더 자세히 알아본다.

위 예제를 보며 알 수 있는 두 가지점은 아래와 같다.
* `Minstrel`이 POJO다. aspect로 사용될 것임을 나타내는 내용은 Spring context에서 선언적으로 수행된다.
* `BraveKnight`가 `Minstrel`을 명시적으로 호출하지 않아도 적용될 수 있다. 실제로 `BraveKnight`는 `Minstrel`을 알지 못한다.

logging도 좋긴 하지만, Spring AOP는 선언적 트랜잭션과 보안 등의 서비스를 제공하기 위해 도입되었다. 이와 관련된 자세한 내용은 6장(선언적 트랜잭션)과 9장(보안)에서 설명된다.


## #4. Template을 이용한 상투적인 코드 제거
반복되게 작성되는 상투적인 코드들이 있다. Java API에는 상투적인 코드가 많이 포함되어있는데, 대표적인 예가 JDBC 작업이다.

```
public Employee getEmployeeById(final long id) {
  Connection conn = null;
  PreparedStatement stmt = null;
  ResultSet rs = null;
  
  try {
    conn = dataSource.getConnection();
	stmt = conn.prepareStatement("SELECT id, name " + "employee where id=?");
	stmt.setLong(1, id);
	
	rs = stmt.executeQuery();
	
	Employee employee = null;
	
	if (rs.next()) {
	  employee = new Employee();
	  employee.setId(rs.getLong("id"));
	  employee.setName(rs.getString("name"));
	}
	
	return employee;
  } catch (SQLException e) {
  	// 어떤 작업을 수행해야 될 지에 대한 고민
  } finally {
    if (rs != null) {
	 try {
	   rs.close();
	 } catch (SQLException e) {}
	}
	
	if (stmt != null) {
	 try {
	   stmt.close();
	 } catch (SQLException e) {}
	}
	
	if (conn != null) {
	 try {
	   conn.close();
	 } catch (SQLException e) {}
	}
  }
  
  return null;
}
```

위 코드에서는 실질적인 중요한 작업을 알기에 어렵다. checked exception까지 잡아줘야 한다. 이와 같은 부분들이 상투적인 코드다.

Spring은 template에 상투적인 코드를 캡슐화하여 반복적인 코드를 제거하는 방법을 찾는다. `JdbcTemplate`은 전통적인 JDBC에서 필요한 모든 형식 없이도 데이터베이스 작업을 수행할 수 있게 해준다.

`SimpleJdbcTemplate`을 사용해보았다. (Java5 기능 활용)

```
public Employee getEmployeeById(final long id) {
  return jdbcTemplate.queryForObject(
    "SELECT id, name FROM employee WHERE id = ?",
	new RowMapper<Employee>() {
	  public Employee mapRow(ResultSet rs, int rowNum) throws SQLException {
	    return Employee.builder()
		  .setId(rs.getLong("id"))
		  .setName(rs.getName("name"))
		  .build();
	  }
	},
	id);
}
```

JDBC의 상투적인 코드는 모두 template 내부에서 처리된다. 이처럼 Java 개발의 복잡성은 Pojo 지향 개발, DI, AOP, template을 이용해 처리할 수 있었다. 그리고 XML 기반 설정 파일에서 bean, aspect를 설정하는 방법도 살펴보았다. 이러한 파일들이 어디에, 어떻게 로드되는지 application bean이 위치하는 spring container에 대한 내용을 다음 장에서 살펴본다.


# References
* Craig Walls, Spring in action third edition
* https://leejisoo860911.tistory.com/2
* https://jaimemin.tistory.com/1772
