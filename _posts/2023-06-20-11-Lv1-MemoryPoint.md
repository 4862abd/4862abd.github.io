---
title: Lv1 - 추억 점수, Java
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

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/176963</i><br/>
<br/>

---

### ● 문제

사진들을 보며 추억에 젖어 있던 루는 사진별로 추억 점수를 매길려고 합니다.<br/>
사진 속에 나오는 인물의 그리움 점수를 모두 합산한 값이 해당 사진의 추억 점수가 됩니다.<br/>
예를 들어 사진 속 인물의 이름이 ["may", "kein", "kain"]이고 각 인물의 그리움 점수가 [5점, 10점, 1점]일 때 해당 사진의 추억 점수는 16(5 + 10 + 1)점이 됩니다.<br/>
다른 사진 속 인물의 이름이 ["kali", "mari", "don", "tony"]이고 ["kali", "mari", "don"]의 그리움 점수가 각각 [11점, 1점, 55점]]이고, "tony"는 그리움 점수가 없을 때, 이 사진의 추억 점수는 3명의 그리움 점수를 합한 67(11 + 1 + 55)점입니다.<br/>
<br/>
그리워하는 사람의 이름을 담은 문자열 배열 name, 각 사람별 그리움 점수를 담은 정수 배열 yearning, 각 사진에 찍힌 인물의 이름을 담은 이차원 문자열 배열 photo가 매개변수로 주어질 때, 사진들의 추억 점수를 photo에 주어진 순서대로 배열에 담아 return하는 solution 함수를 완성해주세요.<br/>
<br/>
<b>제한사항</b><br/>
3 ≤ name의 길이 = yearning의 길이≤ 100<br/>
3 ≤ name의 원소의 길이 ≤ 7<br/>
name의 원소들은 알파벳 소문자로만 이루어져 있습니다.<br/>
name에는 중복된 값이 들어가지 않습니다.<br/>
1 ≤ yearning[i] ≤ 100<br/>
yearning[i]는 i번째 사람의 그리움 점수입니다.<br/>
3 ≤ photo의 길이 ≤ 100<br/>
1 ≤ photo[i]의 길이 ≤ 100<br/>
3 ≤ photo[i]의 원소(문자열)의 길이 ≤ 7<br/>
photo[i]의 원소들은 알파벳 소문자로만 이루어져 있습니다.<br/>
photo[i]의 원소들은 중복된 값이 들어가지 않습니다.<br/>
<br/>
<b>입출력 예</b><br/>

| name                       | yearning | photo | result |
|:-----------------------------|:--------|:--------|--------:|
| ["may", "kein", "kain", "radi"] | [5, 10, 1, 3] | [["may", "kein", "kain", "radi"],["may", "kein", "brin", "deny"], ["kon", "kain", "may", "coni"]] | [19, 15, 6] |
| ["kali", "mari", "don"] | [11, 1, 55] | [["kali", "mari", "don"], ["pony", "tom", "teddy"], ["con", "mona", "don"]] | [67, 0, 55] |
| ["may", "kein", "kain", "radi"] | [5, 10, 1, 3]  | [["may"],["kein", "deny", "may"], ["kon", "coni"]] | [5, 15, 0] |

<br/>
입출력 예 설명<br/>
입출력 예 #1<br/>
<br/>
첫 번째 사진 속 "may", "kein", "kain", "radi"의 그리움 점수를 합치면 19(5 + 10 + 1 + 3)점 입니다.<br/>
두 번째 사진 속 그리워하는 사람들인 "may"와 "kein"의 그리움 점수를 합치면 15(5 + 10)점입니다.<br/>
세 번째 사진의 경우 "kain"과 "may"만 그리워하므로 둘의 그리움 점수를 합한 6(1 + 5)점이 사진의 추억 점수입니다.<br/>
따라서 [19, 15, 6]을 반환합니다.<br/>
<br/>
입출력 예 #2<br/>
<br/>
첫 번째 사진 속 그리워하는 사람들인 "kali", "mari", "don"의 그리움 점수를 합치면 67(11 + 1 + 55)점입니다.<br/>
두 번째 사진 속엔 그리워하는 인물이 없으므로 0점입니다.<br/>
세 번째 사진 속 그리워하는 사람은 "don"만 있으므로 55점입니다.<br/>
따라서 [67, 0, 55]를 반환합니다.<br/>
<br/>
입출력 예 #3<br/>
<br/>
설명 생략<br/>
<br/>

---

### ● 소감

역시 동료랑 같이 푼 문제이다.<br/>
이쯤되니 문제는 어렵지 않게 풀리는 것 같다.<br/>
<br/>
소요시간도 5 ~ 10 분으로 읽고 이해하는 것이 더 걸리는 것 같다.<br/>
<br/>

---

### ● 해결 코드

```java
import java.util.Arrays;

public class Solution {
    public int[] solution(String[] name, int[] yearning, String[][] photo) {
        int[] answer = new int[photo.length];
        int point = 0;
        
        for (int i = 0; i < photo.length; i++) {
            for (int j = 0; j < photo[i].length; j++) {
                int nameIndex = Arrays.asList(name).indexOf(photo[i][j]);
                
                if (nameIndex > -1) {
                    point += yearning[nameIndex];
                }
            }
            
            answer[i] = point;
            point = 0;
        }
        
        return answer;
    }
}
```