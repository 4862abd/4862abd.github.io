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
우선 @ControllerAdvice 와 @RestControllerAdvice 는 역할이 비슷하다.<br>
가장 큰 차이점은 @Controller 와 @RestController 의 차이와 비슷하다.<br>
<br>
...<br>




<br>
<br>