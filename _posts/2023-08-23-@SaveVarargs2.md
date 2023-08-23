---
title: 3장 외전, Set.of 메서드의 동작원리를 찾아서... 2편
author: park
date: 2023-08-23 18:36:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 3장 외전, Set.of 메서드의 동작원리를 찾아서... 1편

### 소제목 목록

1. 공식 문서 기반 GPT 의 답변<br>
2. 결국 정답은 디컴파일러<br>
3. 궁금증 1: String..., String[] 다른가?<br>
4. 궁금증 2: 내부동작도 다른가?<br>

<br>
어제 디컴파일러를 통해 메서드 Signature 가 varargs 매개변수로 선언된 메서드가 컴파일 시 어떻게 변하는지 확인했다.<br> 
이제 해당 메서드를 호출하는 과정에서 어떻게 여러 변수가 배열로 관리되는 형태가 될 수 있는지, 확인할 것이다.<br> 

---

## ● 공식 문서 기반 GPT 의 답변

우선 공식 doc 문서 기반의 대답을 요청한 GPT 의 결과부터 확인하자.<br>
질문은 <b>"What happens when you call your custom method, before that method real invoke?"</b> 였다.<br>
<br>

> 구현된 메서드는 다형성을 활용해서 런타임 중 어떤 것이 실행될지 선택된다.<br>
> 그 과정은 다음과 같다.<br>
><br>
> <b>Method Resolution</b><br>
> 객체의 메서드를 호출할 때, JVM은 어떤 특정 구현 메서드가 실행될지 알아내야 한다.<br>
> 이 과정을 Method Resolution 이라고 한다.<br>
> JVM은 객체 클래스의 메서드의 시그니처를 이용해 찾아낸다.<br>
><br>
> <b>Dynamic Method Lookup</b><br>
> Method Resolution 은 동적인데, 이는 참조에 정의된 타입 뿐 아니라 객체의 런타임에 고려한다는 것이다.<br>
> 이건 다형성이 동작하는 중요한 점이다.<br>
> 만약 메서드가 서브 클래스에 의해 오버라이딩 되었다면 JVM은 서브 클래스에 구현된 정확한 것을 찾아낸다.<br>
> <br>
> <b>Linking</b><br>
> 구현된 메서드가 확실하게 결정되면 JVM 은 Linking 을 진행한다.<br>
> 이 과정은 동의 되었는지 확인하는 것을 수반하며, 참조의 상징을 해결하며, 메서드 실행을 위한 준비를 한다.<br>
> JVM 은 클래스에 메서드가 컨테이닝 된 것을 확신하고, 확인하며 필요한 것을 초기화한다.<br>
> <br>
> <b>Method Invocation</b><br>
> 메서드가 풀이되고 링크되면 JVM 은 실재로 메서드의 바이트코드 구조를 실행한다.<br>
><br>
> 이런 모든 구조는 다형성을 구현하고 고등 차원 측면을 개발자가 몰라도 작업할 수 있게 해준다.<br>

<br>

[Method Resolution] (https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-5.html#jvms-5.4.3.3)

[Dynamic Method Lookup, Linking] (https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-2.html#jvms-2.6.3)

[Method Invocation] (https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-2.html)

<br>

> Method Invocation<br>
> 이 부분은 공식 홈페이지에서도 정말 많은 부분에서 다룬다.<br>
> 리턴, 동기화 등에 영향이 있으니 직접 확인해보는 것을 추천한다.<br>

<br>
내가 원하는 답은 듣지 못 했다.<br>
아무리 GPT를 괴롭혀도(?) 공식 문서 상에서도 <b>automatically managed</b> 라는 단어가 외워질 정도로..<br>
그렇다면 원하는 대답은 어떻게 얻을 수 있을까?<br>
<br>
이거 해결하고 난 뒷목을 잡았다.<br>
<br>

---

## ● 결국 정답은 디컴파일러

<br>
자, 그럼 우리가 알고 싶었던 점을 다시 확인하자.<br>
<br>
<b>
varargs 매개변수(이하 가변인자) 는 컴파일 시 배열 타입의 매개변수로 바뀐다고 하더라도,<br>
<span style="text-decoration: underline;">어떻게 여러 동일 타입의 매개변수를 배열화 하는가?</span><br>
</b>
<br>
자... 이틀 만에 찾은 방법..<br>
<b style="color: red;">디컴파일러</b>를 다시 켜자.<br>
가변인자를 받는 메서드를 호출하는 부분과 해당 메서드의 .class 파일을 디컴파일 해보자.<br>
<br>

### 컴파일 이전

```java

// 가변인자를 선언한 메서드를 호출하는 메서드
public long sumAndCompare() {
	MathClass mathClass = new MathClass();
	Integer a = 1;
	Integer b = 2;
	Integer c = 3;
	Integer d = 4;
	Integer e = 5;
	Integer f = 6;

	// 지역 변수로 선언, 초기화한 데이터를 매개변수로 이용
	long alphabetResult = mathClass.sum(a, b, c, d, e, f);
        
	// 지역변수를 직접 배열화 하여 매개변수로 이용
	int[] intArray = new int[] { a, b, c, d, e, f };
	long arrayResult = mathClass.sum(intArray);
        
	return Math.max(alphabetResult, arrayResult);
}

...

public class MathClass {
	public long sum(int... intArgs) {
		// 임의로 0으로 초기화
		BigDecimal result = new BigDecimal(0L);
		for (int arg : intArgs) {
			result = result.add(BigDecimal.valueOf(arg));
		}

        		return result.longValue();
    	}
}

```

---

<br>

### 컴파일 이후

```java

// 가변인자를 선언한 메서드를 호출하는 메서드
public long sumAndCompare() {
    	MathClass mathClass = new MathClass();
    	Integer a = Integer.valueOf(1);
    	Integer b = Integer.valueOf(2);
    	Integer c = Integer.valueOf(3);
    	Integer d = Integer.valueOf(4);
    	Integer e = Integer.valueOf(5);
    	Integer f = Integer.valueOf(6);

	// 네?
    	long alphabetResult = mathClass.sum(new int[] { a.intValue(), b.intValue(), c.intValue(), d.intValue(), e.intValue(), f.intValue() });

    	int[] intArray = { a.intValue(), b.intValue(), c.intValue(), d.intValue(), e.intValue(), f.intValue() };
    	long arrayResult = mathClass.sum(intArray);

    	return Math.max(alphabetResult, arrayResult);
}

...

public class MathClass {
	  public long sum(int[] intArgs) {
    		BigDecimal result = new BigDecimal(0L);
    		for (int arg : intArgs) {
      			result = result.add(BigDecimal.valueOf(arg));
    		}

    		return result.longValue();
  	}
}

```

<br>
<b style="color: red;">그렇다.</b><br>
Java 는 <b>컴파일 언어</b> 이자 인터프리터 언어의 특징을 가진 언어이다.<br>
<b>즉, 컴파일 하면서 다 처리한다.</b><br>
메서드 Signature 중 가변인자를 선언한 메서드는 자동으로 배열 매개변수로 바뀌고,<br>
해당 메서드를 호출하는 메서드는 매개변수로 전달하는 변수들을 new 연산자를 통해 배열화한다.<br>
<br>
이걸 위해서 정말 많이 돌아왔다.<br>
어제 디컴파일러 돌릴 때 .class 파일 하나만 더 확인할 걸.<br>
역시 사람은 꼼꼼해야 한다.<br>
<br>
<br>
ClassLoader 파보고 싶었는데, 이번 기회는 개념만 잡고 가고, 일단 @SaveVarargs 를 정리하자.<br>