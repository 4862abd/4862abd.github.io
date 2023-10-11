---
title: new URL 와 URL.create 의 차이점
author: park
date: 2023-09-20 10:34:00 +0800
categories: [Java, Programming]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## new URL 와 URL.create 의 차이점

<br>
● 들어가기 전<br>
● 코드 비교<br>
● 결론<br>
<br>

---

<br>

## ● 들어가기 전

<br>
처음에는 ResponseEntity 를 공부하다가 이 주제를 발견했다.<br>
<br>

```java
return ResponseEntity.created(URI.create(uriPath)).body(result);
```

<br>
이렇게 ResponseEntity 의 create 메서드 매개변수로 URI 의 create 를 통해 획득한 인스턴스를 이용하는 것을 확인했다.<br>
위 코드 한 줄에서 내가 궁금한 점은 3 가지 였다.<br>
<br>

1. ResponseEntity 의 created 메서드<br>
2. ResponseEntity 의 ok 와 created 의 차이점<br>
3. URI 의 create 메서드<br>

<br>
그 중에서 3번의 매개변수로 이용되는 URI 를 먼저 공부하기로 결심했고,<br>
구글링 하자마자 상단에 나온 주제였다.<br>
<br>
Baeldung 에 작성된 <br>
<b>Difference Between URI.create() and new URI()</b><br>
<br>

[출처](https://www.baeldung.com/java-uri-create-and-new-uri)<br>

<br>
짧은 포스팅이니 한 번쯤 읽어보는걸 추천한다.<br>

<br>

---

<br>

## ● 코드 비교

<br>
<b>new URI()</b><br>
<br>

```java

package java.net;

public final class URI
    implements Comparable<URI>, Serializable
{

    ...

    // String 한 개만 매개변수로 받는 생성자
    public URI(String str) throws URISyntaxException {
        new Parser(str).parse(false);   // parse 메서드의 false 매개변수는 requireServerAuthority 를 초기화한다.
                                        // 변수 명을 보아 서버 보안 필요 여부 값을 나타내는 듯 하다.
    }

    ...

    // 내부 중첩 클래스 Parser
    private class Parser {

        private String input;           // URI input string
        private boolean requireServerAuthority = false;

        Parser(String s) {
            input = s;
            string = s;
        }

        ...

        // 생성자에서 호출하는 메서드
        void parse(boolean rsa) throws URISyntaxException {
            requireServerAuthority = rsa;

            ...

        }

        ...

    }

}

```

<br>
위 소스에서 굳이 Parser 를 다 뜯어보고, URI 를 다 뜯어보고 할 필요는 없었다.<br>
<b>중요한건 String 매개변수를 받는 생성자에서 인스턴스 획득을 위해 호출하는 메서드의 예외처리 부분.</b><br>
<br>
<b>throws URISyntaxException</b><br>
<br>
이 예외는 URI 로 관리하는 변수의 값이 파싱될 수 없는 값임을 알린다고 한다.<br>
그리고 <b>Checked Exception</b> 이다.<br>
명시적인 예외처리가 필요하다.<br>
<br>

---

<br>
<b>URI.create()</b><br>
<br>

```java
    public static URI create(String str) {
        try {
            return new URI(str);
        } catch (URISyntaxException x) {
            throw new IllegalArgumentException(x.getMessage(), x);
        }
    }
```

<br>
간단한 예외처리 코드이지만 중요한 점이 있다.<br>
위 생성자에서 언급된 <b style="color: red;">URISyntaxException 예외</b>를 <b>Unchecked Exception 인 <span style="color: blue;">IllegalArgumentException</span> 로 전환</b> 시켜준다는 점이다.<br>
확실한 <b>예외 전환</b>이다.<br>
이제 checked exception 의 공포에서는 벗어날 수 있다.<br>
<br>

---

<br>

## ● 결론

<br>
위 두 방법의 큰 차이는 직접 예외처리를 해줄 것인가 혹은 예외전환으로 클래스에게 맡길 것인가.<br>
심지어 URI.create() 메서드를 보면 결국 new URI(String) 생성자를 호출한다.<br>
<br>
이러한 점을 <b>Baeldung 의 Kai Yuan 프로그래머</b> 는 이렇게 결론 지었다.<br>
<br>

> <b>Kai Yuan:</b><br>
> <b>If we don’t want to deal with the checked exception, we can use the create() static method to create the URI instance.</b><br>
> <br>
> <b>만약 checked exception 처리에 트레이드 오프 하는 것을 원치 않는다면, create 메서드를 이용해서 URI 인스턴스를 획득할 수 있다.</b><br>

<br>
깔끔하고 납득이 될만한 결론이다.<br>
또한 URI 클래스를 연구하려 접했던 코드를 보면,<br>
<br>

```java
return ResponseEntity.created(URI.create(uriPath)).body(result);
```

<br>
반환부 임을 알 수 있다.<br>
반환부에 예외처리를 하여 코드를 이상하게 만드는 것보다 훨씬 나은 선택이 될 수 있는 상황이라고 생각했다.<br>
