---
title: Lv2 - 소수 찾기, Java
author: park
date: 2023-07-13 16:37:00 +0800
categories: [알고리즘, 완전탐색]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/18.Lv1-FailureRate-59/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

<b>정답률: 불명(알고리즘 연습문제)</b><br>

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/42839</i><br>
<br/>

---

### ● 문제

한자리 숫자가 적힌 종이 조각이 흩어져있습니다.<br>
흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.<br>
<br>
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.<br>
<br>
<b>제한사항</b><br>
numbers는 길이 1 이상 7 이하인 문자열입니다.<br>
numbers는 0~9까지 숫자만으로 이루어져 있습니다.<br>
"013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.<br>
<br>
입출력 예<br>

| numbers | return |
|:--------|-------:|
| "17"    | 3      |
| "011"   | 2      |

<br>
입출력 예 설명<br>
<br>
예제 #1<br>
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.<br>
<br>
예제 #2<br>
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.<br>
<br>
11과 011은 같은 숫자로 취급합니다.<br>
<br>

---

### ● 소감

보자마자 생각난 알고리즘.<br>
DFS(깊이 우선 탐색).<br>
금방 풀수 있을 것 같았고, 실제로 점화식을 굉장히 빠르게 구했다.<br>
20분 정도 소요된것 같은데 정답률이 계속 75 ~ 85 % 를 기록하며 탈락이 뜨는 것이었다.<br>
<br>
정답률이 저 정도 나오는 건, 푸는 방식은 괜찮은데 분명 미스가 있을 것이라는 생각이 들었다.<br>
무엇일까.. 고민을 하다가 동료와 잠시 이야기 했다.<br>
<br>
소수인지 확인하는 메소드에서 등호를 빼먹었다.<br>
ㅋㅋㅋㅋㅋㅋ<br>
등호를 붙이니 바로 해결되었다.<br>
<br>
이런 실수를 줄이는 노력을 해야겠다.<br>
<br>

---

### ● 해결 코드

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    
    Set<Integer> count = new HashSet<>();
    
    public int solution(String numbers) {
        dfs("", numbers);
        System.out.println("☆ count: " + count);
        return count.size();
    }
    
    public void dfs(String current, String left) {
        for (int i = 0; i < left.length(); i++) {
            String currentCopy = current + left.charAt(i);
            if (isPrimeNumber(Integer.parseInt(currentCopy))) {
                count.add(Integer.valueOf(currentCopy));
            }
            String leftCurrent = left.substring(0, i) + left.substring(i + 1);
            dfs(currentCopy, leftCurrent);
        }
    }
    
    public boolean isPrimeNumber(int number) {
        if (number == 0 || number == 1) {
            return false;
        }
        
        for (int i = 2; i <= Math.sqrt(number); i++) {
            if (number % i == 0) {
                return false;
            }
        }
        
        return true;
    }
}
```