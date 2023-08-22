---
title: 3장 외전, Set.of 메서드의 동작원리를 찾아서... 1편
author: park
date: 2023-08-22 18:36:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 3장 외전, Set.of 메서드의 동작원리를 찾아서... 1편

### 소제목 목록

1. @SafeVarargs 애너테이션을 찾아서<br>
2. varargs 매개변수<br>
3. 궁금증 1: String..., String[] 다른가?<br>
4. 궁금증 2: 내부동작도 다른가?<br>

<br>
오늘은 어제 Set 타입의 of 메서드를 통해 Set 을 초기화하는 과정에서 발견된 것들은 공부하려 했다.<br> 

---

## ● @SafeVarargs 애너테이션을 찾아서

우선 가장 먼저 등장한 @SafeVarargs<br>
어떤 역할인지 한 번 뜯어보니, <br>

출처: [doc] (https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html)

> 원문<br>
> A programmer assertion that the body of the annotated method or constructor does not perform potentially unsafe operations on its varargs parameter.<br>
> <br>
> 번역<br>
> 애너테이션이 달린 메서드나 생성자 메서드가 varargs 매개변수를 사용할 때 안전하지 않다는 개발자의 주장.<br>

<br>

---

<br>
자, 먼저 varargs 매개변수가 뭘까?<br>

<br>

## ● varargs 매개변수

말이 영어로 되어 있어서 처음에 뭔가 했다.<br>
하지만 정말 많이 봤다.<br>

```java

public static void main(String... args) {

    ...
}

```

<br>
위의 main 메서드에서 args 매개변수의 타입으로 지정된 것이 그것이다.<br>
<b>String...</b><br>
이처럼 우리는 매개변수로 지정할 타입의 뒤에 <b>...</b> 를 통해 같은 타입의 매개변수라면 최대 int의 max 값(Integer.MAX_VALUE) 으로 받을 수 있게 해준다.<br>
<br>
<b>응?</b> 다른 포스터에서는 무한으로 가능하다고 하던데요?<br>
아니다.<br>
우선 java는 배열을 관리할 때 index 를 통해 관리하는데 index의 값을 이루는 타입이 <b>int</b> 이다.<br>
배열을 편하게 관리하기 위해 주로 사용되는 Arrays 도 index 의 관리는 int로 한다.<br>
매개변수의 개수를 int로 관리하는데 무한으로 매개변수를 받을 수 있을리가 없다.<br>
<br>
<b>그러면 </b> varargs 매개변수와 같은 타입으로 이루어진 배열 매개변수는 같은건가요?<br>
<b>음, 메서드 오버로딩 할 때 확인하면 모든 indentity 가 같은 상황에서 매개변수를 String..., String[] 이렇게 다르게 해도 허용하지 않는 걸 확인할 수 있다.</b>(궁금증 1)<br>
또한 매개변수가 varargs 로 선언된 메서드에 배열을 전달해도 아무 문제 없는 것을 알 수 있다.<br>
<b>그렇다면 둘은 선언 차이만 다를 뿐, 완전히 같은 동작을 구현할까?</b>(궁금증 2)<br>
우선 참조를 보내는 점은 같다.<br>
배열이나 객체 내에 초기화된 값은 호출된 메서드 내에서 참조된 값이 수정되면 메서드가 끝난 후에도 값이 바뀌어 있는 것은 동일하다. -> 테스트 진행 완료<br>
<br>
보통 varargs 매개변수를 사용하는 경우는 두 가지다.<br>
<br>

1. 유연성을 위해<br>
    ㄴ 이펙티브 자바 - 아이템 2에서 나왔던 <b>점층적 생성자 패턴</b> 이 안티패턴인 이유도 비슷한 경우다.<br>
    ㄴ 엄청난 매개변수의 양을 담을 메서드를 만들 필요가 없게된다.<br>
    ㄴ 단순히 같은 타입의 여러 매개변수를 보내고 호출된 메서드는 varargs 매개변수로 선언하면 끝.<br>
2. 매개변수의 양이 너무 많아서 매개변수로 쓰일 배열에 값을 초기화 하는 과정이 더 비용이 큰 경우<br>
    ㄴ varargs 매개변수는 기본적으로 매개변수를 변수로 받는다.<br>
    ㄴ 그 과정에서 배열이 아닌 매개변수를 배열화 하는 과정을 거치는데, 이 과정에서 메모리에 오버헤드가 날 경우가 있다.<br>
    ㄴ 반대로 배열로 된 변수를 매개변수로 보내는 방법이 있다.<br>

<br>

---

## ● 궁금증 1: String..., String[] 다른가..?

<br>
우선 <b>궁금증 1</b> 부터 해결하자.<br>
이건 디컴파일러로 .class 파일 디컴파일 시 바로 알 수 있다.<br>
varargs 매개변수로 선언된 메서드의 매개변수 타입의 경우 같은 타입의 배열을 받는 메서드로 컴파일된다.<br>
그래서 만약<br>

```java
public calss mathTestClass {
    public void intMethod(int... ingArgs) {

        ...
    }

    public void intMethod(int arg, int... intArgs) {

        ...
    }
}
```

<b>이런 형식으로 메서드를 오버로딩하여 정의하면 어떨까?</b><br>
<br>
이건 <b style="color: blue;">허용</b>된다.<br>
다만 첫 번째 메서드의 호출을 위해<br>
<br>

```java

public static void main(String... args) {
    MathTestClass mathTestClass = new MathTestClass();

    // 1번
    mathTestClass.sum(1, 2, 3, 4);                  // 이건 이제 불가능하다. -> 에러

    // 2번
    mathTestClass.sum(1, new int[] { 2, 3, 4 });    // 이건 된다.

    // 3번
    mathTestClass.sum(new int[] { 1, 2, 3, 4 });    // 이것도 된다.
}

```

<br>
1번 처럼 호출을 하면 Syntax 에러가 발생한다.<br>
이런 메서드는 정의되지 않았다, 는 에러이다.<br>
<br>
메서드는 추가로 선언, 정의 했는데 허용, 가동 범위는 줄었다.<br>
<br>

> 참고로 varargs 매개변수는 다른 매개변수들과 같이 선언 시, 가장 마지막에 와야한다.<br>
> 아니면 Syntax 에러가 발생한다.<br>

<br>

---

## ● 궁금증 2: 내부동작도 다른가?

이제 <b>궁금증 2</b> 를 파악해보자.<br>
varargs 매개변수를 필요로 하는 메서드를 바로 위 코드에서 보여준 1번 방식처럼 호출을 하게되는 경우,<br>
배열이 아닌 매개변수들을 어떻게 배열화 하여 이용하는가?<br>

이거 <b>ClassLoader</b> 와 깊게 관련있다.<br>
이 부분은 다음 시간에 알아보자.<br>
2 편에 정리하든 ClassLoader 포스팅을 따로 하든 먼저 짚고 넘어가야겠다.<br>
<br>

> @SafeVarargs 애너테이션을 이해하기 위해 -> varargs 매개변수 -> 매개변수 처리방식 -> ClassLoader<br>
> 까지 왔다는 점..<br>
> 웃긴건 저 @SafeVarargs 애너테이션 마저 이펙티브 자바 - 아이템 10 의 구문 중 Set 을 초기화 하는 Set.of 메서드부터 시작했다는 점.<br>
> 키워드 하나에 포스터 몇 개 분량이 나온다.<br>