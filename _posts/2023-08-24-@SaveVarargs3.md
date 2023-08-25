---
title: 3장 외전, Set.of 메서드의 동작원리를 찾아서... 3편, @SaveVarargs
author: park
date: 2023-08-24 18:15:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 3장 외전, Set.of 메서드의 동작원리를 찾아서... 3편, @SaveVarargs

### 소제목 목록

1. @SafeVarargs 문서<br>
2. 비 구체화 타입<br>
3. Heap Pollution<br>
4. 드디어..<br>

<br>
공식 문서를 읽고 나서..<br>
이틀만에 조금 감격했다..<br>

---

## @SafeVarargs 문서

이제 @SafeVarargs 애너테이션의 정보가 담긴 공식 홈페이지를 확인하자.<br>
<br>

출처: [oracle] (https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html)

<br>

> A programmer assertion that the body of the annotated method or constructor does not perform potentially unsafe operations on its varargs parameter.<br>
> 애너테이션이 달린 메서드나 생성자 메서드가 varargs 매개변수를 사용할 때 안전하지 않다는 개발자의 주장<br>
> <br>
> Applying this annotation to a method or constructor suppresses unchecked warnings about a non-reifiable variable arity (vararg) type and suppresses unchecked warnings about parameterized array creation at call sites.<br>
> 이 주석을 메서드나 생성자 메서드에 적용하면 매개변수화된 배열 타입의 <b>비 구체화 변수</b>를 호출 단에서 경고를 억제할 수 있다.<br>

<br>
자, 이게 무슨 말이냐.<br>
우선 <b>비 구체화 타입</b> 을 먼저 알고 넘어갈 필요가 있다.<br>
<br>

---

<br>

## ● 비 구체화 타입

<br>
이것도 공식 문서를 먼저 보고 가자.<br>

> A reifiable type is a type whose type information is fully available at runtime.<br>
> 구체화 타입은 런타임 중 타입 정보를 완전히 사용 가능한 타입을 의미한다.<br>
> This includes primitives, non-generic types, raw types, and invocations of unbound wildcards.<br>
> 여기에는 초기의, 제네릭이 아닌, raw 타입(제네릭 타입을 받는 타입에 타입 매개 변수를 사용하지 않은 것, ex. List list = ...), 그리고 결합되지 않은 와일드카드가 포함된다.<br>
> <br>
> Non-reifiable types are types where information has been removed at compile-time by type erasure — invocations of generic types that are not defined as unbounded wildcards<br>
> 비 구체화 타입은 컴파일 시에 type erasure 에 의해 타입의 정보가 지워지는 유형이고, 결합되지 않은 와일드카드 처럼 정의되지 않은 실시를 의미한다.<br>
> A non-reifiable type does not have all of its information available at runtime.<br>
> 비 구체화 타입은 런타임 시에 완전하게 사용 가능한 정보를 가지지 않는다.<br>

<br>

출처: [oracle] (https://docs.oracle.com/javase/tutorial/java/generics/nonReifiableVarargsType.html)

<br>
이게 뭔 소리야?<br>
키워드를 어마어마 하게 던진다.<br>
단 3 줄 만에.<br>
<br>
우선 <b>구체화 타입</b> 먼저 보자.<br>
런타임 중 타입 정보를 완전히 사용 가능하다.<br>
그리고 예시로 나온 것을 보면 런타임 중에도 타입을 확실히 알 수 있는 것을 의미하고 있다.<br>
<br>
<b>비 구체화 타입</b>은 반대이다.<br>
컴파일 시에 타입의 정보가 지워진다.<br>
그리고 런타임에도 사용할 타입의 정보를 완전히 가지지 않는다.<br>
슬슬 제네릭 공부할 때 나왔던 개념들인 것이 얼핏 생각난다.<br>
마치 제네릭 타입을 선언하면 컴파일 후 Object 타입으로 바뀌는 것처럼.<br>
<br>
그러면 문서를 더 읽어보자.<br>
<br>

---

<br>

## ● Heap Pollution

<br>

> <b>Heap Pollution</b><br>
> <i>Heap 오염...?</i><br>
> <br>
> Heap pollution occurs when a variable of a parameterized type refers to an object that is not of that parameterized type.<br>
> 매개변수화 된 타입이 매개변수화 되지 않은 객체를 참조할 때 Heap 오염이 발생한다.<br>
><br>
> This situation occurs if the program performed some operation that gives rise to an unchecked warning at compile-time.<br>
> 이런 상황은 프로그램이 컴파일 시, 확인되지 않은 경고를 발생시키는 작업을 수행한 경우 발생합니다.<br>
> <br>
> An unchecked warning is generated if, either at compile-time (within the limits of the compile-time type checking rules) or at runtime, the correctness of an operation involving a parameterized type (for example, a cast or method call) cannot be verified.<br>
> 컴파일 시간(컴파일 시간 타입 검사 규칙의 제한 내) 또는 런타임에 매개 변수화된 유형(예: 캐스트 또는 메서드 호출)과 관련된 작업의 정확성을 확인할 수 없는 경우 확인되지 않은 경고가 생성됩니다.<br>
><br>
> For example, heap pollution occurs when mixing raw types and parameterized types, or when performing unchecked casts.<br>
> 예를 들어, raw 타입과 매개변수화된 타입이 섞일 때, 혹은 확인되지 않은 주문을 할때 Heap 오염은 발생한다.<br>

<br>
위의 내용을 바탕으로 재해석한 코드는 이러하다.(공식 문서)<br>
<br>

```java
public class MainClass {
    public static void main(String... args) {
        VarargsTest test = new VarargsTest();
        test.heapPollutionMethod();
    }
}


...

public class VarargsTest {

    @SafeVarargs                                        // 안전하지 않아도 Possible heap pollution from parameterized varargs type 을 무시
    static void heapPollutionMethod(List<String>... strList) {
        Object[] arr = strList;
        List<Integer> intList = Arrays.asList(10);
        arr[0] = intList;                               // Heap Pollution
        String s = strList[0].get(0);
    }

}
```

<br>
또 다른 예시를 확인해보자.<br>
<br>

```java
public void heapPollutionMethod() {
    Collection dogList = new ArrayList();                   // Dog class 만 처리하고 싶어서 변수명을 dogList로 지었다.

    dogList.add(new Dog());
    dogList.add(new Cat());                                 // 전혀 다른 class 이지만 동작한다.(Heap Pollution)

    for (int i = 0; i < dogList.size(); i++) {
        Dog dog = (Dog) dogList.get(i);                     // 런타임 시, ClassCastException 이 발생한다.
    }
}
```

<br>
이렇게 raw 타입과 매개변수화된 타입이 섞이는 경우, 혼동을 야기할 수 있다.<br>
위 예시는 공식 문서와 여러 포스팅을 비교하여 조합한 코드이다.<br>
<br>
그리고 Non-Reilfiable Types 관련 공식 홈페이지의 마지막 단원을 보면,<br>
<br>

---

<br>

## ● 드디어..

<br>

> Prevent Warnings from Varargs Methods with Non-Reifiable Formal Parameters<br>
> 비 구체화된 매개변수를 사용하여 varargs 메서드의 경고 방지<br>

<br>
라는 단원이 있다.<br>
그리고 바로 하단에 등장하는 <br>
<br>
<b>@SafeVarargs</b><br>
<br>

> If you declare a varargs method that has parameters of a parameterized type, <br>
> and you ensure that the body of the method does not throw a ClassCastException or <br>
> other similar exception due to improper handling of the varargs formal parameter, <br>
> you can prevent the warning that the compiler generates for these kinds of varargs methods by adding<br>
> the following annotation to static and non-constructor method declarations:<br>
><br>
> 만약 네가 매개변수화 할 가변인자 메서드를 정의하고 메서드의 바디에서 가변인자 타입의 매개변수로 인해서 부당한 조작이 이뤄질 때,<br>
> ClassCastException 이나 비슷한 예외를 던지지 않을 것을 확신하면<br>
> 당신은 컴파일러가 동작할 때 가변인자 매서드에 이 애너테이션을 추가함으로 써 경고를 막을 수 있다.<br>

<br>
<b>이야.</b><br>
드디어 원하는 키워드가 등장했다!!<br>
이제 남은건 예시와 완벽을 위한 다듬기이다..!<br>