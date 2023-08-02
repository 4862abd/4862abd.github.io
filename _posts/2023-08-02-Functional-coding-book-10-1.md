---
title: 10장, 일급함수Ⅰ- 1
author: park
date: 2023-08-02 15:30:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 10장 일급함수Ⅰ- 1.

### 소제목 목록
● 시작하며<br/>
● 코드의 냄새: 함수 이름에 있는 암묵적 인자<br/>
● 리팩터링: 암묵적 인자를 드러내기<br/>

---

<!-- ![07](/assets/img/05.Functional-coding/09/07.png) -->

## ● 시작하며

책에서 이전에 잠깐 꺼냈던 키워드가 있다.<br/>
<b>코드의 냄새(Code smell)</b> 가 그것이다.<br/>
이는 더 큰 문제를 가져올 수 있는 코드를 의미하고, 일전에는 코드가 중복되는 함수를 해결할 때 잠깐 등장했다.<br/>
그때는 이 키워드가 더 등장할지 몰라서 따로 다루지 않았지만 이번 챕터부터 본격적으로 등장하는 것 같다.<br/>
<br/>

---
<br/>

## ● 코드의 냄새: 함수 이름에 있는 암묵적 인자

만약 장바구니에서 상품의 이름으로 상품을 찾아 상품의 상태(가격, 세금 등)를 바꿔주는 코드를 추가한다고 해보자.<br/>
<br/>

```javascript

// 맵 형태의 데이터를 받아 카피-온-라이트 형식으로 객체의 값을 변경해서 반환
function objectSet(object, key, value) {
	var copy = Object.assign({ }, object);
	copy[key] = value;

	return copy;
}

// 상품의 명으로 상품을 찾아 가격을 바꿔주는 함수
function setPriceByName(cart, name, price) {
	var item = cart[name];
	var newItem = objectSet(item, 'price', price);
	var newCart = objectSet(cart, name, newItem);

	return newCart;
}

// 상품의 명으로 상품을 찾아 담긴 개수를 바꿔주는 함수
function setQuantityByName(cart, name, quant) {
	var item = cart[name];
	var newItem = objectSet(item, 'quantity', quant);
	var newCart = objectSet(cart, name, newItem);

	return newCart;
}

// 이 외에도 배송여부, 세금을 바꾸는 기능이 함수로 정의되어 있다.
```

<br/>
위의 코드는 코드 냄새로 가득하다.<br/>
물론 함수에 인자를 정확하게 명시하여 본인의 역할은 잘 지어둔 것 같아보인다.<br/>
하지만 비슷한 역할의 코드가 중복해서 정의되는 것을 보면 문제가 많아보인다.<br/>
이렇게 함수 이름에 있는 일부가 인자처럼 동작하는 형태의 냄새를 <b>함수 이름에 있는 암묵적 인자</b>라고 한다.<br/>
값을 명시적으로 전달하지 않고 함수 이름의 일부로 '전달'하고 있는 것이다.<br/>
<br/>
<b>※ 함수 이름에 있는 암묵적 인자(Implicit Argument in Function Name) 의 특징</b><br/>
<br/>

1. <b>함수 구현이 거의 똑같다.</b><br/>
2. <b>함수 이름이 구현의 차이를 만든다.</b><br/>

<br/>
이 냄새 말고도 문제가 더러 보인다.<br/>
변경할 필드의 명 즉, 키를 함수들이 직접 알고 있는 것이 그러하다.<br/>
이런 설계가 계속될 경우 혹시라도 발생할 문제에 대처하기 어려워지고 오류로 인한 피해 영역 산정이 기하급수적으로 늘어나게 된다.<br/>
<br/>
이러한 이유로 위의 함수들을 리팩터링 하려 한다.<br/>
<br/>

> 이는 Http 메소드 호출 시 정한 Api 주소와 비슷하다.<br/>
> 우리는 Api 주소를 설계할 때도 리소스를 위주로 설계한다.<br/>
> 객체가 관리할 데이터에 초점을 두고 다루는 데이터는 명명규칙에 이용하지 않는다.<br/>
> 중복으로 인해 코드 냄새가 발생할 경우 처리하는 방식과 비슷한 명명규칙임에 신기하여 기억이 났다.<br/>
{: .prompt-info }

<br/>

---

<br/>

## ● 리팩터링: 암묵적 인자를 드러내기

<br/>

```javascript
function setFieldByName(cart, name, field, value) {
	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);

	return newCart;
}

...

cart = setFieldByName(cart, "shoe", 'price', 13);
cart = setFieldByName(cart, "shoe", 'quantity', 3);
cart = setFieldByName(cart, "shoe", 'shipping', 0);
cart = setFieldByName(cart, "shoe", 'tax', 2.34);
```

<br/>
문자열만 바뀌면서 코드가 중복되게 만들었던 함수들을 하나로 리팩터링 했다.<br/>
값에는 큰따옴표를 쓰고 키에는 작은따옴표를 썼다.(키로도 쓰고 값으로도 쓸수 있다면 큰따옴표를 쓴다.)<br/>
기존에 암묵적으로 문자열로 이용하던 맵 형태 데이터의의 키를 parameter로 추출했다.<br/>
우리는 이러한 방식을 <b>암묵적 인자를 드러내기</b> 리팩터링 이라고 한다.<br/>
<br/>
<b>암묵적 인자를 드러내기(Express Implicit Argument) 리팩터링</b><br/>
<br/>

1. 함수 이름에 있는 <b>암묵적 인자를 확인</b>한다.<br/>
    -> 함수명에 있는 필드들<br/>
2. <b>명시적인 인자를 추가</b>한다.<br/>
    -> 기존 함수명에 있던 <b>필드명을 파라미터로 추출</b><br/>
3. 함수 본문에 <b>하드코딩된 값을 새로운 인자</b>로 바꿉니다.<br/>
    -> 함수 내에 하드코딩 되어 있던 필드명을 2번과 동일하게 추출(함수명과 함수내 필드명이 같은 값을 가리킴, 파라미터 하나로 추출 가능)<br/>
4. 함수를 <b>부르는 곳을 고칩니다.</b><br/>
    -> 추가한 필드명의 파라미터를 추가하여 호출<br/>

<br/>
이러한 리팩터링 끝에 필드에 따라 함수명과 필드명만 다른 여러 함수들이 하나로 줄여졌다.<br/>
그리고 필드명을 <b>일급 값</b>으로 만들었다.<br/>
이제 암묵적인 이름은 인자로 넘길 수 있는 값이 되었고 이 일급 값은 언어 전체 어디서나 사용 가능하다.<br/>
<br/>

### 일급(first-class)으로 할 수 있는 것

1. 변수에 할당<br/>
2. 함수의 인자로 넘기기<br/>
3. 함수의 리턴값으로 받기<br/>
4. 배열이나 객체에 담기<br/>

<br/>

### 자바스크립트에서 일급이 아닌 것

1. 수식 연산자<br/>
2. 반복문<br/>
3. 조건문<br/>
4. try/catch 블록<br/>

<br/>

---

<br/>
이제 필드명을 문자열로 추출하여 일급화 되었다.<br/>
하지만 <b style="color: red;">필드명을 문자열로 사용하면 버그가 생길 우려</b>가 있다.<br/>
이러한 걱정을 <b>막을 방법</b>은 컴파일 타임에 검사하는 것과 런타임에 검사하는 것이 있다고 한다.<br/>
<br/>
<b style="color: blue;">컴파일 타임에 검사</b>하는 방법은 <b>주로 정적 타입 시스템에서 사용</b>하는 방법이며 자바스크립트는 타입스크립트, 자바에서는 Enum, 하스켈에서는 합 타입 등을 이용하는 등
대수적 데이터 타입을 통해 언어에 맞는 최선의 방법을 찾으라고 했다.<br/>
<br/>
<b style="color: green;">런타임 검사</b>는 컴파일 타임에 동작하지 않고 <b>함수를 실행할 때마다 동작</b>한다.<br/>
그리고 자바스크립트는 정적 타입 언어가 아니기 때문에 런타임 검사 방법을 사용한다고 한다.<br/>
하단의 코드는 런타임 검사 방법으로 필드명이 올바른지 확인하는 코드이다.<br/>
<br/>

```javascript
// 접근 가능한 필드를 추가한다.
var validItemFields = ['price', 'quantity', 'shipping', 'tax'];

function setFieldByName(cart, name, field, value) {
	// 필드명을 확인하는 validation 블록
	// 필드가 일급이기 때문에 런타임에 확인하는 것은 쉽다.
	if(!validItemFields.includes(field)) {
		throw "Not a vlid item field: " + "'" + field + "'";
	}

	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);

	return newCart;
}

function objectSet(object, key, value) {
	var copy = Object.assign({ }, object);
	copy[key] = value;
	
	return copy;
}
```

<br/>
위의 방식에서 몇 가지 문제가 주장된다.<br/>
<br/>

1. 추상화 벽 아래에서 정의한 필드명이 추상화 벽 위에 노출된 것은 추상화 벽의 원칙을 위반하는 것이 아닌가?<br/>
2. API 문서에 필드명을 명시하면 영원히 필드명을 바꾸지 못하는 것이 아닌가?<br/>

<br/>
위의 문제라고 주장한 것은은 모두 문제 없다.<br/>
우선 <b>구현이 외부에 노출된 것이 아니다.</b><br/>
또한 내부에서 정의한 필드명이 바뀐다고 해도 사용하는 사람들은 원래 필드명을 그대로 사용하게 할 수 있다.<br/>
<br/>
만약 'quantity' <b>필드명이</b> 'number'로 <b>바뀐 상황</b>이고 추상화 벽 위에서 'quantity' <b>필드명을 그대로 사용하고 싶다면 내부에서 간단히</b> 필드명을 바꿔주면 된다.<br/>

```javascript
var vlidItemFields = ['price', 'quantity', 'shipping', 'tax', 'number'];
var translations = { 'quantity': 'number' };

function setFieldByName(cart, name, field, value) {
	// 사용할 필드명에 존재하지 않는가?
	if (!validItemFields.includes(field)) {
		throw "Not a valid item field: '" + field + "'.";
	}
	
	// 바뀐 필드명에 속해있는가?
	if (translations.hasOwnProperty(field)) {
		// 그렇다면 바뀐 필드명으로 대입한다.
		field = translations[field];
	}

	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);

	return newCart;
}
```

<br/>
이렇게 되면 바뀐 필드명으로도 문제 없이 validation을 진행하여 런타임 검사와 함께 동작할 수 있다.<br/>