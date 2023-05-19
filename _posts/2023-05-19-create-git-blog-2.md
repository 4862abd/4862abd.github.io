---
title: 깃 블로그 개설하기(Chirpy theme) - 추가
author: park
date: 2023-05-19 10:54:00 +0800
categories: [Git, 블로그 개설(Chirpy theme)]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
---

<!-- Git icon -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4ccf94a7-b9e6-4e7f-93aa-2bc30511faa9)

---

## 깃 블로그 개설하기(Chirpy theme) - 추가

### 소제목 목록
● Fork로 뜬 Repository와 .zip파일로 commit & push한 Repository 차이점<br/>
● 1. Repository의 명이 다르다.<br/>
● 2. 배포할 소스를 담고 있는 Pages의 타겟팅 브랜치가 다르다.<br/>

<br/>

## ● Fork로 뜬 Repository와 .zip파일로 commit & push한 Repository 차이점

<b>우선 Fork 뜬 파일과 .zip 파일을 commit & push 한 Repository 의 차이점은 한 가지가 아니다.</b><br/>
알고 있는 몇 가지를 나열하면 이렇게 된다.<br/>
<br/>
<b>차이점</b><br/>

1. Repository의 명이 다르다.<br/>
2. 배포할 소스를 담고 있는 Pages의 타겟팅 브랜치가 다르다.<br/>
3. Fork 뜰 때, master 브랜치만 가져오는 것이 아닌 production  브랜치를 같이 가져올 수 있다.<br/>
<br/>

우선 3번의 브랜치 관련 차이점은 딱히 건드릴 이유가 없다.<br/>
우리가 그 브랜치를 사용할 것도 아니고, 해당 브랜치가 문제를 일으키는 걸 본 적이 없다.<br/>
<i>(그리고 Fork 할 때 master만 가져오게 하는 설정도 있다.)</i><br/>
<br/>
1번의 차이점은 쉽게 수정 가능하다.<br/>

### ※ &nbsp;&nbsp;1. Repository의 명이 다르다.
<br/>

<!-- repository rename -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a61a8b5f-ce51-40a6-96ea-3573fc1bb87a)
<br/>
<br/>

> 위처럼 Fork 뜬 Repository의 Settings 에서 General - Repository name 의 란에 <br/>
> <b>본인 아이디.github.io</b><br/>
> 형식으로 Repository 이름을 변경하고 우측의 Rename 버튼만 눌러주면<br/>
> <br/>
> <b>끝!</b><br/>
{: .prompt-tip }

---

### ※ &nbsp;&nbsp;2. 배포할 소스를 담고 있는 Pages의 타겟팅 브랜치가 다르다.
<br/>

<!-- turn on Settings -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/499bd6ad-8064-4bb6-9666-ec66e4a3f11d)
<br/>
<br/>

> 1. 위처럼 Fork 뜬 Repository의 Settings 에서 General - Default branch 란에서 2번으로 표시된 ⇔ 버튼을 누른다. <br/>
{: .prompt-tip }
<br/>
<br/>

<!-- change default branch -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/c8b417e1-6c54-4255-bd17-7be567fa582a)
<br/>
<br/>

> 2. 블로그를 관리할 소스가 있는 브랜치를 선택하고 Update 버튼을 누른다. <br/>
{: .prompt-tip }
<br/>
<br/>

<!-- to Pages Settings -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/407b7461-f25f-469c-a029-4095af4e6855)
<br/>
<br/>

> 3. 위처럼 Fork 뜬 Repository의 Settings 에서 사이드바에 있는 Pages 버튼을 누른다.<br/>
{: .prompt-tip }
<br/>
<br/>

<!-- to Pages Settings -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/35aa2800-5cf1-40dc-a516-d8f0f3e704a5)
<br/>
<br/>

> 4. Pages - Build and deployment 에서 build, 배포할 블로그 소스가 있는 브랜치를 설정하고 Save를 해준다.<br/>
><br/>
> <b>끝!</b><br/>
{: .prompt-tip }
<br/>
<br/>

<!-- to Pages Settings -->


---