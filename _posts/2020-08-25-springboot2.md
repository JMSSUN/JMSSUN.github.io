---
layout: post
title: "5장 스프링 웹플럭스"
date: 2020-08-25
author: jmssun
categories: SpringBoot2
---

# 스프링 웹플럭스

- 스프링 웹플럭스 : 스프링 웹플럭스는 스프링5에서 새로 등장한, 웹 애플리케이션에서 리액티브 프로그래밍을 제공하는 프레임워크
	- 비동기-논블록킹 리액티브 개발에 사용
	- 효율적으로 동작하는 고성능 웹 애플리케이션 개발
	- 서비스간 호출이 많은 마이크로서비스 아키텍처에 적합
	
    
#### 스프링 웹플럭스로 반응형 애플리케이션 개발하기
    
- HttpHandler : 스프링 웹플럭스의 가장 하위 구성요소, 단일 핸들러 메소드가 있는 인터페이스
- 웹 요청이 스프링 웹플럭스 애플리케이션으로 들어오면 HandlerAdapter가 먼저 요청을 받음 -> 요청을 처리하는데 필요한 스프링의 애플리케이션 컨텍스트에 설정된 각 구성요소를 구성
- 컨트롤러 클래스가 적절한 핸들러 메소드를 선택하면 요청과 함께 핸들러 메솓의 로직을 호출. 일반적으로 백엔드 서비스를 호출해 요청을 처리
- 핸들러 메소드가 요청을 모두 처리한 후 핸들러 메소드의 반환값으로 표시되는 뷰에게 컨트롤을 위임(핸들러 메소드의 반환 값은 String이나 void가 될 수 있음)
- Mono 객체 : 반응형으로 만들어줌 
	- [https://wedul.site/569](https://wedul.site/569)
	- [https://lts0606.tistory.com/304](https://lts0606.tistory.com/304)


- 반응형으로 요청을 처리하려면 웹플럭스가 가능해야함 spring-boot-starter-webflux 의존성 추가
	- 반응형 실행의 기본 모듈인 네티가 포함돼있음
- @GetMapping 애노테이션은 컨트롤러의 HTTP GET 핸들러 메소드로 만드는데 사용됨


#### 반응형 REST 서비서의 배포와 사용하기

- 반응형으로 만들려면 응답값을 Mono 또는 Flux로 래핑해야 함



#### 템플릿 엔진으로 타임리프 사용하기
- spring-boot-starter-webflux 및 spring-boot-starter-thymeleaf 의존성을 추가하면 Thymeleaf 라이브러리 뿐만 아니라 Thymleaf Spring Dialect도 추가돼 잘 통합된다. ThymeleafReactiveViewResolver를 자동으로 구성할 수 있음


#### 웹플럭스와 웹 소켓
- javax.websocket-api 추가하고 핸들러를 구현하는 반응형 WebSocketHandler 인터페이스를 사용
	- 웹소켓 1.1 버전을 사용해야함
- 콜백 메서드 onopen, onmessage, onclose에 각각 기능을 넣는다. 이 중 가장 중요한 onmessage는 서버에서 메시지를 수신할 때마다 호출되는 콜백





참고링크
[https://brunch.co.kr/@springboot/96](https://brunch.co.kr/@springboot/96)
[https://www.youtube.com/watch?v=j6SFTTxGCK4](https://www.youtube.com/watch?v=j6SFTTxGCK4)
[https://12bme.tistory.com/565](https://12bme.tistory.com/565)