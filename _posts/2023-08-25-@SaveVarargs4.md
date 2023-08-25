---
title: 3장 외전, Set.of 메서드의 동작원리를 찾아서... 4편, @SaveVarargs
author: park
date: 2023-08-25 15:15:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 3장 외전, Set.of 메서드의 동작원리를 찾아서... 4편, @SaveVarargs

### 소제목 목록

1. 문서 독해(?)<br>
2. @SafeVarargs 의 역할<br>
3. 예시<br>
4. @SaveVarargs<br>

---

<br>
이번 포스팅에 @SaveVarargs 마친다.<br>
긴 여정이었다.<br>

---

## ● 문서 독해(?)

<br>

출처: [@SafeVarargs] (https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html)

<br>
직전에 읽다가 Non-reifiable 타입으로 빠졌으니 그 이후의 문서를 확인해보자.<br>
<br>

> In addition to the usage restrictions imposed by its @Target meta-annotation,<br>
> @Target 메타 주석에 의해 주어진(겹쳐진) 사용 제한 외에도,<br>
> <br>
> compilers are required to implement additional usage restrictions on this annotation type;<br>
> 컴파일러는 이 애너테이션을 위해 추가의 사용 제한을 필요로 한다.<br>
> <br>
> it is a compile-time error if a method or constructor declaration is annotated with a @SafeVarargs annotation, and either:...<br>
> 만약 메서드나 생성자 메서드 정의에 @SafeVarargs 애너테이션이 부여되면 컴파일 시간 에러이다, 그리고...<br>

<br>

자, 이해가 되는가?<br>
저걸 읽으면서 황파고의 번역과 내 번역과 구글의 번역과<br>
비교하면 서로 달랐던 것을 보고<br>
<i>아 이거 컴퓨터도 해석 못하네 ㅋㅋㅋ</i><br>
싶었다.<br>
물론 이해가 안 된다는 것은 아니다.<br>
하지만 적어도 이해하기 난해한 것은 확실하다.<br>
<br>
<b>그렇다면 결국 @SafeVarargs 애너테이션의 역할이 무엇인가?</b><br>

---

## ● @SafeVarargs 의 역할

<br>
문서에 이런 말이 있다.<br>
<br>

> Compilers are encouraged to issue warnings when this annotation type is applied to a method or constructor declaration where...<br>
> 이 주석 유형이 메소드 또는 생성자 선언에 적용될 때 컴파일러는 경고를 발행하도록 권장한다.<br>

<br>
그렇다.<br>
개발 혹은 성능적인 역할이 아니다.<br>
<b>비 구체화 타입의 가변인자를 사용하는 메서드의 안전을 보장하는 애너테이션이다.</b><br>
<br>
즉, 우리가 임의로 지정하고 함부로 오버라이딩 하는 메서드 등 변수가 생길 메서드에는 사용할 수가 없다.<br>
실제로 인텔리 J 를 통해 코딩을 진행할 때,<br>
메서드에 final 예약어를 쓰지 않거나, 구체화 타입의 가변인자 타입은 경고하듯 색상이 변한다.<br>

## ● 예시

### final 예약어를 사용하지 않은 경우

![01](/assets/img/04.Java/00.etc/01.png)

아예 에러를 날린다.<br>

---

### 구체화 타입을 가변인자로 받은 경우

![02](/assets/img/04.Java/00.etc/02.png)

---

## ● @SaveVarargs

<br>
재밌었다.<br>
꽤 알찬 것 같다.<br>
아직 공부가 부족해서 가변인자를 많이 사용할지 예상은 되질 않지만,<br>
중간중간 나온 개념이 꽤 재밌었다.<br>
<br>
누군가 이걸 파려고 한다면, 나처럼 'Set.of 를 뜯을거야!', 'List.of 를 뜯을거야!' 하는 목표가 없고 제네릭에 대한 개념이 없다면 쉽지 않을 것 같다.<br>
<br>
다음 시간에는 해당 메서드를 사용하는 <b>Set.of 에서 직접 호출하는 <span style="color: red;">ImmutableCollections</span></b> 를 살펴보겠다.<br>