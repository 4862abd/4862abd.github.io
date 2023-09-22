---
title: HandlerInterceptor, HandlerInterceptorAdapter
author: park
date: 2023-09-22 15:03:00 +0800
categories: [TIL, 2023-09]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## HandlerInterceptor, HandlerInterceptorAdapter 의 차이

<br>
● Filter, Interceptor, AOP<br>
● HandlerInterceptor, HandlerInterceptorAdapter 의 차이<br>
● HandlerInterceptor<br>
● 내 호기심<br>
<br>

---

<br>

## ● Filter, Interceptor, AOP

<br>
만약 <b>모든 동작에 먼저 혹은 후에 동작해야 하는 로직이 있다고 가정</b>해보자.<br>
대표적으로 로그인 상태 확인, 혹은 어떤 동작을 하는지 기록을 남기는 등의 상황이 있겠다.<br>
우리가 API 서비스 로직 상, 매번 그런 동작을 구현해야 한다고 하면 소스 코드의 양 뿐만 아니라, 상황별로 처리해야할 데이터가 달라질 수 있다.<br>
그래서 우리가 주로 이용하는 클래스 들이 있다.<br>
오늘은 그 중에서 <b>Interceptor</b> 에 대해 알아보자.<br>
<br>
<b>Interceptor</b> 에 대한 개념을 잡기 전에 애플리케이션에 API 요청이 도착했을 때, Spring 상에서 가지는 생명주기를 알 필요가 있다.<br>
Interceptor 는 언제 동작하고, 어떤 자원을 이용할 수 있을까?<br>
<br>
위 설명처럼 비슷한 역할을 하는 클래스가 여럿 있다.<br>
대표적으로 <b>Filter, Interceptor, AOP 등</b> 이 있는데 그들이 이용하는 메서드의 Life cycle 은 아래와 같다.<br>

![01](/assets/img/04.java/04.interceptor/API-requests-Life-cycle_with_watermark.png)

<i style="font-size: 5px;">직접 그림판으로 그리느라 힘들었다.</i><br>
<br>
이 중에서 우리가 알아볼 주제는 Interceptor 가 되겠다.<br>
현업에서 사용 중인데 제대로 알고 넘어가고 싶었다.<br>
<br>

---

<br>

## ● HandlerInterceptor, HandlerInterceptorAdapter 의 차이

<br>
우선 HandlerInterceptor 는 <b>인터페이스</b> 이고 HandlerInterceptor<b>Adapter 는 클래스</b>이다.<br>
여러 다른 포스팅을 보면 클래스인 HandlerInterceptorAdapter 를 상속 받아 구현 하는 것이 인터페이스인 HandlerInterceptor 를 구현할 때,<br>
모든 메서드를 오버라이딩 하지 않아도 되서 좋다고 한다.<br>
<br>
하지만 jdk 8 이후로 인터페이스의 정의된 메서드에 <b>default 와 static 예약어</b>를 이용할 수 있다.<br>
실제로 HandlerInterceptor 를 비교해 보면<br>
<br>
<b>Spring 프레임워크 4.0.9</b><br>

```java
public interface HandlerInterceptor {

    boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception;

    ...

}
```

<br>
<b>Spring 프레임워크 5.3.6</b><br>

```java
public interface HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

		return true;
	}

    ...

}
```

<br>
확연히 다르다는 것을 알수 있다.<br>
그로인해 구현하기 편해서 사용했던 HandlerInterceptor<b><span style="color: red;">Adapter</span> 는 최근 버전에선 deprecated 된 상태</b>이다.<br>
언제 사라져도 이상하지 않다는 말이다.<br>
<br>
즉, 우리가 일부러 다운 그레이드를 하지 않는 이상<br>
<b><span style="color: blue;">HandlerInterceptor</span> 를 이용하면 된다.</b><br>
<br>

---

<br>

## ● HandlerInterceptor

<br>
인터셉터를 이용하는 목적은 여러가지이다.<br>
처음 이야기 한 것처럼 비슷한 동작을 하는 클래스는 여럿 있지만,<br>
인터셉터는 그 중에서 <b>계산</b> 과 비슷한 역할을 한다.<br>
비슷한 역할을 필터가 진행하기도 하는데 필터는 스프링 프레임워크와 다른 자원이 관리하니 관심사가 조금 다르다고 할 수 있다.<br>
필터는 웹 컨테이너에 의해 관리되고, 인터셉터는 스프링 컨테이너에 의해 관리된다.<br>
또한 그림을 보면 확인할 수 있듯, 필터는 디스패쳐 서블렛에 도달하기 전과 후에 작업을 처리한다.<br>
<br>
API 호출에 사용자의 권한을 체크,<br>
프로그램 실행시간 계산<br>
<br>
등이 인터셉터의 역할이다.<br>
그러면 구현한 소스를 먼저 확인해보자.<br>
<br>

```java

public class LoginInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}

```

<br>
<b>preHandle</b><br>
<br>
preHandle 메서드 같은 경우는 우리가 호출한 API 가 컨트롤러에 도달하기 전에 실행된다.<br>
그리고 boolean 값을 반환하는데 false 를 반환할 경우 API 동작 전에 막힌다.<br>
javascript 의 이벤트 처리 중 return; 혹은 return false; 와 동일하다.<br>
이 기능을 활용해 권한 등을 관리하여 동작 여부를 설정해준다.<br>
<br>
<br>
<b>postHandle</b><br>
<br>
postHandle 메서드는 우리가 호출한 API 의 컨트롤러가 동작을 실행한 후에 실행된다.<br>
그리고 매개변수가 하나 추가된다.<br>
스프링의 MVC 패턴을 처음 배울 때 볼 수 있는 ModelAndView 가 그것이다.<br>
그리고 반환값은 void 처리하여 따로 없다.<br>
<br>
<br>
<b>afterCompletion</b><br>
afterCompletion 메서드는 뷰가 클라이언트에 응답을 전송한 뒤에 실행된다.<br>
예외가 발생한 경우에 해당 예외를 매개변수로 받아 처리할 수 있다.<br>
<br>
<br>
이렇게 인터셉터를 구현했으면 이제는 인터셉터를 등록해야한다.<br>

```java

@Configuration
public class WebConfiguration implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**");
    }
}

```

<br>
<b>WebMvcConfigurer 인터페이스를 구현</b>하고 <b>@Configuration 애너테이션을 등록</b>한 Confiruation 클래스이다.<br>
addInterceptors 메서드를 재정의하여 원하는 인터셉터의 인스턴스를 매개변수로 등록한다.<br>
그리고 addPathPatterns 메서드를 통해 어떤 API 가 호출됐을 때 인터셉터가 동작할 것인지 조건을 걸 수 있다.<br>
addPathPatterns 메서드에 등록하는 패턴은 <b>Ant Pattern</b> 을 통해 관리된다.<br>
현재는 모든 호출에 동작하도록 설정했다.<br>
그 후로 내가 정의한 인터셉터는 스프링 컨테이너에서 관리되게 된다.<br>
<br>
이제 포스트맨을 통해 API 를 호출하면,<br>
<br>

> 출력<br>
> <br>
> preHandle<br>
> postHandle<br>
> afterCompletion<br>

<br>
이렇게 순서대로 출력되는 것을 확인할 수 있다.<br>
<br>

---

<br>

## ● 내 호기심

<br>
그렇다면 HandlerInterceptor 와 HandlerInterceptor 를 구현한 HandlerInterceptor<b>Adapter</b> 를<br>
<b>모두 구현해서 인터셉터로 등록하면 어떻게 될까?</b><br>
<br>

```java

// LoginInterceptor
public class LoginInterceptor implements HandlerInterceptor {


    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle by Interceptor");
        return true;
    }

}

...

// LoginInterceptorAdapter
public class LoginInterceptorAdapter extends HandlerInterceptorAdapter {
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle by Interceptor adapter");
        return true;
    }
}

...

// WebConfiguration
@Configuration
public class WebConfiguration implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**");
        
        registry.addInterceptor(new LoginInterceptorAdapter())
                .addPathPatterns("/**");
    }
}

```

<br>
우선 애플리케이션 구동은 된다.<br>
그리고 API 를 호출하면<br>
<br>

> 출력<br>
> <br>
> ☆ 홍박사님을 아세요?<br>
> ☆ 뭐가 먼저 돌아?<br>

<br>
<b>나중에 인터셉터로 등록된 인스턴스의 메서드가 더 늦게 돈다.</b><br>
재밌다.<br>