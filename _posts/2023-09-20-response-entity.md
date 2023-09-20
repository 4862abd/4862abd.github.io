---
title: ResponseEntity 란? ResponseEntity.ok?
author: park
date: 2023-09-20 18:03:00 +0800
categories: [TIL, 2023-09]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## ResponseEntity 란? ResponseEntity.ok?

<br>
● ResponseEntity<br>
● ResponseEntity.ok ? ResponseEntity.created ?<br>
<br>

---

<br>

## ● ResponseEntity

<br>
ResponseEntity 는 REST API 의 서비스 처리 반환을 관심으로 두고 HttpEntity 를 상속 받은 클래스이다.<br>
HttpEntity 의 멤버 필드를 보면 <br>
<br>

```java

public class HttpEntity<T> {

    private final HttpHeaders headers;

    @Nullable
    private final T body;

    ...

}

```

<br>
Http 헤더와 바디를 가지고 있는 것을 확인할 수 있다.<br>
즉, 반환을 의미하는 response 뿐 아니라 request 에서도 동일하게 상속받아 구현한다.<br>
작성자는 REST API 가 이용하는 HTTP 프로토콜의 장점을 이끌어 줄 기반이 되는 클래스라고 해석을 했다.<br>

<details>
    <summary><b>짧게 알고 가는 REST API</b></summary>
        <div style="background-color: #e1f5fe; padding: 5px; border-radius: 10px;">
            💬 <b>1. REST API 란</b><br/>
            <b>Re</b>presentational <b>S</b>tate <b>T</b>ransfer <b>A</b>pplication <b>P</b>rogramming <b>I</b>nterface.<br/>
            <br/>
            서버와 연결되어 동작하는 애플리케이션의 기존 구현 방식이 설계된 HTTP 의 성능에 비해 제대로 사용되지 못 하여 등장한 아키텍처.<br/>
            <br/><hr><br/>
            <b>2. REST 의 구성</b><br/>
            <br/>
            ○ <b>자원 (RESOURCE) - URI</b><br/>
            자원은 김영한 강사님의 HTTP 강의에서도 중요하게 다루는 개념이다.<br/>
            자원이란 우리가 행위를 떠나 현재 이용하려는 객체를 의미한다.<br/>
            <br/>
            RESTful 하게 API 를 설계하지 못하는 개발자는 상품을 만들거나 수정하거나 지우는 등의 기능을 수행하는 API 를<br/>
            /item/create or /createItem<br/>
            /item/update or /updateItem<br/>
            /item/delete or /deleteItem<br/>
            <br/>
            이런 식으로 설계하는 경우가 있다.<br/>
            이는 자원이 아닌 행위를 API 의 주 자원으로 인식하여 발생하는 안티패턴 중 하나이다.<br/>
            <br/>
            ○ <b>행위 (Verb) - HTTP Method</b><br/>
            <br/>
            위에서 우리는 행위를 표현하지 않는다는 것을 짚고 넘어갔다.<br/>
            우리는 행위를 URI 가 아닌 HTTP method 로 표현한다.<br/>
            HTTP method 에는 여러가지가 있지만 주로 이용되는 것에는<br/>
            GET, POST, PUT, PATCH, DELETE, OPTIONS 등이 있다.<br/>
            그 중에서도 몇 가지 관례적으로 이용되는 Method 를 확인하면,<br/>
            <br/>
            GET: 자원을 조회할 때 이용된다.<br/>
            POST: 자원을 생성할 때 주로 이용된다. (경우에 따라 수정에도 이용된다.)<br/>
            PUT: 자원을 수정할 때 주로 이용된다.<br/>
            DELETE: 자원을 삭제할 때 이용된다.<br/>
            정도가 되는데, 이를 통해 우리는 자원으로 어떤 행위를 할지 선택한다.<br/>
            <br/>
            ○ <b>표현 (Representations)</b><br/>
            API 를 어떻게 이용할지 를 나타내는 정책으로 본다.<br/>
            작게는 데이터의 타입부터 크게는 URI 의 상 자원의 명명규칙 까지 본다.<br/>
            <br/><hr><br/>
            <b>3. REST 의 특징</b><br/>
            <br/>
            ○ <b>Server - Client 구조를 띈다.</b><br/>
            ○ <b>Stateless 하다.</b><br/>
            이전의 서비스 요청에 대한 기록을 남기지 않으며, 각 API 별 지정된 동작만을 수행한다.<br/>
            ○ <b>Cacheable</b><br/>
            웹 표준 HTTP 프로토콜을 그대로 이용하기에 기존의 인프라를 활용할 수 있다.<br/>
            E-Tag 와 Last-Modified Tag 를 활용하여 캐싱할 수 있다.<br/>
            <br/>
            E-Tag?<br/>
            리소스를 식별하는 식별자로 값이 변하지 않았다면 서버 full 요청을 보내지 않게한다.<br/>
            아파치 (웹서버) 에서는 이미지/css/JS 와 같은 정적파일에 자동으로 E-Tag 를 부여한다.<br/>
            <br/>
            Last-Modified?<br/>
            날짜, 시간을 표현하며 서버에서 리소스를 신뢰하는 가장 마지막 시간을 값으로 갖는다.<br/>
            나노초 단위를 구분할 수 없어서 E-Tag 에 비하여 정확도가 떨어진다.<br/>
            ○ <b>Layered System 계층 구조를 띈다.</b><br/>
            ○ <b>Uniform Interface (인터페이스 일관성)</b><br/>
            본인이 담당한 인터페이스에 한정하여 동작을 수행하는 아키텍처 스타일 형태를 갖는다.<br/>
            동일한 API 를 이용하면 항상 동일한 동작이 이뤄지게 한다.<br/>
            ○ <b>Self-Descriptiveness (자체 표현)</b><br/>
            요청 메세지를 통해 쉽게 해석될 수 있는 자체 표현 구조를 이룬다. (Formatting)<br/>
        </div>
</details>

<br>

---

<br>

```java

public class ResponseEntity<T> extends HttpEntity<T> {

	private final Object status;

    ...

}

```

<br>
ResponseEntity 클래스의 소스 중 일부이다.<br>
HttpEntity 를 상속하여 그 필드를 공유하고, status 필드를 추가하여 관리한다.<br>
우리가 주로 이용하는 ResponseEntity 의 ok 나 created 메서드들이 status 변수의 값을 초기화 해주는 역할을 한다.<br>
즉, 현재 API 의 결과 상태를 반환하는 필드이다.<br>
<br>
정리하자면, <b>ResponseEntity 는 API 동작 수행 결과를 body 로 가지며 응답 헤더와 응답 상태를 관리하는 클래스이다.</b><br>
<br>

---

<br>

## ● ResponseEntity.ok ? ResponseEntity.created ?

<br>
ResponseEntity 의 역할을 파악했다.<br>
ok 와 created 는 각각 결과 상태를 초기화 해주는 메서드라는 것도 파악했다.<br>
소스 코드를 통해 어떻게 되는지 확인해보자.<br>
<br>
<b>ResponseEntity 의 ok 메서드</b><br>
<br>

```java

// ok 메서드 호출부
return ResponseEntity.ok(response);

...

public class ResponseEntity<T> extends HttpEntity<T> {

	private final Object status;

    ...

    // 우리가 호출하는 ok 메서드
    public static <T> ResponseEntity<T> ok(@Nullable T body) {
        return ok().body(body);
    }

    ...

    public static BodyBuilder ok() {

        // HttpStatus.OK 는 응답 상태 코드를 관리하는 Enum 이다.
        // OK(200, Series.SUCCESSFUL, "OK")
        return status(HttpStatus.OK);
    }

    ...

    public static BodyBuilder status(HttpStatus status) {
        Assert.notNull(status, "HttpStatus must not be null");
        return new DefaultBuilder(status);
    }

    ...

    // ReseponseEntity 안에 정의된 내부 중첩 클래스, DefaultBuilder
    private static class DefaultBuilder implements BodyBuilder {

            private final Object statusCode;

            // ok 메서드로 호출된 이 생성자로 생성된 DefaultBuilder 인스턴스를 반환한다.
            public DefaultBuilder(Object statusCode) {
                this.statusCode = statusCode;
            }

            ...

    }

// 여기까지가 ResponseEntity 의 ok 메서드가 호출하는 ok 메서드 동작

    @Override
    public <T> ResponseEntity<T> body(@Nullable T body) {
        return new ResponseEntity<>(body, this.headers, this.statusCode);
    }

    ...

    private ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, Object status) {
        // body 와 header 를 상속 받은 HttpEntity 를 통해 관리한다.
		super(body, headers);
		Assert.notNull(status, "HttpStatus must not be null");

        // 그리고 본인은 결과 상태 코드 값을 관리한다.
		this.status = status;
	}

// 여기까지가 ResponseEntity 의 ok 메서드 호출 후 동작하는 body 메서드 동작

}
```

<br>
실제 ResponseEntity 클래스의 ok 메서드가 호출했을 때, 내부 동작 중 소스코드가 호출되는 순서에 따라 정리한 코드이다.<br>
잘 뜯어 보면 200 의 상태값을 가지며 body 에 API 동작 결과를 담는 것을 확인할 수 있다.<br>
<br>

---

<br>
<b>ResponseEntity 의 created 메서드</b><br>

```java

// created 메서드 호출부
return ResponseEntity.created(URI.create(String.format("product/%d", request.whichOne()))).body(response);

...

public class ResponseEntity<T> extends HttpEntity<T> {

	private final Object status;

    ...

    public static BodyBuilder created(URI location) {
        // CREATED(201, Series.SUCCESSFUL, "Created")
		return status(HttpStatus.CREATED).location(location);
	}

    ...

    public static BodyBuilder status(HttpStatus status) {
		Assert.notNull(status, "HttpStatus must not be null");
		return new DefaultBuilder(status);
	}

    ...

    // ReseponseEntity 안에 정의된 내부 중첩 클래스, DefaultBuilder
    private static class DefaultBuilder implements BodyBuilder {

		private final Object statusCode;

        // created 메서드로 호출된 이 생성자로 생성된 DefaultBuilder 인스턴스를 반환한다.
        public DefaultBuilder(Object statusCode) {
			this.statusCode = statusCode;
		}

        ...

    }

// 여기까지가 ResponseEntity 의 created 가 호출하는 status 메서드 동작

    @Override
    public BodyBuilder location(URI location) {
        this.headers.setLocation(location);
        return this;
    }

    ...

}

    

public class HttpHeaders implements MultiValueMap<String, String>, Serializable {

    public void setLocation(@Nullable URI location) {
        setOrRemove(LOCATION, (location != null ? location.toASCIIString() : null));
    }

    ...

    private void setOrRemove(String headerName, @Nullable String headerValue) {
        if (headerValue != null) {
            set(headerName, headerValue);
        }
        else {
            remove(headerName);
        }
    }
}

// 여기까지가 ResponseEntity 의 created 가 호출하는 location 메서드 동작
// set 과 remove 메서드 설명은 생략하겠다.

public class ResponseEntity<T> extends HttpEntity<T> {

    ...

    // created 메서드로 내부 동작 완료 후, body 메서드로 API 동작 결과 반환
    @Override
    public <T> ResponseEntity<T> body(@Nullable T body) {
        return new ResponseEntity<>(body, this.headers, this.statusCode);
    }

    ...
    
}
```

<br>
확인해보면 ok(@Nullable T body) 메서드와 created(URI location) 메서드의 <b style="color: red;">차이점</b>이 확연히 보인다.<br>
<br>

1. <b>응답 코드 값이 다르다.</b><br>
ok 메서드는 200, created 메서드는 201 을 반환한다.<br>
<br>

2. <b>created 메서드는 응답 헤더에 location 을 추가한다.</b><br>
응답 헤더의 location 은 리디렉션할 URL 을 나타낸다.<br>
응답 코드 값이 3xx (리디렉션) 이거나 201 (생성됨) 상태 응답과 함께 제공될 때, 의미를 갖는다.<br>
<br>

출처: [runebook] (https://runebook.dev/ko/docs/http/headers/location)<br>
&#43; 김영한 강사님의 HTTP 강의<br>
<br>

3. <b>ok 메서드와 다르게 created 는 body 에 담길 반환값을 관리하지 않는다.</b><br>
오직 매개변수로 넘긴 URI 에 따라 내부 동작이 달라질 뿐이다.<br>

<br>

---

<br>

## ● 결과

ResponseEntity 는 결국 <b>API 결과를 반환하는 클래스</b> 임을 확인했다.<br>
호출하는 메서드의 종류에 따라 응답 코드를 지정할 수 있고, body 에 담길 데이터를 관리할 수 있다.<br>
<br>
이전에는 따로 떨어져 있던 지식들이 모여서 이렇게 하나의 결과를 낼 때, 조금은 뿌듯한 것 같다.<br>