---
title: 1장. 오브젝트와 의존관계 - 1 - 작성중
author: park
date: 2024-10-08 23:55:27 +0800
categories: [Java, 토비의 스프링(2024-10  2025-01) - 2회차]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![toby](https://github.com/user-attachments/assets/f6502489-ab03-4163-8bd6-fbedae6ddc52)

---

## 1장. 오브젝트와 의존관계. (p.53 ~ p.90)

### 소제목 목록

● 들어가며<br/>
● 정리한 내용<br/>
● 이전에 받았던 질문 정리<br/>

<br/>

---

## ● 들어가며

2년 만의 2회차인 만큼 제대로 보려했다.<br/>
핵심 키워드, 핵심이 아니지만 등장한 키워드, 키워드를 공부하다가 등장한 키워드 등<br/>
까먹은 기억을 되찾으려 노력했다.<br/>
<br/>
원래는 스터디로 진행하려 했으나 혼자 보게 되었다.<br/>
자유로운 만큼 아쉽지 않게 제대로 학습해보자.<br/>
<br/>
<u>맥북의 매직마우스 베터리 이슈로 1장의 중간 쯤 멈췄다.</u><br/>

---

## ● 정리한 내용

### 1.1 초난감 DAO

책에서 기본으로 안내하는 사항들<br/>

> 
> DAO (Data Access Object): DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트.<br/>
> 

<br>

```java

public class User {

	String id;
    String name;
    String password;

	...getter(), setter()
}

```

```sql

-- h2 DB 를 이용할 예정이다.
create table toby_1_user (
    id varchar(10),
    name varchar(20) not null,
    password varchar(20) not null,
    primary key(id)
);

```


<br/>
map은 X(어떤 값의 집합) 값이 있는 배열을 Y(또 다른 값의 집합) 값이 있는 배열로 변환한다고 볼 수 있다.<br/>
즉, X 집합을 Y 집합으로 바꾸는 함수이다.<br/>
바꾸는 함수는 인자로 받아 사용하며 계산일 경우 가장 사용하기 쉽다.<br/>
책에서 주어진 제시대로 <b>익명 함수(Anonymous function)</b>과 <b>인라인(Inline)</b> 의 개념을 적용해서 구현하자.<br/>
<br/>

### 예제: 모든 고객의 이메일 주소

이메일 주소의 집합을 반환하는 map 함수를 구현해보자.<br/>

```javascript

map (customers, function (customer) {
	return customer.email;
});

```

간단하게 구현이 가능하다.<br/>
하지만 map 함수의 가장 주의점은 집합의 대상이 되는 객체의 속성에 필요한 요소가 없다면, null 이나 undefined 를 뱉어낼 것이다.<br/>
즉, 새로 만들어진 Y 집합에 부정확한 값이 포함될 수 있다는 것이다.<br/>

---

## ● 함수형 도구: filter

![02](/assets/img/05.Functional-coding/12/02.png)

위 처럼 부정확한 값(null, undefined) 가 포함될 수 있는 상황을 피하려 한다.<br/>
그러한 역할을 하는 함수를 filter 이다.<br/>
X 배열 중 특정 조건을 만족하는 데이터에 한해 모아서 Y 배열로 반환한다.<br/>

```javascript

var allEmails = map(customers, function(customer) {
	return customer.email;
});

var emailListWithoutNulls = filter(allEmails, function(email) {
	return !!email;
});

```

---

## ● 함수형 도구: reduce

![03](/assets/img/05.Functional-coding/12/03.png)

reduce는 다른 함수와 조금 다르다.<br/>
누적하는 형태라는 것이 조금 다른 개념이다.<br/>
그래서 가장 쉬운 방법으로 사용 되는 예시로 더하거나 해시 맵이나 문자열로 합치는 것을 주었다.<br/>

```javascript

// 문자열 배열의 값을 모두 합친 문자열 하나를 리턴한다.
reduce(strings, '', function(accum, string) {
	return accum + string;
});

```

익숙해 지기위해 하나 더 확인하고 가자.<br/>
reduce 함수를 활용하여 배열에서 가장 작은 값, 가장 큰 값을 구하려 한다.<br/>
만약 배열이 비어있다면 각각 Number.MAX_VALUE, Number.MIN_VALUE 를 반환하려 한다.<br/>

```javascript

function max(numbers) {
	return reduce(numbers, Number.MIN_VALUE, function(max, number) {
		if (max > number) {
			return max;
		} else {
			return number;
		}
	});
}

function min(numbers) {
	return reduce(numbers, Number.MAX_VALUE, function(min, number) {
		if (max < number) {
			return max;
		} else {
			return number;
		}
	});
}

```

<b>reduce 로 할 수 있는 것들</b>

1. 실행 취소/실행 복귀<br/>
2. 테스트할 때 사용자 입력을 다시 실행하기<br/>
3. 시간 여행 디버깅<br/>
4. 회계 감사 추적<br/>
