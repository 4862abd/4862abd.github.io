---
title: 9장, 계층형 설계 Ⅱ - 3
author: park
date: 2023-08-01 17:30:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 9장 계층형 설계 Ⅱ - 3.

### 소제목 목록
● 그래프로 알 수 있는 코드에 대한 정보는 무엇이 있을까요?<br/>
● 그래프의 가장 위에 있는 코드가 고치기 가장 쉽습니다.<br/>
● 아래에 있는 코드는 테스트가 중요합니다.<br/>
● 아래에 있는 코드가 재사용하기 더 좋습니다.<br/>
● 요약: 그래프가 코드에 대해 알려주는 것.<br/>

---

## ● 그래프로 알 수 있는 코드에 대한 정보는 무엇이 있을까요?

![07](/assets/img/05.Functional-coding/09/07.png)
 
<br/>
<b>호출 그래프</b>의 구조이다.<br/>
이 그래프는 세 가지 중요한 비 기능적 요구사항을 보여준다.<br/>
<br/>

> <b>기능적 요구사항(Functional Requirements)</b><br/>
> 애플리케이션이 정확히 해야 하는 일을 의미한다.<br/>
> <br/>
> <b>비기능적 요구사항(Non-Functional Requirements, NFRs)</b><br/>
> 테스트를 어떻게 하고, 재사용할 수 있는지, 유지보수하기 어렵지 않은가 등의<br/>
> 기능적인 영역을 벗어난 요구사항<br/>
{: .prompt-info }

<br/>
비기능적 요구사항은 소프트웨어를 설계하는 중요한 이유이다.<br/>
호출 그래프로 알 수 있는 세 가지 비기능적 요구사항을 알아보자.<br/>
<br/>

1. <b>유지보수성(maintainability)</b>: 요구 사항이 바뀌었을 때 가장 쉽게 고칠 수 있는 코드는 어떤코드인가?<br/>
2. <b>테스트성(testability)</b>: 어떤 것을 테스트하는 것이 가장 중요한가?<br/>
3. <b>재사용성(reuseability)</b>: 어떤 함수가 재사용하기 좋은가?<br/>

---

## ● 그래프의 가장 위에 있는 코드가 고치기 가장 쉽습니다.

<br/>

![08](/assets/img/05.Functional-coding/09/08.png)
<br/>

제목 그대로 이다.<br/>
아래에 있는 코드일수록 본인의 상위에서 호출하는 횟수가 많아진다.<br/>
즉, <b>아래에 있을수록 의존성이 많이 걸려있는 코드이므로 변경 시에 많은 변수를 가질 수 있다.</b><br/>
그러한 이유 때문에 호출 당하는 횟수가 가장 적은, 제일 상단에 있는 코드는 다른 코드에 영향을 주지 않으며 가장 고치기 쉽다.<br/>
하지만 바뀌는 것이 많은 상태의 함수가 상단 Layer 에 위치하는 코드를 적게 유지하는 것이 좋다.<br/>
<br/>

---

<br/>

## ● 아래에 있는 코드는 테스트가 중요합니다.

![09](/assets/img/05.Functional-coding/09/09.png)
<br/>

모든 코드를 테스트하는 것은 현실적이지 않다.<br/>
모든 컷을 테스트할 수 없다면 장기적으로 좋은 결과를 얻기 위해서 가장 하단의 코드들을 테스트 하는 것이 중요하다.<br/>
<br/>
<b>문제점 1.</b> 물론 상위코드를 테스트 하게 되면 하위코드들도 같이 테스트 될 것이다.<br/>
하지만 하위코드는 중복해서 호출될 것이고, 그렇다면 같은 자원을 이용해 테스트를 진행하는 경우 더 적은 케이스만 테스트할 수 있을 것이다.<br/>
<br/>
<b>문제점 2.</b> 그리고 애플리케이션의 설계가 확실하다면 하단의 코드보다 상단의 코드가 더 많이 바뀔 것이다.<br/>
바로 위에서 짚은 점처럼, 상단의 코드는 하단의 코드보다 변경될 기회에 더 노출되어 있고, 그렇게되면 테스트도 다시 해야할 것이다.<br/>
<br/>
<b>그렇기에 가장 많이 호출되는 하단의 코드들을 테스트 하는 것이 좋다.</b><br/>
<br/>

---
<br/>

## ● 아래에 있는 코드가 재사용하기 더 좋습니다.

너무나도 당연한 말이다.<br/>
상단의 코드는 변경의 기회에 노출될 뿐 아니라 테스트 진행에도 문제가 있다.<br/>
하지만 변경될 기회도 적고, 테스트하기 쉬운 하단의 코드는 어디서든 재사용하기 쉽다.<br/>
실제로 그림을 보면 본인을 호출하여 의존하는 상위의 코드가 많은 하단의 코드들이 눈에 띈다.<br/>
<br/>
즉, <b>아래쪽으로 가리키는 화살표가 많은 함수는 그렇지 않은 함수보다 재사용하기 어렵다.</b><br/>
<br/>

---

<br/>

## ● 요약: 그래프가 코드에 대해 알려주는 것.
<br/>

![10](/assets/img/05.Functional-coding/09/10.png)

<b>유지보수성</b><br/>
<br/>
규칙: 위로 연결된 것이 적은 함수가 바꾸기 쉽다.<br/>
핵심: 자주 바뀌는 코드는 가능한 위쪽에 있어야 한다.<br/>
<br/>
<b>테스트 가능성</b><br/>
<br/>
규칙: 위쪽으로 많이 연결된 함수를 테스트하는 것이 더 가치 있다.<br/>
핵심: 아래쪽에 있는 함수를 테스트하는 것이 위쪽에 있는 함수를 테스트하는 것보다 가치 있다.<br/>
<br/>
<b>재사용성</b><br/>
<br/>
규칙: 아래쪽에 함수가 적을수록 더 재사용하기 좋다.<br/>
핵심: 낮은 수준의 단계로 함수를 빼내면 재사용성이 더 높아진다.<br/>