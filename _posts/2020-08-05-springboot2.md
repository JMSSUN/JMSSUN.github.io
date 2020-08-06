---
layout: post
title: "2장 스프링부트 - 기본"
date: 2020-08-05
author: jmssun
categories: SpringBoot2
---

### 빈 구성
###### 스프링부트에서 클래스를 빈으로 사용
자동으로 클래스를 감지하고 객체를 생성해주는 **@ComponentScan** 활용
@Autowired나 @Value로 주입되는 의존성이나 속성을 함께 사용 가능
또는 생성될 때 빈의 생성자를 사용해 더 많이 제어하도록 @Bean 애노테이션이 추가된 함수를 사용할 수 있음

++@Component 애노테이션이 추가된 클래스는 자동으로 감지돼 스프링 부트에 의해 객체화된다++

@SpringBootApplication 애노테이션이 붙은 클래스를 최상위 패키지에 위치시킨다. 그러면 자동으로 이 패키지와 하위 패키지에 정의된 모든 애노테이션이 붙은 컴포넌트, 구성 클래스를 감지하게 된다. 

```
@Component
public class HelloWorld {

    @PostConstruct
    public void sayHello(){
        System.out.println("Hello");
    }
}
```

@PostConstuct 애노테이션이 붙은 함수는 객체 생성 및 모든 의존성이 주입된 뒤 실행된다. 

##### @Bean 메소드
스프링 컨테이너가 @Bean 메소드를 실행해 빈 오브젝트를 가져오는 방식
펙토리함수 : 생성자 대신 오브젝트를 생성해주는 코드의 도움을 받아서 빈 오브젝트를 생성하는 것 
	(@Bean 애노테이션이 붙은 함수)


###속성 외부화
###### 스프링 부트는 속성을 다양한 위치에서 가져오는 것을 지원한다.

애플리케이션은 다음 리소스가 주어진 순서대로 고려된다.
1. 명령행 인수
2. 패키징된 애플리케이션 외부의 application.properties
3. 패키징된 애플리케이션 내부의 application.properties

2,3번의 경우 활성화된 프로필에 기반한 프로필 관련 속성을 불러올 수 있다. 
프로필을 활성화 하려면 spring.profiles.active 속성을 전달해야하고, 프로필 관련 application-{profile}.properties는 프로필에 관련되지 않은 파일보다 우선한다.

###### @Value 애노테이션 : 스프링이 속성을 찾고 해당 속성의 값을 사용하도록 지시
예를들어 @Value("${lhs}")라고 명시하면 lhs라는 이름을 가진 속성을 감지하고 그 값을 사용한다

**스프링 부트는 기동 시 application.properties를 불러와 해당 파일에 정의된 속성을 사용할 수 있다. 애플리케이션이 실행되면 application.properties에 주어진 값에 맞는 결과를 출력한다.**

#### 외부 application.properties를 사용한 속성 재정의

##### 프로필을 사용한 속성 재정의
스프링 부트는 활성 프로필을 사용해 추가 구성 파일을 불러와 일반적 구성 부분을 전부 대체 또는 일부 재정의할 수 있다.
application-add.properties를 추가하고 op를 다른값으로 입력하면 덧셈이 수행된다.
우선순위가 일반적인 application.properties 보다 더 높기 때문.

##### 명령행 인수를 사용한 속성 재정의

##### 다른 속성 파일로부터 속성 불러오기
application.properties가 아닌 다른 파일을 사용하거나 불러오고자 하는 컴포넌트에 추가 파일을 가지고 있는 경우, 해당 파일을 불러오기 위해 @SpringBootApplication 애노테이션이 붙은 클래스에 @PropertySource 애노테이션을 추가해 사용할 수 있다.

###### @PropertySource 애노테이션 : 시작할 때 추가 속성 파일을 불러올 수 있도록 해준다.
@PropertySource를 사용하는 대신 명령행 매개변수를 사용해 스프링부트가 추가 속성 파일을 불러오게 할 수도 있다.

| 매개변수 | 설명 |
|--------|--------|
| spring.config.name       |  불러올 파일 목록, 쉼표로 구분되고 기본값은 applicataion       |
| spring.config.location       |  속성 파일을 불러올 리소스 위치나 파일, 쉼표로 구분됨       |
| spring.config.additional-location       |  속성 파일을 불러올 추가 리소스 위치나 파일, 쉼표로 구분되고 기본값은 공백      |

### 테스팅

스프링 부트는 테스트 프레임워크의 자동 구성 부분을 확장했다. 빈 관련 Mock 객체를 생성하기 쉽게 모키토를 통합했다.

##### 단위테스트

```
public class MultiplicationTest {
    private final Multiplication addition = new Multiplication();

    @Test
    public void shouldMatchOperation() {
        assertThat(addition.handles('*')).isTrue();
        assertThat(addition.handles('/')).isFalse();
    }

    @Test
    public void shouldCorrectlyApplyFormula() {
        assertThat(addition.apply(2, 2)).isEqualTo(4);
        assertThat(addition.apply(12, 10)).isEqualTo(120);
    }
}
```
함수를 테스트 함수(JUnit4용)로 만들려면 @Test 애노테이션을 추가하면 된다

###### 단위테스트에서 의존성 목 객체 생성
단위 테스트 작성 시 스프링 부트는 자동으로 클래스의 목 객체를 만들고 행동을 기록하는 데 유용한 모키토 프레임워크를 가져온다. 
```
public class CalculatorTest {

    private Calculator calculator;
    private Operation mockOperation;

    @Before
    public void setup() {
        mockOperation = Mockito.mock(Operation.class);
        calculator = new Calculator(Collections.singletonList(mockOperation));
    }

    @Test(expected = IllegalArgumentException.class)
    public void throwExceptionWhenNoSuitableOperationFound() {

        when(mockOperation.handles(anyChar())).thenReturn(false);
        calculator.calculate(2, 2, '*');
    }

    @Test
    public void shouldCallApplyMethodWhenSuitableOperationFound() {

        when(mockOperation.handles(anyChar())).thenReturn(true);
        when(mockOperation.apply(2, 2)).thenReturn(4);

        calculator.calculate(2, 2, '*');

        verify(mockOperation, times(1))
								.apply(2,2);
    }
}
```
@Before 메소드에서 Mockito.mock 메소드를 호출해 연산자 목 객체를 생성한 뒤 목 객체 연산자를 사용해서 Calculator를 구성했다. 

###### 스프링 부트의 통합 테스트
@SpringBootTest로 스프링 부트 기반의 테스트를 지우너한다.
테스트 컨텍스트 프레임워크가 @SpringBootApplication 애노테이션이 포함된 클래스를 찾고 실제 애플리케이션을 시작할 때 사용하게 한다는 의미다.

###### 스프링 부트와 목 객체의 통합 테스트
@MockBean 애노테이션

```
@MockBean
private Calculator calculator
```
Calculator 객체 전체를 목 객체로 대체하고 일반적인 모키토 방식을 사용해 동작을 정의할 수 있도록 만든다.

### 로깅 구성

logging.level.org.springframework.web=DEBUG
springframework.web 로거의 debug 수준 로깅을 활성화 하는 것
logging.level.root=<수준>을 사용해 root 로거의 수준을 설정할 수 있다.

기본적으로 스프링 부트는 콘솔에 로깅하는데 파일로 작성하길 원한다면 **logging.file** 또는 **logging.path** 속성을 정의하면 된다.
```
logging.file=application.log
logging.path=/var/log
logging.file.max-history (기본값은 0, 무제한)
logging.file.max-size (기본값 10MB)
```

#####기존 설정 재사용
기존의 설정을 가져오려면 @Import 또는 @ImportResource 애노테이션을 @Configuration 또는 @SpringBootApplication 애노테이션이 붙은 클래스를 추가한다.


























