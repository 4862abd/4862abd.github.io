---
title: HandlerInterceptor, HandlerInterceptorAdaptor - 작성중
author: park
date: 2023-09-21 18:03:00 +0800
categories: [TIL, 2023-09]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## HandlerInterceptor, HandlerInterceptorAdaptor 의 차이

<br>
● Filter, Interceptor, AOP<br>
● HandlerInterceptor, HandlerInterceptorAdaptor 의 차이<br>
● HandlerInterceptor<br>
<br>

---

<br>

## ● Filter, Interceptor, AOP

<br>
만약 <b>모든 동작에 먼저 혹은 후에 동작해야 하는 로직이 있다고 가정</b>해보자.
대표적으로 로그인 상태 확인, 혹은 어떤 동작을 하는지 기록을 남기는 등의 상황이 있겠다.
우리가 API 서비스 로직 상, 매번 그런 동작을 구현해야 한다고 하면 소스 코드의 양 뿐만 아니라, 상황별로 처리해야할 데이터가 달라질 수 있다.
그래서 우리가 주로 이용하는 클래스 들이 있다.
오늘은 그 중에서 <b>Interceptor</b> 에 대해 알아보자.

<b>Interceptor</b> 에 대한 개념을 잡기 전에 애플리케이션에 API 요청이 도착했을 때, Spring 상에서 가지는 생명주기를 알 필요가 있다.
Interceptor 는 언제 동작하고, 어떤 자원을 이용할 수 있을까?

위 설명처럼 비슷한 역할을 하는 클래스가 여럿 있다.
대표적으로 <b>Filter, Interceptor, AOP 등</b> 이 있는데 그들이 이용하는 메서드의 Life cycle 은 아래와 같다.

![01](/assets/img/04.java/04.interceptor/API-requests-Life-cycle_with_watermark.png)

<i style="font-size: 5px;">직접 그림판으로 그리느라 힘들었다.</i>

이 중에서 우리가 알아볼 주제는 Interceptor 가 되겠다.
현업에서 사용 중인데 제대로 알고 넘어가고 싶었다.

---


추가 자료

HandlerInterceptor 와 HandlerInterceptorAdaptor 차이
https://3rdlab.github.io/2019-09-29/HandlerInterceptor

&#43; jdk 8 버전 이후 인터페이스 메서드 default 예약어

---

Interceptor 간단 설명
https://velog.io/@gillog/Spring-InterceptorHandlerInterceptor-HandlerInterceptorAdapter

---

각 클래스 별, 구현 이유
https://velog.io/@miot2j/Spring-Filter-Interceptor-AOP-%EC%B0%A8%EC%9D%B4-%EB%B0%8F-AOP%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-Logging%EC%9D%84-%EA%B5%AC%ED%98%84%ED%95%9C-%EC%9D%B4%EC%9C%A0
https://jake-seo-dev.tistory.com/83

---

갓대희
https://goddaehee.tistory.com/154

---

AOP(추후)
https://engkimbs.tistory.com/746