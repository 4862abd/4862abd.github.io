---
title: Lv2 - 연속 부분 수열 합의 개수, Java
author: park
date: 2023-06-22 11:06:00 +0800
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/131701</i><br/>
<br/>

---

### ● 문제

철호는 수열을 가지고 놀기 좋아합니다.<br/>
어느 날 철호는 어떤 자연수로 이루어진 원형 수열의 연속하는 부분 수열의 합으로 만들 수 있는 수가 모두 몇 가지인지 알아보고 싶어졌습니다.<br/>
원형 수열이란 일반적인 수열에서 처음과 끝이 연결된 형태의 수열을 말합니다.<br/>
예를 들어 수열 [7, 9, 1, 1, 4] 로 원형 수열을 만들면 다음과 같습니다.<br/>
<br/>

![01](/assets/img/03.Algorithm/15.Lv2-ConsecutivelyArrayPartSum/01.png)

<br/>
원형 수열은 처음과 끝이 연결되어 끊기는 부분이 없기 때문에 연속하는 부분 수열도 일반적인 수열보다 많아집니다.<br/>
원형 수열의 모든 원소 elements가 순서대로 주어질 때, 원형 수열의 연속 부분 수열 합으로 만들 수 있는 수의 개수를 return 하도록 solution 함수를 완성해주세요.<br/>
<br/>
<b>제한사항</b><br/>
3 ≤ elements의 길이 ≤ 1,000<br/>
1 ≤ elements의 원소 ≤ 1,000<br/>
<br/>

입출력 예<br/>
| elements                       | result |
|:------------------------|--------:|
| [7,9,1,1,4]                 | 18  |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
길이가 1인 연속 부분 수열로부터 [1, 4, 7, 9] 네 가지의 합이 나올 수 있습니다.<br/>
길이가 2인 연속 부분 수열로부터 [2, 5, 10, 11, 16] 다섯 가지의 합이 나올 수 있습니다.<br/>
길이가 3인 연속 부분 수열로부터 [6, 11, 12, 17, 20] 다섯 가지의 합이 나올 수 있습니다.<br/>
길이가 4인 연속 부분 수열로부터 [13, 15, 18, 21] 네 가지의 합이 나올 수 있습니다.<br/>
길이가 5인 연속 부분 수열로부터 [22] 한 가지의 합이 나올 수 있습니다.<br/>
이들 중 중복되는 값을 제외하면 다음과 같은 18가지의 수들을 얻습니다.<br/>
[1, 2, 4, 5, 6, 7, 9, 10, 11, 12, 13, 15, 16, 17, 18, 20, 21, 22]<br/>
<br/>

---

### ● 소감

DP의 응용이라는 것을 알수 있다.<br/>
거기에 중복제거를 곁들인.<br/>
<br/>
문제는 정말 빨리 풀었고, 내가 푼 방식은 마지막에 남길 코드와 살짝 달랐다.<br/>
나는 모든 해를 구하는 방식으로 Set에 담아서 중복만 제거하여 리턴 하였다.<br/>
근데 확인하니 더 깔끔한 방법이 있어서 그렇게 진행해봤다.<br/>
<br/>

---

### ● 해결 코드

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
        
    public int solution(int[] elements) {
        return this.initDP(elements);
    }
    
    public int initDP(final int[] elements) {
        int[] dpArray = new int[elements.length * 2];
        
        for(int i = 0; i < elements.length; i++) {
            dpArray[i] = dpArray[i + elements.length] = elements[i];
        }
        
        Set<Integer> set = new HashSet<>();
        
        for(int i = 1; i <= elements.length; i++) {
            for(int j = 0; j < elements.length; j++) {
                set.add(Arrays.stream(dpArray, j, j + i).sum());
            }
        }
        
        return set.size();
    }
}
```