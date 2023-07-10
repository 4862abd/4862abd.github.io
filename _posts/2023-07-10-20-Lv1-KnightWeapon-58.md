---
title: Lv1 - 기사단원의 무기, Java
author: park
date: 2023-07-10 13:43:00 +0800
categories: [알고리즘, 연습문제]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/18.Lv1-FailureRate-59/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

<b>정답률: 58%</b><br>

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/136798</i><br>
<br/>

---

### ● 문제

숫자나라 기사단의 각 기사에게는 1번부터 number까지 번호가 지정되어 있습니다.<br/>
기사들은 무기점에서 무기를 구매하려고 합니다.<br/>
각 기사는 자신의 기사 번호의 약수 개수에 해당하는 공격력을 가진 무기를 구매하려 합니다.<br/>
단, 이웃나라와의 협약에 의해 공격력의 제한수치를 정하고, 제한수치보다 큰 공격력을 가진 무기를 구매해야 하는 기사는 협약기관에서 정한 공격력을 가지는 무기를 구매해야 합니다.<br/>
<br/>
예를 들어, 15번으로 지정된 기사단원은 15의 약수가 1, 3, 5, 15로 4개 이므로, 공격력이 4인 무기를 구매합니다.<br/>
만약, 이웃나라와의 협약으로 정해진 공격력의 제한수치가 3이고 제한수치를 초과한 기사가 사용할 무기의 공격력이 2라면, 15번으로 지정된 기사단원은 무기점에서 공격력이 2인 무기를 구매합니다.<br/>
무기를 만들 때, 무기의 공격력 1당 1kg의 철이 필요합니다.<br/>
그래서 무기점에서 무기를 모두 만들기 위해 필요한 철의 무게를 미리 계산하려 합니다.<br/>
<br/>
기사단원의 수를 나타내는 정수 number와 이웃나라와 협약으로 정해진 공격력의 제한수치를 나타내는 정수 limit와<br/>
제한수치를 초과한 기사가 사용할 무기의 공격력을 나타내는 정수 power가 주어졌을 때,<br/>
무기점의 주인이 무기를 모두 만들기 위해 필요한 철의 무게를 return 하는 solution 함수를 완성하시오.<br/>
<br/>
<b>제한사항</b><br/>
1 ≤ number ≤ 100,000<br/>
2 ≤ limit ≤ 100<br/>
1 ≤ power ≤ limit<br/>
<br/>
입출력 예<br/>

| number     | stalimitges  | power       | result  |
|:-----------|:-------------|:------------|--------:|
|  5         | 3            | 2           |   10    |
|  10        | 3            | 2           |   21    |

<br/>
입출력 예 설명<br/>
<br/>
입출력 예 #1<br/>
1부터 5까지의 약수의 개수는 순서대로 [1: 1, 2: 2, 3: 2, 4: 3, 5: 2]개입니다.<br/>
모두 공격력 제한 수치인 3을 넘지 않기 때문에 필요한 철의 무게는 해당 수들의 합인 10이 됩니다.<br/>
따라서 10을 return 합니다.<br/>
<br/>
입출력 예 #2<br/>
1부터 10까지의 약수의 개수는 순서대로 [1, 2, 2, 3, 2, 4, 2, 4, 3, 4]개입니다.<br/>
공격력의 제한수치가 3이기 때문에, 6, 8, 10번 기사는 공격력이 2인 무기를 구매합니다.<br/>
따라서 해당 수들의 합인 21을 return 합니다.<br/>
<br>

---

### ● 소감

나는 이 문제를 보고 나서 약수를 구하는 로직을 짰다.<br/>
그리고 내가 우너하는 대로 결과가 나왔고, 그대로 채점을 진행했다.<br/>
<br/>
... 그리고 탈락.<br/>
<br/>
분명히 약수의 개수를 구하고 약수의 개수가 limit를 넘어가면 power 를 대신 더해주어 answer를 정확하게 구해내었다.<br/>
<br/>
하지만 탈락한 이유는 오답이 아니었다.<br/>
<br/>
<b>시간초과</b><br/>
<br/>
시간초과가 나의 발목을 잡았고, 약수를 구하는 로직을 다시 생각하다가,<br/>
회사 동료가 알려준 약수의 개수를 구하는 알고리즘의 힌트를 듣고 다시 생각했다.<br/>
<br/>
문제를 푸는 시간이 생각보다 오래 걸렸지만, 오히려 깔끔해진 것 같아 만족스럽다.<br/>

<br>

---

### ● 해결 코드

```java
class Solution {
    public int solution(int number, int limit, int power) {
        int answer = 0;
        
        for (int i = 1; i <= number; i++) {
            int count = 0;
            
            for (int j = 1; (j * j) <= i; j++) {
                if (j * j == i) {
                    count++;
                } else if (i % j == 0) {
                    count += 2;
                }
            }
            
            if (count > limit) {
                answer += power;
            } else {
                answer += count;
            }
        }
        
        return answer;
    }
}
```