---
title: Oracle 의 메서드 네이밍 규약
author: park
date: 2023-09-12 18:10:00 +0800
categories: [Java, Programming]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## Method Naming Conventions

<br>
● 들어가기 전<br>
● 정리<br>
● 활용<br>
● 느낀점<br>
<br>

---

<br>

### ● 들어가기 전

<br>
업무를 할 때 Entity (이하, 엔티티) 의 개념을 적용하더라도 <span style="color: red;">엔티티를 그대로 반환</span>하는 경우가 많았다.<br>
내 공부가 부족했던 것이 컸고, 가공이 필요 없다는 생각도 있었으며, 기존 프로젝트의 코드 스타일도 그대로 유지하였다.<br>
'그냥 그렇게 하는 줄 알았다. 눈치를 봤다.'<br>
그리고 좋아하는 선배가 DTO 개념의 공부를 추천하면서, 어느정도 공부가 됐다고 생각했을 때 <span style="color: blue;">Numble 의 토이프로젝트</span> 개발에 참가한 적이 있다.<br>
<br>
호되게 혼났다.<br>
최근 추세인 DDD 에 맞지 않는 프로젝트 구성, 엔티티를 그대로 반환하는 구현의 목적, 메서드 네이밍에 관한 내 노력의 흔적 등<br>
많은 문제점을 발견했고 메이트들은 그걸 모두 짚어주었다.<br>
<br>
그걸 반환점으로 공부를 시작한 것도 있다.<br>
우선 오늘의 목표는 조회한 엔티티를 반환할 DTO 클래스를 공부 중 발견한 <b>메서드 명명규칙 규약</b>을 살피고 넘어가겠다.<br>
<br>

---

<br>

### ● 정리

[Oracle - Method Naming Conventions](https://docs.oracle.com/javase/tutorial/datetime/overview/naming.html)<br>

<br>

| Prefix       | Method Type | Use |
|:-------------|:-------|:--------|
| of | static factory    | Create an instance where the factory is primarily validating the input parameters, not converting them. |
|  |     | 번역: factory 에서 파라미터를 검사하여 가공하지 않고 인스턴스를 획득 합니다. |
| from | static factory    | Converts the input parameters to an instance of the target class, which may involve losing information from the input. |
|  |     | 번역: 파라미터를 가공하여 원하는 클래스의 인스턴스를 획득합니다. 파라미터에서 정보를 소실할 수도 있습니다. |
| parse | static factory    | Parses the input string to produce an instance of the target class. |
|  |     | 번역: 입력된 문자열을 분석하여 원하는 클래스의 인스턴스를 획득합니다. |
| format | instance    | Uses the specified formatter to format the values in the temporal object to produce a string. |
|  |     | 번역: 특정 포맷을 이용하여 현재 객체를 문자열로 만듭니다. |
| get | instance    | Returns a part of the state of the target object. |
|  |     | 번역: 타겟 객체의 상태 중 일부를 반환합니다. |
| is | instance    | Queries the state of the target object. |
|  |     | 번역: 타겟 객체의 상태에 관한 질문을 합니다. |
| with | instance    | Returns a copy of the target object with one element changed; this is the immutable equivalent to a set method on a JavaBean. |
|  |     | 번역: 타겟 객체의 한 요소를 변경하고 그 본사본을 반환합니다; 이건 자바빈의 수정자 메서드처럼 불변입니다. |
| plus | instance    | Returns a copy of the target object with an amount of time added. |
|  |     | 번역: 시간이 지남에 따른 타겟 객체의 본사본을 반환합니다. |
| minus | instance    | Returns a copy of the target object with an amount of thime subtracted. |
|  |     | 번역: 이전 시간에 따른 타겟 객체의 본사본을 반환합니다. |
| to | instance    | Converts this object to another type. |
|  |     | 번역: 객체를 다른 타입으로 변환합니다. |
| at | instance    | Combines this object with another. |
|  |     | 번역: 다른 객체와 합칩니다. |

<br>

---

<br>

### ● 활용

<br>
<b>of</b><br>
<br>

> Create an instance where the factory is primarily validating the input parameters, not converting them.<br>
> factory 에서 파라미터를 검사하여 가공하지 않고 인스턴스를 획득 합니다.<br>

<br>
처음 이 키워드를 접한건 Set 의 of 메서드였다.<br>
받은 인스턴스를 이용해 새로운 Set 을 획득했던 메서드였다.<br>
그리고 내가 이 키워드를 활용했던 부분은 DTO 였던 것 같다.<br>
그것도 토이 프로젝트 경연에서 1등을 한 사람의 메서드를 뜯다가 발견했다.<br>
엔티티에서 필요한 정보만을 추출하여 DTO 에 담아 반환했고, 물론 요청에 따른 반환이기에 메서드에 <b>response</b> 라는 키워드도 사용했다.<br>
그렇게 완성된 메서드 명은 <b>responseOf</b> 였다.<br>

```java

public class Car {
    // PK
    private int id;

    // 모델 명
    private String name;

    // 가격
    @JsonFormat(pattern = "#,###")
    private BigDecimal price;

    // 출시일
    private Timestamp releaseAt;

    ...

}

---

public class CarInfoResponse {

    private final int id;

    private final String name;

    @JsonFormat(pattern = "#,###")
    private final BigDecimal price;

    private final Timestamp releaseAt;

    public CarInfoResponse(int id, String name, BigDecimal price, Timestamp releaseAt) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.releaseAt = releaseAt;
    }

    // 공부한 개념이 접목된 메서드 명명규칙
    public CarResponse responseOf(Car car) {
        return new CarInfoResponse(car.getId(), car.getName(), car.getPrice(), car.getReleasAt());
    }

    ...
}

```

<br>
위 DTO 는 단순 조회용 DTO 로 예시 코딩하기 위해 생성한 클래스이다.<br>
responseOf 라는 네이밍을 활용하여 요청 시, Car 클래스의 데이터를 획득할 수 있는 방법을 확실히 표현했다.<br>
<br>

---

<br>

<br>
<b>from</b><br>
<br>

> Converts the input parameters to an instance of the target class, which may involve losing information from the input.<br>
> 파라미터를 가공하여 원하는 클래스의 인스턴스를 획득합니다. 파라미터에서 정보를 소실할 수도 있습니다.<br>

<br>
of 메서드와는 상충한다.<br>
<b>가공한다는 것</b>과 <b>정보를 소실할 수 있다</b>는 점.<br>
하지만 DTO 에서 of 네이밍을 이용한 메서드라고 하더라도 엔티티로 지정한 VO 의 모든 인자를 받아서 구현하는 것은 아니다.<br>
그것보다 <b>중점은 가공</b> 이라고 생각하면 될것 같다.<br>
<br>
from 메서드의 예시를 구상하다가 Period 클래스의 from 메서드를 적으려 했는데, 오히려 다른 코드가 생각났다..<br>

```java

public class Person {

    private String name;

    private int age;

    public static Person fromNameAndAge(String name, int age) {
        Person person = new Person();
        person.setName(name);
        person.setAge(age);
        return person;
    }

    ...

}


```

<br>
간단한 코드로 from 키워드의 취지를 확인할 수 있는 메서드다.<br>
문자열 name 과, 정수형 age 매개변수를 통해 Person 인스턴스를 쉽게 생성했다.<br>
물론 검증 과정도 빠진 메서드 이지만, from 키워드를 확인하기엔 적합했다.<br>
<br>

---

<br>

<br>
<b>parse</b><br>
<br>

> Parses the input string to produce an instance of the target class.<br>
> 입력된 문자열을 분석하여 원하는 클래스의 인스턴스를 획득합니다.<br>

<br>
나온 가장 익숙한 키워드 중 하나이다.<br>
<br>

```java

public static void main(String... args) {

    String stringNumber = 123;
    Integer number = Integer.parseInt(stringNumber);

}

```

<br>
초급 개발자가 이 키워드를 접하는건 분명,<br>
이런 Wrapper 클래스의 parsexx 메서드 일것이다.<br>
이렇게 문자열을 받아 원하는 클래스의 인스턴스를 획득할 때, parse 키워드를 주로 이용한다.<br>
<br>

---

<br>

<br>
<b>format</b><br>
<br>

> Uses the specified formatter to format the values in the temporal object to produce a string.<br>
> 특정 포맷을 이용하여 현재 객체를 문자열로 만듭니다.<br>

<br>
메서드 타입을 보면 Instance, 즉, "구현 방식은 클래스에 위임한다" 는 것을 알 수 있다.<br>
그래서 String 클래스의 format 메서드를 보면 정적으로 정의된 것을 알수 있다.<br>
<br>

---

<br>

<br>
<b>get, is</b><br>
<br>

> get<br>
> Returns a part of the state of the target object.<br>
> 타겟 객체의 상태 중 일부를 반환합니다.<br>
> <br>
> is<br>
> Queries the state of the target object.<br>
> 타겟 객체의 상태에 관한 질문을 합니다.<br>

<br>
개발을 조금이라도 해봤다면 접해봤을 키워드들이다.<br>
위 두 키워드 를 이용한 메서드는 IDEA, 라이브러리 등에서 구현에 도움을 줄 정도로 정말 잘 알려진 키워드이다.<br>
참고하고 넘어가면 될 것 같다.<br>
<br>

---

<br>

<br>
<b>with</b><br>
<br>

> Returns a copy of the target object with one element changed; this is the immutable equivalent to a set method on a JavaBean.<br>
> 타겟 객체의 한 요소를 변경하고 그 본사본을 반환합니다; 이건 자바빈의 수정자 메서드처럼 불변입니다.<br>

<br>
가장 헷갈리는 키워드 중 하나이다.<br>
설명을 잘못 읽으면 <b>from 키워드와 흡사</b>해보인다.<br>
<br>
하지만 전혀 다르다.<br>
영어를 내가 직역하다가 발생한 경우이고 코드를 보면 확실하다.<br>

```java

public class Person {

    private String name;

    private int age;

    // from 키워드
    public static Person fromNameAndAge(String name, int age) {
        Person person = new Person();
        person.setName(name);
        person.setAge(age);
        return person;
    }

    public static class Builder() {

        private String name;

        ...

        // with 키워드
        public Builder withName(String name) {
            this.name = name;
            return this;
        }

        ...
    }
    
}

```

with 키워드는 이렇게 요소를 변경한다.<br>
즉, from 처럼 이용해서 새로운 인스턴스를 획득하는 것이 아닌, 요소의 값을 바꾸고 그 인스턴스 자체를 반환함을 의미한다.<br>
예시코드는 Builder 패턴을 이용했다.<br>
<br>

---

<br>

<br>
<b>plus, minus</b><br>
<br>

> plus<br>
> Returns a copy of the target object with an amount of time added.<br>
> 시간이 지남에 따른 타겟 객체의 본사본을 반환합니다.<br>
><br>
> minus<br>
> Returns a copy of the target object with an amount of time subtracted.<br>
> 이전 시간에 따른 타겟 객체의 본사본을 반환합니다.<br>

<br>
직역에는 시간이라고 한다.<br>
하지만 나는 이 키워드를 실제 숫자 데이터 자료형의 계산 로직에서 사용하는 것을 봤다.<br>
아직도 정말 단순 계산용 키워드인지, 혹은 상태를 관리할 때 더하거나 빼는 등의 로직도 시간에 따른 결과로 치는 것인지 조금 헷갈린다.<br>
<br>

---

<br>

<br>
<b>to, at</b><br>
<br>

> to<br>
> Converts this object to another type.<br>
> 객체를 다른 타입으로 변환합니다.<br>
> <br>
> at<br>
> Combines this object with another.<br>
> 다른 객체와 합칩니다.<br>

<br>
우선 to 도 굉장히 많이봤을 키워드이다.<br>
대표적인 메서드는 String 클래스의 toLowerCase 메서드이다.<br>
모든 문자를 소문자로 만들어주는 메서드이고, 개발을 접했다면 최소 한 번은 사용해봤을 것이다.<br>
<br>
at 키워드는 조금 생소하다.<br>
String 클래스의 charAt 을 생각했지만, 키워드의 설명과 보면 절대 아닐 것 같다.<br>
이것도 plus, minus 와 함께 조금 더 봐야할 것 같다.<br>
<br>
<br>