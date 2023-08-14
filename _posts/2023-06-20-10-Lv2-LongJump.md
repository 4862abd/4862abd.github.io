---
title: Lv2 - 멀리 뛰기, Java
author: park
date: 2023-06-20 13:15:00 +0800
categories: [알고리즘, 연습문제]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/05.Lv1-SecretMap/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/12914</i><br/>
<br/>

---

### ● 문제

효진이는 멀리 뛰기를 연습하고 있습니다.<br/>
효진이는 한번에 1칸, 또는 2칸을 뛸 수 있습니다.<br/>
칸이 총 4개 있을 때, 효진이는<br/>
<br/>
(1칸, 1칸, 1칸, 1칸)<br/>
(1칸, 2칸, 1칸)<br/>
(1칸, 1칸, 2칸)<br/>
(2칸, 1칸, 1칸)<br/>
(2칸, 2칸)<br/>
<br/>
의 5가지 방법으로 맨 끝 칸에 도달할 수 있습니다.<br/>
멀리뛰기에 사용될 칸의 수 n이 주어질 때, 효진이가 끝에 도달하는 방법이 몇 가지인지 알아내, 여기에 1234567를 나눈 나머지를 리턴하는 함수, solution을 완성하세요.<br/>
예를 들어 4가 입력된다면, 5를 return하면 됩니다.<br/>
<br/>
<b>제한 사항</b><br/>
n은 1 이상, 2000 이하인 정수입니다.<br/>
<br/>
<b>입출력 예</b><br/>
| n                       | result |
|:-----------------------------|--------:|
| 4                     | 5  |
| 3                     | 3  |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
위에서 설명한 내용과 같습니다.<br/>
<br/>
입출력 예 #2<br/>
(2칸, 1칸)<br/>
(1칸, 2칸)<br/>
(1칸, 1칸, 1칸)<br/>
총 3가지 방법으로 멀리 뛸 수 있습니다.<br/>
<br/>

---

### ● 소감

친한 동료가 먼저 푼 문제이다.<br/>
다 풀고 나서 내가 뒤에 가서 슥 보고 재밌어보였다.<br/>
<br/>
그러다가 아무리 채점해도 탈락이 나오길래 문제를 다시 봤더니, 1234567를 나눈 나머지를 리턴하라고 되어 있어서,<br/>
설마.. 하는 마음으로 그 부분만 고치지 바로 해결됐다.<br/>
나처럼 문제를 끝까지 읽지 않고 낚이는 사람이 없기를 바란다.<br/>
<br/>

---

### ● 해결 코드

```java
public class Solution {
    int[] DP;
    
    public long solution(int n) {
        long answer;
        this.initDP(n);
        answer = DP[n] % 1234567;
        return answer;
    }
    
    public void initDP(int n) {
        DP = new int[n + 1];
        
        DP[0] = 0;  // 0 거리는 갈 방법이 없다.
        if (n < 1) return;
        DP[1] = 1;  // 1 거리는 1 칸 점프만 가능하다.
        if (n < 2) return;
        DP[2] = 2;  // 2 거리는 두 가지 방법이 있다.(1, 1), (2)
        
        for (int i = 3; i < n + 1; i++) {
            DP[i] = DP[i - 2] % 1234567 + DP[i - 1] % 1234567;
        }
    }
}
```