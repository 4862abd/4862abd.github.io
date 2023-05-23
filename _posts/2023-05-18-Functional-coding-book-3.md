---
title: 3장 액션과 계산, 데이터의 차이를 알기
author: park
date: 2023-05-23 17:18:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

<!-- 표지 -->
<!-- 로컬 -->
<!-- ![image](https://github.com/4862abd/4862abd.github.io/assets/77370682/ce261bb4-c073-43f4-a3df-9b4411144ad4) -->
<!-- 배포 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 3장 액션과 계산, 데이터의 차이를 알기.

### 소제목 목록
● 액션과 계산, 데이터<br/>
● 액션과 계산, 데이터는 어디에나 적용할 수 있다.<br/>
● 장보기 과정에서 배운 것<br/>
● 새로 만드는 코드에 함수형 사고 적용하기<br/>
● 쿠폰을 보내는 과정을 그려보기<br/>
● 쿠폰 보내는 과정 구현하기<br/>
● 이메일 보내기는 액션입니다.<br/>

<br/>
3장에서는 아주 조금 코드를 보여준다.<br/>
코드를 조금이라도 다룬 사람이라면 금방 이해할 수 있을 정도의 수준이다.<br/>
<br/>
하지만 <b>내가 원했던 것과는 다르게 JS 언어를 기반으로 예시를 보여준다는 점.</b><br/>
그래서 <b>나는 JS를 이해하고 해석해서 <span style="color: red;">Java의 코드로 구현할 것</span></b>이며, 이 과정에서 본인에게 불편한게 느껴진다면 편하게 이야기해주면 감사하겠다.<br/>
또한 이런 해석 과정을 거치는 것이 처음이므로 포스팅을 거쳐가며 실력을 발전시키겠다.<br/>
원본의 JS 코드도 같이 첨부할테니 참고 바란다.<br/>

---
<br/>

### ● 액션과 계산, 데이터
<br/>

모든 개발 과정에서 액션과 계산, 데이터를 구분하는 기술을 적용하는 것이 좋다고 한다.<br/>
예시를 들어보자.<br/>
<br/>

1. <b>문제에 대해 생각할 때</b><br/>
아직 코딩을 시작하기 전이고 문제에 대해서 고민하고 있을 때도 문제를 액션과 계산, 데이터로 구분할 수 있다.<br/>
특히 주의해야할 부분(액션), 데이터, 결정을 내릴 부분(계산) 을 명확하게 알수 있다고 한다.<br/>
<br/>

2. <b>코딩할 때</b><br/>
드디어 핵심으로 보이는 말이 하나 나왔다.<br/>
<b>액션에서 빠르게 계산을 분리해낼 줄 알아야하며, 계산에서는 데이터를 분리해낼 줄 알아야한다.</b><br/>
이는 이전에 봤던 책 중에 "토비의 스프링 3.1" 에도 나온 맥락이다.<br/>
크게 따지면 <b>관심사 분리를 통한 메소드 추출</b> 과도 비슷한 의미로 설명할 것으로 보인다.<br/>
<br/>

3. <b>코드를 읽을 때</b><br/>
액션과 계산, 데이터를 구분지어 확인할 줄 알아야 하며, 코드를 읽을 때에도 액션을 계산으로, 계산을 데이터로 분리하는<br/>
방법을 강구하여 리팩토링하는 방법을 강구해야 할 것이다.<br/>
<br/>

> <b>리팩토링?</b><br/>
> <br/>
> <b>결과의 변경이 없이 결과를 내기 위한 코드의 구조를 새로 정립하거나 수정</b>하는 작업을 뜻한다.<br/>
> 여러 이유로 리팩토링을 진행하지만, 보통 공부를 했던 책들에서는,<br/>
> <br/>
> <b>"역할과 책임이 뒤섞인 코드에서 <span style="color: red;">각각의 관심사를 분리</span>해낸다." 를 목표로 진행하는 경향</b><br/>
> <br/>
> 이 있더라.<br/>
> 물론 그런 책만 내가 본 것일 수 있다.<br/>
> <br/>
> <hr/>
> <br/>
> 실제로 리팩토링은 <b>업무 중에도 자주 진행</b>했다.<br/>
> 코드의 <b>가독성을 위해서</b> 진행하는 경우가 가장 많았고, 코드의 <b>성능을 위해</b> 진행하는 경우도 있었다.<br/>
> <br/>
> 가장 기억에 남는 리팩토링은 MyBatis와 xml을 활용해서 결과를 조회하던 로직을 순수 Java코드를 활용한 JPA로 바꾸면서
> 같은 결과를 도출하게 하는 작업이었다.<br/>
> <br/>
> 이걸 리팩토링이라고 볼 수 있을까? &nbsp;&nbsp;그럼, 당연하지.<br/>
> <b>nativeQuery?</b> 사용하지 않았다.<br/>
> GROUP BY 같이 JPA가 지원하지 않는 기능은 Java의 stream을 활용하는 등, Java로 같은 결과를 낼 수 있게 진행하곤 했다.<br/>
> 물론 산출되는 데이터 자체는 다르겠지만, 그 데이터를 이용해 화면을 송출하는 작업에서는 같은 결과를 내게 하였다.<br/>
> <br/>
> 어려웠고, 재밌었다.<br/>
{: .prompt-tip }
<br/>

---
<br/>

### ● 액션과 계산, 데이터는 어디에나 적용할 수 있다.

우리가 일상에서 자주 하는 장보기로 예시를 들었다.<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/d61d405c-0896-4453-9796-c7b48024684b){: width="250" height="250" }

위 과정을 거친다고 하면, 이 모든 단계는 각각의 액션이라고 할 수 있다.<br/>
<br/>

#### 냉장고 확인하기
냉장고를 확인하는 일은 언제 확인하느냐에 따라 재고가 다르기 때문에 액션이다.<br/>
그리고 확인되는 재고는 데이터가 되겠다.<br/>

#### 운전해서 상점으로 가기
이건 누가봐도 액션이다.<br/>
물론 냉장고를 확인하는 것과 마찬가지로 이벤트에 의한 기록인 데이터가 숨어있긴 하다.<br/>
가는 길, 위치, 자동차 종류 등<br/>

#### 필요한 것 구입하기
이것도 액션이다.<br/>
구입하기 위해 파악한 재고로 필요한 재료를 계산하고, 물건을 골라서 사람들을 피해 줄을 서야할 것이다.<br/>
<br/>
위의 과정을 책에서는 간단하게 이렇게 나열했다.<br/>

| 대상 | 구분 |
|:-:|:-:|
| 현재 재고 | 데이터 |
| 필요한 재고 | 데이터 |
| 재고 '빼기' | 계산 |
| 장보기 목록 | 데이터 |
| 목록에 있는 것 구입하기 | 액션 |

#### 운전해서 집으로 오기
이건 두 번째와 마찬가지라고 생각하면 되겠다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/17d6b45e-7963-4fc6-ba50-0815d2ac23c5){: width="490" height="280" }
<br/>
이런 타임라인 그래프가 완성된다.<br/>
필요한 재고는 운전자가 요리를 하기위해 필요한 평소 재고를 기록한 외부의 데이터가 되겠다.<br/>
즉, 상수라고 칠 수 있겠다.<br/>
<br/>
이 과정에서 설명하려는 점은 우리는 <b>액션을 최대한 나눌 수 있을 때까지 계산과 데이터로 나누는 것</b><br/>
이 <b style="color: red;">중요한 포인트</b>라는 것이다.<br/>
그 과정에서 숨어있는 액션이나 계산, 데이터를 발견할 수 있는 것이다.<br/>
<br/>

---
<br/>

### ● 장보기 과정에서 배운 것

1. 액션과 계산, 데이터는 어디에서 든지 적용될 수 있다.<br/>
2. 액션 안에는 계산과 데이터, 또 다른 액션이 숨어 있을 수도 있다.<br/>
3. 계산은 더 작은 계산과 데이터로 나누고 연결할 수 있다.<br/>
4. 데이터는 데이터만 조합할 수 있다.<br/>
5. <b>계산은 때로 '우리 머리 속에서' 일어날 수 있다.</b><br/>
    ㄴ 우리가 위처럼 유용한 방법에도 실제 코딩할 때 적용하지 않는 가장 큰 이유는, <br/>
    이미 우리 머리 속에는 그런 과정을 거친다는 것이다.<br/>
    ㄴ 어디서 어떤 데이터를 어떻게 구해서 사용할지 머리 속으로 다 계산한다는 것이다.<br/>
    ㄴ 우리는 이러한 과정을 나눠서 프로그래밍을 할 줄 알아야 한다.<br/>
<br/>

---
<br/>

> #### 데이터에 대해 자세히 알아보기
> <b>데이터는 무엇인가?</b><br/>
> 이벤트에 대한 사실.<br/>
> 일어난 일의 결과를 기록한 것이다.<br/>
> <br/>
> <b>불변성</b><br/>
> 함수형 프로그래머는 불변 데이터 구조를 만들기 위해 두 가지 원칙을 사용한다고 한다.<br/>
> 1. 카피-온-라이트(Copy-on-Write)<br/>
>     ㄴ 변경할 때 복사본을 만든다.<br/>
> 2. 방어적 복사(Defensive copy)<br/>
>   ㄴ 보관하려고 하는 데이터의 복사본을 만든다.<br/>
> 
> <br/>
> 위의 원칙은 이후 6, 7장에서 다룬다고 한다.<br/>
> <br/>
> <b>데이터의 장점</b><br/>
> 데이터는 그 자체로 할 수 있는 것이 없고, 그것이 가장 큰 장점이라고 한다.<br/>
> <b>"어? 데이터보고 어떤 제품이 더 좋은지 고르거나 구매할 수 있잖아?" 라고 한다면, ● 장보기 과정에서 배운 것의 5번을 다시 봐라.</b><br/>
> 계산은 때로 '우리 머리 속에서' 일어나고 그 데이터는 그 상태 그대로 있을 뿐이었다.<br/>
> 생각이 바뀌고 판단한 것은 우리 머리 속에서 일어난 것이며 계산과 액션을 통해 이뤄진 것이다.<br/>
> <br/>
> 그러면 책에서 설명하는 장점을 살펴보자.<br/>
> 1. <b>직렬화(Serialization)</b><br/>
> 직렬화된 액션과 계산은 다른 곳에서 잘 동작할 것이라는 보장은 없다.<br/>
> 하지만 <b>직렬화된 데이터는 전송하거나 메모리에 저장했다가 읽기 쉬운 장점</b>이 있다.<br/>
> 또한 이 책에서는 JPA에 관해 설명하지 않지만 JPA에서도 직렬화 관련하여 설명하는 부분은 정말 핵심 중 핵심이다.<br/>
> 이는 JPA에 대해 다루는 공부를 할 때, 다시 정리하겠다.<br/>
> <br/>
> 2. <b>동일성 비교</b><br/>
> 계산이나 액션은 비교하기 어렵지만 데이터는 비교하기 쉽다.<br/>
> <br/>
> 3. <b>자유로운 해석</b><br/>
> 그 자체로는 할 수 있는 일이 전혀 없지만 우리는 데이터를 활용해서 많은 것을 할 수 있다.<br/>
> 어떤 것을 구매할지 고르고, 어떤 것은 믿고 거르는 등 서로 다른 기준을 두고 해석하여 사용하기 쉬운 것이 데이터이다.<br/>
>
> <br/>
> <br/>
> <b>데이터의 단점</b><br/>
> 그 자체로 아무것도 할 수 없다는 것이 단점이다.<br/>
> 모순적이지 않은가?<br/>
> 최근에는 빅데이터다 하면서 날 것 그대로의 데이터를 어떻게 가공하여 활용하는가 가 정말 큰 주제 중 하나였다.<br/>
> 하지만 그건 결국, 데이터는 어떤 방식으로든 해석되어 사용되기를 기다려야 하는 피동적인 존재라는 것이고, 그것이 단점이다.<br/>
{: .prompt-tip }
<br/>

---
<br/>

### ● 새로 만드는 코드에 함수형 사고 적용하기
이 과정을 위해 책에서는 사례를 하나 제시한다.<br/>
커다란 이메일 데이터베이스가 있으며, 이메일 별로 각 사용자가 추천한 친구 수를 기록하고 있다.<br/>
각 사용자에게는 추천한 친구 수로 등급을 나누어 쿠폰을 발급할 것이며 쿠폰은 "bad", "good", "best" 가 존재한다.<br/>
그리고 사용자들에게 본인의 등급에 따라 쿠폰을 이메일로 발송할 것이다.<br/>
등급을 나누는 기준은 <b>"best"는 10명 이상의 추천, "good"는 best를 제외한 모든 유저, "bad" 쿠폰은 사용하지 않을 것이다.</b><br/>
마지막으로 "bad" 등급의 쿠폰은 사용할 수 없으니 이메일을 전송하지 않을 것이다.<br/>
<br/>

---
<br/>

### ● 쿠폰을 보내는 과정을 그려보기
책에서 동작을 설명할 때 사용하는 타임라인 다이어그램을 통해 여기서도 동일하게 진행할 것이다.<br/>
우선 DB에서 구독자와 쿠폰 데이터를 가져온다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/7b19d162-c407-49b3-9395-b6924105ddf9)
<i>(사진을 구하지 못 해서 직접 촬영했다. 화질구지..)</i><br/>
<br/>
데이터를 조회해오는 과정은 액션이 되고 조회하는 이벤트에서 기록된 사실은 데이터가 되겠다.<br/>
<br/>
<br/>
<b>책에서는 중간중간 풀어서 과정을 설명하지만, 자세하게 풀어서 한 번에 설명하겠다.</b><br/>
<br/>
우선 쿠폰과 구독자의 데이터가 있으니 등급을 나눌 필요가 있다.<br/>
<b>기준은 추천 수 10 이상</b>이 되겠다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/d5264ccd-5ff7-45ac-97a5-8a180fafa07b)
즉, 이러한 과정을 거치게 될 것이다.<br/>
<br/>
이렇게 나눠진 등급을 이용해서 각 이용자에게는 다른 형식의 이메일이 만들어질 것이다.<br/>
그 과정을 계산과 데이터로 나누면 이런 형식이 완성된다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/647b6924-b5b6-4532-861d-bd908efb9aed)
이메일을 만드는 과정은 액션이 필요하지 않다.<br/>
안에 들어가는 내용 자체는 기준이 잡힌 구독자, 쿠폰, 등급 데이터를 이용해서 단순히 작성만 하면 되는 작업이기 때문이다.<br/>
하지만 이를 보내는 작업은 액션이 될것이다.<br/>
<br/>

---
<br/>

### ● 쿠폰 보내는 과정 구현하기
구독자 데이터를 예시로 할당하는 것과 쿠폰 등급을 선언, 초기화 하는 과정을 빠르게 정리하겠다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/c91fd50f-daf8-47fe-8d26-cb9d70287b15)<br/>
<i>(DB에서 조회된 데이터를 JS의 객체로 표현한 사진)</i><br/>

```javascript
var subscriber = {
    email: "sam@pmail.com",
    rec_count: 16,
};
```

```java
// Subscriber 객체 정의
public class Subscriber {
    private String email;
    private int rec_count;

    // Getter, Setter 메소드
    public String getEmail() {
        return this.email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
    
    public int getRec_count() {
        return this.rec_count;
    }
    public void setRec_count(int rec_count) {
        this.rec_count = rec_count;
    }
}

...

public void testMethod() {
    // 쿠폰 등급
    String rank1 = "best";
    String rank2 = "good";


    // 객체 선언, 기본 생성자 메소드를 통한 초기화, 참조형(Reference type) 변수의 초기화
    Subscriber subscriber = new Subscriber();

    subscriber.setEmail("sam@pmail.com");
    subscriber.setRec_count(16);
}
```
<br/>
자, 이제 함수(Method)를 통해 구독자의 등급을 나누는 서비스 로직을 구현할 것이다.<br/>
기준은 이렇게 되었다.<br/>
<br/>

1. 추천 수 10개 이상은 best<br/>
2. 이외의 모든 유저에게는 good<br/>

<br/>
책에서는 이를 분기처리를 통해 등급을 구분하고 있었다.<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/2672552e-c455-4d93-b4f2-eb8c69c60b2f)<br/>

```javascript
function subCouponRank(subscriber) {
    if(subscriber.rec_count >= 10)
        return "best";
    else
        return "good";
}
```
```java
// Subscriber 객체를 파라미터로 받음
public String subCouponRank(Subscriber subscriber) {
    // 추천수(rec_count)가 10 이상일 경우 "best" return, 아닌 경우 "good" return
    if(subscriber.getRec_count >= 10) 
        return "best";
    else
        return "good";
}

```

<br/>
이렇게 만들어진 위의 <b>등급을 나누는 method는 언제든지 재사용 가능하고 명확하며 구분하기 쉽다.</b><br/>
그리고 위의 method는 같은 값을 넣으면 항상 같은 결과를 도출해낼 것이다.<br/>
즉, 이렇게 만들어진 기능은 <b>계산의 특징을 띄게된다.</b><br/>
<br/>
책에서 설명하는 계산 로직을 몇 개 더 나열하겠다.<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/0193c604-8812-4876-add3-a62c9f0875a8)
<i>(쿠폰 데이터 조회하기)</i><br/>
<br/>
위의 쿠폰 데이터를 등급별로 정리할 예정이다.<br/>
JS 코드의 원본을 먼저 보자.<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/2ec2fdcb-10c2-4549-8051-099598574bcd)

```javascript
function selectCouponsByRank(coupons, rank) {
    var ret = [];
    for(var c = 0; c < coupons.length; c++) {
        var coupon = coupons[c];
        // === 는 양측 데이터의 타입까지 비교한다.
        if(coupon.rank === rank)
            ret.push(coupon.code);
    }

    return ret;
}
```

```java
// DB에서 조회해오는 쿠폰 데이터를 담을 객체
public class Coupon {
    private String code;
    private String rank;

    // Getter, Setter 생략
}

...


public List<String> selectCouponsByRank(List<Coupons> coupons, String rank) {
    // return을 담을 List<String>
    List<String> ret = new ArrayList<Coupon>();

    // 파라미터로 받은 List의 크기 만큼 동작
    for(int c = 0; c < coupons.size(); c++) {
        Coupon coupon = coupons.get(c);

        // === -> .equals()
        if(coupon.getRank().equals(rank))
            ret.add(coupon.getCode());
    }

    return ret;
}
```

위의 method는 이제 기준 랭크를 파라미터로 받는다면 List에 담긴 Coupon 객체의 rank 속성과 비교해서 등급 별 리스트를 줄것이다.<br/>
이는 같은 리스트, 같은 기준의 rank를 주면 항상 같은 결과값을 주는 계산의 역할을 한다.<br/>
<br/>

---
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/923c0314-8731-4f48-9d49-69336d86ed6b)

<br/>
유저의 등급 데이터와 쿠폰 데이터를 이용해서 <b>이메일</b>을 만드는 계산이다.<br/>
(계속하여 계산만 예시로 보여줘서 스킵하겠다.)<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/007c68f0-b32c-4e3e-80ca-16f8efccab27)

<br/>
위의 사진은 여러 계산을 통해 구독자들에게 보낼 <b>이메일 List</b>를 만드는 로직이다.<br/>
바로 위의 emailForSubscriber function을 구독자 수 만큼 반복하여 만들어진 이메일의 List 를 return 한다.<br/>
<br/>

---
<br/>

### ● 이메일 보내기는 액션입니다.

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/6e26f140-63db-4f73-8332-921fe89fb5f6)

> 해당 사진을 보면 <b>fetch 라는 접두사(Prefix)</b>가 붙은 function이 두 번 호출된다.<br/>
> fetch는 <b>Fetch API</b>에서 따온 단어로 JavaScript에서 HTTP 파이프라인을 구성하는 요청과 응답 등의 요소에 접근하고 조작할 수 있게 하는 인터페이스에서 따왔다.<br/>
> 데이터를 조회하고 응답 받아 결과를 내는 특징을 가지는 JavaScript function의 Prefix로 fetch를 붙이는 상황이 통용된다.<br/>
> <i>참고: https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch</i><br/>
> <br/>
> 그래서 위에서 호출하는 function 두 개를 보면 <b>fetchCouponsFromDB(), fetchSubscribersFromDB()</b> 이다.<br/>
> 위의 정의와 function의 명명규칙에 의거하면 각각 <b>"DB에서 쿠폰 데이터 가져오기", "DB에서 구독자 데이터 가져오기"</b> 가 되겠다.<br/>
> <br/>
> 또 반복문 안의<br/>
> <b>emailSystem.send(email);</b><br/>
> 을 보면 <b>책에서 보여주지 않은</b> 이메일을 보내거나 받는 <b>관리 프로그램은 분명 따로 있다</b>는 뜻.<br/>
> 분명 <b>관심사 분리의 개념을 접목해서 공통적인 서비스 로직을 추상화하여 구현했을 것이라고 것을 예상</b>할 수 있다.<br/>
{: .prompt-info }

<br/>

---
<br/>

> #### 계산에 대해 자세히 알아보기
> <b>계산(순수 함수, Pure function), 수학 함수(Mathmatical function)</b> 은 무엇인가?<br/>
> 계산은 입력값으로 출력값을 만드는 것이다.<br/>
> 실행 시점과 횟수에 관계없이 같은 입력값에 대해 항상 같은 출력값을 돌려준다.<br/>
> <br/>
> <b>왜 액션보다 계산이 좋은가?</b><br/>
> 1. 테스트하기 쉽다.<br/>
> 2. 기계적인 분석이 쉽다.<br/>
>     ㄴ 학술 연구에 정적 분석이라는 것이 있다.<br/>
>     ㄴ 정적 분석에서 자동화된 분석은 중요하다고 한다.<br/>
>         ㄴ 정적 분석: 정적 분석 도구를 이용해서 개발 단계에서부터 소스코드 상 보안약점을 진단하는 분석 방법이다.<br/>
>         ㄴ 즉, 소스 코드의 실행 없이 정적으로 프로그램의 문제를 찾는 과정을 의미한다.
3. 계산은 조합하기 좋다.<br/>
>     ㄴ 위의 예시 중 구독자에게 보낼 이메일 List를 산출하는 과정처럼, 여러 작은 계산을 모아서 큰 계산을 만들 수 있다.<br/>
>     ㄴ 이 상황에서 일급(High-order) 계산을 사용하는데 14장에서 자세히 알려준다고 한다.
> 
> <b>계산의 단점</b><br/>
> 계산와 액션은 실행하기 전에 어떤 일이 발생할 수 없다.<br/>
> 코드를 읽어서 정적 분석으로 예상은 할 수 있지만, 소프트웨어 측면에서 캡슐화된 객체는 블랙박스이다.<br/>
> 까보거나 입력값을 넣어 실행하기 전에 정확하게 알 수 없다는 것이다.<br/>
> <br/>
> <b>
> 함수형 프로그래밍의 대부분은 계산을 가지고 진행한다.<br/>
> 계산은 일반적으로 함수형 프로그래밍 외부에 있는 액션을 통해 수행된다.<br/>
> </b>
{: .prompt-tip }

<br/>

---
<br/>

> #### 액션에 대해 자세히 알아보기
> 순수하지 않은 함수, Impure function<br/>
> 부수 효과 함수, Side-effecting function<br/>
> 부수 효과가 있는 함수, Function with Side effects<br/>
> <br/>
> 책에서 예시로 나온 액션의 코드를 분석하면 이렇다.<br/>
> 계산과 데이터로 분리하지 않은 액션은 <b>본인 뿐 아니라 본인을 호출하는 method 까지, 본인과 관련된 모든 것을 액션으로 만들어버린다.</b><br/>
> 즉, 액션을 사용할 때에는 함수형 사고를 적용하여 <b>계산과 데이터로 분리하는 작업을 거쳐야 한다.</b><br/>
> <br/>
> <b>액션은 실행 시점과 횟수에 의존한다.</b>
> 1. 언제 실행되는지 - 순서
> 2. 얼마나 실행되는지 - 반복
> 
> <br/>
> <b>어떻게 액션에 의미를 담을 수 있을까?</b>
> 액션으로 외부 세상에 영향을 중 수 있다고 한다.<br/>
> 요청에 의해 호출되는 액션은 그 결과가 반드시 필요한 곳에 사용된다는 것.<br/>
> <br/>
> <b>액션은 쉽지 않다.</b>
> 1. 다루기 힘들다.<br/>
> 2. 우리가 <b>소프트웨어를 실행하는 가장 중요한 이유</b>이다.<br/>
> 3. 가능한 적게 사용하면서 액션 대신 계산이 사용될 수 있는지 생각하자.<br/>
>     ㄴ 이 점은 15장에서 다룬다고 한다.<br/>
> 4. 액션은 가능한 작게 만들어야 한다.<br/>
>     ㄴ 예를 들어 액션의 동작에서 필요 없는 결정이나 계획은 과감하게 분리하여 계산으로 구현하자.<br/>
> 5. 액션이 외부 세계와 상호작용하는 것을 제한할 수 있다.<br/>
>     ㄴ 내부에는 계산과 데이터만 존재하고 가장 바깥쪽에 액션이 있는 것이 가장 이상적이다.<br/>
>     ㄴ 이 점은 18장에서 어니언 아키텍처(Onion architecture)를 다룰 때 다시 확인하자.<br/>
> 6. 액션이 호출 시점에 의존하는 것을 제한하자.<br/>
>     ㄴ 함수형 프로그래머는 액션이 <b>호출 시점과 횟수에 덜 의존하도록 하는 기술을 알아야 하고, 더 쉽게 사용할 줄 알아야 한다.</b><br/>
{: .prompt-tip }