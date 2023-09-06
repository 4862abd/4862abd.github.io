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

우선 <b>equals 를 재정의하지 않는 상황</b>을 나열한다.<br>

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
> <br>
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

---

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
Timestamp 에서 관리하는 나노초를 비교하는 구문을 이용하지 않고 상위 클래스인 Date 의 equals 가 호출 된다면 벌어질 일이다.<br>
비교에 대한 출력 자체는 모두 true 를 뱉어낼 것이다.<br>
하지만 실제로 timestamp1 과 timestamp2 가 관리하는 나노초 필드는 전혀 다른 값을 나타낼 것이다.<br>
이게 우리가 SOLID 에서 L 를 지키는 대표적인 이유이다.<br>
<br>

> 리스코프 치환 법칙: 서브클래스는 상위클래스로 완벽히 대체될 수 있어야 한다.<br>

---

### 일관성

<b>null 이 아닌 모든 참조 값 x, y 에 대해, x.equals(y) 를 반복해서 호출하면 항상 true 를 반환하거나 항상 false 를 반환한다.</b><br>
<br>
이 규약도 어려운 규약 자체는 아니다.<br>
하지만 중요한 개념은 <b>불변 객체</b>라는 상황에서 다를 수 있다는 점이다.<br>
불변 객체는 한번 true 가 나온 두 인스턴스라면 어떤 상황에서도 false 가 반환되면 안된다.<br>
또한 필수적으로 동치성 비교가 필요한 필드를 equals 에서 비교하지 않으면 equals 가 나중에 어떤 동작을 할지 모르게 된다.<br>
<br>
즉, 필수적인 필드를 비교하기 위해서 형변환이 필수적일텐데, 이를 책에서는 이렇게 코드로 풀어냈다.<br>

```java

@Override
public boolean equals(Object o) {
    if (!(o instanceof MyType)) {   // 이 코드는 묵시적으로 null 도 검사할 수 있다.
        return false;
    }

    MyType myType = (MyType) o;

    ...
}

```

우선 본인 혹은 본인의 하위 클래스인지 확인한다.<br>
그 후 해당 조건문에서 false 를 반환하지 않았다면 매개변수를 equals 를 재정의하는 본 클래스로 형변환 한다.<br>
이제 우리는 하단에서 매개변수와 이 클래스의 핵심 필드들을 편하게 비교할 수 있게 된다.<br>

<br>

---

equals 를 재정의할 때의 규약은 위의 5가지이다.<br>
그렇다면 이를 지켜가며 구현할 때 어떻게 해야할까?<br>
위에 잠깐 등장한 Timsstamp 를 확인해보자.<br>
책에서는 이렇게 상위 클래스를 관리하는 하위클래스에 필드가 추가된다면 상위클래스를<br>
<b>"상속 대신 <span style="color: red;">컴포지션</span>을 사용하라"</b><br>
라는 방식으로 해결하길 추천한다.<br>
<br>
컴포지션은 상속을 받는 것이 아닌,<br>

```java
public class Timestamp {
    
    private final java.util.Date date;
    private final int nanos;
    
    Timestamp(Date date, int nanos) {
        this.date = date;
        this.nanos = nanos;
    }
    
    ...
}
```

<br>
이렇게 필드에 선언하여 관리하는 것을 의미한다.<br>
is - a 관계가 깨질 것이 분명하기에 has - a 혹은 is - part - of 관계를 확실하게 하는 것이다.<br>
상속으로 발생할 유연성의 파괴를 다시 획득할 수 있으며, 두 클래스간 결합도도 낮출 수 있을 것이다.<br>
<br>

---

여태까지 살펴본 규약들을 지키며 책에서 말하는 구현 방법을 단계 별로 정리해보자.<br>
<br>

1. <b>== 연산자</b>를 사용해 입력이 자기 자신의 참조인지 확인한다.<br>
    ㄴ 자기 자신이면 true 를 반환한다.<br>
    ㄴ 이는 추후 조건문, 비교 등의 이전에 작성하여 성능 최적화 용도로 이용된다.<br>
2. <b>instanceof 연산자</b>로 입력이 올바른 타입인지 확인한다.<br>
3. 입력을 <b>올바른 타입으로 형변환</b>한다.<br>
    ㄴ 앞서서 instanceof 로 확인을 했기에 문제가 없다.<br>
4. 입력 객체와 자기 자신의 대응되는 <b>'핵심' 필드들이 모두 일치</b>하는지 하나씩 검사한다.<br>
    ㄴ 이는 어떤 필드를 먼저 비교하는지 그 순서도 성능 상 차이가 있을 수 있다.<br>
    ㄴ 만약 비교에 조금 더 적은 비용이 발생한 다면 우리는 숏컷을 활용할 수 있다.<br>
5. 실수형 자료형의 비교는 각 자료형의 <b>랩퍼 클래스가 지닌 compare 메서드</b>를 이용한다.<br>
6. 배열 혹은 Collection 기반의 자료형 이라면, 각 원소를 위의 단계에 맞춰 비교한다.<br>
    ㄴ 배열의 모든 원소가 핵심 필드라면 Arrays.equals 메서드들 중 하나를 사용하자.<br>
7. 때로 null 도 정상 값 취급하는 참조타입 필드도 있다.<br>
    ㄴ 이는 <b>정적 메서드인 Objects.equals</b>(Object, Object) 로 비교하여 예방하자.<br>

<br>
내가 위의 단계로 직접 구현하며 정리했던 equals 를 먼저 소개 하겠다.<br>
<br>

```java
public class Parent {
    private final int x;
    private final float y;
    private double z;

    ...

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (!(o instanceof Parent)) {
            return false;
        }

        Parent parameter = (Parent) o;

        return this.x == parameter.x
            && Float.compare(this.y, this.y);
    }
}
```

<br>
위에 재정의한 equals 는 문제 없이 핵심 필드의 동치성 비교만 할수 있을 것이다.<br>
<br>
equals 재정의 할 때, 추가로 몇 가지 주의사항만 남기겠다.<br>
<br>

1. equals 를 재정의 할 때에는 <b>hashCode 도 반드시 재정의</b> 하라.<br>
2. 너무 복잡하게 해결하려 하지 말자.<br>
3. Object 외 구체적인 다른 클래스를 매개변수로 받는 equals 는 재정의하지 말자.<br>

<br>

> 아이템 10 에서 획득한 키워드<br>
> <br>
> 1. 심볼릭 링크<br>
> <br>
> 마지막 equals 의 주의사항 중 2번 에서 나온 키워드 이다.<br>
> File 클래스의 equals 에 대한 예시 중 나왔는데, 심볼릭 링크란<br>
> '절대, 상대 경로의 형태로 된 다른 파일이나 디렉터리에 대한 참조를 포함하는 파일 등'<br>
> 조금은 이해하기 어려워서 File 의 equals 를 뜯어 보았다.<br>
> 우선 File 클래스의 equals 는 FileSystem 을 상속하는 WinNTFileSystem 클래스의 compare 메서드를 이용한다.<br>
> 그리고 해당 클래스는 String 의 compareToIgnoreCase 메서드를 이용한다.<br>
> 동작 자체의 원리는 이해 했지만, 아직 심볼릭 링크는 이해하지 못 한것 같다.<br>
> 아니면 내가 생각하는 복잡한 것이 아닌 단순하게 경로를 저장하는 파일 등을 일컫는 말일 수 있겠다.<br>
{: .prompt-tip }

--- 

## ● 아이템 11: equals 를 재정의하려거든 hashCode 도 재정의하라

이 책 뿐만 아니라 여러 책에서도 다루는 주제이다.<br>
그만큼 <b><span style="color: blue;">equals 메서드</span>와 <span style="color: blue;">hashCode 메서드</span>는 <span style="color: red;">결합도</span>가 굉장히 강하다.</b><br>
이는 <b>Object 명세</b>에도 나타나 있는데,<br>
간단하게 정의하면 이렇다.<br>
<br>

1. 애플리케이션을 재구동하는 것이 아니고 <b>equals 비교에 사용되는 정보가 바뀐 것이 아니라면,<br>
hashCode 는 항상 일정</b>한 값을 반환해야한다.<br>
2. <b>equals 가 두 객체를 같다고 판단</b>했다면, 두 객체의 <b>hashCode 는 똑같은 값</b>을 반환해야 한다.<br>
3. <b><span style="color: red;">*</span>equals 가 두 객체를 다르다고 판단</b>했더라도, 두 객체의 <b>hashCode 가 다른 값을 반환할 필요는 없다.</b><br>
    ㄴ 단 다르다고 판단된 두 객체가 각각의 hashCode 값을 반환해야 해시테이블의 성능이 좋아진다.<br>

<br>
위 3 가지 중 3번이 궁금해서 조금 뜯어 보았다.<br>

```java
public synchronized V put(K key, V value) {
    // Make sure the value is not null
    if (value == null) {
        throw new NullPointerException();
    }

    // Makes sure the key is not already in the hashtable.
    Entry<?,?> tab[] = table;
    int hash = key.hashCode();
    int index = (hash & 0x7FFFFFFF) % tab.length;

    @SuppressWarnings("unchecked")
    Entry<K,V> entry = (Entry<K,V>)tab[index];
    for(; entry != null ; entry = entry.next) {
        if ((entry.hash == hash) && entry.key.equals(key)) {
            V old = entry.value;
            entry.value = value;
            return old;
        }
    }

    addEntry(hash, key, value, index);
    return null;
}
```

<br>
위의 코드는 <b>실제 Hashtable 의 put</b> 메서드이다.<br>
딱 봐도 변수명 <b>index</b><br>
너무너무 중요해보이는 변수명 아닌가.<br>
구하는 방식은 매개변수로 받은 제네릭 타입인 <b>key 의 <span style="color: red;">hashCode</span></b> 를 뽑아서 0x7FFFFFFF 과 & 비트연산을 한 후에,<br>
현재 저장된 table 의 크기로 나눈 나머지가 index 가 된다.<br>
물론 책에서 설명한 것처럼 이를 그대로 이용하는 것이 아니다.<br>
<b>반복문을 통해 해당 index 와 매개변수 key 의 값으로 저장된 데이터가 있는지 찾아낸다.</b><br>
<u>hashCode 가 굳이 다를 필요는 없지만, 결국 반복문을 통해 Hashtable 의 성능이 떨어질 수 있다</u>는 점은 간과할 수 없다.<br>
위의 코드 뿐 만이 아니다.<br>
hashCode 를 활용한 HashSet 등의 자료형은 모두 영향을 받을 수 있다.<br>
그래서 우리는 <b>equals 메서드를 재정의 했다면 필수적으로 hashCode 메서드도 재정의</b> 해야한다.<br>
<br>
내가 간단하게 구현한 코드를 통해 예시를 들어보겠다.<br>

```java
// main 메서드
public static void main(String... args) {
    Car car1 = new Car(4, 2);
    Car car2 = new Car(4, 2);
    
    List<Car> carList = new ArrayList<>();
    carList.add(car1);

    System.out.println("contain? " + carList.contains(car2)); 
    
    // -----------------------------------------------------
    
    Hashtable<Car, String> hashtable = new Hashtable<>();
    hashtable.put(car1, "myCar");
    System.out.println("get? " + hashtable.get(car2));
}

...

// Car 클래스
public class Car {
    private final int wheel;
    private final int mirror;
    
    Car(int wheel, int mirror) {
        this.wheel = wheel;
        this.mirror = mirror;
    }
}

...

// ArrayList 의 contains 메서드
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}


public int indexOf(Object o) {
    return indexOfRange(o, 0, size);
}

int indexOfRange(Object o, int start, int end) {
    Object[] es = elementData;
    if (o == null) {
        for (int i = start; i < end; i++) {
            if (es[i] == null) {
                return i;
            }
        }
    } else {
        for (int i = start; i < end; i++) {
            if (o.equals(es[i])) {              // Object 의 equals 메서드를 이용하는 것을 알 수 있다.
                return i;
            }
        }
    }
    return -1;
}

```

> 출력<br>
> <br>
> contain? false<br>
> get? null<br>

<br>
위의 Car 클래스는 equals 도 hashCode 도 재정의하지 않은 상태이다.<br>
완전히 동일한 값을 가지는 인스턴스 car1 과 car2 를 획득하여 car1 을 ArrayList, Hashtable 에 추가해줬다.<br>
그리고 하단은 ArrayList 의 contains 메서드와 그를 이루는 동작이다.<br>
<br>
<b>ArrayList 는 Object 의 <span style="color: red;">equals 를 이용하여 contains 메서드를 구현</span></b>한 것을 확인할 수 있다.<br>
그러면 equals 를 재정의하고 동작해보자.<br>
<br>

```java
// main 메서드
public static void main(String... args) {
    ...
}

...

// Car 클래스
public class Car {
    private final int wheel;
    private final int mirror;
    
    Car(int wheel, int mirror) {
        this.wheel = wheel;
        this.mirror = mirror;
    }

    // 추가된 equals 메서드
    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (!(o instanceof Car)) {
            return false;
        }

        Car parameter = (Car) o;

        return this.wheel == parameter.wheel
            && this.mirror == parameter.mirror;
    }
}
```

> 출력<br>
> <br>
> contain? true<br>
> get? null<br>

<br>
<b>equals 가 재정의</b> 됨에 따라 <b>contains 메서드에서 이용하는 equals 가 Car 클래스의 equals 를 따라가는 것</b>을 알 수 있다.<br>
이는 오버라이딩에 적합한 목적이고, 디버깅으로 확인한 확실한 동작이다.<br>
hashCode 는 진행하며 결과가 어떻게 달라지는지 테스트 해볼것이다.<br>
<br>
우선 hashCode 를 잘못 재정의 했을 때 문제가 되는 점은 두 번째다.<br>
<b>2. equals 가 두 객체를 같다고 판단했다면, 두 객체의 hashCode 는 똑같은 값을 반환해야 한다.</b><br>
이는 내가 위에 예시로 보인 코드와 같다.<br>
equals 는 재정의 되어 분명 물리적으로 다른 인스턴스여도 같다고 판단을 했다.<br>
하지만 Hashtable 에서는 내가 원하는 문자열을 반환하지 않고 null 을 반환한다.<br>
<br>
즉, hashCode 를 재정의 하지 않았기에 논리적 동치가 일치함에도 처음 넣은 객체가 아니라면 <b>같은 객체로 인식하지 않는다.</b><br>
물론 이 문제는 hashCode 를 재정의하면 해결되는 문제이다.<br>
하지만 이를 해결하기 위해 하면 안되는 것이 있는데,<br>
<br>

```java

@Override
public int hashCode() {
    return 42;
}

```

<br>
이렇게 본인 클래스를 이용해 인스턴스를 획득한 모든 hashCode 값을 같은 값으로 반환하는 것이다.<br>
불변 클래스도 아니고 모든 인스턴스의 hashCode 값이 같아진다면 기존 <b>O(1)</b> 의 시간 복잡도를 가진 해시를 <b>O(n)</b> 으로 사용하게 된다.<br>
혹시라도 생성, 저장할 객체가 많아진다면,<br>
성능 상 <b>구현을 할 필요가 전혀 없어</b>진 것이다.<br>
<br>
<b>hashCode 를 재정의하기 위한 규약</b>은 여러 개 있다.<br>
하지만 핵심만 골라서 말한다면 이렇게 될 것이다.<br>
<br>

1. <b>가장 중요한 핵심 필드의 hashCode</b> 를 먼저 구한다.<br>
2. <b style="color: red;">31<b> 에 그 값을 곱한 후, 논리적 동치를 구하기 위한 다른 핵심 필드의 hashCode 값을 더해준다.<br>
3. 참조 타입 필드면서 이 클래스의 equals 가 이 필드의 equals 를 재귀적으로 호출해 비교한다면, 이 필드의 hashCode 를 호출한다.<br>
    ㄴ 이 과정이 복잡해 진다면, 이 필드의 표준형 (Canonical Representation) 을 만들어 그 표준형의 hashCode 를 호출한다.<br>
4. 참조가 배열이라면 각 원소를 1, 2, 3 의 규약을 따라 필드처럼 hashCode 를 재정의한다.<br>
    ㄴ <b>배열에 원소가 하나도 없다면 상수 (0 이 대표적이다.) 를 반환</b>한다.<br>
5. 필드의 <b>값이 null 일 경우 상수 (0 을 전통적으로 사용한다.) 를 반환</b>한다.<br>

<br>
이제 이에 맞춰 위 코드의 Car 클래스의 hashCode 를 재정의 해보겠다.<br>
그리고 아까처럼 값이 완전히 같은 car1 과 car2 를 만들어서 Hashtable 에 car1 을 키로 넣은 후, car2 로 get 을 시도해보겠다.<br>
<br>

```java
// main 메서드
public static void main(String... args) {
    Car car1 = new Car(4, 2);
    Car car2 = new Car(4, 2);
    
    List<Car> carList = new ArrayList<>();
    carList.add(car1);

    System.out.println("contain? " + carList.contains(car2)); 
    
    // -----------------------------------------------------
    
    Hashtable<Car, String> hashtable = new Hashtable<>();
    hashtable.put(car1, "myCar");

    // hashCode 를 재정의 하고 같은 값을 가진 다른 객체를 키로 get 메서드를 호출했다.
    System.out.println("get? " + hashtable.get(car2));
}

...

// Car 클래스
public class Car {
    private final int wheel;
    private final int mirror;
    
    Car(int wheel, int mirror) {
        this.wheel = wheel;
        this.mirror = mirror;
    }

    // 추가된 equals 메서드
    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (!(o instanceof Car)) {
            return false;
        }

        Car parameter = (Car) o;

        return this.wheel == parameter.wheel
            && this.mirror == parameter.mirror;
    }

    @Override
    public int hashCode() {
        int result = Integer.hashCode(wheel);
        result = 31 * result + Integer.hashCode(mirror);
        return result;
    }
}

```

<br>

> 출력<br>
> <br>
> contain? true<br>
> get? myCar<br>

<br>
물리적으로 다른 인스턴스를 키로 이용해서 Hashtable 에서 조회를 했음에도 이제는 서로 같은 값을 도출하는 것을 알 수 있다.<br>
<br>

---

<br>
그리고 31 을 곱하는 과정에 대해 많은 최적화 방법이 있다고 한다.<br>
책에서 설명한 대표적인 예시로는<br>
<br>
<b>31 * i = (i << 5) - i</b><br>
<br>
라고 한다.<br>
이 뿐만 아니라 해시 코드의 충돌이 적은 방법을 써야 한다면 구아바의 com.google.common.hash.Hashing 을 추천하기도 했다.<br>
이는 나중에 찾아봐야 할 것 같다.<br>
<br>
만약 클래스가 불변이고 해시코드를 계산하는 비용이 크다면, 캐싱을 추천했다.<br>
혹은 지연 초기화 전략을 제안했다.<br>
<br>

> 지연 초기화는 이전부터 몇 번 등장하는 키워드 이다.<br>
> 중요한 키워드라는 것은 확실하다.<br>

<br>

```java

// 지연 초기화 적용
private int hashCode;

@Override
public int hashCode() {
    // immutable 하게 하기 위하여 기본자료형인 result 에 hashCode 를 이용해 초기화 한다.
    int result = hashCode;

    if (result == 0) {    // 할당된 값이 없는 경우 hashCode 구하기

        ...

    }

    return result;
}

```

> 아이템 11 에서 획득한 키워드<br>
> <br>
> 1. 구아바<br>
> 이는 나중에 다시 확인해야 한다.<br>
{: .prompt-tip }

---

## ● 아이템 12: toString 을 항상 재정의하라

보통 toString 을 재정의 하지 않은 클래스를 출력하게 되면,<br>
<br>

> <b>클래스_이름@16진수로_표시한_해시코드</b><br>

<br>
의 형태를 출력한다.<br>
실제로 나도 많이 본 것이다.<br>
<br>
하지만 나도 이걸 이용하는 상황은 단순히 두 인스턴스가 같은 인스턴스 인지 확인할 때 였는데,<br>
아이템 11 의 hashCode 규약에서 말한 것처럼 "같은 hashCode 를 반환하는 객체도 다른 값을 나타낼 수 있다" 를 보고 생각이 많이 바뀌었다.<br>
<b>확실히 toString 의 역할이 생길 것 같다고.</b><br>
또한 Audit 등 우리가 로그 등을 통해 기록을 남겨야 하는 상황이라면 toString 의 역할은 더 커질 수 있다.<br>
이런 클래스를 디버깅하는 것도 쉬워질 것이다.<br>
<br>
물론 구현 하는 방식은 개발자의 마음이다.<br>
더 기록에 잘 남게, 더 눈에 띄게, 더 많은 양을 저장할 수 있게...<br>
"+" 연산자를 이용하기, StringBuilder 를 이용하기, String 의 concat, join 등을 이용하기 등등<br>
또한 책에서 설명하는 toString 의 팁은 이러하다.<br>
<b>실전에서 toString 은 그 객체가 가진 주요 정보 모두를 반환하는 게 좋다.</b><br>
다만 책에서는 toString 이 반환하는 문자열에 대한 포맷 (양식) 은 제공하는 것을 추천한다.<br>
<br>

```java

/**
 * 이 동물 클래스의 정보를 반환한다.
 * 이름, 나이, 무게를 표시하며 무게는 0.0 형태로 실수형으로 표시된다.
 * 
 * 상세 형식은 정해지지 않았으며, 향후 변경될 수 있다.
 * "{ 이름: name, 나이: age, 무게: weight }"
 */
@Override
public String toString() { ... }

```

그리고 포맷과 상환 없이 <b>toString 이 반환한 값에 포함된 정보를 얻어올 수 있는 API 를 제공하라</b> 라고 한다.<br>
이 정보가 없다면 개발자는 toString 으로 얻은 문자열을 다시 파싱하여 데이터를 뜯어야할 수도 있다.<br>
<br>
그리고 대다수의 컬렉션 구현체는 추상 컬렉션 클래스들의 toString 메서드를 상속해 쓴다.<br>
<br>

> <b>※ 아이템 12 의 독후감</b><br>
> <br>
> <b>읽은 직후</b><br>
> 만약 내가 개발하는 애플리케이션의 클래스에 toString 을 재정의 한다면,<br>
> 클래스의 역할, 반환 빈도, 저장할 데이터의 양 등에 따라 구분해서 toString 을 재정의 할 것 같다.<br>
> <br>
> <b>며칠 후 내 생각</b><br>
> 물론 책임을 확실하게 지고 구분하는 방식도 좋다.<br>
> 하지만 내역 조회처럼 그 데이터를 다시 파싱하여 해석해야 하는 경우,<br>
> 저마다 다른 양식에 따라 어떻게 해석할지, 그 코드는 어떻게 구현할 것이며 혹시라도 발생할 예외는 어떻게 회복할지 몇 배로 고민해야 할 것이다.<br>
> 데이터베이스의 비정규화 개념이 단순히 이론적으로 존재하는 것이 아니다.<br>
> 우리는 성능을 위해 이상과 타협하는 순간을 알아야 할 것이다.<br>
{: .prompt-info }

<br>

---

## ● 아이템 13: clone 재정의는 주의해서 진행하라

아, 이번 아이템은 정리하기 전에 한 마디만 하겠다.<br>
이번 아이템 초반 4장에 걸쳐서 Cloneable 에 대하여 열심히 설명하더니,<br>
마지막에 하는 말이..<br>
<br>
<b>"final 클래스라면 Cloneable 을 구현해도 위험이 크지 않지만.."</b><br>
<b>"복제 기능은 생성자와 팩터리를 이용하는게 최고"</b><br>
<br>
...?<br>
<br>
이걸 정리를 해야해 말아야 해.<br>
물론 클래스를 복제하는 개념이 있어서 읽기에는 좋았고, 왜 생성자와 팩터리가 좋은지 설명해 가는 과정도 좋았다.<br>
<br>
하지만 힘빠지는 기분이랄까..<br>
<br>

---

<br>

책에서 설명하는 주요개념만 확인하면 이렇게 된다.<br>
<br>

1. Cloneable 은 Object 안에 정의되어 있는 <b>clone 메서드 만을 위한 인터페이스</b> 이다.<br>
    ㄴ Cloneable 을 구현함으로 protected 접근제한자가 붙은 clone 을 사용할 수 있다.<br>
2. Cloneable 을 구현함으로 얻는 이득도 많지만 <b style="color: red;">실도 많다.</b><br>
    ㄴ private 으로 접근을 제한한 클래스도 clone 메서드를 호출함으로 원래의 목적에 모순적이게 복제가 가능하다.<br>
3. 관례상, <b>반환된 객체와 원본 객체는 독립적</b>이어야 한다.<br>
    ㄴ 이를 만족하려면 super.clone 으로 얻은 객체의 필드 중 하나 이상을 반환 전 수정해야 할 수도 있다.<br>
4. Cloneable 뿐 아니라 객체를 복사하는 모든 방법은 <b>얕은 복사를 주의</b>해야 한다.<br>
    ㄴ 컴포지션 등 내부에 참조 객체가 선언되어 있거나 Collection 객체 등을 복사한다면 특히 조심하자.<br>
    ㄴ 자칫 <b>잘못하면 원본의 객체와 가변 상태를 공유할 수 있다.</b><br>
    ㄴ 깊은 복사 방식을 선택하여 이를 원본 객체의 Muttable 하게 해결하자.<br>
5. 기존의 clone 메서드는 Object 타입을 반환하지만 우리는 <b>공변 반환 타이핑</b> 을 통해 원하는 타입으로 반환가능하다.<br>


<b>Clonable 적용 예시</b>

```java

public class TargetClass implements Cloneable {

    private TargetClassSuper targetClassSuper = new TargetClassSuper();

    @Override
    public TargetClass clone() {
        try {
            // 공변 반환 타이핑
            TargetClass targetClass = (TargetClass) super.clone();
            targetClass.TargetClassSuper = this.TargetClassSuper.clone();   // 컴포지션으로 관리한 클래스도 Cloneable 을 구현해야 한다.
            return targetClass;
        } catch(CloneNotSupportedException e) {                             // 검사 예외 (Checked Exception) 이다.

            // 이 코드는 CloneNotSupportedException 이 사실 비검사 예외 (Unchecked Exception) 여야 했음을 의미한다.
            throw new AssertionError();
        }
    }

}

```

위의 AssertionError 이 CloneNotSupportedException 가 검사예외가 아니어야 했음을 의미한다고 한다.<br>
<br>

> Thrown to indicate that the clone method in class Object has been called to clone an object, but that the object's class does not implement the Cloneable interface.<br>
> 클래스 객체의 clone 메서드가 호출되었지만 해당 클래스는 Cloneable 인터페이스를 구현하지 않았음을 나타내기 위함이다.<br>
> <br>
> 출처: https://docs.oracle.com/javase/8/docs/api/java/lang/CloneNotSupportedException.html<br>

<br>
즉, clone 코드 구현 시 try / catch 문은<br>
"Cloneable 을 구현하여 clone 메서드 재정의만 잘하면 필요 없는 예외처리다" 라는 의미인 것이다.<br>
<br>
또한 컴포지션으로 관리하는 클래스가 final 예약어를 이용해 선언되었다면 clone 을 해도 초기화가 불가능하다.<br>
이 점은 <b>Cloneable 아키텍처가 제공하는 문서 중 "가변 객체를 참조하는 필드는 final 로 선언하라"</b> 와 충돌한다.<br>
<br>
이것 말고도 상충되거나, 본래의 목적을 잃거나, 스택 프레임을 무수히 소비하거나 (배열을 재귀 호출로 clone 할 경우) 정말 많은 문제가 발생할 수 있다.<br>
<br>
그래서 이번 아이템 마지막에 나온 말들.<br>
<br>
<b>복사 생성자와 복사 팩터리는 인터페이스 타입의 인스턴스를 인수로 받을 수 있고, 인터페이스 기반 복사 생성자와 복사 팩터리의 더 정확한 이름은 '변환 생성자 (Conversion Constructor)' 와 '변환 팩터리(Conversion Factory)' 이다.</b><br>
<br>
<b>"복제 기능은 생성자와 팩터리를 이용하는게 최고"</b><br>
<br>

![01](/assets/img/06.Effective-Java/13.WatchOutCloneable/01.jpg)

---

## ● 아이템 14: Comparable 을 구현할지 고려하라

들어가기 전,<br>
<br>
우선 Comparable 도 직접 구현한 적이 있다.<br>
당연히 정렬을 위해서 구현했던 인터페이스였다.<br>
기억에는 M 으로 시작하는 3글자 프로젝트였는데 제일 좋아했고 재밌던 프로젝트였다.<br>
신입이 처음 맡은 프로젝트 였는데 JPA 를 쓸수 있던 프로젝트라 너무 좋았다.<br>
DB 도 Oracle 이 아닌 PostgreSQL 이었다.<br>
PostgreSQL 은 아이템 10 의 Timestamp 에서도 관련이 있었다.<br>
정말 재밌는 경험이었다.<br>

---

Comparable 도 Cloneable 처럼 유일한 메서드를 사용할 수 있게 해준다.<br>
<b>compareTo</b> 가 그러하다.<br>
인터페이스가 정의된 소스 먼저 보고 가자.<br>

```java

public interface Comparable<T> {
    public int compareTo(T o);
}

```

주석 빼고 나면 이게 전부다.<br>
정말 이게 전부다.<br>
<br>
우선 compareTo 덕분에 자동 정렬되는 컬렉션 관리도 쉽게 할 수 있다고 한다.<br>
그건 String 이 Comparable 을 구현한 덕분이라고 하는데 이건 조금 더 확인해야 할 것 같다.<br>
<br>
compareTo 의 중요 규약을 살펴보자.<br>
<br>

1. 두 객체 참조의 순서를 바꿔 비교해도 예상한 결과가 나와야한다.<br>
2. 첫 번째가 두 번째 보다 크고 두 번째가 세 번째보다 크면, 첫 번째는 세 번째보다 커야한다.<br>
3. 크기가 같은 객체들끼리 어떤 객체와 비교하더라도 항상 같아야한다.<br>

<br>
어디선가 본 규약들 같다.<br>
<b>compareTo 의 규약</b>은 <b>equals 의 규약</b>과 <b style="color: blue;">흡사</b>하다.<br>
<b>대칭성, 추이성, 반사성을 만족 해야한다</b>는 것이다.<br>
이 규약을 잘 지켜 compareTo 를 구현하게 되면 equals 의 동치성 비교와 같은 결과가 나오게 된다.<br>
<br>
그렇다면 compareTo 를 구현하는 이유는 무엇일까?<br>
<b>Collection</b> 들은 동치성을 비교할 때 <b>equals 대신 compareTo를 사용</b>하기 때문이다.<br>
담당하는 역할이 정렬이니 동치성 비교인 equals 만큼 중요한 문제는 아니겠지만 주의해야 한다.<br>
