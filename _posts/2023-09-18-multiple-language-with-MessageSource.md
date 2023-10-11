---
title: Spring-boot MessageSource 로 다국어 구현하기
author: park
date: 2023-09-18 18:20:00 +0800
categories: [Java, Programming]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## Spring-boot MessageSource 로 다국어 구현하기

<br>
● 다국어를 적용하려던 계기<br>
● MessageSource<br>
● 한글이 물음표 (?) 로 출력되는 현상<br>
<br>

---

<br>

## ● 다국어를 적용하려던 계기 

<br>

진행하는 Snack 구독 서비스 애플리케이션의 구현을 진행 중, 내가 예외 메세지를 이렇게 남기고 진행했었다.<br>
<br>

<!-- ![01](/assets/img/04.java/03.multiple-language/01.jpg)<br> -->

![01](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/1619331a-86f7-4165-9ec5-c5e41800dfc5)

우선 메세지가 그대로 노출된다.<br>
마치 Javascript 의<br>

```javascript

alert("☆ 그런 유저 없다요.");

```

처럼 말이다.<br>
이렇게 메세지를 모두 하드코딩 할 경우, 수정에 굉장히 민감해질 수 밖에 없다.<br>
파급효과 (사이드 이팩트) 가 클 것이 명확하고, 관리하기에 불편함이 클 것이다.<br>
그래서 이번 기회에 직접 메세지를 따로 관리할 수 있는 방법을 찾아보려 했다.<br>
<br>
그렇게 발견한 것이 Spring 이 제공하는 <b>MessageSource</b> 의 <b>getMessage</b> 메서드였다.<br>
<br>
<i>코드에 남긴 별표, 혹은 별표 주석은 내가 추후에 작업할 것을 위해 남기는 나만의 to do list 같은 것이다.</i><br>
<br>

---

<br>

## ● MessageSource

[MessageSource 6.0.12](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/MessageSource.html)
<br>
Spring 프레임 워크에서 지원하는 <b>MessageSource</b> 는 다국어 지원 시, 많이 이용하는 방법이다.<br>
작성자 같은 경우는 현업에서 <b>사용자 정의 예외 메세지</b>를 편하게 관리하기 위해 사용했던 경험이 있다.<br>
적용하고 있는 토이 프로젝트는 Spring-boot 2.7.15 를 이용하는 프로젝트였기에 따로 빌드 관리 도구에 의존을 추가하진 않았다.<br>
MessageSource 는 Spring 프레임워크 초기 버전부터 지원하는 API 였다.<br>
<br>
MessageSource 는 Internationalization (국제화, 이하 i18n) 을 지원하는 API 이다.<br>
즉, 각 언어와 문자 체계에 따른 충돌을 해결하는 방법 중 하나이다.<br>
이 API 가 지원하는 국가와 언어는 <b>Locale</b> 클래스를 이용해서 진행하는데, 멤버 필드로 정의된 변수를 보면 이렇게 지원하는 듯 하다.<br>

```java
public final class Locale implements Cloneable, Serializable {

    /** Useful constant for language.
     */
    public static final Locale ENGLISH;

    public static final Locale FRENCH;

    public static final Locale GERMAN;

    public static final Locale ITALIAN;

    public static final Locale JAPANESE;

    public static final Locale KOREAN;

    public static final Locale CHINESE;

    public static final Locale SIMPLIFIED_CHINESE;

    public static final Locale TRADITIONAL_CHINESE;

    /** Useful constant for country.
     */
    public static final Locale FRANCE;

    public static final Locale GERMANY;

    public static final Locale ITALY;

    public static final Locale JAPAN;

    public static final Locale KOREA;
    
    public static final Locale UK;

    public static final Locale US;

    public static final Locale CANADA;

    public static final Locale CANADA_FRENCH;

    ...

}

```

지원하는 국가과 언어는 MessageSource 가 이용하는 <b>Locale 클래스</b> 를 뜯으면 확인할 수 있다.<br>
<br>
헤더에 속성 중, <b>accept-language</b> 를 정해진 i18n 규격에 맞춰 요청을 하면 정해진 메세지를 이용할 수 있다.<br>
표기법은 이러하다.<br>
<br>
<b>ko</b>_<b>KR</b><br>
<b>언어</b>_<b>국가</b><br>
<br>
앞의 언어를 나타내는 값은 ISO 639-1 표준 형식을 따른다고 하고,<br>
뒤의 국가는 ISO 3166-1 표준 형식을 따른다고 한다.<br>
<i style="font-size: 5px;">이렇게 자세하게 알 필요는 없을 것 같다.</i><br>
<br>
보통 인코딩 방식을 표기할 때 이렇게 표기하는데 이런 형태를 띈다.<br>
<br>
ko_KR.UTF-8, en_US.UTF-8<br>
<br>
우리가 이용하는 웹 브라우저에서도 쉽게 파악할 수 있는데,<br>
방법은 남겨두겠다.<br>
<br>

<details>
    <summary><b>브라우저에서 이용 중인 언어_국가.인코딩 타입 설정 확인하기</b></summary>
        <br/>
        1. 우선 <b>개발자 도구</b> 를 연다.<br>
        windows OS 환경이라면 f12 등으로 쉽게 열 수 있다.<br>
        Mac OS 환경이라면 구글에 "Mac 개발자 도구 열기" 라고 치면 잘 나온다.<br>
        개발자 도구를 한 번도 이용하지 않은 사람이라면 언어 설정이 default 로 영어로 잡혀 있을 수 있다.<br>
        그 점은 작성자가 보여주는 사진과 다를 수 있는 점, 인지해야한다.<br>
        <!-- <img src="/assets/img/04.java/03.multiple-language/03.jpg" /> -->
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/3832fd83-9598-49b6-9be8-97e1367b2b53" />
<br>
<br>
<hr>
        2. 개발자 도구 상단의 탭 중에서 <b>"네트워크" 를 클릭</b> 한다.<br>
        처음 네트워크에 들어가면 기록을 남기지 않아서 빈 창이 뜰 것이다.<br>
        그 상태로 새로고침 하여 출력되는 목록을 확인하자.<br>
        <!-- <img src="/assets/img/04.java/03.multiple-language/04.jpg" />
        <img src="/assets/img/04.java/03.multiple-language/05.jpg" />
        <img src="/assets/img/04.java/03.multiple-language/06.jpg" /> -->
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a1b40bd3-2361-4aad-a162-ef785bc81aff" />
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/df793c12-2f58-4d66-a6b8-469fd93e063b" />
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/ff5a61b7-0be6-41b5-b572-6e6b94c9408c" />
<br>
<br>
<hr>
        3. 이름을 보면 <b>Http 메서드를 사용한 url</b> 이 눈에 띌 것이다.<br>
        눌러서 요청헤더를 확인하자.<br>
        특히 페이지가 그려지기 위해 서버와 통신을 하는 페이지의 경우 Get 메서드를 이용하는 경우가 많은데, 이런 점을 알고 넘어가는 것도 중요하다.<br>
        <!-- <img src="/assets/img/04.java/03.multiple-language/07.jpg" /> -->
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/01c55f21-247e-457b-9a15-cb1d3975885c" />
<br>
<br>
<hr>
        4. 사이트탭의 헤더 탭에서 쭉 내리면 <b>요청 헤더에 "Accept-Language" 와 "Content-Type"</b> 이 보일 것이다.<br>
        그 값을 확인해보면 한글로 설정한 크롬의 경우 default 인 "ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7" 로 되어 있을 것이다.<br>
        언어별 우선 순위를 둔 것이고, 현재 한국어가 가장 먼저 그려지게 헤더가 설정되어 있다.<br>
        <!-- <img src="/assets/img/04.java/03.multiple-language/08.jpg" /> -->
        <img src="https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4a6e447b-2237-4570-becd-da5697d24f1a" />
<br>
<br>
        5. "Accept-Language" 의 언어 설정은 크롬의 설정 - 언어 탭에서 변경 가능하다.<br>
<br>
</details>

<br>
<br>

---

<br>

<b>MessageSource</b> 는 위에서 서술한 <b>Accept-Language 의 값으로</b> 출력할 문자열이 담긴 <b>properties 파일을 선택</b>한다.<br>
<b>내가 구현한 방식은</b> MessageSource 를 사용하기 위해 선언한 것과 출력할 문자열이 담긴 프로퍼티 파일을 추가해준 것 밖에 없다.<br>
설정하는 방식은 버전, 혹은 개발자 별로 상이하니 이런 방식도 있다, 라고 읽으면 된다.<br>
참고로 작성자는 직접 구현했던 이 방식이 가장 쉬웠다.<br>
<br>
우선적으로, MessageSource 가 이용하게 될 properties (이하, 프로퍼티) 의 명명 규칙을 알아보자.<br>
프로퍼티 파일들의 이름은 여러 방법 (ex. xml 파일로 관리, 애너테이션을 통한 관리) 을 통해 변경할 수 있지만, 작성자는 그렇게 하진 않았다.<br>
MessageSource 가 제공하는 구현 방식으로 진행했다.<br>
<br>
가장 기본이 되는 프로퍼티 파일은<br>
<br>
<b>messages.properties</b><br>
<br>
이다.<br>
위 프로퍼티 파일이 없으면 다른 언어 프로퍼티 파일을 잘 작성해도 동작하지 않고 <b>NoSuchMessageException</b> 를 뱉어낸다.<br>
RuntimeException 을 상속한 언체크 예외라서 애플리케이션 동작에는 문제가 없지만, 우리가 원하는 상황은 아니다.<br>
이런 이유가 나는 이유는 messages.properties 파일이 MessageSource 가 이용하는 기본이 될 프로퍼티 파일이기 때문이다.<br>
<br>
우선 위 파일을 추가해준다.<br>
출력할 메세지를 messages.properties 에 <b>키 = 값 형태</b> 로 추가해준다.<br>
<br>

```properties

test.properties = 동작하니?

```

<br>
그리고 MessageSource 를 관리할 클래스를 작성한다.<br>
<br>

```java

@Component                                                  // Spring Bean 으로 등록하기 위한 애너테이션
@RequiredArgsConstructor                                    // 롬복에서 제공하는 생성자 DI 지원 애너테이션
public class MessageUtil {
    
    private final MessageSource messageSourceBean;          // 직접 Bean 으로 관리되는 MessageSource
    
    private static MessageSource messageSource;             // 우리가 구현에 사용할 정적 MessageSource
    
    @PostConstruct                                          // 애너테이션을 통해
    public void initialize() {
        messageSource = messageSourceBean;                  // 의존성이 주입된 messageSourceBean 의 인스턴스를
                                                            // messageSource 에 초기화
    }
    
    public static String getMessage(String messageCode) {   // 프로퍼티에 작성된 문자열을 획득하는 메서드 구현
        return messageSource.getMessage(messageCode, null, LocaleContextHolder.getLocale());
    }
}

```

<br>
이제 MessageUtil 클래스에 의해 우리는 프로퍼티 파일에 등록된 문자열을 조회하여 반환할 수 있다.<br>
messageSource 의 getMessage 메서드는 매개변수로 Locale 을 받긴 하지만, 매개변수인 Locale 이 null 인 경우, <br>
Locale 클래스의 getDefault 메서드를 통해 default Locale 을 획득한다.<br>
<br>
이제 커스텀 예외를 정의하고 getMessage 메서드를 호출하자.<br>
<br>

```java

// 예외 throw
public class TestClass {

    public void testMethod() {

        throw new TestException("test.properties");
    }
}

...

// 커스텀 예외, RuntimeException 을 상속한 언체크 예외이다.
public class TestException extends RuntimeException{
    
    public TestException(String message) {
        super(MessageUtil.getMessage(message));
    }
}

```

<br>
위의 testMethod 에서 출력하고 싶은 message 를 매개변수로 호출한다.<br>
나는 예외 상황에서 로그를 기록하기 위해 예외 상황 이라는 케이스를 이용했지만, 사실 getMessage 메서드는 어디서든 호출해도 된다.<br>
내가 매개변수로 넘긴 "test.properties" 는 이제 <b>문자열 형태의 키 값</b> 으로 활용되어 <b>정의된 프로퍼티 파일 중 키 값에 해당하는 value 를 조회하고 반환</b> 한다.<br>
<br>
그래서 위 메서드를 동작하면,<br>
<br>

> 로그 출력<br>
><br>
> 프로젝트 패키지 경로.test.exception.TestException: 동작하니?<br>

<br>
예외가 원하는 메세지로 발생하는 것을 알 수 있다.<br>
<br>

### 다국어 처리

<br>
다국어 처리는 더 쉽다.<br>
프로퍼티 파일 명에 언더바, 각 언어만 기재해주면 된다.<br>
<br>
messages_ko.properties<br>
messages_en.properties<br>
<br>
이렇게 생성하고 같은 키 값으로 value 만 다르게 지정해주면 된다.<br>
구글에서 찾아보면 MessageSource 를 구현하는 방식이 다양한데 작성자는 이 방식이 가장 간단하고 편리했다.<br>
<br>

---

<br>

## ● 한글이 물음표 (?) 로 출력되는 현상

<br>
작성자는 IntelliJ 를 이용하니 IntelliJ 기준으로 설명 하겠다.<br>
<br>
Settings 의 Editor - File Encodings 에 들어간다.<br>
File Encodings 에서 <br>
<b>Default encoding for properties files</b> 의 값을 <b>UTF-8</b> 로 맞춰주면 되는데,<br>
<br>
이 값을 바꾸면 기존 프로퍼티 파일의 value 들이 바뀌는 현상이 있다.<br>
그래서 이 설정은 프로퍼티 파일 데이터 기입 전에 하는게 좋다.<br>
설정이 변경되면 본인이 등록한 문자열들이 전부 <b style="color: red;">물음표로 변할 수 있다.</b><br>
<br>

<!-- ![09](/assets/img/04.java/03.multiple-language/09.jpg)<br> -->
![09](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/eec4479a-efbe-4c5e-a66c-9bc7f0b82ae3)