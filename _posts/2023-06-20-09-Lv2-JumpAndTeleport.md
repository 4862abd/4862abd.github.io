---
title: Lv2 - 점프와 순간 이동, Java
author: park
date: 2023-06-20 11:06:00 +0800
categories: [알고리즘, Summer/Winter Coding(~2018)]
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/12980</i><br/>
<br/>

---

### ● 문제

OO 연구소는 한 번에 K 칸을 앞으로 점프하거나, (현재까지 온 거리) x 2 에 해당하는 위치로 순간이동을 할 수 있는 특수한 기능을 가진 아이언 슈트를 개발하여 판매하고 있습니다.<br/>
이 아이언 슈트는 건전지로 작동되는데, 순간이동을 하면 건전지 사용량이 줄지 않지만, 앞으로 K 칸을 점프하면 K 만큼의 건전지 사용량이 듭니다.<br/>
그러므로 아이언 슈트를 착용하고 이동할 때는 순간 이동을 하는 것이 더 효율적입니다.<br/>
아이언 슈트 구매자는 아이언 슈트를 착용하고 거리가 N 만큼 떨어져 있는 장소로 가려고 합니다.<br/>
단, 건전지 사용량을 줄이기 위해 점프로 이동하는 것은 최소로 하려고 합니다.<br/>
아이언 슈트 구매자가 이동하려는 거리 N이 주어졌을 때, 사용해야 하는 건전지 사용량의 최솟값을 return하는 solution 함수를 만들어 주세요.<br/>
<br/>
예를 들어 거리가 5만큼 떨어져 있는 장소로 가려고 합니다.<br/>
아이언 슈트를 입고 거리가 5만큼 떨어져 있는 장소로 갈 수 있는 경우의 수는 여러 가지입니다.<br/>
<br/>
처음 위치 0 에서 5 칸을 앞으로 점프하면 바로 도착하지만, 건전지 사용량이 5 만큼 듭니다.<br/>
처음 위치 0 에서 2 칸을 앞으로 점프한 다음 순간이동 하면 (현재까지 온 거리 : 2) x 2에 해당하는 위치로 이동할 수 있으므로 위치 4로 이동합니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이때 1 칸을 앞으로 점프하면 도착하므로 건전지 사용량이 3 만큼 듭니다.<br/>
처음 위치 0 에서 1 칸을 앞으로 점프한 다음 순간이동 하면 (현재까지 온 거리 : 1) x 2에 해당하는 위치로 이동할 수 있으므로 위치 2로 이동됩니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이때 다시 순간이동 하면 (현재까지 온 거리 : 2) x 2 만큼 이동할 수 있으므로 위치 4로 이동합니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이때 1 칸을 앞으로 점프하면 도착하므로 건전지 사용량이 2 만큼 듭니다.<br/>
위의 3가지 경우 거리가 5만큼 떨어져 있는 장소로 가기 위해서 3번째 경우가 건전지 사용량이 가장 적으므로 답은 2가 됩니다.<br/>
<br/>
<b>제한 사항</b><br/>
숫자 N: 1 이상 10억 이하의 자연수<br/>
숫자 K: 1 이상의 자연수<br/>
<br/>
<b>입출력 예</b><br/>
| N                       | result |
|:-----------------------------|--------:|
| 5                     | 2  |
| 6                     | 2  |
| 5000                     | 5  |

<br/>
<b>입출력 예 설명</b><br/>
입출력 예 #1<br/>
위의 예시와 같습니다.<br/>
<br/>
입출력 예 #2<br/>
처음 위치 0 에서 1 칸을 앞으로 점프한 다음 순간이동 하면 (현재까지 온 거리 : 1) x 2에 해당하는 위치로 이동할 수 있으므로 위치 2로 이동합니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이때 1 칸을 앞으로 점프하면 위치3으로 이동합니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이때 다시 순간이동 하면 (현재까지 온 거리 : 3) x 2 이동할 수 있으므로 위치 6에 도착합니다.<br/>
&nbsp; &nbsp; &nbsp; &nbsp; 이 경우가 건전지 사용량이 가장 적으므로 2를 반환해주면 됩니다.<br/>
<br/>
입출력 예 #3<br/>
위와 같은 방식으로 합니다.<br/>
<br/>

---

### ● 소감

회사의 친한 동료랑 같이 푼 문제이다.<br/>
<br/>
낚시 문제 중 하나였다.<br/>
'아니, 곱하기만 계속써서 만들면 되는거 아냐?!'<br/>
<br/>
맞다.<br/>
홀수가 나오면 -1 (점프) 하고 짝수면 나누기 2 (순간이동) 하면 된다.<br/>
<br/>
그게 끝이다.<br/>
<br/>

---

### ● 해결 코드

```java
public class Solution {
    public int solution(int n) {
        int ans = 0;
        int copiedN = n;
        
        while (copiedN != 1) {
            if ((copiedN & 1) == 1) {
                copiedN -= 1;
                ans++;
            }
            
            copiedN /= 2;
        }
        
        ans++;

        return ans;
    }
}
```