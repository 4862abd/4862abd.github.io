---
title: 9장, 계층형 설계 Ⅱ - 2
author: park
date: 2023-08-01 15:30:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 9장 계층형 설계 Ⅱ - 2.

### 소제목 목록
● 패턴 3: 작은 인터페이스(Minimal Interface)<br/>
● 패턴 3 리뷰: 작은 인터페이스<br/>

---

## ● 패턴 3: 작은 인터페이스(Minimal Interface)

이 패턴은 새로운 코드를 추가할 위치에 관한 것이다.<br/>
인터페이스를 최소화하면 하위 계층에 불필요한 기능이 쓸데없이 커지는 것을 막을 수 있다고 한다.<br/>
<br/>

---

### 예시 케이스 1: 시계 할인하기

![04](/assets/img/05.Functional-coding/09/04.png)

<br/>
마케팅 담당자가 이번에 새로운 마케팅 전략을 가져왔다.<br/>
장바구니 총합이 100$를 넘고 시계를 구매한다면 시계의 가격을 10% 할인해주는 것이다.<br/>
<br/>
<b>요구사항</b>: 장바구니 총합이 100$를 넘고 시계를 구매하는 고객에 한해 시계 가격을 10% 할인해주기.<br/>
<br/>
이를 위해 할인해주는 코드를 추가하려 하는데 이를 구현하기에 앞서 두 가지 방법을 설명한다.<br/>
<br/>

![05](/assets/img/05.Functional-coding/09/05.png)

#### 방법 1: 추상화 벽에 만들기

추상화 벽 계층에 있으면 해시 맵 데이터 구조로 되어 있는 장 바구니에 접근할 수 있다.<br/>
하지만 <b>같은 계층의 기능을 이용할 수 없으니 직접 코드로 구현</b>해야 한다.<br/>

```javascript
function getsWatchDiscount (cart) {
	var total = 0;
	// 장바구니의 모든 상품의 정보로
	var names = Object.keys(cart);

	// 상품의 가격을 반복하여 더해준다.
	for (var i = 0; i < names.length; i++) {
		var item = cart[names[i]];
		total += item.price;
	}

	// 장바구니 상품 가격의 합이 100이 넘고 시계를 포함하고 있으면 true를 반환한다.
	return total > 100 && cart.hasOwnProperty('watch');
}
```

---

#### 방법 2: 추상화 벽 위에 만들기

추상화 벽 위에 만들면 해시 맵 구조인 장바구니를 직접 건드릴 수 없다.<br/>
이를 위해 정의해두었던 <b>더 낮은 Layer의 함수를 호출</b>하여 계산을 만들거나 더 낮은 레벨의 새로운 계산을 추가 정의하여 호출하여 사용해야 할 것이다.<br/>

```javascript
function getsWatchDiscount (cart) {
	// 장바구니 상품 가격의 합
	var total = calcTotal(cart);
	// 장바구니에 시계 상품이 있는가
	var hasWatch = isInCart('watch');

	// 모두 true라면 true 반환
	return total > 100 && hasWatch;
}
```

#### 추상화 벽 위에 있는 계층에 구현하는 것이 더 좋다.

<br/>
첫 번째 방법을 채택하면 하면 하위 Layer의 코드가 늘어난다.<br/>
또한 첫번 째 방법은우리가 추상화 벽을 구현하는 <b>정책을 위반</b>하게 된다.<br/>
마케팅 팀은 코드에 낮은 Layer에 위치하는 코드에 관여하지 않고 함수명(Api 정의서)만 보고 구현을 해야하는데<br/>
첫 번째 방법을 선택하게 되면 마케팅 팀이 직접적으로 낮은 Layer에 위치한 코드를 관리하게 되는 것이다.<br/>
<br/>
그리고 위에 설명한 방법 중 두 번째인 추상화 벽 위에 있는 계층에 만드는 것이 더 직접 구현에 가깝다.<br/>
두 번째 방법을 채택함으로 마케팅 팀은 기존과 동일한 Layer 의 함수만으로 동작하는 기능을 관리할 수 있게된다.<br/>
<br/>

---


### 예시 케이스 2: 마케팅팀은 장바구니에 제품을 담을 때 로그를 남기려고 한다.

![06](/assets/img/05.Functional-coding/09/06.png)

<br/>
제품을 장바구니에 담기만하고 구매하지 않는 손님들을 분석하기 위해, 로그를 남기려 한다.<br/>
로그는 상품을 장바구니에 담을 때 남기려는 요구사항을 받았다.<br/>
<br/>
<b>요구사항</b>: 장바구니에 상품을 추가할 때, 로그 남기기<br/>
<br/>
로그를 남기는 함수는 DB에 데이터를 남겨야 하니 액션이고, 이는 미리 정의해 두었다고 하자.<br/>

```javascript
// 로그를 남기는 함수
function logAddToCart(user_id, item) {
	// 대충 액션 코드..
}
```

<br/>
머리에 떠오르는 대로 가장 쉬운 방법을 선택하면,<br/>
<br/>

```javascript
function add_item (cart, item) {
	logAddToCart(global_user_id, item);
	return objectSet(cart, item.name, item);
}
```

<br/>
기존에 장바구니에 상품을 넣기만 하던 add_item 함수에 로그를 남기는 logAddToCart 함수를 추가했다.<br/>
깔끔하고 쉬워 보인다.<br/>
<br/>
<b>그렇다면 이게 제일 나은 방법일까?</b><br/>

---

#### 코드 위치에 대한 설계 결정

<br/>
위의 코드는 편리하고 쉬워보이지만 문제가 많다.<br/>
우선 <b>기존 add_item 함수</b>는 액션, 계산, 데이터 중 <b>계산</b>에 해당했다.<br/>
언제 어떻게 호출하든 같은 데이터를 넣으면 같은 반환값을 주는 상태였다.<br/>
<br/>
하지만 액션 함수의 규칙, <b>"액션을 호출하면 해당 함수는 액션이 된다."</b><br/>
그래서 <b>로그를 남기는 액션 함수를 추가함으로 add_item 함수는 이제 액션이 되었다.</b><br/>
<br/>
기존 우리가 설계한 추상화 Layer 조차도 무시되는 것이다.<br/>
그러면 add_item을 호출하는 상위 Layer의 함수를 확인하자.<br/>

```javascript
function update_shipping_icons (cart) {
	// 화면의 버튼 가져오기
	var buttons = get_buy_buttons_dom();
	
	for (var i = 0; i < buttons.length; i++) {
		var button = buttons[i];
		var item = button.item;
		// 카피-온-라이트 패턴
		// ※※ 여기에 로그를 남기는 액션을 추가한다면? ※※
		var new_cart = add_item(cart, item);

		if (gets_free_shipping(new_cart)) {
			buttons.show_free_shipping_icon();
		} else {
			buttons.hide_free_shipping_icon();
		}
	}
}
```

<br/>
add_item 함수를 호출하는 update_shipping_icons 함수에서 add_item 함수 호출 직전에 로그를 남긴다면?<br/>
좋은 생각일까?<br/>
<br/>
아니다.<br/>
우선 낮은 Layer에 위치하는 update_shipping_icons 함수가 너무 무거워 진다.<br/>
그리고 여기서 로그를 남기면 사용자가 제품을 추가하지 않고 사용자에게 제품이 표시될 때마다 로그를 남기게 된다.<br/>
마치 물건을 담은 것처럼 말이다.<br/>
<br/>
이러한 이유들로 로그를 남기는 액션 함수는 <b>추상화 벽 위에 있는 계층에서 호출하는게 좋다.</b><br/>
그러면 update_shipping_icons 를 호출하는 상위 Layer 의 함수를 살펴보자.<br/>

```javascript
function add_item_to_cart (name, price) {
	var item = make_cart_item(name, price);
	shopping_cart = add_item(shopping_cart, item);
	var total = calc_total(shopping_cart);
	set_cart_total_dom(total);
	update_shipping_icons(shopping_cart);
	update_tax_dom(total);

	// 로그 남기기
	logAddToCart();
}
```

<br/>
<b>장바구니에 상품을 추가하는 이벤트 핸들러 함수</b>에 추가했다.<br/>
이렇게 되면 함수의 역할이 확실해지고 그로인해 <b>책임을 확실하게 지게할 수 있다.</b><br/>
또한 <b>기존의 낮은 Layer 의 함수들은 깨끗하고 단순하고 믿을 수 있는 인터페이스가 되며</b> 인터페이스가 많아져서 생기는 불필요한 변경이나 확장을 막아 준다.<br/>

---

## ● 패턴 3 리뷰: 작은 인터페이스

<br/>

1. 추상화 벽에 코드가 많을수록 구현이 변경되었을 때 고쳐야 할 것이 많다.<br/>
2. 추상화 벽에 있는 코드는 낮은 수준의 코드이기 때문에 더 많은 버그가 있을 수 있다.<br/>
3. 낮은 수준의 코드는 이해하기 더 어렵다.<br/>
4. 추상화 벽에 코드가 많을수록 팀간, 코드간 조율해야 할 것도 많아진다.<br/>
5. 추상화 벽에 인터페이스가 많으면 알아야 할 정보가 늘어나서 사용하기 어렵다.<br/>

<br/>
현재 네 개의 패턴 중 교육 단계<br/>

1. 직접 구현 <span style="color: red;">✔<span><br/>
2. 추상화 벽 <span style="color: red;">✔<span><br/>
3. 작은 인터페이스 <span style="color: red;">✔<span><br/>
4. 편리한 계층<br/>

<br/>

---
<br/>

## ● 패턴 4: 편리한 계층(Comfortable Layer)

앞서 설명한 세 가지 패턴과 다르게 조금 더 현실적이고 실용적인 Layer 이다.<br/>
<br/>
앞에 설명한 Layer 를 큰 사이즈로 설계, 구현한다면 개발자 본인이 뿌듯한 것을 넘어 강력하고 효율적일 것이다.<br/>
이러한 추상화는 가능한 일과 불가능한 일의 차이를 보여주기도 한다.<br/>
책에서 예시로든 자바스크립트 언어는 개발자가 자바스크립트 언어가 어떻게 기계어로 컴파일 되는지 이해하지 않아도<br/>
코딩을 할 수 있게 해준다.<br/>
자바스크립트 언어도 일종의 거대한 추상화 계층인 것이다.<br/>
이러한 큰 사이즈의 계층을 완성하고 유지보수 하는 것은 쉽지 않다.<br/>
만약 이렇게 계층을 만들 때, 구체적인 것을 너무 많이 알아야 하거나 코드가 지저분 하다고 느껴진다면,<br/>
패턴을 다시 적용해 보는 것도 좋은 방법이다.<br/>
<br/>
완벽에 가까운 코드를 만든다는 것은 너무 이상적인 말이다.<br/>
어느 선에서 멈추고 추상화 계층을 관리, 사용하게 될 것인데 이를 알려주는 것이 <b>편리한 계층</b>이다.<br/>