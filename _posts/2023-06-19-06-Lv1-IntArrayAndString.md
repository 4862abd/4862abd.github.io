---
title: Lv1 - 숫자 문자열과 영단어, Java
author: park
date: 2023-06-19 15:33:00 +0800
categories: [알고리즘, 2021 카카오 채용연계형 인턴십]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
image: 
  path: /assets/img/03.Algorithm/06.Lv1-IntArrayAndString/01.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

### 소제목 목록
● 문제<br/>
● 소감<br/>
● 해결 코드<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/81301</i><br/>
<br/>

---

### ● 문제

네오와 프로도가 숫자놀이를 하고 있습니다.<br/>
네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.<br/>
<br/>
다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.<br/>
<br/>
1478 → "one4seveneight"<br/>
234567 → "23four5six7"<br/>
10203 → "1zerotwozero3"<br/>
이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다.<br/>
s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.<br/>
<br/>
참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.<br/>

| 숫자                       | 영단어 |
|:-----------------------------|--------:|
| 0                     | zero  |
| 1                     | one  |
| 2                     | two  |
| 3                     | three  |
| 4                     | four  |
| 5                     | five  |
| 6                     | six  |
| 7                     | seven  |
| 8                     | eight  |
| 9                     | nine  |

<b>제한사항</b><br/>
1 ≤ s의 길이 ≤ 50<br/>
s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.<br/>
return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.<br/>

<b>입출력 예</b><br/>

| 입력                       | result |
|:-----------------------------|--------:|
| "one4seveneight"                     | 1478  |
| "23four5six7"                     | 234567  |
| "2three45sixseven"                     | 234567  |
| "123"                     | 123  |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
<br/>
문제 예시와 같습니다.<br/>
입출력 예 #2<br/>
<br/>
문제 예시와 같습니다.<br/>
입출력 예 #3<br/>
<br/>
"three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.<br/>
입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.<br/>
입출력 예 #4<br/>
<br/>
s에는 영단어로 바뀐 부분이 없습니다.<br/>
<br/>
<b>제한시간 안내</b><br/>
정확성 테스트 : 10초<br/>

<br/>

---

### ● 소감

회사의 친한 동료랑 같이 푼 문제이다.<br/>
<br/>
문제를 보자마자 답이 떠올랐다.<br/>
재밌었다.<br/>
<br/>

---

### ● 해결 코드

```java
public class Solution {
    final String[] NUMBER_WORD_ARRAY = { "zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine" };

    public int solution(String s) {
        int answer = 0;
        
        for (int i = 0; i < NUMBER_WORD_ARRAY.length; i++) {
            if (s.contains(NUMBER_WORD_ARRAY[i])) {
                s = s.replaceAll(NUMBER_WORD_ARRAY[i], String.valueOf(i));
            }
        }
        
        answer = Integer.parseInt(s);
        
        return answer;
    }
}
```