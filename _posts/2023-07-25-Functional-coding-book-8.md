---
title: 8장, 계층형 설계 1
author: park
date: 2023-07-27 17:56:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 8장 계층형 설계 Ⅰ.

### 소제목 목록
● 시작하며<br/>
● 직접 구현<br/>
● 직접 구현 패턴 리뷰<br/>

---

## ● 시작하며

우선 이 책에서 설명하는 계층은 Layer의 개념이라는 것을 알고 가야한다.<br>
그리고 익히 알려진 3계층 아키텍쳐의 그것은 아닐 것 같다.<br>
<br>

<b>책에 나온 키워드</b><br>

> <b>오프-바이-원(off-by-one) 에러</b><br>
> <br>
> 주로 배열을 반복해서 처리할 때 '크다' 혹은 '크거나 같다' 같은 비교문을 잘못 선택해서 의도하지 않게<br>
> 마지막 항목을 처리하지 못하거나 처리하는 오류<br>
{: .prompt-info }
<br>

---

## ● 직접 구현

만약 넥타이를 사면 넥타이 핀을 공짜로 주는 행사를 한다고 가정하고 구현해보자.<br>

```javascript
function freeTieClip(cart) {
	var hasTie = false;
	var hasTieClip = false;

	for (var i = 0; i < cart.length; i++) {
		var item = cart[i];
		if (item.name === 'tie')
			hasTie = true;
		if (item.name === 'tie clip')
			hasTieClip = true;
	}

	if (hasTie && !hasTieClip) {
		var tieClip = make_item('tie clip', 0);	// 넥타이 핀을 0원으로 생성
		return add_item(cart, tieClip);		// 카트에 넥타이 핀 추가
	}

	return cart;
}
```

<br>
이 코드에서 보이는 추상화 수준은 전부 다르다.<br>
그림으로 표현하면 이러하다.<br>
<br>

![01](/assets/img/05.Functional-coding/08/01.png)
<br>

이러한 이유는 함수에 종속되어 이 함수를 호출해야만 사용할 수 있는 반복문, index 값과<br>
함수로 정의하여 어떤 상황에서든 사용할 수 있는 함수 두 개의 레벨이 다르기 때문이다.<br>
<br>
그러면 위의 freeTieClip 함수의 반복문을 보자<br>
같은 반복문 내에서 각 상황에 맞는 상품명을 찾고 boolean 값을 돌려준다.<br>
그렇다면 해당 반복문은 카트와 상품이름을 매개변수로 받는 함수 하나로 추출할 수 있겠다.<br>

```javascript
function freeTieClip(cart) {
	var hasTie = isInCart(cart, 'tie');
	var hasTieClip = isInCart(cart, 'tie clip');

	if (hasTie && !hasTieClip) {
		var tieClip = make_item('tie clip', 0);	    // 넥타이 핀을 0원으로 생성
		return add_item(cart, tieClip);		        // 카트에 넥타이 핀 추가
	}

	return cart;
}

function isInCart(cart, name) {
	for (var i = 0; i < cart.length; i++) {
		if (cart[i].name === name) {		        // 같은 상품명이 있는 것을 확인하면 바로 true값 return
			return true;
		}
	}
	
	return false;
}
```

<br>
이렇게 카트 내에 같은 상품명의 제품이 있는지 확인하는 반복문을 추출함으로 우리는 <br>
각 함수간의 추상화 레벨을 맞출 수 있다.<br>
이제 freeTieClip 함수는 어디서든 사용할 수 있는 비슷한 추상화 레벨의 함수 몇 가지로 이루어진 함수가 됐다.<br>
더 이상 해당 함수에 종속된 코드는 없다고 볼수 있다.<br>
그리고 이렇게 같은 추상화 계층의 함수로만 이루어진 형태를 <b>직접 구현 패턴</b> 이라고 한다.<br>
<br>

---

<br>
만약 이러한 함수를 새로 정의한다고 해보자<br>
remove_item_by_name()<br>
<br>
함수명을 보면 어떤 함수인지 가늠할수 있다.<br>
우선 상품명으로 카트에서 상품을 제거하는 기능을 할 것이다.<br>
책에서 제시했던 함수 중에 비슷한 것이 무엇이 있었을까.<br>
<br>
add_item(), make_item(), isInCart()..<br>
<br>
이 함수들은 <b>상품을 다루는 계층</b>의 함수들이다.<br>
그렇다면 우리가 새로 정의할 remove_item_by_name() 함수와 적어도 <b>비슷한 계층의 함수가 될 것임</b>을 알 수있다.<br>
<br>
그렇다면 위에서 설명한 freeTieClip() 함수와 같은 계층일까?<br>
아닌것으로 보인다.<br>
freeTieClip()은 상품과 카트를 다루는 여러 계산이 합쳐지면서 만들어진 함수이다.<br>
상품을 다루기만 하는 함수보다는 높은 계층, 하나의 큰 계산이라고 할 수 있다.<br>
방금 다룬 것처럼 잘 정리된 함수로 정의한 계층은 이후 추가될 애플리케이션의 확장성에 영향을 줄수 있다.<br>
<br>

![02](/assets/img/05.Functional-coding/08/02.png)

이렇게 계층을 나눌 때 중요한 포인트가 몇 가지 있다.<br>

1. <b>모든 함수는 계층을 그래프로 표현할 수 있어야 한다.</b><br>
2. <b>함수 안에서 다른 함수를 호출한다면 반드시 확인하고 계층을 나누어야 한다.</b><br>
3. <b>화살표(호출)은 옆이나 위가 아닌 아래로 향해야만 한다.</b><br>

<br>
위의 규칙을 지킨 상태로 여태 책에 나온 여러 함수의 계층을 나눈 그래프는 이러하다.<br>
<br>

![03](/assets/img/05.Functional-coding/08/03.png)

<br>
자, 이제 remove_item_by_name() 함수를 정의해보자<br>

```javascript
function remove_item_by_name(cart, name) {
	var idx = null;
	
	for (var i = 0; i < cart.length; i++) {
		if (cart[i].name === name) {
			idx = i;
		}
	}

	if (idx != null) {
		return removeItems(cart, idx, 1);
	}

	return cart;
}
```

그렇다면 위에 정의된 <b>remove_item_by_name() 함수는 직접 구현 패턴을 유지하고 있는가?</b><br>
<b style="color: red;">아니다.</b><br>
반복문을 이용하면서 동시에 removeItems라는 그보다 상위 계층인 removeItems() 함수를 호출한다.<br>
직접 구현 패턴의 형태를 띄기에는 서로 다른 계층의 코드를 이용하는 문제가 있다.<br>
이 문제를 해결하기 위해 책에서는 같은 상품명의 카트 내 index를 찾아주는 반복문 부분을 추출해서 함수화하는 방법을 제안한다.<br>
<br>
그렇게 되면 코드는 이렇게 변경된다.<br>

```javascript
function remove_item_by_name(cart, name) {
	var idx = indexOfItem(cart, name);

	if (idx != null) {
		return removeItems(cart, idx, 1);
	}

	return cart;
}

function indexOfItem(cart, name) {
	for (var i = 0; i < cart.length; i++) {
		if (cart[i].name === name) {
			return i;
		}
	}

	return null;
}
```

![04](/assets/img/05.Functional-coding/08/04.png)

<br>
위처럼 나누면 indexOfItem()과 removeItems() 가 같은 계층의 함수 같아 보이지만 사실은 아니다.<br>
왜냐하면 indexOfItem()은 name 이라는 데이터의 속성 을 가지고 있어야만 하기 때문이다.<br>
하지만 removeItems() 는 데이터가 어떤 형태인지, 알지 않아도 된다.<br>
즉, indexOfItem() 이 조금 더 고차원의 함수라는 것을 알 수 있다.<br>
<br>
이러한 방식을 적용하여 함수 하나를 리팩토링 하려 한다.<br>

```javascript
function setPriceByName(cart, name, price) {
	var cartCopy = cart.slice();
	var idx = indexOfItem(cart, name);

	if (idx !== null) {
		cartCopy[idx] = setPrice(cartCopy[idx], price);
	}

	return cartCopy;
}
```

위 함수를 리팩토링 진행할 것인데, 우선 문제를 몇 가지 짚어보자.<br>

1. 배열을 복사하는 과정은 Javascript 소스를 사용하는 가장 하단 계층이다.(cart.slice())<br>
이 부분을 함수화 하여 계층을 평준화 하자.<br>
2. 데이터를 복사하는 과정은 결국 데이터가 변경될 시 기존의 데이터를 안전하게 하기 위함이다.<br>
하지만 현재 가격을 바꿀 상품의 상품명이 같은 상품이 카트에 존재하지 않으면 그저 복사만 하고 반환한다.<br>
위 부분도 수정하자.<br>
3. index가 null 이 아닐 경우 복사된 카트의 데이터를 직접 바인딩해주는 코드가 존재한다.<br>
위 부분을 배열의 index에 맞춰 상품을 반환하는 함수로 추출해보자.<br>
<br>
그렇게 되면 2개의 함수를 추가로 정의하게 되는데,<br>

```javascript
function setPriceByName(cart, name, price) {
	var i = indexOfItem(cart, name);
	
	if (i !== null) {
		// 배열과 요소의 인덱스를 주면 반환하는 함수로 3번의 문제를 해결
		var item = arrayGet(cart, i);
		// 배열의 데이터가 변경될때 복사하며 안전하게 변경함으로 1, 2 번 문제를 해결
		return arraySet(cart, i, setPrice(item, price));
	}

	return cart;
}

// 배열 cart에서 주어진 name에 맞는 요소를 찾고 요소의 index 값을 반환, 없을 경우 null 반환
function indexOfItem(cart, name) {
	for (var i = 0; i < cart.length; i++) {
		if (arrayGet(cart, i).name === name) {
			return i;
		}
	}

	return null;
}

// 매개변수인 array를 복사하고 요소를 변경하여 반환
function arraySet(array, idx, value) {
	var copy = array.slice();
	copy[idx] = value;

	return copy;
}

// 주어진 배열에서 index에 맞는 요소를 반환
function arrayGet(array, idx) {
	return array[idx];
}
```

<br>

## ● 직접 구현 패턴 리뷰

1. <b>직접 구현한 코드는 한 단계의 구체화 수준에 관한 문제만 해결한다.</b><br>
2. <b>계층형 설계는 특정 구체화 단계에 집중할 수 있게 도와준다.</b><br>
3. <b>호출 그래프는 구체화 단계에 대한 풍부한 단서를 보여준다.</b><br>
예시: <br>

![03](/assets/img/05.Functional-coding/08/03.png)
<br>

4. <b>함수를 추출하면 더 일반적인 함수로 만들 수 있다.</b><br>
5. <b>일반적인 함수가 많을수록 재사용하기 좋다.</b><br>
6. <b>복잡성을 갑추지 않는다.</b><br>