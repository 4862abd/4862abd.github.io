---
title: Lv2 - 귤 고르기, Java
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/138476</i><br/>
<br/>

---

### ● 문제

경화는 과수원에서 귤을 수확했습니다.<br/>
경화는 수확한 귤 중 'k'개를 골라 상자 하나에 담아 판매하려고 합니다.<br/>
그런데 수확한 귤의 크기가 일정하지 않아 보기에 좋지 않다고 생각한 경화는 귤을 크기별로 분류했을 때 서로 다른 종류의 수를 최소화하고 싶습니다.<br/>
<br/>
예를 들어, 경화가 수확한 귤 8개의 크기가 [1, 3, 2, 5, 4, 5, 2, 3] 이라고 합시다.<br/>
경화가 귤 6개를 판매하고 싶다면, 크기가 1, 4인 귤을 제외한 여섯 개의 귤을 상자에 담으면, 귤의 크기의 종류가 2, 3, 5로 총 3가지가 되며 이때가 서로 다른 종류가 최소일 때입니다.<br/>
<br/>
경화가 한 상자에 담으려는 귤의 개수 k와 귤의 크기를 담은 배열 tangerine이 매개변수로 주어집니다.<br/>
경화가 귤 k개를 고를 때 크기가 서로 다른 종류의 수의 최솟값을 return 하도록 solution 함수를 작성해주세요.<br/>
<br/>
<b>제한사항</b><br/>
1 ≤ k ≤ tangerine의 길이 ≤ 100,000<br/>
1 ≤ tangerine의 원소 ≤ 10,000,000<br/>
<br/>
<b>입출력 예</b><br/>
| k                       | tangerine | result |
|:-----------------------------|:------|--------:|
| 6                     | [1, 3, 2, 5, 4, 5, 2, 3]  | 3  |
| 4                     | [1, 3, 2, 5, 4, 5, 2, 3]  | 2  |
| 2                     | [1, 1, 1, 1, 2, 2, 2, 3]  | 1  |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
본문에서 설명한 예시입니다.<br/>
<br/>
입출력 예 #2<br/>
경화는 크기가 2인 귤 2개와 3인 귤 2개 또는 2인 귤 2개와 5인 귤 2개 또는 3인 귤 2개와 5인 귤 2개로 귤을 판매할 수 있습니다.<br/>
이때의 크기 종류는 2가지로 이 값이 최소가 됩니다.<br/>
<br/>
입출력 예 #3<br/>
경화는 크기가 1인 귤 2개를 판매하거나 2인 귤 2개를 판매할 수 있습니다. 이때의 크기 종류는 1가지로, 이 값이 최소가 됩니다.<br/>
<br/>

---

### ● 소감

문제를 보자마자 딱 감이 왔다.<br/>
<br/>
제일 많은 것부터 정렬해서 빼면 끝이겠다.<br/>
그리고 그 생각은 바로 맞았다.<br/>
<br/>
물론 해결 방식에서 성능이 더 좋은 방법이 있을텐데, 그것도 알아봐야겠다.<br/>
<br/>

---

### ● 해결 코드

```java
import java.util.stream.IntStream;

public class Solution {
    public int solution(int k, int[] tangerine) {
        int answer = 0;
        
        int[] sortedDistinctTangerine = IntStream.of(tangerine).sorted().distinct().toArray();
        int[] kindOfTangerine = new int[sortedDistinctTangerine.length];
        
        for (int i = 0; i < sortedDistinctTangerine.length; i++) {
            final int finalI = i;
            int count = (int) IntStream.of(tangerine).filter(e -> e == sortedDistinctTangerine[finalI]).count();
            kindOfTangerine[i] = count;
        }
        
        kindOfTangerine = IntStream.of(kindOfTangerine).map(e -> -e).sorted().map(e -> -e).toArray();
        
        for (int i = 0; k > 0; i++) {
            k -= kindOfTangerine[i];
            answer++;
        }
        
        return answer;
    }
}
```