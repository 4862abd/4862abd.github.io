---
title: 11장, 일급 함수 Ⅱ
author: park
date: 2023-08-04 18:30:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 11장 일급 함수 Ⅱ.

### 소제목 목록

● 들어가며<br/>
● 배열에 대한 카피-온-라이트 리팩터링<br/>
● 함수를 리턴하는 함수<br/>

<br/>

---

## ● 들어가며

![00](/assets/img/05.Functional-coding/11/00.jpg)

이 챕터를 진행할 수록 토비의 스프링 3.1 교재가 생각이 났다.<br/>
공교롭게도 두 책 모두 코드의 성능과 중복을 줄이기 위해 콜백 패턴을 이용했다.<br/>
참, 재밌다.<br/>
<br/>

---

<br/>

## ● 배열에 대한 카피-온-라이트 리팩터링

이전에 카피-온-라이트 패턴과 함수 본문 콜백으로 바꾸기를 배웠다.<br/>
책에서 왜 두 키워드를 다시 꺼내왔을까?<br/>
챕터의 서두에서 카피-온-라이트 패턴의 1차원적 모습을 보고 코드 냄새를 거론한다.<br/>
카피-온-라이트 패턴의 코드를 보자.<br/>
<br/>

```javascript
function arraySet(array, idx, value) {
	var copy = array.slice();	// 복사
	copy[idx] = value;		    // 본문(수정)
	return copy;		        // 반환
}

function push(array, element) {
	var copy = array.slice();	// 복사
	copy.push(element);		    // 본문(수정)
	return copy;		        // 반환
}
```

카피-온-라이트 패턴의 특징은<br/>

1. 복사본을 만든다.<br/>
2. 복사본을 수정한다.<br/>
3. 복사본을 반환한다.<br/>

위 3가지 차례 중, 같은 데이터 타입을 수정하는 함수들이 많다면 함수 간 <b>차이점은 "2번. 복사본을 수정한다" 밖에 없다.</b><br/>
즉, 같은 데이터 타입의 데이터를 복사하고 반환하는 코드는 카피-온-라이트 패턴에서 반복될 코드라는 것이다.<br/>
이 코드 냄새를 책에서는 <b>함수 본문을 콜백으로 바꾸기</b> 로 해결한다.<br/>
그리고 추상화를 진행할수록 일반적으로 사용하는 상황이 발생하므로 함수명도 일반적으로 바꿔보자.<br/>
<br/>

```javascript
// 리팩터링 전
function arraySet(array, idx, value) {
	var copy = array.slice();	// 복사
	copy[idx] = value;		    // 본문(수정)
	return copy;		        // 반환
}

...

// 리팩터링 후
function arraySet(array, idx, value) {
    return withArrayCopy(array, function(copy) {
        copy[idx] = value;
    });
}

// 카피-온-라이트 원칙을 따르고 재사용 가능한 함수.
function withArrayCopy(array, modify) {
    var copy = array.slice();
    modify(copy);               // 콜백으로 받은 본문 함수를 실행
    return copy;
}
```

이렇게 카피-온-라이트 원칙을 지킨 함수를 추출함으로 우리는 여러 이점을 얻을 수 있다.<br/>
보기만 하면 코드가 오히려 늘어서 이게 리팩터링이 맞나 싶을 수 있다.<br/>
하지만 확실한 장점이 생겼다.<br/>
이제 <b>복사하는 코드를 여러 곳에서 관리하지 않아도 된다.</b><br/>
<br/>
만약 이러한 동작이 필요한 로직이 있다고 해보자.<br/>

1. 배열의 첫 요소를 제거<br/>
2. 새로운 요소를 추가<br/>
3. 새로운 요소를 한 개 더 추가<br/>
4. 배열의 특정 인덱스가 가리키는 요소의 값을 변경<br/>

<br/>
리팩터링 이전과 이후의 로직을 보여주겠다.<br/>

```javascript
// 리팩터링 이전
var a1 = drop_first(array);
var a2 = push(a1, 10);
var a3 = push(a2, 11);
var a4 = arraySet(a3, 0, 42);

...

// 리팩터링 이후
var a4 = withArrayCopy(array, function(copy) {
    copy.shift();
    copy.push(10);
    copy.push(11);
    copy[0] = 42;
});

// 카피-온-라이트 원칙을 따르고 재사용 가능한 함수.
function withArrayCopy(array, modify) {
    var copy = array.slice();
    modify(copy);               // 콜백으로 받은 본문 함수를 실행
    return copy;
}
```

리팩터링 이전의 소스를 보면,<br/>
복사만 4개를 하고 4개의 배열이 새로 생성된다.<br/>
<b>이와 다르게</b> 리팩터링 이후의 로직을 보면 배열 한 개만 새로 생성하여 복사본에 모든 수정을 진행한다.<br/>
이게 이 리팩터링의 목표였다.<br/>
<br/>
GC의 성능을 무시하는 것이 아니다.<br/>
굳이 같은 코드를 여러 곳에서 관리하고 굳이 메모리를 낭비하는 필요를 줄인 것이다.<br/>
<br/>

---
<br/>

## ● 함수를 리턴하는 함수

우리는 이전에 try/catch 블록을 이용하고 동작 실패 시 로그를 남기는 로직을 정리한 적이 있다.<br/>
당시에 try/catch 블록의 중복을 획기적으로 줄이긴 했지만 만족할 수 없었다.<br/>
왜냐하면 현재 이러한 상태이기 때문이다.<br/>

![01](/assets/img/05.Functional-coding/11/01.png)

단순히 코드 라인의 수를 줄인 것이 다였다.<br/>
이 코드마저 한 번에 관리하고 싶다는 것이 이번의 요구사항이다.<br/>
<br/>
<b>요구사항</b>: 여러 곳에서 호출되어 각자 관리해줘야 하는 로깅함수를 한 번에 관리하기.<br/>
<br/>
목표:<br/>

![02](/assets/img/05.Functional-coding/11/02.png)

이에 책에서는 직전에 배운 <b>고차 함수</b> 를 이용하자고 한다.<br/>
우선 기존의 코드는 이러했다.<br/>

```javascript
try {
    saveUserData(user);
} catch (error) {
    logToSnapErrors(error);
}

...

try {
    fetchProduct(productId);
} catch (error) {
    logToSnapErrors(error);
}
```

<br/>
우선 로그를 남기지 않는 즉, 무사히 성공한 상태를 확실하게 표현할 수 있게 try 블록에 들어갈 함수의 명을 고치자.<br/>
그리고 이를 포장하고 있는 withLogging 함수의 명도 확실하게 변경하자.<br/>
<br/>

```javascript
function saveUserDataWithLogging(user) {        // sufFix로 WithLogging 을 붙여 로깅 동작과 같이 구현한 함수임을 밝혔다.
    try {
        saveUserDataNoLogging(user);            // sufFix로 NoLogging 을 붙여 성공 시 로그를 남기지 않음을 밝혔다.
    } catch (error) {
        logToSnapErrors(error);
    }
}

...

function fetchProductWithLogging(productId) {    // sufFix로 WithLogging 을 붙여 로깅 동작과 같이 구현한 함수임을 밝혔다.
    try {
        fetchProductNogging(productId);          // sufFix로 NoLogging 을 붙여 성공 시 로그를 남기지 않음을 밝혔다.
    } catch (error) {
        logToSnapErrors(error);
    }
}
```

<br/>
이제 본문을 제외한 곳의 코드들의 중복을 줄여보자.<br/>
우선 함수의 본문을 추출한 코드를 담은 함수에 이름이 없다고 생각하고(익명함수) 이를 포장한 함수의 이름도 일반적으로 바꾼다.<br/>
<br/>

```javascript
function wrapLogging(func) {                    // 함수를 인자로 받는다.
    return function(arg) {                      // 함수를 리턴하고 함수로 감싼 코드는 나중에 실행된다.
        try {
            func(arg);
        } catch (error) {
            logToSnapErrors(error);
        }
    }
}

...

// 호출
// 리턴값을 변수에 할당해 이름을 붙인다.
// 로그를 남기지 않는 함수를 변환하기 위해 wrapLogging() 함수를 호출한다.
var saveUserDataWithLogging = wrapLogging(saveUserDataNoLogging);
```

<br/>
이제 로그를 남기지 않는 버전(함수)을 로그를 남기는 버전으로 쉽게 바꿀 수 있다.<br/>
이렇게 말이다.<br/>
<br/>

```javascript
var saveUserDataWithLogging = wrapLogging(saveUserDataNoLogging);
var fetchProductWithLogging = wrapLogging(fetchProductNoLogging);
```

<br/>
또한 중복도 없어졌다.<br/>
어떤 함수도 로그를 남기는 버전으로 쉽게 바꿀 수 있다.<br/>
<br/>
이를 그래프로 표현하면 이렇게 된다.<br/>

![03](/assets/img/05.Functional-coding/11/03.png)

<i>책에서는 이렇게 한 함수로 묶은 함수의 강력함을 슈퍼 파워라고 명명했다.</i><br/>
<i style="font-size: 5px;">모듈에 슈퍼맨 망토도 달아뒀다.. 귀엽..</i><br/>