---
layout: post
title: "1장 스프링부트 - 소개"
date: 2020-08-05
author: jmssun
categories: Spring Boot2
---

## 스프링부트2

스프링부트는 프레임워크를 확장해 자동 구성을 가능하게 한다

###### JMS, JDBC, JPA, 래빗MQ 등과 같은 하부 구조를 자동으로 구성한다
- JMS : Java Message Service
- JPA :  Java Persistence API (자바 ORM 기술에 대한 API 표준 명세)
    한마디로 ORM을 사용하기 위한 인터페이스를 모아둔 것 이라고 볼 수 있다.

스프링부트 소스 코드는 메이븐에서 빌드됨
메이븐은 필요한 의존성을 가져오고, 코드를 컴파일하며, 아티팩트(jar 파일)를 생성

#### 스프링 초기 구성기(STS)를 사용해 스프링 부트 애플리케이션 만들기
1. [http://start.spring.io](http://start.spring.io) 에 접속
2. 메이븐 또는 그래들 중 생성하고자 하는 방식을 선택하고 스프링 부트 버전 선택
3.  Generate Project 버튼 클릭

##### maven gradle 차이
```
maven : 내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데 필요한 다른 라이브러리들까지 관리하여 네트워크를 통해 자동으로 다운 받아준다.
gradle : 기본적으로 빌드 배포 도구. 별도의 빌드스크립트를 통하여 사용할 어플리케이션 버전, 라이브러리 등의 항목을 설정할 수 있다. 스크립트 언어로 구성되어 있기때문에, XML과 달리 변수선언, if, else, for등의 로직이 구현가능하여 간결하게 구성 가능하다. 
```

pom.xml에 spring-boot-starter-test 라는 추가 의존성이 있는데 테스트에 필요한 스프링 테스트, 모키토, Junit 4, AssertJ 같은 의존성을 내려받는다. 이 의존성 하나로 테스트할 준비가 완료된다.

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>


```

이 플러그인은 최종 jar파일을 생성하는데 사용된다. 
애플리케이션을 가동하려면 java -jar <애플리케이션>.jar를 실행하면 된다.

#####  jar 빌드하기
스프링 초기 구성기를 사용하면 모든 프로젝트에는 메이븐 래퍼가 포함돼 애플리케이션을 쉽게 빌드할 수 있다.
cmd창에서 프로젝트가 있는 폴더를 탐색한 뒤 ./mvnw package 또는 ./gradlew build를 실행한다
이 명령은 target 폴더에 수행 가능한 아티펙트를 생성한다.































