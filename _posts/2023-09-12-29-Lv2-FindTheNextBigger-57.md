---
title: Lv2 - 뒤에 있는 큰 수 찾기, Java
author: park
date: 2023-09-12 15:12:00 +0800
categories: [알고리즘, 연습문제]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/28.Lv2SeparateElectronic/01.png
---

<b>정답률: 57 %</b><br>

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/154539</i><br>
<br/>

---

### ● 문제

정수로 이루어진 배열 numbers가 있습니다.<br>
배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.<br>
정수 배열 numbers가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요.<br>
단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.<br>

<br>
<b>제한사항</b><br>
4 ≤ numbers의 길이 ≤ 1,000,000<br>
1 ≤ numbers[i] ≤ 1,000,000<br>

<br>
입출력 예<br>

| numbers    | result |
|:-----|-------:|
|  [2, 3, 3, 5]   | [3, 5, 5, -1]      |
|  [9, 1, 5, 3, 6, 2]   | [-1, 5, 6, 6, -1, -1]      |

<br>
입출력 예 설명<br>
<br>
입출력 예 #1<br>
2의 뒷 큰수는 3입니다.<br>
첫 번째 3의 뒷 큰수는 5입니다.<br>
두 번째 3 또한 마찬가지입니다.<br>
5는 뒷 큰수가 없으므로 -1입니다.<br>
위 수들을 차례대로 배열에 담으면 [3, 5, 5, -1]이 됩니다.<br>
<br>
입출력 예 #2<br>
9는 뒷 큰수가 없으므로 -1입니다.<br>
1의 뒷 큰수는 5이며, 5와 3의 뒷 큰수는 6입니다.<br>
6과 2는 뒷 큰수가 없으므로 -1입니다.<br>
위 수들을 차례대로 배열에 담으면 [-1, 5, 6, 6, -1, -1]이 됩니다.<br>

---

### ● 소감

오랜만에 알고리즘 문제를 푼 덕분일까.<br>
10분 만에 냈던 정답은 시간초과가 발생했다.<br>
20번 테스트부터 4개가 연달아 시간초과 였다.<br>
<br>
같이 푸는 동기도 같은 문제에 직면했다.<br>
처음에는 2 중 반복문이 문제일까 했는데, 알고보니 반복문을 full 로 돌리니 문제가 발생하는 것이었다.<br>
<br>
나중에 힌트를 통해 얻은 방법은 <b>Stack</b> 이었다.
하지만 힌트를 개떡같이 알아들은 나는 index 를 활용하지 않고 값을 활용해버렸다.<br>
조금더 생각하고 푸는 습관을 들이자.<br>
<br>

---

### ● 해결 코드

```java
import java.util.Stack;

class Solution {
    
    public int solution(int n, int[][] wires) {
        int size = numbers.length;
        if (size == 1) {
            return new int[] { -1 };
        }
        
        int[] answer = new int[size];
        Stack<Integer> stack = new Stack<>();
        
        stack.push(0);
        
        for (int i = 1; i < size; i++) {
            while (!stack.empty() && numbers[stack.peek()] < numbers[i]) {
                answer[stack.pop()] = numbers[i];
            }
            
            stack.push(i);
        }
        
        while (!stack.empty()) {
            answer[stack.pop()] = -1;
        }
        
    }
}
```