---
title: Lv3 - 이중우선순위큐, Java
author: park
date: 2023-06-22 11:06:00 +0800
categories: [알고리즘, 힙(Heap)]
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
● 해결 코드 - 생략<br/>
<br/>

<i>출처: https://school.programmers.co.kr/learn/courses/30/lessons/42628</i><br/>
<br/>

---

### ● 문제

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.<br/>
<br/>
| 명령어                       | 수신 탑(높이) |
|:------------------------|--------:|
| I 숫자                 | 3  |
| D 1                 | 큐에서 최댓값을 삭제합니다.  |
| D -1                 | 큐에서 최솟값을 삭제합니다.  |

<br/>
이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.<br/>
<br/>
<b>제한사항</b><br/>
operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.<br/>
operations의 원소는 큐가 수행할 연산을 나타냅니다.<br/>
원소는 “명령어 데이터” 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.<br/>
빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.<br/>
<br/>

<b>입출력 예</b><br/>
| operations              | return |
|:------------------------|---------:|
| ["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"]	                 | [0,0]  |
| ["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"]                 | [333, -45]  |

<br/>
입출력 예 설명<br/>
<br/>
입출력 예 #1<br/>
16과 -5643을 삽입합니다.<br/>
최솟값을 삭제합니다. -5643이 삭제되고 16이 남아있습니다.<br/>
최댓값을 삭제합니다. 16이 삭제되고 이중 우선순위 큐는 비어있습니다.<br/>
우선순위 큐가 비어있으므로 최댓값 삭제 연산이 무시됩니다.<br/>
123을 삽입합니다.<br/>
최솟값을 삭제합니다. 123이 삭제되고 이중 우선순위 큐는 비어있습니다.<br/>
따라서 [0, 0]을 반환합니다.<br/>
<br/>

입출력 예 #2<br/>
-45와 653을 삽입후 최댓값(653)을 삭제합니다. -45가 남아있습니다.<br/>
-642, 45, 97을 삽입 후 최댓값(97), 최솟값(-642)을 삭제합니다. -45와 45가 남아있습니다.<br/>
333을 삽입합니다.<br/>
이중 우선순위 큐에 -45, 45, 333이 남아있으므로, [333, -45]를 반환합니다.<br/>
<br/>

---

### ● 소감

이 문제는 해결법을 올리지 않을 것이다.<br/>
<br/>
스스로 만족을 할 수도 없고, 너무 난잡한 해결방법 이었다.<br/>
즉, 마음에 들지 않는다.<br/>
<br/>
힙(Heap) 관련해서는 문제를 계속 풀어야 할 것 같다.<br/>