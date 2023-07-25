---
title: 5장 더 좋은 액션 만들기
author: park
date: 2023-06-09 11:43:00 +0800
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

## 5장 더 좋은 액션 만들기.

### 소제목 목록
● 시작하며<br/>
● 비즈니스 요구 사항과 설계를 맞추기<br/>

---

## 시작하며

이번 챕터에서는 조금더 중요한 개념이 나온다.<br/>
간만에 이 책 포스팅을 진행하는 이유는 최근 회사에서 좋아하는 선배의 조언이 있기 때문이었다.<br/>
딱, 이번 챕터에서 그 부분에 대한 개념이 설명을 하고, 함수가 조금씩 발전하는 것이 눈에 띄는 단계이다.<br/>
그래서 깔끔하게 포스팅을 해보려 한다.<br/>
<br/>
또한, 앞으로 간단한 코드의 경우 java로 컨버팅하는 작업을 생략하려 한다.<br/>
<br/>

## 비즈니스 요구 사항과 설계를 맞추기

![03](/assets/img/05.Functional-coding/05/01.png)

```javascript

// 무료배송 확인 함수
function gets_free_shipping(total, item_price) {
    return item_price + total >= 20;
}

// 전체 금액 계산 함수
function calc_total(cart) {
    var total = 0;
    for (var i = 0; i < cart.length; i++) {
        var item = cart[i];
        total += item.price;
    }
    return total;
}

```

책에서 위와같은 함수를 두 개 보여줬다.<br/>
확인해보면 무료배송을 확인할 때에도 전체 금액을 호출하고 전체 금액을 계산할 때에도 전체 금액을 호출한다.<br/>
또한 total, item_price는 위의 이미지에서 설명하는 것처럼 <b style="color: red;">합계 급액과 제품 가격에 대한 무료 배송 여부</b>가 아니고 <br/>
<b style="color: blue;">주문 결과에 대한 무료 배송 여부</b>를 확인해야 한다고 한다.<br/>
<br/>
우선 gets_free_shipping을 책에서 설명할 코드대로 바꿔준다.<br/>
<br/>

```javascript
function gets_free_shipping(cart) {
    return calc_total(cart) >= 20;
}
```

1. 매개변수로 받던 total, item_price를 cart 로 변경하여 카트에 담긴 즉, 주문 결과에 대한 무료 배송 여부를 확인하게 변경한다.<br/>
2. 또한 카트를 calc_total 의 매개변수로 이용하여 중복되던 전체 금액을 계산하는 로직을 줄여준다.<br/>
<br/>

> 이렇게 함수 자체의 동작을 바꾼 작업은 리팩토링이 아니라고 설명한다.
> 리팩토링 작업은 동작 방식과 결과는 그대로 두는 것이 가장 중요한 핵심이니 이해했다.
{: .prompt-info }

<br/>
그러면 이전에 작성한 java 코드를 가져와보자.<br/>

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

    ...

    // 총합과의 비교로 제품 당 무료배송 버튼의 출력여부 제어
    public void updateShippingIcons() {
        List<BuyButton> buyButtons = this.getBuyButtonsDom();
        List<BuyButton> proccessedBuyButtons = this.getShowFreeButtonFlag(buyButtons);
        
        ...
    }

    ...

    // 조회한 상품 데이터의 가격과 현재 카트의 총합을 합하여 20이 넘는 상품의 무료 배송 버튼 출력 여부 flag 변수를 true로 바인딩
    public List<BuyButton> getShowFreeButtonFlag(List<BuyButton> buyButtons) {
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

    ...

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

    ...

    // 파라미터로 받은 buttons를 전역변수인 activedButtons에 바인딩
    public void setActivedButtons(List<BuyButton> buttons) {
        this.activedButtons = buttons;
    }

    // 인스턴스 변수에 파라미터로 받은 세금을 바인딩
    public void setTaxDom(Double tax) {
        this.shoppingCartTotal = tax;
    }
}
```

<br/>
위의 코드는 이전 포스팅부터 내가 형태를 많이 바꾼 형태이다.<br/>
<br/>
<del>와 큰일났다.</del><br/>
<br/>
책에서 문제 삼은 부분들을 내가 java로 컨버팅하면서 수정한 부분이 너무 많다.<br/>
지금 이 상황이다.<br/>
<br/>
'어.. 책에서 여기가 문제라고 했지.. 그러면..'<br/>
'어딨지...?'<br/>
<br/>
이미 고쳐버렸다.<br/>
<del>사실 제대로 고친지도 모르겠다.</del><br/>
<br/>
내 기준 하에 문제가 될 것 같은 코드를 내 마음대로 수정해버린 것이다.<br/>
<br/>
예를 들어, 책에서는 변경된 카트(new_cart)를 매개변수로 이용해서 무료배송 여부를 확인한다.<br/>
하지만 내가 수정한 코드는 클래스 상 인스턴스 변수, 즉 전역으로 선언해버렸다.<br/>
<br/>
책에서 앞으로 어떻게 코드가 수정될지 모르겠지만.. 내 기준하에 더 효율적이라고 생각했다.<br/>
물론 디자인 패턴 등에 의해 내가 변경한 코드가 원칙과 다른 기준을 따를 수 있다.<br/>
<br/>

---

<b>이제 확실히 알았다.</b><br/>
<br/>
조금만 알고, 확실하지 않은 상태로, 코딩을 하지 말자.<br/>
<br/>
이번 책의 정기 포스팅은 <b style="color: red;">중지</b>될 것이다.<br/>
다만 읽다가 중요한 개념이 등장하면 내 공부를 위해서라도 포스팅 할 것이다.<br/>
<br/>
음.. 공부가 모자라다.<br/>