---
title: 4장 액션에서 계산 빼내기
author: park
date: 2023-06-02 17:18:00 +0800
categories: [CS, 쏙쏙 들어오는 함수형 코딩(2023-05 ~]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

<!-- 표지 -->
<!-- 로컬 -->
<!-- ![image](https://github.com/4862abd/4862abd.github.io/assets/77370682/ce261bb4-c073-43f4-a3df-9b4411144ad4) -->
<!-- 배포 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/25f9604c-29c7-4858-af75-82d6da2653c7)

---

## 4장 액션에서 계산 빼내기.

### 소제목 목록
● 시작하며<br/>
● MegaMart.com에 오신 것을 환영합니다<br/>
● 이제 리팩토링이 완료된 JS 의 원본 소스를 보고 Java로 컨버팅 해보자.<br/>


---
<br/>

## ● 시작하며

이번 챕터에서는 여태 설명했던 액션과 계산, 데이터의 영역 중 <b>액션에서 계산을 분리하는 것을 다룰 예정</b>이라고 한다.<br/>
<b>액션</b>은 호출 시기, 상태에 따라 결과가 다르고 사이드 이펙트가 존재하는 <b>순수하지 않은 함수</b>이다.<br/>
그 액션 안에서 순수한 함수, 계산을 분리해내려 한다.<br/>
즉, 함수 중심의 프로그래밍을 이어가는 과정 중 중요한 챕터가 될 것이다.<br/>
<br/>
이후에 나오지만 <b>이 책에서는 순수한 함수인 계산을 일급 객체로 정의</b>한다.<br/>
하지만 우리가 백단의 프로그래밍을 <b>객체지향으로 진행할 때는 클래스(객체, Object)를 일급 객체로 정의</b>한다.<br/>
이를 잘 살피고, JS의 코드를 잘 살펴 Java로 코딩하는 과정이 중요할 것이다.<br/>
특히, 객체지향의 경우 클래스 디자인과 객체들의 관계가 중심이 되어 상태, 멤버변수, 메소드 등 각 객체가 긴밀한 관계를 가지지는 것에 반해<br/>
함수형 프로그래밍은 값의 연산, 값의 도출을 중심으로 코드가 이루어진다.<br/>
일급 객체가 될 계산은 인자로 받은 값을 별도로 저장(사이드 이펙트)하지 않고 최대한 간결한 과정으로
처리하는 데에 목적을 두는 것이 큰 차이가 될 것이다.<br/>

> 일급 객체: 다른 요소들과 아무런 차별이 없는 객체.<br/>
> 일급 객체는 함수의 실질적인 매개변수가 될 수 있다.<br/>
> 일급 객체는 함수의 반환 값이 될 수 있다.<br/>
> 일급 객체는 할당의 대상이 될 수 있다.<br/>
> 일급 객체는 비교 연산(==, equals)을 적용할 수 있다.<br/>
{: .prompt-tip }
<br/>

---
<br/>

## ● MegaMart.com에 오신 것을 환영합니다

우선 책에서는 앞으로 수정해갈 것으로 보이는 코드를 소개한다.

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/d56ab99b-ce7a-4205-9f81-38934638388d)

<br/>
봐라.<br/>
<b>딱 봐도 이상하다.</b><br/>
우선 add_item_to_cart() 는 calc_cart_total() 을 호출하는 부모 함수가 될 것이다.<br/>
그리고 calc_cart_total() 을 보면 첫 줄 부터 전역 변수의 값을 초기화한다.<br/>
이건 <b>진작에 분리됐어야 할 영역이다.</b><br/>
관심사의 분리가 제대로 되지 않은 것.<br/>
차라리 지역 변수를 선언하여 0 값으로 이용하고 return 하여 부모 함수에서 전역 변수에 값을 할당하는 것이 낫지 않았을까.<br/>
이제 첫 줄 읽었다 ㅋㅋ<br/>
<br/>
다음 라인은 반복문, 즉 루틴이다.<br/>
이 반복문을 2년차 프론트엔드 전문 친구에게 보내주자마자 그의 첫 말, "아, 일반 반복문 극혐"<br/>
반복문 안의 로직을 보면 메인이 되는 서비스 로직임을 알 수 있다.<br/>
카트(shopping_cart)에 있는 item을 하나 씩 꺼내서 그 값을 전역 변수에 계속 초기화하며 계산한다.<br/>
이런.. 타입이라도 깨지거나 잘못된 값이 들어가면 어쩌려고..<br/>
<br/>
마지막 라인은 DOM에 데이터를 바인딩 하는 함수(set_cart_total_dom())라고 한다.<br/>
<br/>

### 일단 현재까지 내가 공부한 내용으로 위의 코드를 리팩토링 하자면 이렇게 될 것이다.

** Java 코드 소스 1
```java
public class ShoppingItem {
    private String name;
    private Integer price;

    // 수정자, 접근자 메소드
}

// 호출, 동작 메소드
public class MegaMart {

    // 카트
    List<ShoppingItem> shoppingCart = new ArrayList<>();
    // 카트에 담긴 상품 가격 총합
    Integer shoppingCartTotal = 0;

    // 호출되어 컨트롤러 역할을 하는 메소드
    public Integer mainController(String name, Integer price) {
        // 전달 받은 파라미터로 상품 생성
        ShoppingItem shoppingItem = this.getShoppingItem(name, price);
        // 상품을 카드에 추가
        this.addItemToCart(shoppingItem);
        // 카트에 담긴 상품의 가격 계산 총합
        Integer totalPrice = this.calcCartTotal();
        // 인스턴스 변수인 shoppingCartTotal(카트에 담긴 상품 총 가격 합산)에 데이터 바인딩
        this.setShoppingCartTotal(totalPrice);

        return shoppingCartTotal;
    }

    // 상품을 카드에 추가하는 메소드
    public void addItemToCart(ShoppingItem shoppingItem) {
        this.shoppingCart.add(shoppingItem);
    }

    // 전체 상품의 가격을 총합하는 메소드
    public Integer calcCartTotal() {
        int totalPrice = 0;

        for(ShoppingItem shoppingItem : shoppingCart) {
            Integer price = shoppingItem.getPrice();

            if(price != null)
                totalPrice += price;
        }

        return totalPrice;
    }

    // 인스턴스 변수인 shoppingCartTotal(카트에 담긴 상품 총 가격 합산)에 데이터 바인딩
    public void setShoppingCartTotal(Integer totalPrice) {
        this.shoppingCartTotal = totalPrice;
    }

    // 파라미터를 받아 새로운 상품을 돌려주는 메소드
    public ShoppingItem getShoppingItem(String name, Integer price) {
        ShoppingItem shoppingItem = new ShoppingItem();
        shoppingItem.setName(name);
        shoppingItem.setPrice(price);

        return shoppingItem;
    }
}
```

> 원본의 코드자체가 굉장히 간단한 편이라 다른 디자인 패턴 등은 적용할 일이 없었고, 단순히 SRP의 원칙을 접목시켰다.<br/>
> 생각해보니 이 책에서 설명하려는 계산이라는 일급함수는 SRP의 원칙을 지켜 짜여진 메소드로 생각될 수 있겠다.<br/>
{: .prompt-info }

---

## ● 무료 배송비 계산하기
### 절차적인 방법으로 구현하기

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/5658eca3-8cb5-4774-8ade-02be753f00a4)
<br/>

> 상단의 코드는 JS로 구현한 무료 배송 가능한 상품을 찾아 무료 배송 버튼의 출력 여부를 결정하는 로직이다.<br/>
> 역시나 Java로 컨버팅을 진행하면, <br/>
{: .prompt-info }

** Java 코드 소스 2
```java
public class MegaMart {

    public class BuyButton {
        // 상품 정보
        private ShoppingItem shoppingItem;
        // 무료 배송 버튼의 출력 여부를 담당한 flag 변수
        private boolean showFreeButtonFlag;

        // 수정자, 접근자 메소드
    }

    public List<BuyButton> updateShippingIcons() {
        // 아니 이렇게 하는 방식이 맞나
        // 데이터를 조회하는 Data Access 레벨에서 처리해도 되잖아
        // 그러면 조회 쿼리 하나 날리는 걸로 끝날텐데..
        List<BuyButton> buyButtons = this.getBuyButtonsDom();

        // 그래, 책에서 이렇게 하라고 하니 일단 한다.
        List<BuyButton> proccessedBuyButtons = this.setShowFreeButtonFlag(buyButtons);

        return proccessedBuyButtons;
    }

    // 화면에 출력할 데이터를 조회하는 과정
    // 책에서는 JS 기반이며 DOM에서 해당 데이터를 얻는 것으로 보이지만,
    // 웹 개발을 조금이라도 해봤다면 정말 비효율 적이라는 것을 알것이다.
    // 백단에서 flag 변수로 무료 배송 버튼 출력 여부만 제어해주면 굳이 이미 그려진 데이터를 다시 가공하여 사용할 필요가 없다.
    // 물론 제어에 필요한 shoppingCartTotal 은 화면에서 받아도 되고 미리 선언하여 데이터를 가지고 있던 변수를 활용해도 좋다.
    // 혹은 원본 소스 상, 아이템이 속한 버튼의 분기처리 값을 20으로 걸었는데, 만약 한 번에 엄청난 양을 출력하는 페이지라면?
        // 만약 무한 스크롤을 걸어서 계속해서 출력되는 페이지라면?
        // 차라리 20이 넘어가는 순간, 전부 무료배송으로 출력하면 되는 것 아닌가..?
    // 위의 내가 컨버팅한 코드에서는 인스턴스 변수로 선언하여 사용했지만, 클래스 변수로 활용하면 그 방법도 활용 가능할 것이다.
    public List<BuyButton> getBuyButtonsDom() {
        // DB에서 조회해오는 로직
        return ...;
    }

    public List<BuyButton> setShowFreeButtonFlag(List<BuyButton> buyButtons) {
        List<BuyButton> coppiedBuyButtons = new ArrayList<>();

        for(BuyButton buyButton : buyButtons) {
            ShoppingItem shoppingItem = buyButton.getShoppingItem();
            int price = shoppingItem.getPrice();
            
            if(price + shoppingCartTotal >= 20) {
                buyButton.setShowFreeButtonFlag(true);
            }

            coppiedBuyButtons.add(buyButton);
        }

        return coppiedBuyButtons;
    }
}

```
<br/>

> 그리고 위의 코드를 calcCartTotal() 의 마지막에 호출한다고 한다.<br/>
> 이게 뭐야..<br/>
> <i><strike>지 마음대로 코딩하는구만..</strike></i><br/>
<br/>

---

<br/>

이후에도 여러 다른 기능이 추가되는 코드를 보여준다.<br/>
하지만 <i>굳이 이를 기록하면서 수정해가는 과정은 필요 없을 것 같다.</i><br/>
결국 위처럼 <b>관심사를 나누고 액션과 계산을 나누며, 액션에서 계산을 분리하고, 계산에서 더 작은 계산을 분리해서 큰 계산을 만들 발판을 삼는다.</b><br/>
뒤의 페이지를 보다가 나온 내용은 내가 컨버팅한 Java의 소스 1에서 calcCartTotal() 메소드처럼 메소드의 <b>중간부의 루틴을 분리하는 과정을 서브루틴 추출하기(Extract Subroutine)</b> 이라고 하며,<br/>
addItemToCart() 메소드처럼 파라미터에 의존하는 <b>메소드를 분리하는 작업은 함수 추출(메소드 추출)</b> 이라고 한다.<br/>
<br/>
그 이후, <b>인스턴스 변수(전역 변수)의 값을 읽는 것이 입력</b>, <b>인스턴스 변수의 값을 변경하면 출력</b>이라고 한다.<br/>

<br/>

---

## ● 이제 리팩토링이 완료된 JS 의 원본 소스를 보고 Java로 컨버팅 해보자.
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4762f87d-5fba-4dae-92ab-26bc435c6ecb)

> 원문
{: .prompt-info }

<br/>

** Java 코드 소스 3
```java
@Getter
@Setter
public class ShoppingItem {
    private String name;
    private Double price;

    // 수정자, 접근자 메소드
}
@Getter
@Setter
public class BuyButton {
    // 상품 정보
    private ShoppingItem shoppingItem;
    // 무료 배송 버튼의 출력 여부를 담당한 flag 변수
    private boolean showFreeButtonFlag;

    // 수정자, 접근자 메소드
}

// 호출, 동작 메소드
public class MegaMart {

    // 카트
    List<ShoppingItem> shoppingCart = new ArrayList<>();
    // 카트에 담긴 상품 가격 총합
    Double shoppingCartTotal = 0.0;

    // 호출되어 컨트롤러 역할을 하는 메소드
    public Double mainController(String name, Double price) {
        ShoppingItem shoppingItem = this.getShoppingItem(name, price);
        this.addItemToCart(shoppingItem);
        Double totalPrice = this.calcCartTotal();
        this.setShoppingCartTotal(totalPrice);
        this.updateShippingIcons();
        this.updateTaxDom();

        return shoppingCartTotal;
    }

    // 파라미터를 받아 새로운 상품을 돌려주는 메소드
    public ShoppingItem getShoppingItem(String name, Double price) {
        ShoppingItem shoppingItem = new ShoppingItem();
        shoppingItem.setName(name);
        shoppingItem.setPrice(price);

        return shoppingItem;
    }

    // 상품을 카드에 추가하는 메소드
    public void addItemToCart(ShoppingItem shoppingItem) {
        this.shoppingCart.add(shoppingItem);
    }

    // 전체 상품의 가격을 총합하는 메소드
    public Double calcCartTotal() {
        Double totalPrice = 0.0;

        for(ShoppingItem shoppingItem : shoppingCart) {
            Double price = shoppingItem.getPrice();

            if(price != null)
                totalPrice += price;
        }

        return totalPrice;
    }

    // 인스턴스 변수인 shoppingCartTotal(카트에 담긴 상품 총 가격 합산)에 데이터 바인딩
    public void setShoppingCartTotal(Double totalPrice) {
        this.shoppingCartTotal = totalPrice;
    }

    // 총합과의 비교로 제품 당 무료배송 버튼의 출력여부 제어
    public List<BuyButton> updateShippingIcons() {
        List<BuyButton> buyButtons = this.getBuyButtonsDom();
        List<BuyButton> proccessedBuyButtons = this.setShowFreeButtonFlag(buyButtons);

        return proccessedBuyButtons;
    }

    // DB에서 버튼, 아이템을 조회하는 로직
    public List<BuyButton> getBuyButtonsDom() {
        return null;
    }

    // 조회한 상품 데이터의 가격과 현재 카트의 총합을 합하여 20이 넘는 상품의 무료 배송 버튼 출력 여부 flag 변수를 true로 바인딩
    public List<BuyButton> setShowFreeButtonFlag(List<BuyButton> buyButtons) {
        List<BuyButton> coppiedBuyButtons = new ArrayList<>();

        for(BuyButton buyButton : buyButtons) {
            ShoppingItem shoppingItem = buyButton.getShoppingItem();
            Double price = shoppingItem.getPrice();
            
            if(price + shoppingCartTotal >= 20) {
                buyButton.setShowFreeButtonFlag(true);
            }

            coppiedBuyButtons.add(buyButton);
        }

        return coppiedBuyButtons;
    }

    // 인스턴스 변수인 카트 내 상품 가격의 총합으로 세금을 계산하고 화면에 적용
    public void updateTaxDom() {
        Double tax = this.calcTax(this.shoppingCartTotal);
        this.setTaxDom(tax);
    }

    // 세금 계산 메소드
    public double calcTax(Double amount) {
        double result = 0;
        if (amount != null) {
            result = amount * 0.1;
        }
        return result;
    }

    // 인스턴스 변수에 파라미터로 받은 세금을 바인딩
    public void setTaxDom(Double tax) {
        this.shoppingCartTotal = tax;
    }
}
```

<br/>

> 책에서 원하는 바와 조금 다를 수 있긴하다.<br/>
> 값을 관리하던 데이터의 자료구조도 정수 타입에서 실수 타입으로 바뀌었다.<br/>
> (책에서 표현하는 가격이 원이 아닌 달러 기준인 것과 세금 계산을 위해)<br/>
> 하지만 어디까지나 <b>책에서 설계한 객체의 방식이 기존 유지하는 도메인 위주의 설계 방향에 맞지 않아서, 크게 수정할 수 있는 부분이 없다.</b><br/>
> 만약 수정하게 된다면 묶어진 메소드를 어떻게 나눠서 개별 관리하냐, 가 될 것이다.<br/>
{: .prompt-info }

<br/>