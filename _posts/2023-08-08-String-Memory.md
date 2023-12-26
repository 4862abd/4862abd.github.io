---
title: String 이 메모리에 상주할 때
author: park
date: 2023-08-08 13:05:00 +0800
categories: [Java, 메모리]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

## String 이 메모리에 상주할 때

### 소제목 목록

● String 초기화하기<br/>
● 추가 팁<br/>

<br/>

---

## ● String 초기화하기

String 을 초기화하는 여러 방법이 있다．<br>
<br>

1. 리터럴로 초기화하기<br>
2. 인스턴스로 초기화하기<br>

<br>
특별하다고 하는 String 클래스가 어떻게 메모리에 상주하게 되는지,<br>
위 두 가지 방법을 코드와 함께 비교해보자.<br> 
<br>

```java
public class StringMemory {
	public static void main(String... args) {
		String literalAAA = "aaa";
		String instanceAAA = new String("aaa");

		System.out.println(literalAAA == instanceAAA);
		System.out.println(literalAAA.equals(instanceAAA));
	}
}
```

<br>

> 출력<br>
> <br>
> false<br>
> true<br>

<br>
변수 명대로 리터럴로 초기화 한 변수 literalAAA 와 new 연산자를 통해 인스턴스로 초기화한 instanceAAA 가 있다.<br>
동등성을 비교하는 equals 메소드에서는 true를 반환한다.<br>
당연하다.<br>
같은 문자열을 비교하기에 동등성은 true 가 확실하다.<br>
하지만 동일성에서는 다르다.<br>
Java는 new 연산자를 이용할 때 새로운 인스턴스를 획득한다.<br>
<br>

> 인스턴스란?<br>
> <br>
> 구현체를 의미하며 초기화를 통해 새로 생성된 객체를 의미한다.<br>
> 모든 객체를 인스턴스라고 하진 않고 Heap 영역에 생성되는 객체를 인스턴스라고 표현하며 Stack 영역에 저장되는<br>
> 기본 자료형, 호출된 메서드 등은 인스턴스라고 하지 않는다.<br>
> <br>
> 인스턴스는 이펙티브 자바 서적의 첫 키워드 이기도 하다.<br>
{: .prompt-tip }

<br>
그렇다면 아무리 특별한 클래스라고 표현하는 String 이더라도 new 를 쓰면 다른 객체를 반환하는가?<br>
<br>
그렇다.<br>
그러면 String 을 생성하는 몇 가지 방법을 더 확인하자.<br>
<br>

```java
public class PlayGround {

	public void stringMemory() {
		String aaa1 = new String("aaa");
        		String aaa2 = new String("aaa");
        		String aaa3 = "aaa";
        		String aaa4 = "aaa";
        		StringBuilder stringBuilder1 = new StringBuilder();
        		stringBuilder1.append("aaa");
        		StringBuilder stringBuilder2 = new StringBuilder("aaa");
        		
        		String aaa5 = stringBuilder1.toString();
        		String aaa6 = stringBuilder2.toString();

		his.print(aaa1, aaa2, aaa3, aaa4, aaa5, aaa6);
	}

	public void print(String... param) {
        		for (int i = 0; i < param.length; i++) {
            			String str = param[i];
			System.out.println("☆ aaa" + (i + 1) + ": " + System.identityHashCode(str));
		}
	}
}

```

> 출력<br>
> <br>
> ☆ aaa1: 1784662007<br>
> ☆ aaa2: 234698513<br>
> ☆ aaa3: 1121172875<br>
> ☆ aaa4: 1121172875<br>
> ☆ aaa5: 649734728<br>
> ☆ aaa6: 1595953398<br>

<br>
자, 어떤 값이 같아 보이는가?<br>
aaa3과 aaa4의 값이 같다.<br>
같은 참조 주소값을 가리킨다고 볼 수 있다.<br>
<br>
이와 같은 출력값이 발생하는 이유는 String 은 <b>특별한 경우</b>이기 때문이다.<br>
<br>
String 을 new 연산자 없이 리터럴로 선언할 경우 내부적으로 String의 intern() 메서드가 호출된다.<br>
<br>

> intern() 메소드란?<br>
> <br>
> java.lang.String 클래스의 intern 메서드 문서 주석을 확인하자.<br>
> 문서 주석 첫 줄에 이런 문구가 있다.<br>
> <br>
> Returns a canonical representation for the string object.<br>
> 번역: 문자열 개체에 대한 표준 표현을 반환한다.<br>
> <br>
> 문자열 풀(string constant pool)은 클래스에 의해 private 하게 유지된다.<br>
> 그리고 intern 메서드가 동작하게 될 경우 새로 선언하는 String 의 this 를 이용해 풀에 같은 문자열이 있는지 확인한다.<br>
> 같은 문자열이 있는지 확인할 때에는 equals 메서드를 활용하며 같은 문자열이 있을 경우 반환한다.<br>
> 그렇게 반환된 문자열의 경우 동일성이 확보된다.<br>
><br>
> 즉, 리터럴로 초기화된 문자열의 경우 intern 메서드에 의해 동일한 풀에서 unique 한 값을 보장 받는다는 것이다.<br>
{: .prompt-tip }

<br>
위에 설명한 intern 메서드에 의해 리터럴로 초기화된 문자열의 경우 우선적으로 문자열 풀에 동등한 값을 가지는 문자열을 찾는다.<br>
그리고 찾았다면 해당 문자열이 가리키는 주소값을 반환한다.<br>
만약 없다면 문자열 풀에 값을 넣고 새로운 주소값를 반환한다.<br>
<br>

```java
String aaa1 = "aaa";
String aaa2 = "aaa";

System.out.println(aaa1 == aaa2);
```

<br>

> 출력<br>
><br>
> true<br>

<br>
위처럼 같은 값의 문자열을 리터럴로 초기화하면 두 객체는 같은 주소값을 가지며 같은 값을 가리킨다고 할수 있다.<br>

---

## ● 추가 팁

<br>

1. jdk 버전에 따른 메모리 영역 변경<br>
Java 6 -> 7 에서 변화점 중 하나는 기존 문자열 풀(string constant pool) 의 메모리상 위치가 Perm 에서 Heap 으로 변경되었다는 것이다.<br>
왜 이러한 변경이 됐을까?<br>
<br>
우선, Perm 영역은 고정된 사이즈라고 한다.<br>
많은 양의 메모리가 누적될 경우 OOM (Out Of Memory) 문제를 야기할 수 있는 것이다.<br>
그렇다면 Heap 영역으로 변경하여 얻는 이점이 무엇일까?<br>
<br>
<b>문자열 풀이 GC의 대상으로 잡힐 수 있다는 점이다.</b><br>
<br>
참조: [참조](https://medium.com/@joongwon/string-%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0-57af94cbb6bc)
<br>

2. 문자열 풀(string constant pool) 의 크기<br>
문자열 풀도 메모리로써 우리가 임의로 크기를 할당할 수 있다.<br>
그리고 메모리 크기를 할당 할 때 소수로 할당하는 것이 효율에 좋다고 하는데 이는 Hash 와 관련이 있다고 한다.<br>
<br>

> Hash? Hash Table? Hashing?<br>
><br>
> <b>Hash</b> 란 Hash 함수를 통해 변환한 값을 index로 삼아 key와 value를 저장하는 자료 구조를 말한다.<br>
> 그렇다면 <b>Hash 함수</b>란 무엇일까?<br>
> 입력받은 데이터를 일정한 길이의 값으로 매핑하는 함수를 의미한다.<br>
> 또한 Hash 함수를 통해 얻어진 일정한 길이의 값을 key와 value의 형태로 저장해 두는데, 그 저장소를 <b>Hash Table</b> 이라고 한다.<br>
> 그렇게 매핑하는 과정을 <b>Hashing</b> 이라고 한다.<br>
> 그래서 Hashing 이라고 하면 변환하여 저장하는 과정과 변환된 값을 기준으로 그 값이 가리키는 주소의 value 를 가져오는 것, 두 가지 모두 Hashing 이라고 한다.<br>
> Hash 자료구조는 key 값을 통해 value를 조회하는 구조로써 평균 조회속도가 O(1) 에 수렴한다.<br>
><br>
> 추후 Hash 는 다시 다룰 것이다.<br>
{: .prompt-tip }

<br>
참조: [참조](https://bloodstrawberry.tistory.com/37)

<br>
위 블로그에서 Hash 메모리의 크기를 직접 할당해가며 성능을 비교한 부분이 있는데,<br>
소수 vs 소수가 아닌 수 의 성능 비교 시에 400,000 을 제외하고 소수인 경우에 더 빠르다고 한다.<br>