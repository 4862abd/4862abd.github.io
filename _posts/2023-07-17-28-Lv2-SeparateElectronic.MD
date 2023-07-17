---
title: Lv2 - 전력망을 둘로 나누기 , Java
author: park
date: 2023-07-17 15:12:00 +0800
categories: [알고리즘, 완전탐색]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
image: 
  path: /assets/img/03.Algorithm/28.Lv2SeparateElectronic/01.png
---

<b>정답률: 불명(알고리즘 연습문제)</b><br>

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/86971</i><br>
<br/>

---

### ● 문제

n개의 송전탑이 전선을 통해 하나의 트리 형태로 연결되어 있습니다.<br>
당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다.<br>
이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.<br>
<br>
송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다.<br>
전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.<br>
<br>
<b>제한사항</b><br>
n(송전탑의 개수)은 2 이상 100 이하인 자연수입니다.<br>
wires는 길이가 n-1인 정수형 2차원 배열입니다.<br>
wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.<br>
<br>
1 ≤ v1 < v2 ≤ n 입니다.<br>
전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.<br>
<br>
입출력 예<br>

| n    |    wires                           | result |
|:-----|:--------------------------------------|-------:|
|  9  | { { 1, 3 }, { 2, 3 }, { 3, 4 }, { 4, 5 }, { 4, 6 }, { 4, 7 }, { 7, 8 }, { 7, 9 } } | 3      |
|  4  | { { 1, 2 }, { 2, 3 }, { 3, 4 } } | 0      |
|  7  | { { 1, 2 }, { 2, 7 }, { 3, 7 }, { 3, 4 }, { 4, 5 }, { 6, 7 } } | 1      |

<br>
입출력 예 설명<br>
<br>
입출력 예 #1<br>
다음 그림은 주어진 입력을 해결하는 방법 중 하나를 나타낸 것입니다.<br>

![01](/assets/img/03.Algorithm/28.Lv2SeparateElectronic/01.png)
<br>

4번과 7번을 연결하는 전선을 끊으면 두 전력망은 각 6개와 3개의 송전탑을 가지며, 이보다 더 비슷한 개수로 전력망을 나눌 수 없습니다.<br>
또 다른 방법으로는 3번과 4번을 연결하는 전선을 끊어도 최선의 정답을 도출할 수 있습니다.<br>
<br>

---

입출력 예 #2<br>
다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.<br>

![02](/assets/img/03.Algorithm/28.Lv2SeparateElectronic/02.png)
<br>

2번과 3번을 연결하는 전선을 끊으면 두 전력망이 모두 2개의 송전탑을 가지게 되며, 이 방법이 최선입니다.<br>
<br>

---

입출력 예 #3<br>
다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.<br>

![03](/assets/img/03.Algorithm/28.Lv2SeparateElectronic/03.png)
<br>
3번과 7번을 연결하는 전선을 끊으면 두 전력망이 각각 4개와 3개의 송전탑을 가지게 되며, 이 방법이 최선입니다.<br>
<br>

---

### ● 소감

처음으로 완전탐색 알고리즘 형태에서 힌트를 썼다.<br>
점수가 18점 가량 깎였다.<br>
마음이 아프지만 내 역량으로 힘들어보였다.<br>
<br>
처음에는 Map<Integer, int[]> 를 활용하여 풀려고 했다.<br>
그 Map도 구현은 완료했고, 이후 depth만 구하면 될 것 같았다.<br>
<br>
하지만 depth를 구하는 방식에서 시간이 넘어갈 것 같았다.<br>
어쩔수 없이 본 답지는 보고서도 이해하는데 시간이 조금 걸렸다.<br>
<br>
그래도 확인해보니 이전에 활용했던 방식과 비슷한 점(dfs, 깊이 우선 탐색)이 있어서 답지를 내 경험에 맞춰 다시 수정해봤다.<br>
<br>

---

### ● 해결 코드

```java
class Solution {
    int N, MIN;
    int[][] ROUTE;
    boolean[] VISITED;
    
    public int solution(int n, int[][] wires) {
        N = n;
        MIN = n;
        ROUTE = new int[n + 1][n + 1];
        VISITED = new boolean[n + 1];
        
        for (int[] wire : wires) {
            int a = wire[0];
            int b = wire[1];
            ROUTE[a][b] = ROUTE[b][a] = 1;
        }
        
        dfs(1);
        return MIN;
    }
    
    int dfs(int n) {
        VISITED[n] = true;
        int child = 1;
        
        for (int i = 1; i <= N; i++) {
            boolean visited = VISITED[i];
            boolean isOne = ROUTE[n][i] == 1;
            
            if (!visited && isOne) {
                VISITED[i] = true;
                child += dfs(i);
            }
        }
        
        MIN = Math.min(MIN, Math.abs(child - (N - child)));
        return child;
    }
}
```