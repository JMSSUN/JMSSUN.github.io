---
layout: post
title: "3장 스프링 MVC"
date: 2020-08-10
author: jmssun
categories: SpringBoot2
---

# 스프링 MVC

#### Spring MVC (Model / View / Controller) 패턴
- Controller(서블릿), Model(JavaBean), View(JSP)가 서로 상호작용하면서 유지보수가 쉽게 원활한 구조를 가지고 있음
- 웹브라우저 -> 디스패쳐서블릿 -> 컨트롤러 -> ModelAndView -> 컨트롤러 -> 디스패쳐서블릿 -> 웹브라우저
- ModelAndView는 실제 JSP 정보를 갖고있지 않기 때문에 ViewResolver가 실제 jsp 이름으로 변환하여 해당 view를 검색함

참고 : [https://min-it.tistory.com/7](https://min-it.tistory.com/7)

#### 스프링부트는 스프링 MVC에 필요한 컴포넌트를 사용하기 위해 자동 구성을 진행
- 클래스패스에서 스프링 MVC를 감지해야하는데 의존성에 spring-boot-starter-web을 추가해야함

``` 
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```
- DispatcherServlet을 설정하기 위해 구성을 추가함
- 내장형 톰캣 서버를 시작하는데 필요한 jar 파일을 모두 내려받음

##### @RestController : 클래스가 @Controller이며 스프링 부트로부터 감지될 대상이라는 것을 가리키게된다.
##### @ResponseBody : 모든 요청 처리 메소드에 추가돼 클라이언트에 결과를 전송한다는 것을 알려줌
##### @GetMapping : 메소드를 / URL에 도착하는 모든 GET요청에 매핑함


#### 스프링 MVC로 REST 리소스 노출하기

##### JSON 마샬링
- 마샬링 : 메모리에 있는 오브젝트를 Ascii 또는 Binary  형태로 가지런히 정렬해서 네트워크로 날려버린다는 뜻
- 언마샬링 : 가지런한 데이터를 모아서 원래 오브젝트로 원복시켜서 메모리에 로드하는 것
- spring-boot-start-web 의존성에는 이미 Jackson 라이브러리가 기본으로 포함돼 있음
- Jackson : 높은 성능을 제공하는 JSON parser


#### 스프링 부트에서 타임리프 사용하기

##### 타임리프(Thymeleaf) : 스프링 부트에서 사용하는 뷰템플릿으로 jsp를 대신해 사용자에게 보여줄 화면 처리를 위한 파일
스프링에서 밀고있다고 함
- 템플릿 엔진으로 th:xx 형식으로 속성을 html 태그에 추가하여 값이나 처리 등을 페이지에 심을 수 있음.
- .html 확장자로 끝나서 다른 자바 프레임 워크에 갖다 붙이기가 상당히 용이
- 스프링에서 공식 지원하므로 추가적인 뷰설정을 따로 해주지 않아도 됨
- spring-boot-starter-thymeleaf를 의존성으로 추가
- 변수식 : ${}
- 메시지식 : #{} - messages.properties
- 링크식 : @{}
- 객체의 변수식 : *{}

##### 인덱스 페이지 추가
```
<html xmlns:th="http://www.thymeleaf.org">
```
- 타임리프의 네임스페이스를 활성화함
- 타임리프 명령어는 모델의 데이터를 사용하기 위해 th:text="${}" 와 [[${}]]로 이용해서 사용함

##### 컨트롤러와 뷰 추가
- 요청을 처리하고 모델을 준비하는 컨트롤러
- 렌더링하는 뷰
- @Controller 

##### 상세 페이지 추가
- 요청 인자는 @RequestParam 애노테이션이 붙은 메소드 인자로 사용됨. Get 요청을 처리

참고 : [https://jongminlee0.github.io/2020/03/12/thymeleaf/](https://jongminlee0.github.io/2020/03/12/thymeleaf/)

#### 예외 처리 다루기

- 스프링 부트에서 기본적으로 오류 처리가 활성화
- server.error.whitelabel.enabled 속성을 false로 하면 전체를 비활성화 
- src/main/resources/templates 폴더에 error.html 추가
- server.error.include-exception을 true, server.error.include-stacktrace를 always로 설정하면 맞춤형 오류페이지에 예외 클래스명과 스택트레이스가 포함됨

오류 페이지 모델의 속성들

| 속성 | 설명 |
|--------|--------|
|   timestamp     |     오류가 발생한 시간   |
|   status     |     상태 코드   |
|   error     |     오류 원인   |
|   exception     |     상위 예외의 클래스 이름(설정된 경우)   |
|   message     |     예외 처리 메시지   |
|   errors     |     BindingResult의 ObjectError(바인딩이나 검증을 사용한 경우)   |
|   trace     |     예외 스택트레이스(설정된 경우)   |
|   path     |     예외가 발생한 url 경로   |


- ErrorAttributes 컴포넌트 사용으로 가능

#### 애플리케이션 국제화
- 스프링은 MessageSource 인터페이스를 구현해, 메시지 소스를 사용한 텍스트 메시지 처리가 가능
- src/main/resources 에서 messages.properties를 찾았을 때 자동으로 MessageSource를 구성

#### 사용자 언어 결정하기

- 스프링 MVC는 LocaleResolver를 이용해서 웹 요청과 관련된 Locale을 추출하고, 이 Locale 객체를 이용해서 알맞은 언어의 메시지를 선택
- 스프링이 제공하는 LocaleResolver 구현 클래스

| 클래스 | 설명 |
|--------|--------|
|   AcceptHeaderLocaleResolver     |   웹 브라우저가 전송한 Accept-Language 헤더로부터 Locale을 선택한다. setLocale()메서드를 지원하지 않는다. 사용자 웹 브라우저에서 설치된 운영체제의 언어에 따라 설정됨    |
|   CookieLocaleResolver     |    쿠키를 이용해서 Locale 정보를 구한다. setLocale() 메서드는 쿠키에 Locale 정보를 저장한다.    |
|   SessionLocaleResolver     |   세션으로부터 Locale 정보를 구한다. setLocale() 메서드는 세션에 Locale 정보를 저장한다. 세션속성이 존재하지 않은 경우 defaultLocale 속성을 설정할 수 있음    |
|   FixedLocaleResolver     |  웹 요청에 상관없이 특정한 Locale로 설정한다. setLocale() 메서드를 지원하지 않는다.  고정이므로 사용자 언어를 변경할 수 없다.    |

참고 [https://devbox.tistory.com/entry/Spring-Locale-%EC%B2%98%EB%A6%AC](https://devbox.tistory.com/entry/Spring-Locale-%EC%B2%98%EB%A6%AC)

- 사용자 언어 변경하려면 LocaleResolver.setLocale() 또는 LocaleChangeInterceptor를 매핑하는 방식

#### 내장된 서버 선택 및 구성

- 스프링 부트는 기본적으로 톰캣을 컨테이너로 사용한다 (spring-boot-starter-web 아티펙트에서 spring-boot-starter-tomcat 의존성을 통해 표현됨)
- 스프링 MVC를 사용하면 속성을 저장할 때 HTTP 세션을 사용하려고 함


#### 서블릿 컨테이너에 SSL 구성하기
- SSL : 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고, 서버 브라우저가 민감한 정보를 주고받을 때 이것이 도난당하는 것을 막아줌
- server.ssl.key-store를 사용하면 내장된 컨테이너에 HTTPS 접속만 허용하도록 구성됨





