---
title: 10장, 일급함수 Ⅰ - 2
author: park
date: 2023-08-02 18:30:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 10장 일급함수 Ⅰ - 2.

### 소제목 목록
● 고차함수(Higher-order function) 만들기<br/>
● 반복문 예제: 먹고 치우기<br/>
● 리팩터링: 함수 본문을 콜백으로 바꾸기<br/>

## ● 고차함수(Higher-order function) 만들기

<b><span style="color: red;">일급 함수를 인자(매개변수)</span>로 받는 <span style="color: red;">고차 함수</span>를 만들려 한다.</b><br/>
일급(First-class) 는 인자로 전달할 수 있다는 말이며, 고차(Higher-order) 는 함수가 다른 함수를 인자로 받을 수 있다는 말이다.<br/>
즉, <b>일급 함수가 없다면 고차 함수를 만들 수 없다.</b><br/>
책에서는 이러한 <b>고차 함수를 이용해서 콜백으로 바꾸기(Replace body with callback) 리팩터링</b>을 하려한다.<br/>

---

## ● 반복문 예제: 먹고 치우기

배열을 순회하는 일반적인 반복문 두 가지가 있다.<br/>
첫 번째 반복문은 음식을 준비하고 먹는 일을 한다.<br/>

```javascript
// 준비하고 먹기
fof (var i = 0; i < foods.length; i++) {
	var food = foods[i];
	cook(food);
	eat(food);
}
```

<br/>
두 번째 반복문은 지저분한 식기를 설거지 한다.<br/>

```javascript
for (var i = 0; i < dishes.length; i++) {
	var dish = dishes[i];
	wash(dish);
	dry(dish);
	putAway(dish);
}
```

<br/>
반복문을 쓰는 두 코드는 비슷하지만 하는 일이 다르다.<br/>
코드가 완전히 같지 않아서 두 코드를 따로 호출해야 하지만 이를 하나로 구현하는 일이 가능하다면 어떠할까?<br/>
<br/>
우선 배열(음식들, 그릇들)의 크기만큼 반복문이 실행되는 것이 동일하다.<br/>

1. <b>배열을 인자로 받아 사용하는 함수로 구현하자</b><br/>

그리고 두 코드에서 다른 부분을 살펴보면, 반복문 블록 내에 있는 실행부가 될 것이다.<br/>
하나는 cook, eat 즉, 요리하고 먹고, 다른 하나는 wash, dry, putAway 즉, 씻고, 말리고, 치운다.<br/>

2. <b>이 실행부를 추출하고, 각 함수에 쓰일 음식과 그릇을 인자로 추출하자</b><br/>

```javascript
// 1. 배열을 인자로 받아 그 크기만큼 반복문 실행하는 함수
function cookAndEatFoods(foods) {
	for (var i = 0; i < foods.length; i++) {
		var food = foods[i];
		cookAndEat(food);
	}
}

// 2. 실행부를 추출, 인자로 받은 음식을 요리하고 먹기
function cookAndEat(food) {
	cook(food);
	eat(food);
}

...

// 1. 배열을 인자로 받아 그 크기만큼 반복문 실행하는 함수
function cleanDishes(dishes) {
	for (var i = 0; i < dishes.length; i++) {
		var dish = dishes[i];
		clean(dish);
	}
}

// 2. 실행부를 추출, 인자로 받은 그릇을 씻고, 말리고, 치우기
function clean(dish) {
	wash(dish);
	dry(dish);
	putAway(dish);
}
```

자, 각 반복문을 인자로 받는 형태이며 반복문 내에서 실행문을 호출하는 형태로 바꿔줬다.<br/>
이제 코드 자체의 중복이 눈에 띄게 보인다.<br/>

1. <b>그러면 각각 음식과 그릇으로 이루어진 암묵적인 인자가 들어간 함수명을 조금 더 추상화하자</b><br/>
2. <b>그리고 추출한 실행부를 인자로 하는 반복문으로 추상화하여 반복문 코드의 중복을 줄이자</b><br/>

```javascript
// 1. 2. 추상화한 배열 타입의 데이터(음식들, 그릇들) 와 추출한 실행부를 인자로 받아 반복하는 함수
function forEach(array, func) {
	for (var i = 0; i < array.length; i++) {
		var item = array[i];
		func(item);
	}
}

function cookAndEat(food) {
	cook(food);
	eat(food);
}

function clean(dish) {
	wash(dish);
	dry(dish);
	putAway(dish);
}

// 리팩터링을 진행한 함수를 호출하는 법
forEach(foods, cookAndEat);
forEach(dishes, clean);
```

<br/>
자, 이제 <b>익명 함수(Anonymous function)</b> 개념을 대입하려 한다.<br/>
익명 함수는 이름이 없는 함수이고, 필요한 곳에 <b>인라인(Inline)</b> 형태로 사용 가능하다.<br/>

```javascript
forEach(foods, function(food) {
	cook(food);
	eat(food);
});

forEach(dishes, function(dish) {
	wash(dish);
	dry(dish);
	putAway(dish);
});
```

이제 배열 전체를 순회하여 콜백 형태로 받은 함수를 실행할 수 있는 함수가 만들어졌다.<br/>
만약 처리해야할 배열과 처리하는 서비스 로직만 있으면 forEach를 통해 편하게 반복하여 실행할 수 있다.<br/>
<br/>
forEach 함수는 <b>함수를 인자로 받는 <span style="color: red;">고차 함수</span></b> 이다.<br/>
고차 함수의 좋은 점은 코드를 추상화 할수 있어서 위 코드의 예시라면 앞으로 작성할 반복문 코드 자체를 줄일 수 있다.<br/>
<br/>
앞에서 진행했던 리팩터링은 이러한 방법을 거쳤다.<br/>

1. <b>코드를 함수로 감싸기</b><br/>
2. <b>더 일반적인 이름으로 바꾸기</b><br/>
3. <b>암묵적 인자를 드러내기</b><br/>
4. <b>함수 추출하기</b><br/>

이렇게 많은 방법을 하나로 묶어서 <b>함수 본문을 콜백으로 바꾸기(Replace body with callback)</b> 이라고 한다.<br/>

---

## ● 리팩터링: 함수 본문을 콜백으로 바꾸기

![01](/assets/img/05.Functional-coding/10/01.PNG)

이제 데이터가 입출력 되는 함수들의 예외상황에 로그를 남기는 로직을 추가하려 한다.<br/>
try/catch 블록을 이용하여 로그를 남길 것이다.<br/>
<br/>
<b>요구사항</b>: 데이터의 입출력이 이루어지는 로직에 예외가 발생하면 로그를 남기는 코드를 추가<br/>

```javascript
try {
	saveUserData(user);
} catch (error) {
	logToSnapErrors(error);
}
```

하지만 데이터가 입출력 되는 함수가 이것 뿐일까..?<br/>
책에서 상황을 설명하는 그림에 등장하는 테스트 담당 조지는 이런 코드를 적용하면 총 45000 줄 고쳐야 한다고 한다.<br/>
그리고 이런 상황을 확인한 개발팀 제나는 위에서 공부한 <b>함수 본문을 콜백으로 바꾸기</b> 를 접목하면 더 적은 코드로 해결이 될 것이라고 한다.<br/>
<br/>
우선, 로그를 남기는 모든 상황에서 try/catch 블록을 선언하는건 엄청난 양의 코드 중복 즉, 코드 냄새를 만들어낸다.<br/>
방법을 찾아보자.<br/>

1. <b>그렇다면 try/catch 블록을 추상화하여 일반적인 함수명의 함수로 추출한다.</b><br/>
2. <b>그리고 예외 상황 시 로그를 남기는 시점인 데이터의 입출력이 일어나는 함수의 바디 블록의 코드를 익명 함수 형태로 인자로 보내준다.</b><br/>

<br/>
자, 그러면 코드를 확인해보자.<br/>

```javascript
// 1. try/catch 블록을 추상화하고 일반적인 함수명의 함수로 추출한다.
// 2. 콜백 함수를 받아 호출하는 try 블록을 정의한다.
function withLogging(func) {
	try {
		func();
	} catch (error) {
		logToSnapErrors(error);
	}
}

...

// 호출하여 사용하는 경우
// 본문을 전달하여 콜백 형태로 빼냅니다.
withLogging(function() {
	saveUserData(user);
});
```

---

### 이러한 콜백 문법은 여러가지 형태를 띌 수 있다.

1. <b>전역으로 정의하기</b><br/>

```javascript
function saveCurrentUserData() {
	saveUserData(user);
}

...

// 함수 이름으로 다른 함수에 전달하기
withLogging(saveCurrentUserData);
```

2. <b>지역적으로 정의하기</b><br/>

```javascript
function someFunction() {
	var saveCurrentUserData = function() {
		saveUserData(user);
	};

	// 함수 이름으로 다른 함수에 전달하기
	withLogging(saveCurrentUserData);
}
```

3. <b>인라인(Inline) 으로 정의하기</b><br/>

```javascript
// 쓰는 곳에서 바로 함수 정의, 이름이 없는 익명 함수가 된다.
withLogging(function() { saveUserData(user) });
```

### 이렇게 여러 방식으로 실행될 수 있는 이유는 인자로 하는 함수가 <b>일급 함수</b> 이기 때문에 가능한 일이다.

<b>일급 값(First-class value)</b><br/>
<b>일급 함수(First-class function)</b><br/>
<b>고차 함수(High-order function)</b><br/>
<br/>
위 세 가지는 액션, 계산, 데이터 이후에 나온 함수형 프로그래밍에서 중요한 개념 중 하나이다.<br/>
이 세 가지로 함수형 프로그래밍의 강력함을 다룰 것이다.<br/>