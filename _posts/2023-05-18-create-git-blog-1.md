---
title: 깃 블로그 개설하기(Chirpy theme)
author: park
date: 2023-05-18 17:53:00 +0800
categories: [Git, 블로그 개설(Chirpy theme)]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

<!-- Git icon -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4ccf94a7-b9e6-4e7f-93aa-2bc30511faa9)

---

## 깃 블로그 개설하기(Chirpy theme)

### 소제목 목록
● 테마 소개<br/>
● 방법 선택(.zip)<br/>
● 참고<br/>
● 시작<br/>

<br/>
최근에 핫한 <b>깃 블로그를 개설하는 법</b>을 정리하려 한다.<br/>
<b>OS: Windows 10 64bit</b><br/>
<br/>
정말 많은 포스팅들이 MacOS나 Linux 환경에서 진행했다.<br/>
그래서 Windows 10 에서 진행하는 것을 포스팅 하려한다.<br/>
<br/>
<b>+ 내가 겪었던 <span style="color: red;">심각한 오류</span>도 하나 정리할 것이다.</b><br/>
<br/>

---

## ● 테마 소개

<!-- chirpy theme screen shot -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/f64f1932-bca2-4624-9f12-acf5d9a729cd)
<br/>
내가 이용하려 하는 테마<br/>
<br/>
일명 <b>저키 테마</b><br/>
많은 사람들이 이용하는 테마이기도 하고 좋아하는 회사 선배들이 추천해준 테마이다.<br/>
<br/>

깃허브: &nbsp;&nbsp;[**jekyll-theme-chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy)
<br/>

---
<br/>

## ● 방법 선택(.zip)

블로그를 개설할 수 있는 여러 방법 중 .zip 파일을 다운로드 받아서 개설하는 방법을 선택했다.<br/>
방법 종류<br/>

1. <b>원본 Repository의 Fork를 따서 Repository 명만 바꾸고 진행하기</b><br/>
2. <b>.zip 파일을 다운 받아서 Git에 commit & push 하고 진행 <span style="color: red;">(선택)</span></b><br/>
등등<br/>
<br/>

Fork로 진행하는 방법도 굉장히 유용하지만, 한 계정 당 같은 Repository는 두 개 이상의 Fork가 불가능 하다.<br/>
<br/>
<i>(그리고 나는 이미 같은 테마의 Fork 를 받아서 사용 중이며, 이 포스팅은 처음 하는 사람을 위한 것.)</i>
<br/>
그래서 현재 진행하는 방법은 .zip 파일로 받은 후 진행할 것이다.<br/>
<br/>

> <b>Fork로 진행하는 법은 초기 설정만 다르다.</b><br/>
> 우선 Fork를 받았다면 자동으로 본인 계정의 Repository로 소스를 가져온다.<br/>
> 그 후, 탭 중 Settings 에 들어가서 General - Repository name 을<br/>
> <b> 본인 아이디.github.io</b><br/>
> 로 변경만 해주면 내가 진행하려는 <b>.zip 파일을 GitHub와 연동한 직후의 상태( 번)</b>와 동일해진다.<br/>
> 혹시 Fork를 딴 후, 진행상황을 모르겠다면 번 이후의 방법을 진행하면 된다.<br/>
> <br/>
> 편한 방법으로 선택하고 진행하면 된다.
{: .prompt-info }
<br/>

---
<br/>

### ● 참고
우선 우리는 .zip파일로 진행을 하니까 <b>.git 폴더</b>를 만들어 주는 작업, 즉, <b>Git과 연동이 필요</b>하다.<br/>
해당 폴더는 Git과 연결하는 데에 필요한 정보를 가진 폴더이다.<br/>
혹시라도 지우거나 수정 시, 새로 연결을 잡아야 할 것이다.<br/>
<br/>
<u>그리고 이 포스팅에서는 개설에 필요한 Ruby, Git 등은 전부 설치가 되어 있다는 가정 하에 작성할 것이다.</u><br/>
그런 내용은 구글링만 해도 정말 쉽게 나오니 참고하길 바란다.<br/>
<i>(특정 명령어를 읽을 수 없습니다. 등의 에러는 찾으면 금방 나온다.)</i><br/>

<br/>

---
<br/>

### ● 시작
<br/>

#### 1. Repository 생성, Git 연결

<!-- Create new Repository -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/f87a05c8-882d-44fa-948d-db485733e8f7)
<br/>

> <b>본인아이디.github.io</b><br/>
> 1 - 1. 위의 형태로 Repository 명을 생성해준다.<br/>
> 그리고 공개 타입을 <b>Public</b>으로 설정해야한다.<br/>
{: .prompt-tip }


<br/>

<!-- Git bash 실행 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/8b1c4381-4940-409e-b92b-2ca76c95efaa)
<br/>

> 1 - 2. 그리고 블로그의 소스를 관리할 폴더를 하나 만들어서 <b>Git bash</b>를 실행한다.<br/>
{: .prompt-tip }


<br/>

<!-- GitHub Id, Email 등록 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/da5b87f4-00d9-4f2d-a772-56f487f4c3cb)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/177e1935-60ac-474a-91b3-b537dcfa2e2d)
<br/>
<br/>

```bash
---
git config --global user.name 유저 아이디
git config --global user.email 유저 이메일
---
```

<br/>

> 1 - 3. 우선 global 옵션으로 본인의 GitHub 아이디와 이메일을 등록합니다.<br/>
> global로 지정하지 않으면 추후 Git 작업을 할 때마다 등록을 해야할 수 있다.<br/>
{: .prompt-tip }


<br/>

<!-- Git bash 실행 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/bfcd0b22-b06e-4041-90c8-7745ede7a3dd)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/04b0b4d1-7931-4fc9-b6b1-4937c05f258f)
<br/>
<br/>

```bash
---
git clone https://github.com/유저 아이디/유저 아이디.github.io.git
---
```

> 그 후, 아까 새로 생성한 Repository의 url을 이용해서 아까 만든 빈 폴더에 git clone을 해준다.<br/>
> 정상적으로 clone이 되었다면 exit 명령어를 통해 bash를 종료합니다.<br/>
{: .prompt-tip }


<br/>

<!-- K-054 부터 적으면 된다. -->
<!-- 라인은 33 행 부터 -->

<br/>
