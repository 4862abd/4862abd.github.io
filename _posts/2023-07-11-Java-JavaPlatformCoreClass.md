---
title: Java platform core Class
author: park
date: 2023-07-11 15:46:00 +0800
categories: [Java, Java platform core Class]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/18.Lv1-FailureRate-59/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

이번 챕터 시작 키워드: <b>GC -> JVM -> JRE -> Java platform core classes</b><br/>
키워드 출처: https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html<br/>

---


### 소제목 목록
● 들어가며<br/>
● JRE<br/>
● Java platform core Classes<br/>
ㄴ java.lang<br/>
ㄴ java.util<br/>
ㄴ java.io<br/>

---

### ● 들어가며

원래는 <b>GC(Garbage Collection)</b>를 공부하려 했다.<br/>
그리고 GC를 파다가 나온 <b>JVM(Java Virtual Machine)</b> 공부.<br/>
<br/>
JVM 마다 GC의 변천사가 있었고, 그에따라 JVM 을 공부하던 중,<br/>
<b>JRE(Java Runtime Edition)</b> 가 나왔다.<br/>
<br/>
그리고 JRE와 JDK를 구성하는 것중 하나인 <b>Java platform core Classes</b> 가 눈에 띄었고, 그것을 정리할 것이다.<br/>
위의 내용들도 내게 기본 데이터들이 쌓이는대로 포스팅 할것이다.<br/>
<br/>

### ● JRE

![JRE](/assets/img/04.Java/01.2023-07-11-Java-JavaPlatformCoreClass/01.JDK.png)<br>

<i>위의 사진은 자바의 신 서적에도 나온 것으로, Java를 익히는데 중요한 키워드가 많다.</i><br/>
<br>
JRE는 위의 사진에서처럼 JDK보다는 적은 기술을, JSE보다는 많은 기술을 지원한다.<br/>
그 중에서도 핵심이라고 할 수 있는 3가지가 있다.<br>
(출처: https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)<br/>
<br/>

1. <b>Java platform core classes</b><br/>
2. <b>Java platform libraries</b><br/>
3. <b>JVM</b><br/>

위 세 가지는 Java 애플리케이션 구동시 전부 필요하다.<br/>
Java 7에서는 Java 애플리케이션이 운영체제에서 Desktop Application<b style="color: red;">*</b> 처럼 구동하지만<br/>
Java Web Start를 사용할 때 웹에서 다운로드 되거나, 브라우저에서 Web Embedded 응용 프로그램(JavaFX 사용)<b style="color: red;">*</b> 으로 실행된다.<br/>

이번 챕터는 그 중에서도 1번, <b>Java platform core classes</b> 를 다룰 것이다.<br/>
<br/>

---

<br/>

### ● Java platform core Classes

자바 플랫폼 핵심 클래스들을 말하는 것으로, 여러가지가 포함되어 있다.<br/>
하지만 핵심으로 뽑는 package가 몇 개 있는데 나열해보겠다.<br/>

1. <b>java.lang</b><br/>
2. <b>java.util</b><br/>
3. <b>java.io</b><br/>

어디서 들어본 것 같지 않은가?<br/>
분명, 분명히 Java를 한 번이라도 접해봤다면 모를 수가 없을 것이다.<br/>
<br/>
그러면 각 package의 대표 class를 알아보자.<br/>
<br/>
<br/>

### ● java.lang

스크롤을 여기까지 내릴 시간을 주었다.<br/>
이제는 기억이 나야한다.<br/>
<br/>
<b>Object</b><br/>
<br/>
지금이라도 잘 외워둬라.<br/>
모든 Java 객체의 뿌리 클래스이다.(이 표현은 documetary에 나온 방식이다.)<br/>
<br/>
<b>String</b><br/>
문자를 담은 문자열 데이터 타입.<br/>
가끔 String은 기본자료형으로 보고 '여러 기능이 존재하는 기본자료형이구나' 하는 사람들이 있는데,<br/>
절대 아니다.<br/>
String은 참조자료형으로 클래스이다.<br/>
<br/>
<b>박싱과 언박싱</b><br/>

1. Integer, Double, Boolean 등<br/>

Java 애플리케이션을 프로그래밍 시, 자주 접하게 되는 개념이 있는데 바로 박싱과 언박싱.<br/>
심지어 개발자 당사자가 제대로 알지도 못하고 이용하는 경우가 빈번하다.<br/>
간단하게만 설명하면 기본자료형을 참조자료형으로 변환 하는 것이 <b>박싱</b>, 참조자료형을 기본자료형으로 변환 하는 것을 <b>언박싱</b> 이라고 한다.<br/>
이러한 박싱 언박싱에는 묵시적인 방법과 명시적인 방법이 있으며, 간단하게 이러하다.<br/>

```java
// 묵시적 박싱
int intValue = 10;
Integer integerValue = intValue;

// 묵시적 언박싱
int intValue2 = integerValue;

// 명시적 박싱
Integer integerValue2 = (Integer) intValue;

// 명시적 언박싱
int intValue3 = (int) intValue;
```


> 위의 코드는 학습용 예제이다, 그러니까..<br/>
> 절대 위처럼 <b>변수명</b> 짓지 마라.<br/>
{: .prompt-warning }