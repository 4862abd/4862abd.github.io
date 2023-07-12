---
title: Lv1 - 최소직사각형, Java
author: park
date: 2023-07-12 14:08:00 +0800
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/86491</i><br>
<br/>

---

### ● 문제

명함 지갑을 만드는 회사에서 지갑의 크기를 정하려고 합니다.<br>
다양한 모양과 크기의 명함들을 모두 수납할 수 있으면서, 작아서 들고 다니기 편한 지갑을 만들어야 합니다.<br>
이러한 요건을 만족하는 지갑을 만들기 위해 디자인팀은 모든 명함의 가로 길이와 세로 길이를 조사했습니다.<br>
<br>
아래 표는 4가지 명함의 가로 길이와 세로 길이를 나타냅니다.<br>

| 명함 번호 | 가로 길이 | 세로 길이 |
|:---------|:----------|---------:|
|  1       | 60        | 50       |
|  2       | 30        | 70       |
|  3       | 60        | 30       |
|  4       | 80        | 40       |

가장 긴 가로 길이와 세로 길이가 각각 80, 70이기 때문에 80(가로) x 70(세로) 크기의 지갑을 만들면 모든 명함들을 수납할 수 있습니다.<br>
하지만 2번 명함을 가로로 눕혀 수납한다면 80(가로) x 50(세로) 크기의 지갑으로 모든 명함들을 수납할 수 있습니다.<br>
이때의 지갑 크기는 4000(=80 x 50)입니다.<br>
<br>
모든 명함의 가로 길이와 세로 길이를 나타내는 2차원 배열 sizes가 매개변수로 주어집니다.<br>
모든 명함을 수납할 수 있는 가장 작은 지갑을 만들 때, 지갑의 크기를 return 하도록 solution 함수를 완성해주세요.<br>
<br>
<b>제한사항</b><br>
sizes의 길이는 1 이상 10,000 이하입니다.<br>
sizes의 원소는 [w, h] 형식입니다.<br>
w는 명함의 가로 길이를 나타냅니다.<br>
h는 명함의 세로 길이를 나타냅니다.<br>
w와 h는 1 이상 1,000 이하인 자연수입니다.<br>
<br>
입출력 예<br>

|    sizes                                      | result   |
|:----------------------------------------------|---------:|
| [[60, 50], [30, 70], [60, 30], [80, 40]]      | 4000     |
| [[10, 7], [12, 3], [8, 15], [14, 7], [5, 15]] | 120      |
| [[14, 4], [19, 6], [6, 16], [18, 7], [7, 11]] | 133      |


<br>
입출력 예 설명<br>
입출력 예 #1<br>
문제 예시와 같습니다.<br>
<br>
입출력 예 #2<br>
명함들을 적절히 회전시켜 겹쳤을 때, 3번째 명함(가로: 8, 세로: 15)이 다른 모든 명함보다 크기가 큽니다.<br>
따라서 지갑의 크기는 3번째 명함의 크기와 같으며, 120(=8 x 15)을 return 합니다.<br>
<br>
입출력 예 #3<br>
명함들을 적절히 회전시켜 겹쳤을 때, 모든 명함을 포함하는 가장 작은 지갑의 크기는 133(=19 x 7)입니다.<br>
<br>

---

### ● 소감

어제푼 피로도(Lv2, 완전탐색 문제)의 하위호환 문제이다.<br>
문제를 읽고 오해하면 '가장 큰 수 두개 곱해서 반환하면 되겠다.' 할수 있지만, 그렇지 않다.<br>
<br>
폯, 높이의 길이를 비교하여 명함을 돌려서 보관하면 굳이 각 길이를 늘이지 않아도 된다.<br>
이는 문제에서도 설명하고 있다.<br>
<br>
'어, 변수 2개를 변수 2개씩 들어오는 배열의 요소로 비교해서..'<br>
위처럼 생각하면 답은 구해도 시간초과를 볼 것 같다.<br>
<br>
이 알고리즘 문제의 핵심은, 모든 명함이 들어가야 한다는 점.<br>
즉, 폯이든 높이든 하나는 가장 긴 길이를 가지고 있어야한다.<br>
그리고 나머지 변수 하나에만 폯, 길이 중 더 작은 값을 비교하여 보관하면 되는 것이다.<br>
<br>
그렇게 생각하니 금방 풀렸다.<br>
<br>

---

### ● 해결 코드

```java    
class Solution {
    boolean[] ROUTE;
    int MAX_W = 0;
    int MAX_Y = 0;
    
    public int solution(int[][] sizes) {
        int answer;
        
        ROUTE = new boolean[sizes.length];
        dfs(sizes, 1);
        answer = MAX_W * MAX_Y;
        
        return answer;
    }
    
    public void dfs(int[][] sizes, int level) {
        for (int i = 0; i < sizes.length; i++) {
            int w = sizes[i][0];
            int h = sizes[i][1];
            
            MAX_W = Math.max(MAX_W, w);
            MAX_W = Math.max(MAX_W, h);
            
            if (!ROUTE[i] && Math.min(w, h) > MAX_Y) {
                ROUTE[i] = true;
                MAX_Y = Math.min(w, h);
                dfs(sizes, level + 1);
                ROUTE[i] = false;
            }
        }
    }
}
```