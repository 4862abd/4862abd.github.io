---
title: 예외처리에 관한 고찰 - 1 - 작성중
author: park
date: 2023-10-20 18:04:48 +0800
categories: [Java, Programming]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## 목차

<br>
● 들어가기 전<br>
● 목적<br>
● 1. 어드바이저를 통한 예외처리의 편리함<br>
● 2. 예외 결과를 반환할 DTO<br>
● 3. 그러면 스프링이 제공하는 다른 예외들은?<br>
<br>

---

<br>

### ● 들어가기 전

예상했던 것보다 포스팅이 늦어졌다.<br>
그 당시 내가 느낀점과 내 고민을 놓친 기분이라 포스팅을 잘 마무리 할 수 있을까.<br>
하지만 예외처리는 어떤 애플리케이션을 구현해도 겪을 상황일 것이다.<br>
지금 하는 고찰은 언제든 갱신될 수 있다는 것이다.<br>
그래서 나는 이번에 내가 겪은 것을 기록하려 한다.<br>

<br>

---

<br>

### ● 목적

1. <b>어드바이저를 통한 예외처리의 편리함</b><br>
2. <b>추상화가 잘 이루어진 예외처리</b><br>
3. <b>커스텀 예외와 예외 메세지를 포장하여 클라이언트에게 반환할 서비스</b><br>
4. <b>같은 예외 코드, 상황에 맞는 예외 메세지</b><br>


위 네 가지가 큰 목적이었다.<br>
그리고 위 순서는 내가 예외처리를 구현하기 위한 순서였다.<br>

<br>

---

<br>

### ● 1.어드바이저를 통한 예외처리의 편리함

구현 방식은 현업의 프로젝트를 조금 클론코딩 한 것으로 부터 온다.<br>
물론 내 스타일대로 조금은 바꿨다.<br>
<br>
처음 구현을 시작했을 때 눈에 들어온 <b>애너테이션이 3 개</b> 있다.<br>

```java

@ControllerAdvice
@RestControllerAdvice
@ExceptionHandler

```

<br>

문서: [@ControllerAdvice](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)<br>
문서: [@RestControllerAdvice](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestControllerAdvice.html)<br>

<br>
@ControllerAdvice 는 공식문서를 확인해도 @ExceptionHandler 를 언급할 정도로 예외처리에는 어느정도 규격화된 양식이다.<br>
@ExceptionHandler 에 의해 빈으로 등록된 예외에 따라 현재 발생된 예외를 처리할 수 있게 도와준다.<br>
즉, 같은 예외를 담당하는 두 메서드는 동시에 등록할 수 없다.<br>
그건 매개변수가 아닌 예외 처리 범위를 지정하는 방식으로도 등록할 수 없다.<br>

```java

@ControllerAdvice
public class GeneralExceptionAdvice {

    // 방법 1 - 막힘
    // 매개변수 타입으로 담당할 예외 지정
    @ExceptionHandler
    public String errorHandler(CustomException exception) {
        return exception.getMessage();
    }

    @ExceptionHandler
    public String errorHandler2(CustomException exception) {
        return exception.getMessage();
    }

    // ----------------------------------------------------

    // 방법 2 - 막힘
    // @ExceptionHandler 에 처리할 예외 범위를 직접 등록하는 방식
    @ExceptionHandler
    public String errorHandler(CustomException exception) {
        return exception.getMessage();
    }

    @ExceptionHandler(CustomException.class)
    public String errorHandler2(Exception exception) {
        return exception.getMessage();
    }
}

```

<br>
@ControllerAdvice 클래스에 등록된 메서드 같은 경우 전역으로 적용된다.<br>
그리고 @RestControllerAdvice 애너테이션은 doc 첫 문장이 이렇다.<br>
<br>

> A convenience annotation that is itself annotated with @ControllerAdvice and @ResponseBody.<br>
> 해석: @ControllerAdvice 와 @ResponseBody 를 같이 쓸 편리한 애너테이션이다.<br>

<br>

### @ExceptionHandler

문서: [@ExceptionHandler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html)<br>

<br>
이 애너테이션은 매개변수를 받을 수 있다.<br>
대표적으로는 선언된 메서드가 처리할 예외의 클래스를 정의할 수 있다.<br>
<br>
반환도 여러 타입으로 할 수 있다.<br>
ModelAndView, Model 부터 @ResponseBody 를 통해 원하는 컨텐츠를 반환할 수 있다.<br>
필자는 @RestControllerAdvice 와 @ExceptionHandler 애너테이션을 통해 ResponseEntity 를 반환할 것이다.<br>

---

### ● 2. 예외 결과를 반환할 DTO

대부분의 개발자들은 예외를 그대로 반환하지 않는다.<br>
그대로 throw 처리를 하게 두지 않는다.<br>
상황에 따른 메세지를 로깅, 전달, 저장 하여 예외를 확인할 수 있게 해둔다.<br>
나또한 이번에 그렇게 하려 했다.<br>
반환하고 싶은 상태는 딱 두개 였다.<br>

1. 상황에 맞는 예외 메세지<br>
2. 상황별 에러 코드<br>

그 중에서 나를 고민에 들게 한 것은 2번 이었다.<br>
<br>

```java

// 예외들
// 요청 시 발생한 예외의 도메인을 맡을 클래스
public class RequestException extends RuntimeException {
    
    public RequestException(String message) {
        super(MessageUtil.getMessage(message));
    }
    
}

// 잘못된 요청이 왔을 경우 반환할 클래스
public class BadRequestException extends RequestException {

    // 문제의 그 부분
    private int error;
    
    public BadRequestException(String message) {
        super(message);
    }
    
}

// 예외 상태를 반환할 DTO
@Getter
@Setter
@AllArgsConstructor
public class ExceptionResult {
    
    private final String message;
    
    private int error;
}

```

난 DTO 와 예외 클래스에 에러코드를 담고 싶었다.<br>
하지만 저렇게 에러코드를 따로 추가하는 순간 <b>논리적 동치성</b>이 깨진다.<br>
이팩티브 자바에서 설명하는 <b>Timestamp</b> 클래스의 오류를 그대로 가져온 것이다.<br>
그래서 에러코드를 어떻게 표현할까 고민을 하다가 또 다른 오류를 만들었다.<br>

```java 

// 요청 시 발생한 예외의 도메인을 맡을 클래스
public abstract class RequestException extends RuntimeException {
    
    public RequestException(String message) {
        super(MessageUtil.getMessage(message));
    }
    
    abstract public int getError();
}

// 잘못된 요청이 왔을 경우 반환할 클래스
public class BadRequestException extends RequestException {

    // 문제의 그 부분
    public BadRequestException(String message) {
        super(message);
    }
    
    @Override
    public int getError() {
        return HttpStatus.BAD_REQUEST.value();
    }
}

```

RuntimeException 을 상속하는 최상위 도메인 예외 클래스를 <b style="color: red;">추상</b>으로 만드는 것.<br>
그리고 에러 코드를 반환하는 메서드를 재정의할 수 밖에 없게 추상 메서드로 만들어버린 것이다.<br>
<br>
<b style="color: red;">'아, 상태값을 갖지 않으니, 이러면 Timestamp 의 오류는 벗어난거겠지'</b><br>
라고 생각한 내 착각이었다.<br>
하지만 이러한 생각도 결국 논리적 동치성을 파괴한 것은 마찬가지 였다.<br>
메서드를 재정의 함으로서 상태를 지녀 발생하는 has-a 관계는 피했지만 <b>대칭성</b> 은 파괴되었다.<br>
물론 완전히 같은 클래스를 구현하려는 것은 아니지만, 리스코프 치환 법칙은 지키고 싶었다.<br>
<br>
그리고 해결책은 생각보다 간단했다.<br>
<b>에러코드를 관리하지 않게할 것.</b><br>
에러코드는 메세지를 담은 클래스를 생성할 <b>팩터리 클래스를 호출하는 클래스에서 담당</b>하기로 했다.<br>
즉, 팩터리 클래스는 DI 된 매개변수로 반환될 클래스의 인스턴스만 획득하면 되고, 어드바이스에 그 문제를 위임하는 것이다.<br>

```java

@RestControllerAdvice
public class GeneralExceptionAdvice {

    @ExceptionHandler
    public ResponseEntity<ExceptionResult> errorHandler(BadRequestException exception) {

        // 반환하고자 하는 에러코드, 상태는 이곳에서 담당한다.
        return this.getError(exception.getMessage(), HttpStatus.BAD_REQUEST);
    }

    private ResponseEntity<ExceptionResult> getError(String message, HttpStatus status) {
        if (StringUtils.isNotBlank(message)) {
            return ExceptionService.response(message, status);
        }

        // 여기는 예외 블랙홀이 될수 있으니 추후 수정
        return null;
    }
}

// DI 받은 매개변수로 인스턴스를 획득하는 팩토리 클래스
public class ExceptionService {
    
    public static ResponseEntity<ExceptionResult> response(String message, HttpStatus status) {
        return new ResponseEntity<>(new ExceptionResult(message), status);
    }

}

@Getter
@AllArgsConstructor
public class ExceptionResult {
    
    private final String message;
    
}

```

<b>ResponseEntity 클래스는 Http 의 반환 상태를 관리할 수 있다.</b><br>
즉, 내가 억지로 규칙들을 어겨가며 예외코드를 관리하려 하지 않아도 되는 것이다.<br>
오히려 클라이언트에 전달할 메세지만 깔끔하게 전달할 수 있었다.<br>

<br>

---

<br>

### ● 3. 그러면 스프링이 제공하는 다른 예외들은?

<br>
예시를 들어, Spring 에는 Validation 을 구현할 수 있게 해주는 Spring validation 이 존재한다.<br>
그리고 @Min 등에 의해 우리는 받아온 값에 의한 예외처리를 해준다.<br>
그때 반환하는 예외에는 <b>ConstraintViolationException, MethodArgumentNotValidException 등이</b> 있다.<br>
<br>
하지만 이 예외들은 내가 피하고자 했던 상황을 이용한다.<br>
일반적인 Exception 이 관리하는 <b>message 말고도 본인이 가진 상태가 더 있다는 것이다.</b><br>
<br>

```java

// ConstraintViolationException
public class ConstraintViolationException extends ValidationException {

	private final Set<ConstraintViolation<?>> constraintViolations;

    // 이하 소스 생략

}

// MethodArgumentNotValidException
public class MethodArgumentNotValidException extends BindException {

	private final MethodParameter parameter;

    // 이하 소스 생략

}



```

<br>
소스를 봐도 그러하다.<br>
본인들이 상속하는 예외들은 거의 그런 형태를 띄지 않는다.<br>
하지만 우리가 접하는 예외들은 본인이 가진 상태가 더 있으며 정보를 더 제공한다.<br>
<br>
하지만 여기에는 짧게 생각하면 안 되는 <b>Trade-off</b> 가 존재했다.<br>
물론 본인들이 상속하는 예외의 끝에는 Exception 과  Throwable 이 존재한다.<br>
추가적인 상태를 관리하면 리스코프 치환법칙에 어긋날 수 있다는 이야기 이지만, 그들은 그렇게 했다.<br>
<br>
여기서 제일 중요한 포인트는, <b>"본인들의 상태를 직접 관리하지 않는다."</b> 에 있다.<br>
본인들은 DI 받은 매개변수만을 이용할 뿐, 본인의 상태를 직접 관리하지 않는다.<br>
기껏해야 Type 이 Set 인 final 로 관리되는 인스턴스 변수를 HashSet 으로 초기화해주는 데에 그친다.<br>
또한 상위 클래스에서 필요할 message 등도 본인이 관리하는 매개변수에 할당하기 위해 받은 같은 파라미터를 통해 할당한다.<br>
상태는 추가했지만 어느정도 관리는 되는 것이다.<br>
<br>
이와같은 <b>Trade-off</b> 를 통해 우리는 더 많은 정보를 얻을 수 있을 뿐 아니라 해당 예외의 인스턴스를 획득하는 로직에서도 다양한 반환 결과를 구현할 수 있게 되었다.<br>
물론 스프링이 제공하는 모든 예외가 그렇진 않을 것이다.<br>
Timestamp 처럼 어딘가 오류가 있는 것도 있을 것이다.<br>
그런것도 천천히 알아가자.<br>
<br>
아직 공부할 것이 많다.<br>



<br>
<br>