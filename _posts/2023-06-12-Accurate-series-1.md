---
title: 1장 유튜버 쉬운코딩 님의 백발백중 시리즈
author: park
date: 2023-06-12 09:36:00 +0800
categories: [CS, 백발백중 시리즈(2023-06 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
image: 
  path: /assets/img/02.Accurate-series/00.main.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

## 1장 유튜버 쉬운코딩 님의 백발백중 시리즈

> 유튜브: &nbsp;&nbsp;[**백발백중 시리즈**](https://youtube.com/playlist?list=PLcXyemr8ZeoT-_8yBc_p_lVwRRqUaN8ET)
{: .prompt-tip }

<br/>

---
<br/>

### 소제목 목록
1 ~ 4 편
● 1. 객체와 클래스 완벽 설명!!
● 2. 변수와 값!
● 3. 함수!!
● 4. 변수와 객체와 메모리의 관계!

<br/>

---

<br/>

## ● 1. 객체와 클래스 완벽 설명!!

1편, 객체와 클래스의 완벽 설명 에서는 객체와 클래스의 정의, 정의에 따른 보편적 해석을 위주로 다룬다.
쉽게 정리해보자.<br/>
<br/>
객체(Object)란?<br/>
<b>실체가 존재하는 현실의 사물</b>이라 할 수 있다.<br/>
해당 객체에게 의존하는 <b><span style="color: red;">상태</span>와 <span style="color: red;">행동</span></b>을 가지고 있으며, 객체지향 프로그래밍의 꽃이다.<br/>
예를 들어, 아반떼라는 자동차 중 하나인 객체가 있다면 정지, 주행 등의 상태가 있으며, 해당 상태를 변경하는 행동인 출발, 제동 등이 있다.<br/>
<br/>
<b>클래스(Class)는 그런 객체를 정의한 정의서</b>라고 할 수 있다.<br/>
영상에서는 일종의 Type 이라고 표현한다.<br/>
자동차라는 Type을 실체화 하면 객체가 된다는 것이다.<br/>
<br/>
코드로 예시를 들어보자.<br/>

---

```java
public class Car {
    private String name;
    private String status;

    public Car() {}

    public Car(String name) {
        this.name = name;
    }

    public void start() {
        ...
    }

    public void stop() {
        ...
    }
}

...

public class CarController {
    public void carMethod() {
        Car myCar = new Car("아반떼");
        Car yourCar = new Car("소나타");
    }
}
```

상단의 코드를 보면 Car 라는 이름의 클래스를 정의한 것이 보인다.<br/>
그리고 안에 name, status 라는 속성을 정의했고, start(), stop() 등의 행동이 있다.<br/>
<br/>
<b>name, status 는 인스턴스 변수로 실체화 될 객체의 상태</b>를 나타낸다.<br/>
<b>start(), stop() 은 메서드로 실체화 될 객체의 행동에 의존</b>된다.<br/>
<br/>
그리고 CarController의 carMethod() 에서 보면 new 키워드와 생성자를 이용해서 객체 두 개를 만드는 것을 확인할 수 있다.<br/>
속성은 name으로 하나에 값을 할당하지만, 저 마다 다른 값을 할당하여 다른 객체 두 개를 선언, 초기화 해준다.<br/>
<br/>
즉, <b>같은 클래스(Type)를 이용해서 다른 객체(Object) 두 개를 만든 것</b>이다.<br/>
<br/>

---
<br/>

## ● 2. 변수와 값!

우선 코드부터 보고 시작하자.

```java

int age = 29;
boolean success = true;
Car myCar = new Car("아반떼");

```

> 객체는 값(데이터)을 가리키는 주소 값을 가지고 있는 그릇이다.<br/>
> 그리고 그 값은 특정 예약어를 사용하는 것이 아니라면 재 할당 가능하다.<br/>
{: .prompt-info }

```java

int age = 29;
age = 28;

```

```python
age = "이제 곧 30.."
```

처럼 값을 바꿔 담을 수 있다.<br/>
특히 객체지향 언어인 Python은 우리에게 익숙한 Java와 다르게 객체의 타입에 의존하지 않고 데이터를 할당할 수 있다.<br/>
<br/>
그러니 어떤 변수에 <b>어떤 값이 들어갈 지 제대로 파악하고 <span style="color: red;">변수의 명</span>을 짓는 것이 중요</b>할 것이다.<br/>
<br/>

---
<br/>

## ● 3. 함수!!

> 위 영상을 시청하기 전, 댓글을 보면 유튜버 쉬운코딩님의 첨언이 있다.<br/>
> 영상 설명 상, 함수라고 표현한 부분을 메서드(Method)와 다르게 이해하면 될 것이다.<br/>
> <b>OOP(객체 지향 프로그래밍, Object Orient Programming) 의 관점에서 처음 등장한 용어인 Method는 <span style="color: red;">객체에 종속되어 동작하는 액션</span>을 의미</b>한다.<br/>
> 하지만 <b style="color: red;">함수는 개별로 존재</b>하여 동작하는 액션을 의미한다.<br/>
> 라고 댓글에 남겨 주셨으니 잘 기억해두고 영상을 시청하면 좋다.<br/>
{: .prompt-tip }

---

영상에서는 초반에 코드 두 개를 준다.<br/>

```python
class Adder:
    def add(self, a, b):
        return a + b

와

def add(a, b):
    return a + b
```

위 두 코드 중에 함수는 어떤 것일까? 로 영상의 시작을 알린다.<br/>
<br/>

### 함수(Function)란?
- <span style="color: red;">독립적으로 존재</span>하며 임무(task)를 수행하는 코드들의 집합
- 함수 이름으로 호출한다.
- 매개 변수(Parameter)를 받을 수도 받지 않을 수도 있다.
- 결과 값을 리턴할 수도 하지 않을 수도 있다.
- 재사용이 가능하다.

```python
def add(a, b);
    return a + b

# 이렇게 정의된 함수는

result = add(1, 2)
```

> 이렇게 <b>함수명으로 호출</b> 가능하며 결과 값을 result 라는 변수에 할당 가능하다.<br/>
> 또 다른 예시를 보면<br/>
{: .prompt-info }

```python
def checkBloodPressure(bloodPressure):
    if bloodPressure <= 90:
        return "low"
    if bloodPressure >= 140:
        return "high"
    return "normal"

print(checkBloodPressure(87))
print(checkBloodPressure(120))
```

> low<br/>
> normal<br/>
{: .prompt-info }

위처럼 Parameter 를 이용해 결과 값을 만들어 낼 수 있고 그 결과 값을 반환할 수 있다.<br/>
그리고 이렇게 함수를 <b>SRP</b> 개념을 접목하여 잘 정의하면 <b>중복되는 코드를 줄일 수</b> 있으며, <b>재사용하기 쉬운 장점</b>이 있다.<br/>
<br/>

---
<br/>

### 메서드(Method)란?
- <span style="color: red;">객체(Object) 혹은 클래스(Class)에 종속</span>되어 임무(task)를 수행하는 코드들의 집합
- 클래스나 객체의 상태 정보에 접근도 가능
- 메서드 이름으로 호출한다.
- 매개 변수(Parameter)를 받을 수도 받지 않을 수도 있다.
- 결과 값을 리턴할 수도 하지 않을 수도 있다.
- 재사용이 가능하다.

```python
class Adder:
    def add(self, a, b):
        return a + b

#로 정의된 메서드를

adder = Adder()
result = adder.add(1, 2)
```

> 클래스를 객체화하고 메서드 명을 호출하여 사용하고 결과 값을 result 라는 변수에 할당 가능하다.<br/>
{: .prompt-info }
<br/>

---
<br/>

### 종속이란?

```java
class Counter {

    private int count = 0;

    public void increment() {
        count++;
    }

    public int get() {
        return count;
    }
}

public class CounterController {

    public void counterMethod() {

        Counter c1 = new Counter();
        c1.increment();
        c1.increment();
        int result1 = c1.get();
        // result1 == 2

        Counter c2 = new Counter();
        c2.increment();
        int result2 = c2.get();
        // result2 == 1;

    }
}
```

위처럼 객체에 정의된 메서드를 호출함으로 객체 내 정의된 속성 값을 수정했다.<br/>
그리고 get() 메서드를 이용해서 result1, result2 에 값을 초기화 해주었는데, 이 결과 값 들은 같은 클래스에 정의된 같은 속성 값을 가져오지만,<br/>
서로 다른 객체에 의존하는 속성, 메서드에 의존하여 각 값은 2, 1 로 다르게 초기화 된다.<br/>
<br/>

> 그리고 코드를 보면 count라는 변수를 메서드 명에 속하지 않게 한 것이 눈에 띈다.<br/>
> 즉, 변수 명명규칙을 확실하게 잡은 것.<br/>
> 초급 개발자들이 흔히 하는 과정인데, 위의 메서드 명을 <br/>
> <br/>
> incrementCount()<br/>
> getCount()<br/>
> <br/>
> 로 정의된 인스턴스 변수의 변수명을 추가하는 개발자가 많다.<br/>
> <br/>
> 이는 보통 VO 객체 등에 선언된 인스턴스 변수들을 접근자, 수정자를 통해 각 변수에 값을 할당하고 가져오게 하는 과정에 생긴 습관으로,<br/>
> 위처럼 도메인 설계가 확실한 상황이면 그런 변수명은 빼주는 것이 더 명확하게 설계할 수 있다.<br/>
> 소소하게 유튜버 쉬운코드 님의 실력을 알 수 있는 부분이었다.<br/>
{: .prompt-tip }
<br/>

이제 영상 초반에서 보여준 코드를 다시 보면<br/>

```python
class Adder:
    def add(self, a, b):                # -> 이건 메서드
        return a + b

# 와

def add(a, b):                          # -> 이건 함수
    return a + b
```

> 바로 구분 지을 수 있게 된다.<br/>
{: .prompt-info }
<br/>

---
<br/>

## ● 4. 변수와 객체와 메모리의 관계!
<br/>
오늘 정리할 4편 중 가장 긴 길이의 영상이다.<br/>
정리할 양이 조금 될 것으로 예상된다.<br/>
<br/>

---
<br/>

### 애플리케이션은 어떻게 실행되는가?

- 애플리케이션: 일반 사용자가 사용할 기능을 제공하는, 컴퓨터가 실행할 수 있는 명령어의 집합
- 메모리: 실행한 애플리케이션이 상주하는 곳
- cpu: 명령어를 실행하는 주체
<br/>

### 메모리 구조
- 애플리케이션에 할당되는 메모리는 내부적으로 여러 영역으로 나뉨
- 그 중에 stack 메모리와 heap 메모리가 있음
- stack 메모리: 함수나 메서드의 지역 변수(local variable)와 매개 변수(parameter)가 저장됨,
    함수나 메서드가 호출될 때 마다 스택 프레임(stack frame)의 형태로 적재된다.
- heap 메모리: 객체가 저장됨

---

### <span style="color: red;">Stack 메모리</span>

```java

public class Main {

    public static main(String[] args) {

        int a = 7;
        int b = 3;
        int c = a + b;

    }
}
```

양동이 처럼 생긴 Stack 메모리 안에 Stack frame이 잡힌다.<br/>
상단의 코드가 있다면, 파라미터인 args가 먼저 Stack 메모리에 있는 main 메서드의 Stack frame에 적재된다.<br/>
그 후, a가 7로 ,b가 3으로, c가 그 둘을 합친 10이 main 메서드의 Stack frame에 적재가 된다.<br/>

> 그리고 main 메서드의 파라미터인 args는 Java 애플리케이션을 실행하면서 추가적인 Input 데이터를 받기 위해 만들어진 파라미터이며, 이번 영상에서는 사용하지 않을 것이라고 한다.

```java
public class Main {

    public static main(String[] args) {
        
        int a = 100;
        a = wow(a);

    }

    public static int wow(int num) {
        int b = num * 4;
        return b;
    }
}
```

1. main 메서드의 Stack frame에 args가 적재되고 a가 100의 값으로 적재된다.<br/>
2. 호출한 wow() 메서드에 대한 Stack frame이 Stack 메모리에 잡힌다.<br/>
  ㄴ 양동이 안에 큰 틀이 두개 들어있는 것이며, main 메서드의 것이 밑에 적재되고 이후에 호출된 wow 메서드의 틀이 위에 적재된다.<br/>
3. 이제 wow 메서드의 Stack frame에 파라미터인 a가 num이라는 변수로 Stack frame에 적재된다.<br/>
4. 그리고 4를 곱한 b가 400의 값으로 Stack frame에 적재된다.<br/>
5. 그리고 400의 값을 가진 b를 return 하며 wow 메서드의 Stack frame이 Stack 메모리 상에서 사라진다.<br/>
6. 그리고 main 메서드의 Stack frame에 있는 100의 값을 가진 a의 값이 400으로 변경된다.<br/>
<br/>

---
<br/>

### <span style="color: red;">Heap 메모리</span>

<br/>
우선, 쉬운 코드를 먼저 보고 설명할 것이다.<br/>
그 후, 코드를 조금 추가하고, 그림판을 이용해서 메모리에 어떻게 적재되는지 확인하며 진행할 것이다.<br/>

```java
public class Main {

    public static main(String[] args) {

        Counter c = new Counter();

    }

    public class Counter {
        private int state = 0;
        public void increment() { state++; }
        public int get() { return state; }
    }
}
```

위의 코드로 Stack, Heap 메모리를 설명해보자.<br/>
우선 변수의 종류를 나눠보자.<br/>

- args 라는 매개변수(파라미터) 가 있다.
- c 라는 Counter 클래스를 실체화한 지역변수가 있다.
- Counter 클래스 안에 state라는 인스턴스의 상태를 나타내는 인스턴스 변수(instance variable)가 있다.
<br/>

1. 우선 main 메서드를 실행하는 순간 main의 파라미터인 args가 Stack 메모리에 있는 main 메서드의 Stack frame 에 적재된다.<br/>
2. 그 후 바로 Counter의 Stack frame이 Stack 메모리에 적재된다.<br/>
3. 그리고 Counter의 state가 0의 기본 값을 가지고 heap에 적재된다.<br/>
4. 그 후, Stack 메모리에서는 Counter의 Stack frame이 지워진다.<br/>
5. 그리고 Stack 메모리의 main 메서드 Stack frame의 args 위에 c 가 적재가 된다.<br/>
6. 그 c는 아까 Heap 메모리 영역에 담김 Counter 를 실체화한 객체의 주소를 담게된다.<br/>

<br/>
글로 설명하니, 아무것도 모르고 이 글을 본다면 이해하기 정말 힘들 것이다.<br/>
이제 이미지를 첨부하여 진행하겠다.<br/>
<br/>
<b style="color: red;">4편은 사실 이 부분만 봐도 된다.</b><br/>
<br/>

```java
public class Main {

  public static void main(String[] args) {
      Counter c = new Counter();
      two(c);
      int count = c.get();
  }

  public static void two(Counter c) {
      c.increment();
      c.increment();
  }
}

public class Counter {
  private int state = 0;
  public void increment() { state++; }
  public int get() { return state; }
}
```

그리고.. 그림판의 등장.<br/>

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/711e2cff-4e06-4695-9d75-8e0c99a4d8a9)

```java
  public static void main (String[] args) {
    ...
```

> 가장 먼저 Stack 메모리에 main 메서드가 얹혀지고, main 메서드의 <b style="color: orange;">Stack frame</b>안에 파라미터인 <b>args</b>가 적재된다.<br/>
{: .prompt-info }

---


![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a2777db0-e5e7-4ea0-8c59-d6ea6920b977)

```java
  public static void main(String[] args) {
      Counter c = new Counter();
      ...
```

> 그리고 실체화 될 <b style="color: skyblue;">Counter 클래스의 생성자 메소드</b>가 Stack 메모리에 적재가 된다.<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/22f0166f-4f20-469a-a982-15ef379c4af7)

> 그리고 <b style="color: green;">Counter 객체는 heap 메모리</b>에 0이라는 state값을 가지고 적재 된다.<br/>
> <b style="color: skyblue;">생성자는 일종의 메서드</b>로, 인스턴스의 메서드를 호출하면 숨어있는 this 키워드(하늘색)가 Heap메모리의 Counter 가 실체화된 객체의 주소(초록색)를 가리키게 되며,<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/eef0114e-13d3-4291-ae37-6a89fcfef2c8)

> 그리고 this 가 가리키는 객체의 주소는 반환되어 <b style="color: orange;">c 라는 이름을 가진 <span style="color: green;">객체의 주소</span>를 가진 변수</b>로 main 메서드의 Stack frame에 적재가 된다.<br/>
> 그리고 본인의 동작이 끝난 <b style="color: skyblue;">생성자 메소드</b>는 Stack 메모리에서 제거된다.<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/6eaba577-a4b8-4a72-8538-987478d67a82)

```java
  public static void main(String[] args) {
    Counter c = new Counter();
    two(c);
    ...
```

> 그리고 나서 <b style="color: purple;">two 메서드</b>를 호출하여 Stack 메모리에 적재된다.<br/>
> two 메서드의 <b style="color: green;">매개변수인 Counter c</b> 는 <b style="color: purple;">two 메서드</b>의 Stack frame에 적재된다.<br/>
> <b style="color: green;">매개변수 c</b> 도 같은 주소를 가지고 heap 메모리의 <b style="color: green;">Counter 클래스의 객체</b>를 가리킨다.<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/550433eb-bf03-47a5-9ffc-3f70f4257669)

```java
  public static void main(String[] args) {
    Counter c = new Counter();
    two(c);
    ...
  }

  public static void two(Counter c) {
    c.increment();
    ...
```

> 루틴 1<br/>
> 그 후, two 메서드 내에서 다시 객체 c의 <b style="color: hotpink;">increment 메서드</b>를 호출하며 increment 메서드는 Stack 메모리에 적재된다.<br/>
> 그리고 인스턴스에 대한 메서드 호출이기에 increment 메서드의 Stack frame에는 this 키워드가 암묵적으로 동작하고, 이 this는 <b style="color: green;">객체 c</b>를 가리킨다.<br/>
> 그 후 increment 메서드의 바디 블록의 후위증감연산자(++) 로 인해 <b style="color: green;">객체 c에 선언된 state 속성</b>의 값이 ++ 된다.<br/>
> 증감 작업을 마치고 나서 <b style="color: hotpink;">increment 메서드의 Stack frame</b>이 Stack 메모리에서 제거된다.<br/>
> <br/>
> 위의 작업은 increment() 메서드를 두 번 호출하여 한 번 더 이루어진다.<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/1788d04f-436c-4b39-88a6-7231e0a866eb)

```java
  public static void main(String[] args) {
    Counter c = new Counter();
    two(c);
    ...
  }

  public static void two(Counter c) {
    c.increment();
    c.increment();
  }
```

> 이 <b style="color: hotpink;">increment 메서드 호출은 두 번</b>에 걸쳐 이어짐으로 위의 루틴 작업을 다시 동작한 후, Stack메모리 영역에서 제거된다.<br/>
> 그리고 ++가 두번 된 <b style="color: green;">객체 c가 가진 state 속성의 값은 2</b>가 된다.<br/>
> <br/>
> 그렇게 <b style="color: purple;">two 메서드 호출이 끝</b>나면서 <b style="color: purple;">two 메서드</b>의 Stack frame이 Stack 메모리에서 지워지고,<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4af5c0d3-b82e-4a76-b503-663a8f3a384b)

```java
  public static void main(String[] args) {
    Counter c = new Counter();
    two(c);
    int count = c.get();
  }

  public static void two(Counter c) {
    c.increment();
    c.increment();
  }
```

> <b style="color: green;">객체 c 의 get 메서드</b>를 호출하면 다시 Stack 메모리에 <b style="color: blue;">get 메서드</b>의 Stack frame이 적재되면서<br/>
> 인스턴스에 종속된 메서드를 호출하니 <b style="color: blue;">get 메서드의 Stack frame 안에 this</b> 가 암묵적으로 동작한다.<br/>
> 그리고 this.get 메서드가 호출되며 <b style="color: green;">객체 c</b>의 인스턴스 변수인 <b style="color: green;">state의 값을 return</b> 하게 된다.<br/>
{: .prompt-info }

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/6b4f3d79-09d5-4d42-ace4-bcd549008e24)

```java
public class Main {

  public static void main(String[] args) {
      Counter c = new Counter();
      two(c);
      int count = c.get();
  }

  public static void two(Counter c) {
      c.increment();
      c.increment();
  }
}

public class Counter {
  private int state = 0;
  public void increment() { state++; }
  public int get() { return state; }
}
```

> 즉, 2 가 반환되고, 다시 <b style="color: blue;">get 메서드</b>가 제거되며 반환된 값 2는 <b style="color: orange;">count</b>라는 변수 명으로 main 메서드의 Stack frame에 쌓이게 된다.<br/>
> 이렇게 이 간단한 코드의 메모리 영역의 설명을 간단하게 마쳤다.<br/>
{: .prompt-info }

---

### 쓰레기 객체(garbage object)

```java
public class Main {

  public static void main(String[] args) {
      Counter c = make();
  }

  public static Counter make() {
      Counter c = new Counter();
      return new Counter();
  }
}

public class Counter {
  private int state = 0;
  public void increment() { state++; }
  public int get() { return state; }
}
```
<br/>
위의 코드 중 make() 메서드를 보면 c 라는 객체를 new 키워드와 기본 생성자를 통해 생성 해준다.<br/>
<b style="color: red;">하지만 return 될 때 새로 생성해서 리턴</b>한다.<br/>
<br/>
이렇게 될 경우, make() 메서드가 종료되어 Stack 메모리 상에서 사라질 경우, <b>Heap 메모리 영역의 c 객체는 접근할 수 없는 객체</b>, 즉 <b style="color: red;">쓰레기 객체</b>가 되어버린다.<br/>
분명 메모리 상 존재는 하지만 이용할 수 없고 메모리 누수만 유발하는 객체인 것이다.<br/>
<br/>
이런 경우 보편적인 해결법은 두 가지 있는데<br/>

1. <b>프로그래머가 직접 객체의 메모리 할당을 해제</b>하거나<br/>
2. <b>Java</b>나 <b>Python</b> 같은 언어의 경우 <b>GC(Garbage Collector) 를 통해 자동으로 쓰레기 객체를 정리</b>하게 하여 메모리 누수를 피하고자 한다.<br/>
GC가 어떻게 동작하냐에 따라 애플리케이션의 성능이 갈리기도 한다.<br/>
<i style="font-size: 20px;">추후 자바의 신 도서와 doc에서 찾은 자료들로 GC도 정리할 것이다.</i><br/>
<br/>

---

<br/>
이렇게 정리하고 나니 어떤 편은 생각보다 키워드나 정리할 내용이 많고 어떤 편은 설명할 것도 없이 정말 간단하게 끝난다는 것을 알았다.<br/>
그래서 이후 편들은 시청하다가 중요하다고 판단될 경우에 정리하고자 한다.<br/>

<br/>