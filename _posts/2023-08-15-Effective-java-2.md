---
title: 객체 생성과 파괴
author: park
date: 2023-08-15 09:04:00 +0800
categories: [Java, Effective Java]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

# 2장, 객체 생성과 파괴

### 아이템 목록

1. 생성자 대신 정적 팩터리 메서드를 고려하라<br>
2. 생성자에 매개변수가 많다면 빌더를 고려하라<br>
3. private 생성자나 열거 타입으로 싱글턴임을 보증하라<br>
4. 인스턴스화를 막으려거든 private 생성자를 사용하라<br>
5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라<br>
6. 불필요한 객체 생성을 피하라<br>
7. 다 쓴 객체 참조를 해제하라<br>
8. finalizer와 cleaner 사용을 피하라<br>
9. try-finally 보다는 try-with-resources 를 사용하라<br>

---

## ● 아이템 1: 생성자 대신 정적 팩터리 메서드를 고려하라

<br>
<b>정적 팩터리 메서드 (static factory method)</b><br>
<br>
우선 키워드를 느낀대로 정리해보자.<br>
<br>
정적: static, 메모리 상 거주하게 되는 객체<br>
팩터리: 객체를 생성하는 역할을 의미하는 키워드<br>
<br>
아이템 1의 제목과 연관지어 해석하면,<br>
생성자 메서드 대신에 정적으로 객체의 인스턴스를 획득할 수 있는 방식의 메서드의 채택을 고려하라.<br>
<br>
<b>장점</b><br>
<br>
첫 번째. 이름을 가질 수 있다.<br>
메서드 명명규칙의 정책을 잘만 설계하면 메서드가 반환하는 객체의 특성을 쉽게 묘사할 수 있다.<br>
하지만 일반 개발자가 이렇게 짜여진 레거시 코드를 활용해 프로그래밍 한다면 익숙해 지기 전까지<br>
클래스 하나하나를 뜯어서 팩터리 역할을 하는 메서드를 찾아내야하는 번거로움이 생길 수 있다.<br>
그래서 책에서도 설명하지만 <b>관용적으로 사용되는 getInstance, INSTANCE</b> 등을 예시로 사용하는 모습을 볼 수 있다.<br>
<br>
두 번째. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.<br>
불변 클래스 (immutable class, 상태 값을 임의로 수정하지 않게 설계한 클래스) 를 활용하거나 캐싱을 이용할 때 불필요한 인스턴스 획득을 막을 수 있다고 한다.<br>
그래서 생성 비용이 큰 객체가 자주 요청되는 상황에 성능을 크게 끌어올릴 수 있다고 한다.<br>
<br>
세 번째, <b>반환 타입의 하위 타입 객체를 반환할 수 있는 능력</b>이 있다.<br>
반환 값으로 구현 클래스를 공개하지 않고도 반환할 수 있어서 API를 작게 유지할 수 있다.<br>
그리고 이 기술은 인터페이스를 정적 팩터리 메서드의 반환 타입으로 사용하는 인터페이스 기반 프레임워크(아이템 20) 를 구현하는 핵심 기술이 된다고 한다.<br>
<br>
네 번째, 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.<br>
책에서는 이를 활용한 클래스인 EnumSet 을 예시로 든다.<br>
코드를 확인해보자.<br>

```java
public class EnumSetMainClass {

    public enum Week { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY; }
    
    public enum AllCase {
        A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,
        QW,WE,ER,RT,TY,YU,UI,IO,OP,PA,AS,SD,DF,FG,GH,HJ,JK,KL,LZ,ZX,XC,CV,VB,BN,NM,
        QWE,WER,ERT,RTY,TYU,YUI,UIO,IOP,OPA,PAS,SDF,DFG,FGH,GHJ,HJK,JKL,KLZ,LZX,ZXC,XCV,CVB,VBN,BNM;
    }
    
    public static void main(String... args) {
        EnumSet<Week> weekEnumSet = EnumSet.allOf(Week.class);
        System.out.println("allCaseEnumSet.count: " + Week.values().length);
        System.out.println("allCaseEnumSet.class: " + allCaseEnumSet.getClass());

        EnumSet<AllCase> allCaseEnumSet = EnumSet.allOf(AllCase.class);
        System.out.println("allCaseEnumSet.count: " + AllCase.values().length);
        System.out.println("allCaseEnumSet.class: " + allCaseEnumSet.getClass());
    }
}
```

> 출력<br>
> <br>
> weekEnumSet.count: 7<br>
> weekEnumSet.class: class java.util.RegularEnumSet<br>
> allCaseEnumSet.count: 74<br>
> allCaseEnumSet.class: class java.util.JumboEnumSet<br>

<br>
열거형 데이터의 개수와 그 열거형 데이터를 저장할 EnumSet 의 구현 클래스를 출력한 코드이다.<br>
확인하면 7개의 개수를 가지는 EnumSet은 RegualEnumSet 이 구현됐고, 74개의 개수를 가진 EnumSet은 JumboEnumSet 이 구현됐다.<br>
이는 EnumSet 의 구현부분 코드를 확인하면 더 명확한데,<br>

```java
// 확인하면 noneOf 메서드를 호출하는 것을 확인할 수 있는데,
public static <E extends Enum<E>> EnumSet<E> allOf(Class<E> elementType) {
    EnumSet<E> result = noneOf(elementType);
    result.addAll();
    return result;
}

// noneOf 케서드에서는 매개변수로 전달된 elementType 에 들어있는 열거형 데이터의 집합을 제네릭 와일드 카드를 이용해서 배열로 담아둔다.
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
    Enum<?>[] universe = getUniverse(elementType);
    if (universe == null)
        throw new ClassCastException(elementType + " not an enum");

    if (universe.length <= 64)
        return new RegularEnumSet<>(elementType, universe);
    else
        return new JumboEnumSet<>(elementType, universe);
}
```

클라이언트가 호출한 allOf() 메서드를 보면 바디 첫 줄에 noneOf 가 호출된다.<br>
그리고 noneOf 에서는 담아둔 데이터의 개수에 따라 구현할 인스턴스인 RegualEnumSet, JumboEnumSet 을 상황에 맞춰 반환한다.<br>
위처럼 상황에 맞춰 구현할 클래스를 직접 선택하여 반환하게 하는 것이 우리가 네 번째 장점으로 뽑은 것이다.<br>
<br>
다섯 번째 장점도 위에 설명한 바와 크게 다르지 않으니 우선 생략하고 <b>단점</b>을 보겠다.<br>

---

<b>단점</b><br>
<br>
첫 번째, 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.<br>
정적 팩터리 메서드는 기존의 기본 생성자 등을 private 으로 묶어 구현하는 형태로 될 것이다.<br>
그렇게 될 경우, 본인을 상속한 자식 클래스에서 생성자에 의해 super(); 가 호출될 수 없게된다.<br>
즉, 상속을 할 수가 없다.<br>
하지만 이 제약은 상속(is-a) 보다 컴포지션(has-a)을 사용하도록 유도하며, 불변 타입으로 만들 수 있어서 오히려 장점으로 받아들여 진다.<br>
<br>
두 번째, 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.<br>
이 단점은 내가 첫 번째 장점에 정리해뒀으니 패스 하겠다.<br>
<br>

> 아이템 1 에서 획득한 키워드<br>
> <br>
> 1. BigInteger, BigDecimal<br>
> java.math 패키지의 하위 클래스이다.<br>
> 크기가 큰 정수를 다루기 위한 class 로, 특히 부동소수점에 의해 정확한 계산이 필요한 정수타입의 경우, BigDecimal 을 사용할 것을 추천한다.<br>
> <br>
> 2. 플라이웨이트 패턴 (FlyWeight pattern), 브리지 패턴 (Bridge pattern)<br>
> 플라이웨이트 패턴: 캐싱의 일종으로 구조적 디자인 패턴 중 하나이다.<br>
> 같은 속성의 값으로 같은 인스턴스를 획득하려 하면, 임의로 지정한 저장소에서 해당 속성의 값을 가지는 인스턴스가 저장되어 있는지 확인한다.<br>
> 만약 인스턴스가 존재하면 해당 인스턴스의 참조값(주소) 를 반환하고, 없을 경우 인스턴스를 새로 획득하여 저장소에 저장해두고 후에 조회될 경우 반환한다.<br>
> 브리지 패턴: 두 개의 큰 클래스 혹은 밀접한 클래스들을 따로 구분하여 개발할 수 있게 하는 패턴이다.<br>
> 이는 구현 뿐 아니라 추상화에서도 접목할 수 있는 패턴이며 구분된 클래스들은 연관이 있으면서도 개별적으로 확장이 가능하다.<br>
{: .prompt-tip }

--- 

## ● 아이템 2: 생성자에 매개변수가 많다면 빌더를 고려하라

정적 팩터리 메서드의 단점을 먼저 이야기 한다.<br>
초기화에 필요한 매개변수가 많아지는 생성자의 경우 대응하기 어려워 진다는 것이다.<br>
<br>
그에따라 몇 해결 방법을 제안하는데, 빌더 패턴이 등장하기 전까지는 모두 그냥저냥한 패턴이다.<br>
<br>
패턴 1. <b>점층적 생성자 패턴 (Telescoping Constructor Pattern)</b><br>
<br>
매개변수가 필요한 상황, 책에서는 멤버 변수로 선언된 모든 상황을 설명하려 하는 것 같다.<br>
그렇게 될 경우 생성자 메서드가 오버로드 되어 생성될 개수는 멤버 변수로 만들 수 있는 집합의 경우의 수에 해당한다.<br>
<br>
정말 너무 많은 수가 등장 할 수 있다.<br>
그리고 이렇게 선언된 생성자 메서드의 경우 순서에 따라 데이터의 역할이 굉장히 큰 변수를 만들 수 있다.<br>
이는 일반 개발자가 소스 코드를 확실하게 파악해야 하며 개발에도, 유지보수에도 어려움을 겪을 가능성이 크다.<br>
<br>
<b>즉, 효율성, 가독성이 땅을 친다.</b><br>
<br>
패턴 2. <b>자바빈즈 패턴 (JavaBeans Pattern)</b><br>
<br>
쉽게 생각하면 인스턴스를 획득한 후, 수정자 메서드 (setter) 를 통해 필요한 모든 값을 바인드 해주는 패턴이다.<br>
내가 근무할 때 많이 사용했던 패턴이기도 하고 제일 무난하다고 생각했다.<br>
하지만 책에서 설명하는 단점이 여럿 있다.<br>
<br>

1. 우선 객체 하나를 위해 수 많은 메서드를 호출해야 한다.<br>
2. 객체가 완전히 생성되기 전까지는 <b>일관성 (consistency)</b> 이 무너진 상태에 놓인다.<br>
    ㄴ 계속해서 객체가 바뀌어야 하기 때문.<br>
3. 클래스를 불변으로 만들 수 없다.<br>
    ㄴ 불변이라고? 그러면 단점 2번이 성립할 수 없고, 애초에 자바빈즈 패턴이 성립하지 못 한다.<br>

<br>
이러한 자바빈즈 패턴의 문제점을 위해 생성이 끝난 객체를 수동으로 얼리고 (freezing) 얼리기 전에는 사용할 수 없게 하는 방법도 있다고 한다.<br>
이 방법은 이번 아이템에서 내가 찾아내지 못한 방법이다.<br>
내가 발표자가 아니라면 꼭 질문에 넣을 것이다. ^^<br>
<br>
패턴 3. <b>빌더 패턴 (Builder Pattern)</b><br>
<br>
드디어 책에서 설명하고 하는 패턴이다.<br>
위 패턴을 보고 여태 롬복에서 제공하는 @Builder  애노테이션이 같은 역할을 한다고 믿었다.<br>
하지만 @Builder 애노테이션으로 생성되는 소스를 디컴파일러를 통해 확인하니 빌더패턴과 다르게 단순히 모든 멤버 변수를 매개 변수로 받는 생성자 메서드를 추가하는 것이었다.<br>
<br>
음... 하지만 디컴파일을 통해 생성된 코드와 다르게 몇 메서드가 추가된 것을 보면 내가 삽질을 덜 한 것일 것 같다.<br>
우선 빌드 패턴의 소스를 보자.<br>

```java
public class BuilderPatternClass1 {
    // 필수
    private final int a;
    private final int b;
    // 선택
    private final int c;
    private final int d;
    private final int e;
    
    private BuilderPatternClass1(Builder builder) {
        this.a = builder.a;
        this.b = builder.b;
        this.c = builder.c;
        this.d = builder.d;
        this.e = builder.e;
    }
    
    public static class Builder {
        // 필수
        private final int a;
        private final int b;
        
        // 선택
        private int c;
        private int d;
        private int e;
        
        public Builder(int a, int b) {
            this.a = a;
            this.b = b;
        }
        
        public Builder c(int c) {
            this.c = c;
            return this;
        }
        
        public Builder d(int d) {
            this.d = d;
            return this;
        }
                
        public Builder e(int e) {
            this.e = e;
            return this;
        }
        
        public BuilderPatternClass1 build() {
            return new BuilderPatternClass1(this);
        }
    }
}

...

public class MainClass {

    public static void main(String... args) {
        
        BuilderPatternClass1 builderPatternClass1 = new BuilderPatternClass1
                .Builder(12, 8)
                .c(100)
                .d(200)
                .e(300)
                .build();
    }

}
```

우선 특징 몇 가지를 살펴보자.<br>
<br>

1. Builder 를 매개변수로 하는 생성자 메서드가 private 으로 선언되어 있다는 점<br>
2. 정적 중첩 클래스로 Builder 로 정의되어 있다는 점<br>
3. 모든 멤버 변수가 final 로 선언되어 함부로 변경할 수 없는 불변객체가 되었다는 점<br>
4. Builder 클래스 내에 build 메서드가 상위 클래스의 생성자 메서드를 호출한다는 점 (의존성이 강하다.)<br>

<br>
우선 3번 특징에 의하여 한 번 만들어진 BuilderPatternClass1 클래스의 인스턴스는 <b>불변</b>이다.<br>
그리고 한 번 인스턴스를 생성할 때 메서드를 이어지듯 연결하여 사용할 수 있는데, 이를 <b>플루언트 API (Fluent API)</b>, 혹은 <b>메서드 연쇄 (Method Chaining)</b> 이라고 한다.<br>
그리고 1번, 2번, 4번의 특징이 섞인 것이 내가 예시로 놓은 MainClass 클래스의 main 메서드에서 새로 인스턴스를 얻는 과정의 그것이다.<br>
직접 new 예약자에 의해 호출되는 것은 정적 중첩 클래스 내부의 Builder 메서드 이다.<br>
그 후 1번의 특징처럼 private 으로 선언된 멤버 변수를 메서드를 통해 수정하는 모습을 보인다.<br>
그리고 build 메서드를 통해 private 으로 정의되고 Builder 를 매개변수로 받는 BuilderPatternClass1 클래스의 생성자 메서드를 호출한다.<br>
이로서 우리는 인스턴스를 얻을 수 있다.<br>
<br>
자바의 빌더 패턴은 파이썬과 자바스크립트의 선택적 매개변수를 따라한 것이라고 한다.<br>
하단은 자바스크립트의 선택적 매개변수의 예시 코드이다.<br>

```javascript
function choosableParameter(a, b = {}, c = null) {
    ...
}
```

아이템 2의 마지막 부분에 재귀적 타입 한정을 이용하는 클래스를 구현했는데, 이는 정리할 시간이 조금 부족할 것 같아 생략한다.<br>
<br>

> 아이템 2에서 획득한 키워드<br>
> <br>
> <b>공변반환 타이핑 (covariant return typing)</b><br>
> 하위 클래스의 메서드가 상위 클래스의 메서드가 정의한 반환 타입이 아닌 그 하위 타입을 반환하는 기능.<br>
{: .prompt-tip }

---

## ● 아이템 3: private 생성자나 열거 타입으로 싱글턴임을 보증하라,
## ＋ 아이템 4: 인스턴스화를 막으려거든 private 생성자를 사용하라

<br>
4번 아이템은 내용이 많지 않기도 하고, 싱글턴에도 적용할 수 있어 합쳤다.<br>
우선 자바빈으로 등록하지 않고 싱글턴을 만들 수 있는 방법을 몇 가지 나열하겠다.<br>
<br>
public static final 필드 방식의 싱글턴<br>

```java
public class FirstSingletonClass {
    
    public static final FirstSingletonClass INSTANCE = new FirstSingletonClass();
    private FirstSingletonClass() {
        System.out.println("이거 실행되면 안되는데 FirstSingletonClass");
    }

}
```

우선 기본 생성자 메서드를 private 으로 정의한 상태이다.<br>
그리고 인스턴스는 static final 예약자를 통하여 싱글턴임을 보장하는 방식이다.<br>
<br>
하지만 이 방법은 리플렉션 API인 AccessibleObject.setAccessible 을 사용해 private 생성자가 호출될 수 있다.<br>
<br>
적용 예시<br>

```java

public class FirstSingletonMainClass {
    
    public static void main(String... args) {
        FirstSingletonClass firstSingletonClass = FirstSingletonClass.INSTANCE;
        firstSingletonClass.setString("무야호~");
        firstSingletonClass = FirstSingletonClass.INSTANCE;
        System.out.println("그만큼 신나신 거지~ " + firstSingletonClass.getString());

        // private 생성자를 reflect 를 통해 접근하기
        FirstSingletonClass isNotAgreedInstance = null;
        
        try {
            // 클래스를 제네릭 와일드카드를 통해 가져오기
            Class<?> targetClass = FirstSingletonClass.class;
            
            // 파라미터가 없고 private 으로 선언된 기본 생성자를 가져오기(NoSuchMethodException 발생 가능(메서드가 없는 경우 예외 던짐))
            Constructor<?> privateConstructor = targetClass.getDeclaredConstructor();
            
            // setAccessible 메서드를 통해 private 등의 접근 제한자로 막힌 메서드의 접근을 허용 - 이 부분이 책에서 설명한 내용
            privateConstructor.setAccessible(true);
            
            // newInstance() 메서드를 통해 Object 를 반환할 때, 명시적 박싱을 통해 원하는 클래스로 형변환
            // InvocationTargetException: invoke 시에 메서드에서 발생한 exception 이 마치 invoke 구문에서 발생한 것처럼 보이기 때문에 InvocationTargetException 자체의 stack trace 만으로 에러를 해결하기 어렵다.
                // 그래서 Wrapping 한 예외 클래스이다.
            // InstantiationException: newInstance() 메서드를 통해 인스턴스 생성 시에, 지정된 객체의 인스턴스를 생성할 수 없는 경우에 throw
            // IllegalAccessException: 허가되지 않은 객체, 메서드, 필드 등에 접근하려 할 경우 발생
            isNotAgreedInstance = (FirstSingletonClass) privateConstructor.newInstance();
            
            // 접근성을 되돌려 코드 무결성을 유지
            privateConstructor.setAccessible(false);
        } catch (NoSuchMethodException noSuchMethodException) {
            // 이건 사실 추천하지 않는 방법이다.
            // 로그에 정확한 예외 발생 위치가 노출되기 때문에 보안에 위협이 될 수 있다.
            noSuchMethodException.printStackTrace();
        } catch (InvocationTargetException invocationTargetException) {
            invocationTargetException.printStackTrace();
        } catch (InstantiationException instantiationException) {
            instantiationException.printStackTrace();
        } catch (IllegalAccessException illegalAccessException) {
            illegalAccessException.printStackTrace();
        }

        System.out.println("firstSingletonClass: " + System.identityHashCode(firstSingletonClass));
        System.out.println("isNotAgreedInstance: " + System.identityHashCode(isNotAgreedInstance));
        System.out.println("firstSingletonClass.getString: " + firstSingletonClass.getString());
        System.out.println("isNotAgreedInstance.getString: " + isNotAgreedInstance.getString());
    }
}

```

> 이거 실행되면 안되는데 FirstSingletonClass<br>
> 그만큼 신나신 거지~ 무야호~<br>
> 이거 실행되면 안되는데 FirstSingletonClass<br>
> firstSingletonClass: 209813603<br>
> isNotAgreedInstance: 312116338<br>
> firstSingletonClass.getString: 무야호~<br>
> isNotAgreedInstance.getString: null<br>

<br>
그리고 두 번째 방식은 INSTANCE 를 private 으로 선언하고 해당 객체를 호출하는 메서드를 추가한다.<br>
하지만 해당 방법도 리플렉션에 안전하지 않은 상태이다.<br>
<br>
쓰레드에 안전하며 가장 효율적인 방법은 enum 을 활용하는 것이라고 한다.<br>

---

그리고 아이템 4에서 정말 물어보고 싶은 개념이 나왔다.<br>
<br>
java.util.Collections 처럼 특정 인터페이스를 구현하는 객체를 생성해주는 정적 메서드 (혹은 팩터리) 를 모아놓을 수도 있다..<br>
<br>
이게 뭐야<br>
동료랑 상의해도 정확한 설명을 모르겠다.<br>
뭘까...<br>

＋ @<br>
이것도 질문 추가해야지ㅎㅎㅎ<br>

## ● 아이템 5: 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

만약 특정 클래스에 의존하는 멤버 변수가 있다고 해보자.<br>
특히 책에서는 문자열 확인을 위해 바탕이 되는 참조 변수를 멤버 변수로 선언하여 예시를 든다.<br>
<br>
이러한 객체는 클래스 별로 직접 구현하는 방법도 있지만 여러 곳에서 각 객체 별로 구현되는 경우도 있고, 직접적으로 사용되지 않는 경우도 있기에 초기화를 하는 방식에 차등을 주어 구현하고자 한다.<br>
바로 정적 유틸리티 클래스나 싱글턴 방식으로 구현하는 것.<br>
하지만 이렇게 사용하는 자원에 따라 동작이 달라지는 클래스에는 <b>정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.</b><br>
이 보다는 인스턴스를 획득할 때 생성자에 필요한 자원을 넘겨주는 방식이 낫다.<br>
즉, <b>DI</b> 를 활용하는 편이 낫다고 이야기 한다.<br>

```java
public class SpellChecker {

    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    ...
}
```

이러한 패턴을 <b>의존 객체 주입 패턴</b> 이라고 한다.<br>
토비의 스프링에서는 이를 활용한 패턴 중 하나로 <b>팩터리 메서드 패턴</b>을 이야기 하기도 했고 책에서도 짧게 설명을 한다.<br>
특히 DI에 의한 프레임워크라고 하는 스프링에서는 굉장히 중요한 개념이 될 것이다.<br>
<br>

> 핵심 정리<br>
><br>
> 클래스가 내부적으로 하나 이상의 자원에 의존할 때,<br>
> 그 자원이 클래스의 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다.<br>
{: .prompt-tip }

---

## ● 아이템 6: 불필요한 객체 생성을 피하라

이번 아이템에서는 deprecated 된 문법을 몇 가지 소개한다.<br>

```java

String s = new String("bikini");

Boolean b = new Boolean(true);

```

우선 문자열의 경우 리터럴로 초기화된 문자열에 기반에 본인만의 저장소를 운영한다.<br>
또, 논리 자료형의 경우 생성자 대신 Boolean.valueOf(String) 팩터리 메서드를 사용하는 것이 좋다.<br>
그리고 생성 비용이 아주 비싼 경우도 있는데,<br>

```java
static boolean isRomanNumeral (String s) {
    return s.matches("^...");
}
```

우리가 java에서 문자열의 정규표현식을 활용해 테스트를 진행할 때,<br>
자주 사용하는 것이 있는데 바로 Pattern 인스턴스이다.<br>
문자열의 matches 를 통해 내부적으로 인스턴스를 획득하게 되는 Pattern 은 인스턴스를 획득할 때마다 <b>유한 상태 기계</b>를 만들기 때문에 인스턴스 획득의 비용이 높다.<br>
<br>

> <b>유한 상태 기계?</b><br>
><br>
> 특정 조건들에 의거하여 성능을 끌어올린 애플리케이션<br>
> <br>
> 
> 1. 유한한 상태를 가진다.<br>
> 2. 유한한 입력을 가진다.<br>
> 3. 초기값을 가진다.<br>
> 4. 입력에 따라 상태가 변화하는 상태 전이 함수가 존재한다.<br>
>
> <br>
> 입력 별 유한한 상태를 계산, 제한하여 이를 상수화 해놓고 애플리케이션의 성능을 끌어올리는 구현 방식을 유한 상태 기계라고 한다.<br>
{: .prompt-tip }

그렇다면 이렇게 인스턴스 획득 비용이 비싼 경우 어떤 방식이 구현하기에 좋을까?<br>
책에서는 정적 상수형 변수로 선언, 초기화 하여 사용될 때마다 재사용 되는 것을 노렸다.<br>
그리고 인스턴스의 획득 자체가 비싸니 정적으로 상시 올려놓는 것이 아닌 <b>지연 초기화 (Lazy Initialization)</b> 를 통해, 획득 자체를 미뤄두다가 첫 호출 시 인스턴스를 획득하게 하는 것을 제안한다.<br>
하지만 지연 초기화는 코드를 복잡하게 만들 뿐 아니라 성능의 효율성을 크게 높이지 못 하는 경우가 많아서 추천하지 않는다.<br>
<br>
그래서 내가 생각한 방식은 이렇다.<br>
클래스 주체가 본인의 변수를 확실하게 알고 있다는 문제점을 고려했다.<br>
만약 해당 정규표현식을 비슷한 계층의 다른 서비스 로직, 도메인 에서도 활용해야 한다면, 상위 계층의 로직에서 DI 주는 방식도 고민해볼 것이다.<br>
<br>
또 책에서는 피해야할 안티 패턴을 몇 설명한다.<br>
<b>오토박싱 (Auto Boxing)</b> 을 예로 들었다.<br>

```java
public static void main(String... args) {
//    Long sum = 0L;    // takes: 5.997 sec
    long sum = 0L;    // takes: 0.627 sec

    for (long i = 0L; i < Integer.MAX_VALUE; i++) {
        sum += i;
    }
}
```

위의 로직에서 Wrapper 클래스인 Long 을 사용하면 반복문의 기본 자료형으로 선언된 i와 다른 자료형으로 인해 반복문의 바디가 동작할 때마다 오토 박싱을 진행하게 된다.<br>
그로인한 실행 시간은 내가 주석으로 남긴 부분과 같다.<br>
정말 어마어마하게 오래걸리더라..<br>
<br>

> 아이템 6 에서 획득한 키워드<br>
><br>
> 어댑터 패턴: 관계가 없는 인터페이스 들을 연결하는 패턴이다.<br>
> 두 인터페이스를 구현해야 하는 상황이라면 하나의 인터페이스는 implements 하여 구현을 강제한다. (정적 메서드로 선언한 것 제외)<br>
> 그리고 오버라이딩 된 메서드에서 다른 인터페이스의 메서드를 호출한다.<br>
> 이렇게 연관이 없던 두 인터페이스 간의 연결이 구현으로 인해서 이루어지는 패턴이다.<br>
{: .prompt-tip }

---

## ● 아이템 7: 다 쓴 객체 참조를 해제하라

이번 아이템에서는 초반에 Stack 을 구현하는 코드를 보여주고 독자에게 메모리 누수가 일어날 법한 곳을 찾아보라고 한다.<br>
당신은 단 번에 찾았는가?<br>
<br>
<i>난 답지 보고 알아따...</i><br>
<br>
책에서 Stack을 구현할 때 pop 메서드에서 제거된 객체의 참조를 그대로 두었다.<br>

```java
public Object pop() {
    if (size == 0) {
        throw new EmptyStackException();
    }

    // 이 부분에서 elements[--size] 에 해당하는 객체를 따로 선언, 초기화 하여 저장하고,
    // Stack 자료형을 이루는 객체의 집합인 elements 에서 인덱스에 의한 참조를 해제해줘야 한다.

    return elements[--size];
}
```

책에서는 위의 주석 부분에서 참조를 해제해줘야 하는 구문에 한해 인덱스가 가리키는 값을 <b>null 로 처리</b> 한다.<br>
하지만 이렇게 null 로 처리하는 일은 예외적인 경우여야 하며 다 쓴 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위 (scope) 밖으로 밀어내는 것이라고 한다.<br>
그리고 이를 간단하게 설명하기 위해 <b>WeakHashMap</b> 을 이야기 하는데, 궁금해서 직접 구현해봤다.<br>

```java
public class MainClass {
    
    public static void main(String... args) {

        WeakHashMap<String, String> map1 = new WeakHashMap<>();
        Map<String, String> map2 = new HashMap<>();

        String key1 = "리터럴1";                           // 이건 weak reference 의 영향을 받지 않는다.
        String key2 = new String("new 연산자");
        
        map1.put(key1, "100");
        map1.put(key2, "200");
        
        map2.put(key1, "100");
        // map2.put(key2, "200");                              // 이 주석을 풀면 put으로 key2의 참조가 다시 생긴다.
                                                            // 즉, 다른 객체에 다시 참조를 건다면 WeakHashMap 의 참조도 잃지 않고 유지된다.
        
        key1 = null;
        key2 = null;
        
        System.gc();

        System.out.println("map1: " + map1);
        System.out.println("map2: " + map2);

    }

}
```

> map1: {new 연산자=200, 리터럴1=100}<br>
> map2: {new 연산자=200, 리터럴1=100}<br>

<br>
map1에 <br>
이렇게 출력이 된다.<br>
이유는 <b>WeakHashMap</b> 이 <b>약한 참조 (Weak Reference)</b> 를 이용하기 때문이다.<br>
약한 참조란 참조를 하는 <b>본인이 GC 의 대상</b>이 될 수도 있다는 것을 의미한다.<br>
그래서 위에서 구현한 소스를 보면 WeakHashMap 인 map1에 put된 key2를 null로 하고 gc를 강제로 한 것을 보면 map1 안에서 해당 데이터가 사라진 것을 확인할 수 있다.<br>
하지만 이는 리터럴로 선언된 String 타입의 키에서는 적용되지 않고, 본인과 같은 키 값을 강한 참조 (String Reference) 하는 객체가 있다면 null로 처리해도 약한 참조 또한 사라지지 않는다.<br>

---

## ● 아이템 8: finalizer와 cleaner 사용을 피하라,
## ＋ 아이템 9: try-finally 보다는 try-with-resources 를 사용하라

자바에서는 객체 소멸자로 <b>finalizer</b> 와 <b>cleaner</b> 를 제공한다.<br>
그 중 finalizer는 예측할 수 없고, 상황에 따라 위험할 수도 있으며 GC의 대상에서 우선순위도 밀린다.<br>
저자는 실제로 finalizer를 구현한 동료가 OOM을 뱉으며 죽는 애플리케이션을 뜯어보자 finalizer로 객체 소멸을 구현하였음에도 <b>쓰레드 상 우선순위에 밀려 (이거 확인하자)</b> 수 많은 객체가 제대로 gc 되지도 못하고 서버를 터트린 이야기를 해준다.<br>
심지어 finalizer 에서 예외가 발생하면 경고초자 출력하지 않는다.<br>
<br>
반면에 cleaner 는 자신을 수행할 스레드를 제어할 수 있다는 점에서 조금 낫다.<br>
하지만 여전히 백그라운드에서 수행되고 GC의 통제하에 있으니 즉각 수행되기는 보장할 수 없다.<br>
<br>
그럼에도 우리가 객체 소멸자를 구현해야 하는 상황을 두 가지 설명한다.<br>

1. 클라이언트가 객체 소멸 로직을 아예 구현하지 않아서 하긴 해야할 때(?)<br>
2. <b>네이티브 피어(객체)</b> 에 대하여 객체가 소멸해야 함을 알릴 때<br>

1번은.. 뭐 실수로 까먹었다고 치자.<br>
2번은 조금 중요한 대목이다.<br>
<br>
<b>네이티브</b>란 키워드는 프로그래밍을 하다가 보면 자주 접할 수 있는 단어이다.<br>
보통 <b>"다른 언어의"</b> 라는 의미를 지니며 JPA 상에서도 네이티브 쿼리를 통해 기존 JPA 문법 상 맞는 쿼리가 아닌 이용하는 DB에 맞춘 쿼리를 작성할 수 있게 하는 기능도 있었다.<br>
이렇게 다른 언어로 작성된 객체같은 경우는 JVM의 GC가 정상적으로 자원을 회수하지 못할 가능성이 있다.<br>
이를 해결하기 위해 <b>cleaner 를 구현하고 try-with-resources 로 회수</b>하는 것이 좋다.<br>

```java
import java.lang.ref.Cleaner;

public class Office implements AutoCloseable {
    
    private static final Cleaner cleaner = Cleaner.create();
    
    // 청소가 필요한 자원, 절대 Office 를 참조해서는 안 된다.
    private static class State implements Runnable {
        // 쓰레기 개수
        int numJunkPiles;
        
        State(int numJunkPiles) {
            this.numJunkPiles = numJunkPiles;
        }
        
        // close 메서드나 cleaner 가 호출한다.
        @Override
        public void run() {
            System.out.println("사무실 청소");
            numJunkPiles = 0;
        }
    }
    
    // 사무실의 상태, cleanable 과 공유한다.
    private final State state;
    
    // cleanable 객체, 수거 대상이 되면 사무실을 청소한다.
    private final Cleaner.Cleanable cleanable;
    
    public Office(int numJunkPiles) {
        state = new State(numJunkPiles);
        cleanable = cleaner.register(this, state);
    }
    
    @Override
    public void close() {
        System.out.println("close()");
        cleanable.clean();
    }
}

...

public static void main(String... args) {
    
    try(Office office = new Office(10)) {
        System.out.println("office.toString(): " + office.toString());
    } catch (Exception e) {
        e.printStackTrace();
    }

    System.out.println("--------------------------------------------");
    
    // 일반 try-catch 를 이용하면 Runnable 을 구현한 state 의 run 이 동작하지 않는다.
    // 이는 AutoCloseable 를 구현한 취지에 맞지 않는 행동이기 때문이다.
    Office office = new Office(10);
    
    try {
        System.out.println("office.toString(): " + office.toString());
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        office.close();
    }
}
```

> office.toString(): item8finalizeAndCleaner.Office@2ff4f00f<br>
> close()<br>
> 사무실 청소<br>
> --------------------------------------------<br>
> office.toString(): item8finalizeAndCleaner.Office@7d417077<br>
> close()<br>
> 사무실 청소<br>

<br>
<b>와, 또 신기한 사실을 알아냈다.</b><br>
위의 코드는 책에서 설명한 cleaner를 구현한 클래스이고 이를 활성화하기 위해 try-with-resources 를 구현한 메인 메서드 이다.<br>
위 코드는 자바 11 환경에서 구현했던 코드였다.(사무실에서 짬이 날 때 작성한 코드이다.)<br>
<b>자바 11</b>에서는 AutoCloseable, Runnable 로 구현된 <b>run 메서드가 분명 동작하지 않았었다.</b><br>
이를 위해 구현된 소스임을 분명 확인하고 내가 직접 구현했으니 기억한다.<br>
하지만 이 포스팅을 위해 다시 소스를 실행한 후 깜짝 놀랐다.<br>
<b>자바 17 에서는 AutoCloseable, Runnable 로 구현한 run 메서드도 동작한다.</b><br>
이게 무슨 일인가<br>
jdk 버전 별로 차이가 많을 수 있다는 것은 알지만 최적화가 이렇게 이루어질 수도 있구나<br>
아이템 8과 9는 내가 다루기에 아직 지식이 모자른 것 같다.<br>
우선 jdk 11과 비교해가며 포스트를 수정해야 할것 같다.<br>

<b>＋ @</b><br>
집에서 확인하니 jdk 11도 run 메서드가 동작하는 것을 알 수 있다.<br>
jdk 8과 비교해보자.<br>
<i>추가: 2023-08-15 08:54</i><br>
<br>
<b>＋ @</b><br>
집에서 확인하니 jdk 11도 run 메서드가 동작하는 것을 알 수 있다.<br>
jdk 8과 비교해보자.<br>
<i>추가: 2023-08-15 08:56</i><br>
<br>
<b>＋ @</b><br>
Cleaner 클래스는 자바 9 부터 추가되었다..<br>
도대체 사무실에서는 동작하지 않았던 run 메서드가 왜 동작하는 걸까..<br>
이게 스레드에서 우선순위로 탈락한 걸까..<br>
<i>추가: 2023-08-15 08:58</i><br>
<br>

> 아이템 8 에서 획득한 키워드<br>
> <br>
> System.runFinalizersOnExit, Runtime.runFinalizersOnExit?<br>
> 위 두 메서드는 수집 대상 인스턴스의 finalize 를 동작시킨다.<br>
> 하지만 스레드에 안전하지 않아 다른 객체에서 동시에 수집 대상 인스턴스를 참조하기 시작하면 교착이 발생할 수 있다.<br>
{: .prompt-tip }

---

## ● TIL

우선, 한번에 아이템 9개 정리하는건 너무 힘들다.<br>
단순 코딩도 아니고 이론과 키워드로 꽉찬 책이다.<br>
그래도 재밌긴 하니 분량을 정해놓고 마음 편히 정리하는게 앞으로 좋을 것 같다.<br>
<br>
그리고 저 Cleaner는 도대체 왜 특정 환경에서 동작하지 않은건지 너무 궁금하다.<br>
예상에는 스레드 상 우선순위 탈락인데..<br>
다른 프로젝트도 실행되어 있는 것이 Cleaner 가 구현된 클래스가 GC 의 대상이 되는데 영향이 있던걸까..<br>