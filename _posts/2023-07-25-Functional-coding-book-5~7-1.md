---
title: 5장 ~ 7장, 카피-온-라이트(copy-on-write), 방어적 복사(defensive copy)
author: park
date: 2023-07-26 16:44:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 카피-온-라이트(copy-on-write) 규칙
### (얕은 복사(shallow copy)의 성질을 띄는 구조적 공유(structural sharing)를 위한)

목적: 기존의 데이터를 직접 수정하지 않고 안전하게 복사하는 방식<br>
1. <b>복사본 만들기</b><br>
2. <b>복사본 변경하기</b><br>
3. <b>복사본 리턴하기</b><br>

<br>

카피-온-라이트는 데이터가 바뀔 때마다 복사를 하고, 이러한 카피-온-라이트는 안전지대를 만든다.<br>
안전지대(safe zone)에서 동작하는 코드가 안전지대를 만드는 형태이다.<br>
쓰기를 읽기로 바꾸어 읽기는 모두 계산(변하지 않는 데이터를 읽는 행위)이 된다.<br>
<br>

### 카피-온-라이트 대표 코드<br>

```javascript
// 호출부
function main() {
	...
	list = getChangedList(list, name);

	...
}

// 배열을 복사해서 변경해야할 객체를 찾고 필요시 수정 함수를 호출, 그 후 반환한다.
function getChangedList(list, name) {
	let copy = list.slice();
	let idx = null;

	for (let i = 0; i < copy.length; i++) {
		if (copy[i].name == name) {
			idx = i;
		}
	}

	if (idx != null) {
		copy[idx] = getChangedObj(copy[idx], name);
	}

	return copy;
}

// 파라미터로 받은 객체를 복사한 후, 복사한 객체의 값을 바꿔 반환한다.
function getChangedObj(obj, name) {
	let copy = Object.assign({ }, obj);
	copy['name'] = name;
	return copy;
}
```
<br>

```java
// 호출부
public void testMethod() {
	...
	list = this.getChangedList(list, name);

	...
}

// 배열을 복사해서 변경해야할 객체를 찾고 필요시 수정 함수를 호출, 그 후 반환한다.
public List<객체> getChangedList(List<객체> list, String name) {
	List<객체> copy = new ArrayList<>(list);
	int idx = -1;

	for (int i = 0; i < copy.size(); i++) {
		if (copy.get(i).getName().equals(name)) {
			idx = i;
		}
	}

	if (idx != -1) {
		copy.set(idx, this.setChangedObj(copy.get(idx), name));
	}

	return copy;
}

// 파라미터로 받은 객체를 복사한 후, 복사한 객체의 값을 바꿔 반환한다.
public 객체 getChangedObj(객체 obj, String name) {
	// 복사하는 여러 방법 등(나는 롬복의 builder 패턴을 쓰겠다.)
	객체 copy = 객체.builder()
		.요소(obj.get요소())
		.요소(obj.get요소())
		.build();
	return copy;
}
```

<br>

---

<br>

## 방어적 복사(defensive copy) 규칙
### (깊은 복사(deep copy))

목적: 안전지대(safe zone)의 데이터를 확인할수 없는 코드를 거치거나 거칠 때 그것의 불변성을 안전하게 지키는 방식<br>

1. <b>데이터가 안전한 코드에서 나갈 때 복사하기</b><br>
	1-1. 불변성 데이터를 위한 깊은 복사본을 만든다.<br>
	1-2. 신뢰할 수 없는 코드로 복사본을 전달한다.<br>
<br>

2. <b>안전한 코드로 데이터가 들어올 때 복사하기</b><br>
	2-1. 변경될 수 있는 데이터가 들어오면 바로 깊은 복사본을 만들어 안전한 코드로 전달한다.<br>
	2-2. 복사본을 안전한 코드에서 사용한다.<br>

또한 방어적 복사와 관련된 코드를 감싸면(wrapping) 더 좋은 코드가 될 수 있다.<br>
<br>
<b>비공유 아키텍처(shared nothing architecture)</b>를 구현하기 좋다.<br>

> 공유 자원을 이용하는 프로세스가 늘어나도 정보공유를 위한 오버헤드만 증가할 뿐 무한으로 처리율이 향상되지 않는 <b>shared architecture의 단점을 극복하기 위해 고안된 architecture</b><br>
> 네트워크 이외의 자원을 모두 분리하는 방식, 서버와 저장소의 세트를 늘리면 병렬처리 때문에 선형적으로 성능이 향상되는 장점이 있다.<br>
> <br>
> <b>장점</b>: 비용 대비 성능이 좋다.<br>
> 같은 구성의 프로레스를 횡으로 나열하기 때문에 구조가 간단하고 원칙적으로 프로세스에 따라 서버가 증가한다.<br>
> <br>
> <b>단점</b>: 저장소를 공유하지 않는다. 즉, 각각의 서버가 동일한 1개의 데이터에 접근할 수 없다.<br>
> 
{: .prompt-info }

<br>

### 깊은 복사 대표 코드<br>

```javascript
function deepCopy(thing) {
	if (Arrays.isArray(thing)) {
		var copy = [];
		for (var i = 0; i < this.length; i++) {
			// 재귀를 통한 복사
			copy.push(deepCopy(this[i]));
		}
		return copy;
	} else if (thing === null) {
		return null;
	} else if (typeof thing === 'object') {
		var copy = { };
		var keys = Objes.keys(thing);
		for (var i = 0; i < keys.length; i++) {
			var key = keys[i];
			// 재귀를 통한 복사
			copy[key] = deepCopy(thing[key]);
		}
		return copy;
	} else {
		// 문자열, 숫자, 불리언, 함수는 불변형이기 때문에 복사할 필요없이 반환하면 된다..
		return thing;
	}
}
```
