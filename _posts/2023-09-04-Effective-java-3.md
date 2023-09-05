---
title: 3장, 모든 객체의 공통 메서드 - 작성중
author: park
date: 2023-09-04 18:20:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 3장, 모든 객체의 공통 메서드

### 아이템 목록

아이템 10. equals 는 일반 규약을 지켜 재정의하라<br>
아이템 11. equals 를 재정의하려거든 hashCode 도 재정의하라<br>
아이템 12. toString 을 항상 재정의하라<br>
아이템 13. clone 재정의는 주의해서 진행하라<br>
아이템 14. Comparable 을 구현할지 고려하라<br>

---

## ● 아이템 10: equals 는 일반 규약을 지켜 재정의하라

우선 equals 를 재정의하지 않는 상황을 나열한다.<br>

1. 각 인스턴스가 본질적으로 고유하다.<br>
    ㄴ ex. Thread<br>
2. 인스턴스의 '논리적 동치성' 을 검사할 일이 없다.<br>
    ㄴ ex. java.util.regex.Pattern<br>
3. 상위 클래스에서 재정의한 equals 가 하위 클래스에도 딱 들어맞는다.<br>
    ㄴ ex. 대부분의 Set 구현체가 이용하는 AbstractSet 의 equals, List 구현체는 AbstractList, Map 구현체들은 AbsractMap<br>
4. 클래스가 private 이거나 package-private 이고 equals 메서드를 호출할 일이 없다.<br>
    ㄴ equals 를 막아버리기<br>

<br>

```java

// 4번 케이스의 예시코드
@Override
public boolean equals(Object o) {
    throw new AssertionError();     // 호출금지
}

```

<br>

equals 를 재정의 해야 하는 상황은, <b>"상위 클래스의 equals 가 논리적 동치성을 비교하도록 재정의되지 않았을 때"</b> 이다.<br>
물론 이러한 클래스도 같은 값의 인스턴스가 둘 이상 생성되지 않음을 보장하는 <b>인스턴스 통제 클래스</b> 는 equals 를 재정의 하지 않아도 된다고 한다.<br>
<br>

> <b>인스턴스 통제 클래스 (아이템 1)</b><br>
><br>
> 특징: private 생성자, static final<br>
> Enum 포함<br>

<br>
equals 를 재정의 할 때의 규약은 이러하다.<br>
<br>

1. 반사성 (Reflexivity)
2. 대칭성 (Symmetry)
3. 추이성 (Transitivity)
4. 일관성 (Consistency)
5. null - 아님

우선 1번의 반사성과 5번의 null - 아님은 왠만하면 문제가 발생할 일이 없다.<br>
먼저 확인하고 넘어가자.<br>
<br>

---

### 반사성

<b>null 이 아닌 모든 참조 값 x 에 대해, x.equals(x) 는 true 다.</b><br>
<br>
즉, 자기는 자기와 같아야한다.<br>
이건 굳이 안 짚어도 코드 상 문제가 있는게 아니면 넘어가도 된다.<br>

### null - 아님

<b> null 이 아닌 모든 참조 값 x 에 대해, x.equals(null) 은 false 다.</b><br>
<br>
5번 또한 마찬가지이다.<br>
인스턴스 생성이 확정된 객체가 null 과 같을 수 없기 때문이다.<br>
<br>
그리고 우리는 2, 3, 4 번을 유심히 볼 필요가 있다.<br>

---

### 대칭성 (Symmetry)

<b>null 이 아닌 모든 참조 값 x, y 에 대해, x.equals(y) 가 true 면 y.equals(x) 도 true 이다.</b><br>
<br>
즉, 내가 너와 같다면, 너도 나와 같아야 한다.<br>
간단하고 당연해 보이는 건가?<br>
대칭성은 어긋나기 쉬운 규약이다.<br>
코드로 짚고 넘어가자.<br>
<br>

```java

public final class CaseInsensitiveString {

    private final String s;
    private final int n;
    private final float i;

    private String s2;

    public CaseInsensitiveString(String s) {
        this.s = Objects.requireNonNull(s);
    }

    // 대칭성 위배!
    @Override
    public boolean equals(Object o) {
        if (o instanceof CaseInsensitiveString) {
            return s.equalsIgnoreCase(
                ((CaseInsensitiveString) o).s
            );
        }

        if (o instanceof String) {
            return s.equalsIgnoreCase((String) o);
        }

        return false;
    }
}

```

<br>
위 코드에서 무엇이 대칭성을 위배 했을까?<br>
분명 CaseInsensitiveString 클래스 본인의 값을 넣어도 동치성을 비교를 해줄 것이고, 문자열을 넣어도 비교를 해줄 것이다.<br>
<br>
x.equals(y) = true ? y.equals(x) : false;<br>
<br>
이 삼항 방정식에서 틀린 부분은 y.equals(x) 가 성립하지 않는다는 것이다.<br>
<b>위 클래스는 String 을 직접 비교하여 논리값을 반환</b>한다.<br>
그렇다면 <b>반대로 String 이 CaseInsensitiveString 클래스를 받아서 동치성 비교를 할 수 있을까?</b><br>
절대 아니다.<br>
즉, 클래스 본인은 String 을 알고 있지만, 직접적인 비교 대상이 되는 String 은 위 클래스를 모르고 있다.<br>
<br>
<b>'음? 그러면 모든 동치 비교에 비교 연산자가 성립이 안되잖아?'</b><br>
책에서 위 구문을 보고 내가 처음 든 생각이었다.<br>
'아니, int 를 가지고 있어서 비교하면 int 는 그 클래스를 모를 것이고, float 도 마찬가지 일텐데, 그러면 equals 를 어떻게 재정의하지?'<br>
정말 <b style="color: red;">짧게 생각하고 생긴 질문</b>이었다.<br>
<br>
위 코드에서 보여주려는 예시는 <b>특정 클래스가 동치성을 비교하기 위해 전혀 다른 타입의 값을 넣어버린 상황</b>인 것이다.<br>
논리적 동치 관계를 파악하기 위해선 본인, 혹은 책에서 설명한 것처럼 상위 클래스를 상속, 구현하는 서브 클래스가 그 비교 대상이 될 것이다.<br>
물론 때에 따라 비교 역할을 맡은 다른 클래스에 위임하는 경우도 있을 것이다.<br>
이는 추후 아이템 11 에서 더 설명이 나올 것이다.<br>

하지만 java 라이브러리 내에서도 이를 어긴 클래스가 하나 있었는데..<br>
바로 <b>java.sql.Timestamp</b> 이다.<br>
Timestamp 는 <b>java.util.Date 를 상속</b>한 시간 관리 클래스이다.<br>
특히 JPA 와 PostGreSQL 을 연동했던 프로젝트에서 시간 관리용 필드로 직접 사용한 적이 있어서 책에서 나왔을 때 반가운 예시였다.<br>
하지만 Timestamp 는 대칭성을 어긴 대표적인 클래스이다.<br>
<br>
그 이유는 <b style="color: blue;">6 개의 필드</b>로 시간을 관리하는 <b style="color: blue;">Date</b> 와 달리 Date 를 상속하면서 <b style="color: red;">나노초 필드를 추가</b>해 관리하는 클래스가 <b style="color: red;">Timestamp</b> 이기 때문이다.<br>
<br>
즉, is - a 관계가 성립하지 않는다.<br>
둘이 다루는 필드가 다르기 때문.<br>
Timestamp 클래스의 equals 를 확인하면,<br>

```java

...

// Timestamp 의 equals 메서드
public boolean equals(Timestamp ts) {
    if (super.equals(ts)) {
        if  (nanos == ts.nanos) {
            return true;
        } else {
            return false;
        }
    } else {
        return false;
    }
}

...

```

super.equals() 즉, Date 의 equals 먼저 호출하여 확인한다.<br>
그 후, 본인이 관리하는 nonos 필드를 비교한다.<br>
만약 모든 매개변수를 0으로 초기화 하고 서로의 eqauls 로 서로의 동치를 비교하면<br>
<br>

> System.out.println(date.equals(timestamp));<br>
> 출력: true<br>
> System.out.println(timestamp.equals(date));<br>
> 출력: false<br>

<br>
이 두 클래스를 같이 이용한다면 정상적으로 동작할 수 없다.<br>
<br>

### 추이성

<b>null 이 아닌 모든 참조 값 x, y, z 에 대해, x.equals(y) 가 true 이고 y.equals(z) 가 true 이면 x.equals(z) 도 true 이다.</b><br>
<br>
즉, 첫 번째 객체와 두 번째 객체가 같고, 두 번째 객체가 세 번째 객체와 같을 때, 첫 번째 객체와 세 번째 객체도 같아야 한다.<br>
쉬워보이는 조건 중 하나이다.<br>
하지만 위의 Timestamp 처럼 상속을 한 후, 필드를 추가하는 상황을 예상해보자.<br>
물론 Timestamp 는 추이성을 인식하고 super.equals(ts) 먼저 동작을 하게 했다.<br>
하지만 만약에 저 코드를 사용하지 않고 Timestamp 에서 추가된 본인의 필드를 모르는 Date 의 equals 를 그대로 사용한다면,<br>

```java

public static void main(String... args) {
    Timestamp timestamp1 = new Timestamp(0, 0, 0, 0, 0, 0, 444);  // 마지막 매개변수는 나노초를 관리하는 필드에 저장된다.
    Date date = new Date(0, 0, 0, 0, 0, 0);
    Timestamp timestamp2 = new Timestamp(0, 0, 0, 0, 0, 0, 99999);

    System.out.println(timestamp1.equals(date));
    System.out.println(date.equals(timestamp2));
    System.out.println(timestamp1.equals(timestamp2));
}

```

> 출력<br>
> <br>
> true<br>
> true<br>
> true<br>

<br>
Timestamp 에서 관리하는 나노초를 비교하는 구문을 이용하지 않는다면 벌어질 일이다.<br>
비교에 대한 출력 자체는 모두 true 를 뱉어낼 것이다.<br>
하지만 실제로 timestamp1 과 timestamp2 가 관리하는 나노초 필드는 전혀 다른 값을 나타낼 것이다.<br>
이게 우리가 SOLID 에서 OCP 를 지키는 대표적인 이유이다.<br>
<br>

> OCP: 개방폐쇄원칙, 확장에는 개방적이게, 변경에는 패쇄적이게<br>

---

### 일관성

<b>null 이 아닌 모든 참조 값 x, y 에 대해, x.equals(y) 를 반복해서 호출하면 항상 true 를 반환하거나 항상 false 를 반환한다.</b><br>
<br>
이 규약도 어려운 규약 자체는 아니다.<br>
하지만 중요한 개념은 <b>불변 객체</b>라는 상황에서 다를 수 있다는 점이다.<br>
불변 객체는 한번 true 가 나온 두 인스턴스라면 어떤 상황에서도 false 가 반환되면 안된다.<br>
<br>




> 아이템 10 에서 획득한 키워드<br>
> <br>
> 1. 
{: .prompt-tip }

--- 


