---
title: Lv2 - 괄호 회전하기, Java
author: park
date: 2023-06-22 11:06:00 +0800
categories: [알고리즘, 월간 코드 챌린지 시즌2]
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/76502</i><br/>
<br/>

---

### ● 문제

다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의합니다.<br/>
<br/>
(), [], {} 는 모두 올바른 괄호 문자열입니다.<br/>
만약 A가 올바른 괄호 문자열이라면, (A), [A], {A} 도 올바른 괄호 문자열입니다.<br/>
예를 들어, [] 가 올바른 괄호 문자열이므로, ([]) 도 올바른 괄호 문자열입니다.<br/>
만약 A, B가 올바른 괄호 문자열이라면, AB 도 올바른 괄호 문자열입니다.<br/>
예를 들어, {} 와 ([]) 가 올바른 괄호 문자열이므로, {}([]) 도 올바른 괄호 문자열입니다.<br/>
대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 s가 매개변수로 주어집니다.<br/>
이 s를 왼쪽으로 x (0 ≤ x < (s의 길이)) 칸만큼 회전시켰을 때 s가 올바른 괄호 문자열이 되게 하는 x의 개수를 return 하도록 solution 함수를 완성해주세요.<br/>

<b>제한사항</b>
s의 길이는 1 이상 1,000 이하입니다.<br/>
<br/>
입출력 예<br/>

| s                       | result |
|:------------------------|--------:|
| "[](){}"                 | 3  |
| "}]()[{"                 | 2  |
| "[)(]"                   | 0  |
| "}}}"                    | 0  |

<br/>
입출력 예 설명<br/>
<br/>
입출력 예 #1<br/>
다음 표는 "[](){}" 를 회전시킨 모습을 나타낸 것입니다.<br/>
x	s를 왼쪽으로 x칸만큼 회전	올바른 괄호 문자열?<br/>

| s                       | result | result |
|:------------------------|--------:|--------:|
| 0                 | "[](){}"  | O  |
| 1                 | "](){}["  | X  |
| 2                   | "(){}[]"  | O  |
| 3                    | "){}[]("  | X  |
| 4                    | "{}[]()"  | O  |
| 5                    | "}[](){"  | X  |
올바른 괄호 문자열이 되는 x가 3개이므로, 3을 return 해야 합니다.<br/>
<br/>
입출력 예 #2<br/>
다음 표는 "}]()[{" 를 회전시킨 모습을 나타낸 것입니다.<br/>
x	s를 왼쪽으로 x칸만큼 회전	올바른 괄호 문자열?<br/>

| s                       | result | result |
|:------------------------|--------:|--------:|
| 0                 | "}]()[{"  | X  |
| 1                 | "]()[{}"  | X  |
| 2                   | "()[{}]"  | O  |
| 3                    | ")[{}]("  | X  |
| 4                    | "[{}]()"  | O  |
| 5                    | "{}]()["  | X  |
올바른 괄호 문자열이 되는 x가 2개이므로, 2를 return 해야 합니다.<br/>
<br/>
입출력 예 #3<br/>
s를 어떻게 회전하더라도 올바른 괄호 문자열을 만들 수 없으므로, 0을 return 해야 합니다.<br/>
<br/>
입출력 예 #4<br/>
s를 어떻게 회전하더라도 올바른 괄호 문자열을 만들 수 없으므로, 0을 return 해야 합니다.<br/>
<br/>

---

### ● 소감

문제를 보면 이전에 풀었던 문제의 응용이라는 것을 알수 있다.<br/>
<br/>
저장된 가장 마지막 괄호가 현재 비교하는 괄호와 짝이 맞으면 제거하기,<br/>
즉 Stack 을 사용했던 문제와 결이 비슷했다.<br/>
<br/>
물론 해결 방식에서 성능이 더 좋은 방법이 있을텐데, 그것은 알아봐야겠다.<br/>
<br/>

---

### ● 해결 코드

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Stack;

public class Solution {
    Map<Character, Character> friendBracket;

    public int solution(String s) {
        int answer = 0;
        
        this.initFriendBracket();
        
        Stack<Character> stack;
        
        int sLength = s.length();
        
        for (int i = 0; i < sLength; i++) {
            stack = new Stack<>();

            for (int j = 0; j < sLength; j++) {
                char ch = s.charAt(j);
                
                if (!stack.isEmpty() && (stack.peek() == friendBracket.get(ch))) {
                    stack.pop();
                } else {
                    stack.push(ch);
                }
            }

            if (stack.isEmpty()) {
                answer++;
            }
            
            s = s.substring(1, (sLength)) + s.charAt(0);
        }
        
        return answer;
    }
    
    public void initFriendBracket() {
        friendBracket = new HashMap<>();
        friendBracket.put(')', '(');
        friendBracket.put('}', '{');
        friendBracket.put(']', '[');
    }
}
```