---
layout: post
title: "4장 스프링 MVC - 비동기"
date: 2020-08-19
author: jmssun
categories: SpringBoot2
---

# 스프링 MVC - 비동기

###### spring-boot-starter-web : 일반 웹 애플리케이션 빌드할 때 의존성에 추가
###### spring-boot-starter-webflux : 반응형 애플리케이션을 빌드할 때 추가

[XmlHttpRequest 참고](https://unikys.tistory.com/232)


#### 컨트롤러와 TaskExecutor로 비동기 요청 처리
스프링MVC는 메소드에서 다수의 반환 유형을 지원
1. Callable : 내부에 메세지를 클라이언트에게 반환하기 전에 지연을 가정하는 임의 대기 처리 코드가 있다..?
 - 이 프로세스는 보통 값을 바로 반환하는 것 대신에 컨트롤러는 java.util.concurrent.Callable를 먼저 반환하고, Spring에서 관리하는 별도의 Thread에서 값을 반환
[TaskExecutor 참고](https://linuxism.ustd.ip.or.kr/1018)
2. CompletableFuture : supplyAsync를 호출하면 작업이 실행. CompletableFuture를 반환. 비동기처리에 TaskExecutor를 재사용하는 방법
 - [CompletableFuture 참고1](https://brunch.co.kr/@springboot/267)
 - [CompletableFuture 참고2](https://pjh3749.tistory.com/280)
3. 

#### 응답 작성

ResponseBodyEmitter : 하나의 결과 대신 다수의 객체를 클라이언트에게 반환하고자 할 때 유용
객체를 전달할 때 HttpMessageConverter를 사용해서 결과를 변환
- HttpMessageConverter : HTTP 요청 본문을 객체로 변경하거나, 객체를 HTTP 응답 본문으로 변경할 때 사용 (@RequestBody, @ResponseBody)
- SseEmitter : 서버에서 클라이언트로 이벤트를 전달할 수 있음. 서버-전달-이벤트는 서버에서 클라이언트로 전달되는 메시지
 요청처리 메소드에서 이벤트를 전달하려면 SseEmitter 인스턴스를 만들고 반환해야함
 
####  웹 소켓
http와 다르게 완벽한 신을 제공

1. spring-boot-starter-websocket 의존성 추가해서 필요한 의존성을 가져오고 스프링 부트에서 웹 소켓 지원을 자동으로 구성
 - 웹소켓을 활성화하려면 @EnableWebSocket 추가 (@SpringBootApplication 또는 @Configuration 애노테이션이 붙은 클래스에 추가)
2. WebSocketHandler 생성
 - 


#### STOMP와 웹 소켓
메시지 브로커를 구성하고 메시지를 처리할 @Controller 클래스에서 @MessageMapping 애노테이션이 붙은 메소드를 사용

- STOMP : 텍스트 기반의 매우 간단한 프로토콜, TCP나 웹 소켓 같은 신뢰할 수 있는 양방향 네트워크 프로토콜에서 사용이 가능
 [https://hwiveloper.dev/2019/01/10/spring-boot-stomp-websocket/](https://hwiveloper.dev/2019/01/10/spring-boot-stomp-websocket/ttp://)





























