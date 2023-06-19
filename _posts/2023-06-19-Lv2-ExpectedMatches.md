---
title: Lv2 - 예상 대진표, Java
author: park
date: 2023-06-19 15:33:00 +0800
categories: [알고리즘, 2017 팁스타운]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /assets/img/03.Algorithm/06.Lv1-IntArrayAndString/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/42885</i><br/>
<br/>

---

### ● 문제

△△ 게임대회가 개최되었습니다.<br/>
이 대회는 N명이 참가하고, 토너먼트 형식으로 진행됩니다.<br/>
N명의 참가자는 각각 1부터 N번을 차례대로 배정받습니다.<br/>
그리고, 1번 ↔ 2번, 3번 ↔ 4번, ... , N-1번 ↔ N번의 참가자끼리 게임을 진행합니다.<br/>
각 게임에서 이긴 사람은 다음 라운드에 진출할 수 있습니다.<br/>
이때, 다음 라운드에 진출할 참가자의 번호는 다시 1번부터 N/2번을 차례대로 배정받습니다.<br/>
만약 1번 ↔ 2번 끼리 겨루는 게임에서 2번이 승리했다면 다음 라운드에서 1번을 부여받고, 3번 ↔ 4번에서 겨루는 게임에서 3번이 승리했다면 다음 라운드에서 2번을 부여받게 됩니다.<br/>
게임은 최종 한 명이 남을 때까지 진행됩니다.<br/>
<br/>
이때, 처음 라운드에서 A번을 가진 참가자는 경쟁자로 생각하는 B번 참가자와 몇 번째 라운드에서 만나는지 궁금해졌습니다.<br/>
게임 참가자 수 N, 참가자 번호 A, 경쟁자 번호 B가 함수 solution의 매개변수로 주어질 때, 처음 라운드에서 A번을 가진 참가자는 경쟁자로 생각하는 B번 참가자와 몇 번째 라운드에서 만나는지 return 하는 solution 함수를 완성해 주세요.<br/>
단, A번 참가자와 B번 참가자는 서로 붙게 되기 전까지 항상 이긴다고 가정합니다.<br/>
<br/>
<b>제한사항</b><br/>
N : 2^1 이상 2^20 이하인 자연수 (2의 지수 승으로 주어지므로 부전승은 발생하지 않습니다.)<br/>
A, B : N 이하인 자연수 (단, A ≠ B 입니다.)<br/>
<br/>
입출력 예<br/>

| N             | A    | B       |answer   |
|:--------------|:-----|:--------|--------:|
| 8             | 4    |   7     |   3     |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
첫 번째 라운드에서 4번 참가자는 3번 참가자와 붙게 되고, 7번 참가자는 8번 참가자와 붙게 됩니다.<br/>
항상 이긴다고 가정했으므로 4번 참가자는 다음 라운드에서 2번이 되고, 7번 참가자는 4번이 됩니다.<br/>
두 번째 라운드에서 2번은 1번과 붙게 되고, 4번은 3번과 붙게 됩니다.<br/>
항상 이긴다고 가정했으므로 2번은 다음 라운드에서 1번이 되고, 4번은 2번이 됩니다.<br/>
세 번째 라운드에서 1번과 2번으로 두 참가자가 붙게 되므로 3을 return 하면 됩니다.<br/>
<br/>

<b>입출력 예</b><br/>

| people                       | limit | return |
|:-----------------------------|:-------|--------:|
| [70, 50, 80, 50]             | 100    |   3     |
| [70, 80, 50]                 | 100    |   3     |

---

### ● 소감

회사의 친한 동료랑 같이 푼 문제이다.<br/>
<br/>
처음 접하는 Greedy 방법을 사용한 문제였다.<br/>
그래서 긴장한걸까, 정말 시간을 많이 허비했다.<br/>
그것도 바보처럼.<br/>
<br/>
문제를 잘 읽으면 보트가 작아서 한 보트에 최대 두 명의 인원이 탈 수 있다고 한다.<br/>
<br/>
하지만 난 한 보트의 limit에 도달할 때 까지 많은 인원을 태우려 했다.<br/>
어쩐지 체점하는데 계속 답이 틀렸다고 나오더라..<br/>
<br/>
문제를 잘 읽자.<br/>
<br/>

---

### ● 해결 코드

```java
public class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;

        final int[] sortedPeople = IntStream.of(people)
                .sorted()
                .toArray();

        int index = 0;
        for (int i = sortedPeople.length - 1; i >= index; i--) {
            if (sortedPeople[i] + sortedPeople[index] <= limit) {
                index++;
                answer++;
            } else {
                answer++;
            }
        }

        return answer;
    }
}
```