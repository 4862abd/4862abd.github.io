---
title: 깃 블로그 개설하기(Chirpy theme) - 오류 해결
author: park
date: 2023-05-19 17:19:00 +0800
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

## 깃 블로그 개설하기(Chirpy theme) - 오류 해결
<b style="color: red;">
404 error
.js does not exist<br/>
Error: Process completed with exit code 1.<br/>
Test site<br/>
</b>

### 소제목 목록
● 내가 처음 마주친 에러<br/>
● 해결 과정<br/>
● 실패 했던 방법들<br/>
● 내가 찾은 방법<br/>
● 후기<br/>

### ● 내가 처음 마주친 에러

<br/>
나는 이전까지 로컬에서 실행하기, Git 과 연동해서 Repository에 commit 하기 까지 마무리 했다.<br/>
<br/>
이제 남은 것은 Github의 액션을 통해서 자동으로 배포, 관리되는 과정이다.<br/>
우리가 이름을 바꾼 pages-deply.yml은 우리의 블로그 소스가 자동으로 배포 해줄 수 있게 도와주는 설정을 하는 파일이다.<br/>
<br/>
하지만 문제가 생겼다.<br/>
Github에 등록한 gmail로 배포가 실패했다는 메일이 날라온 것.<br/>
<br/>
내용을 확인하면<br/>
Build & Deploy, 즉 빌드, 배포에 실패했다고 나온다.<br/>
<br/>

> 이 에러를 고쳐가는 과정에 대한 블로그 포스팅이 적지 않다.<br/>
> <b>하지만, <span style="color: red;">Windows 에서 세팅해서 이러한 과정을 확실하게 정리한 사람은 단 한명도 없었다.</span></b><br/>
> 대부분 Linux나 MacOS 환경에서 세팅하더라.<br/>
> 그래서 나는 이 과정을 chirpy theme 의 Github 탭 중 Issues(질문 등을 등록하는 페이지) 와 다른 여러 포스팅, Github의 자동 배포 관련 doc문서, js 파일 컴파일 하는 doc 문서들까지<br/>
> 전부 섭렵하고 나서..<br/>
> 아무런 도움을 얻지 못했다.<br/>
> ㅋㅋㅋㅋㅋ<br/>
{: .prompt-info }

### ● 해결 과정

<br/>
나와 같은 에러를 겪을, 겪은 사람들을 위해서 이 글을 포스팅한다.<br/>
<br/>
우선 _config.yml 파일의 url에 등록했던<br/>
본인 아이디.github.io<br/>
로 접속해본다.<br/>
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a79307c3-8ab9-4603-8e71-cfaa1d136c09)
<br/>

> 404가 발생한다.<br/>
> commit을 진행하면 그 이후 Github에 가입한 본인 이메일로 사유가 날아온다.<br/>
{: .prompt-info }
<br/>

---
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a2709354-48d0-435d-a137-1be953ab7ebb)

---

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/60271acd-1415-4a25-8442-5fc3cda1797c)
<br/>

> 사진을 보면 59초에 걸쳐 진행했지만 실패했다고 나온다.<br/>
> 실패한 과정은 build 과정.<br/>
> x 표시가 떠있는 build를 클릭하면 build의 과정을 확인할 수 있는데,<br/>
{: .prompt-info }
<br/>

---
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/43bbd1e0-e744-41fc-bd5d-6b1a5a8ede41)
<br/>

> Test site 라는 과정에서 문제가 발생했다.<br/>
> 클릭해서 토글을 열고 에러코드를 확인하자.<br/>
{: .prompt-info }
<br/>

---
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/08815520-bb4f-4dc3-9ce8-8b74c87f9238)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/e78f4279-bcc1-452c-989e-2e3c22b3049d)


> 에러 코드<br/>
{: .prompt-warning }

``` shell
---
Run bundle exec htmlproofer _site --disable-external --check-html --allow_hash_href
Running ["ScriptCheck", "LinkCheck", "ImageCheck", "HtmlCheck"] on ["_site"] on *.html... 


Ran on 23 files!


- _site/404.html
  *  internal script /assets/js/dist/page.min.js does not exist (line 1)
- _site/about/index.html
  *  internal script /assets/js/dist/page.min.js does not exist (line 1)
- _site/archives/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/categories/blogging/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/categories/demo/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/categories/index.html
  *  internal script /assets/js/dist/categories.min.js does not exist (line 1)
- _site/categories/tutorial/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/index.html
  *  internal script /assets/js/dist/home.min.js does not exist (line 1)
- _site/posts/customize-the-favicon/index.html
  *  internal script /assets/js/dist/post.min.js does not exist (line 1)
- _site/posts/enable-google-pv/index.html
  *  internal script /assets/js/dist/post.min.js does not exist (line 155)
- _site/posts/getting-started/index.html
  *  internal script /assets/js/dist/post.min.js does not exist (line 29)
- _site/posts/text-and-typography/index.html
  *  internal script /assets/js/dist/post.min.js does not exist (line 22)
- _site/posts/write-a-new-post/index.html
  *  internal script /assets/js/dist/post.min.js does not exist (line 183)
- _site/tags/favicon/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/tags/getting-started/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/tags/google-analytics/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/tags/index.html
  *  internal script /assets/js/dist/commons.min.js does not exist (line 1)
- _site/tags/pageviews/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/tags/typography/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)
- _site/tags/writing/index.html
  *  internal script /assets/js/dist/misc.min.js does not exist (line 1)

HTML-Proofer found 20 failures!
Error: Process completed with exit code 1.
---
```

<br/>

> <b>"Error: Process completed with exit code 1."</b> 코드를 뱉어내고 있다.<br/>
> /assets/js/dist 경로에서<br/>
><br/>
> page.min.js<br/>
> misc.min.js<br/>
> categories.min.js<br/>
> home.min.js<br/>
> post.min.js<br/>
> commons.min.js<br/>
><br/>
> 이런 js 파일들을 찾지 못 한다고 한다.<br/>
{: .prompt-warning }
<br/>

> 처음에는 이 에러를 잡으려고 엄청 노력했다.<br/>
{: .prompt-info }
<br/>

---

## ● 실패 했던 방법들
<b>(구글에서 찾거나 chirpy-theme의 공식 GitHub, js 빌드 관련 공식 doc 등)</b><br/>

<details>
    <summary><b>1. js 파일을 직접 /assets/js/dist 폴더에 넣기 - 실패</b></summary>
    <br/>
    해당 파일들은 _javascript 폴더 안에 존재하는데 배포 과정에서는 /assets/js/dist 에서 찾으려고 한다.<br/>
    그래서 찾아보니 js 파일이 컴파일 되면서 dist 에 들어가게 되는데(npm 을 사용하는 node의 특성 상, build 후 파일들이 dist에 저장된다.)<br/>
    이 과정을 제대로 진행하지 못 한다는 것.<br/>
    <br/>
    <img src="/assets/img/create-git-blog-3_error/K-007.png" />
    <br/>
    <i>(실제로 dist 폴더는 존재하지도 않는다.)</i><br/>
    그래서 <b>임의로 dist 폴더를 생성해서 넣으려다</b>가<br/>
    그런 과정을 정리한 사람을 찾지도 못 했고, 이러한 과정을 거치면 그게 어떻게 자동 배포인가, 하드코딩이지.<br/>
    <br/>
    <b>포기 - <span style="color: red;">실패</span></b><br/>
</details>

---

<details>
    <summary><b>2. _javascript 폴더를 root 경로에 추가하기 - 실패</b></summary>
    <br/>
    해당 파일이 빌드되지 못해서 dist 폴더에 생성되지 않는다면 <b>_javascript 폴더 자체를 루트 경로에 추가</b>하면 되지 않을까 싶었다.<br/>
    확인해보니 _config.yml 에 _javascript 경로를 추가할 수 있는 방법이 있다.<br/>
    _config.yml 상단에<br/>
    <br/>
    <img src="/assets/img/create-git-blog-3_error/K-008.png" />
    <br/>
    <div style="background-color: #f8f9fa;">
        <span style="color: #07a8f7;">include:</span><br/>
	    <span style="color: #000000;">- _javascript</span><br/>
    </div>
    <br/>
    <b>추가하는 방법 - <span style="color: red;"> 실패</span></b><br/>
    <br/>
    <i style="font-size: 5px;">출처: https://velog.io/@lzlko/github-%EB%B8%94%EB%A1%9C%EA%B7%B8</i><br/>
</details>

---

<details>
    <summary><b>3. npm i && npm run build 를 통해 직접 빌드하기</b></summary>
    <br/>
    chirpy theme의 공식 GitHub의 Issues 에서 나온 해결 방법이다.
    npm i 는 되지만 <b>npm run build 시 실패</b>한다. NODE_ENV=production 이라는 설정을 npm에서는 명령어로 받아들이려 한다는 점.<br/>
    그래서 build 자체에 에러가 나는 상황에, 명령어를 조금 수정하거나 빌드 환경 설정을 짜려고 했지만 이미 겉잡을 수 없이 작업의 방향이 엇나갈 가능성이 있을 것 같아서 포기했다.<br/>
    <br/>
    <b>포기 - <span style="color: red;"> 실패</span></b><br/>
    <i style="font-size: 5px;">출처: https://github.com/cotes2020/jekyll-theme-chirpy/issues/1007</i><br/>
</details>

---

<details>
    <summary><b>4. 자동 배포의 빌드가 실패하고 로컬은 성공하니 로컬 환경으로 빌드된 파일을 직접 커밋하기</b></summary>
    <br/>
    우리가 포스팅 한 글은 _posts 폴더에 등록하여 빌드 시, <b>_site 폴더에 .html 파일 등</b>이 만들어 지는데 이를 직접 커밋하려 했다.<br/>
    하지만 .gitignore 에 <br/>
    <br/>
    <b><span style="color: #000000;">_site</span></b><br/>
    가 추가되어 있어서<br/>
    <img src="/assets/img/create-git-blog-3_error/K-009.png" />
    <img src="/assets/img/create-git-blog-3_error/K-010.png" />
    <br/>
    해당 부분에서 포스팅 관련 html을 담고 있는 _site/posts 를 .gitignore 에서 제외하고 반영하려 했지만
    이것도 사실상 하드코딩이 될 가능성이 높아서<br/>
    <br/>
    <b>포기 - <span style="color: red;"> 실패</span></b><br/>
    <br/>
    <i style="font-size: 5px;">.gitignore 제외하기 - 출처: https://nochoco-lee.tistory.com/48</i><br/>
</details>
<br/>

---
<br/>

## ● 내가 찾은 방법

> 이외에도 여러 과정을 거쳐서 스스로 방법을 찾아냈다.<br/>
> <b>에러가 나는 과정은 Test site</b>, 해당 과정이 빌드 시 실패한다는 점.<br/>
> 그리고 <b>Test site 에대한 설정은 pages-deploy.yml 에 포함되어</b> 있다.<br/>
{: .prompt-warning }

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4ab9e3fe-69b3-4855-bee6-d722381de7b2)
<br/>

### 저것만 제외해보자.

---

<br/>

> <b>오, commit & push 이후에 아무리 기다려도 실패했다는 이메일이 오지 않는다.</b><br/>
> 그렇다면 해당 <b>Repository의 Action 탭</b>에 들어가서 확인하자.<br/>
{: .prompt-info }
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/5b4592e9-221d-4f20-a464-36295edd2a0c)
<br/>

> 2분 전에 내가 커밋한 내역이 <b style="color: green;">정상적으로 반영되었다</b>고 한다.<br/>
> 와.. 드디어...<br/>
><br/>
> 내가 커밋한 메세지를 클릭해서 build 과정을 확인하면<br/>
{: .prompt-info }
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/60e2cd65-c7ab-43c0-b45c-b3bc36d62272)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/bda793df-7635-4579-8179-0f46aa64e02a)
<br/>

> <b>build 과정에서 문제가 발생했던 Test site 과정을 빼고 진행한 것을 알 수 있다.</b><br/>
><br/>
> 그렇다면..<br/>
> <b>본인 아이디.github.io</b><br/>
> <b>로 접속</b>해서 정상적으로 배포가 된 것인지 확인하자..!<br/>
{: .prompt-info }
<br/>

---
<br/>

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/1b368a21-ffe8-44fd-817d-91106853e0ea)
<br/>
<br/>

> <b>짜잔!</b><br/>
> 어느정도 기다리면 배포를 마친 나만의 사이트가 완성된다!<br/>
> <br/>
> <b style="font-size: 20px;">이로써 에러 해결!</b><br/>
> <br/>
{: .prompt-tip }

## ● 후기

> 보면 알겠지만 정말 간단한 해결 방법이었다.<br/>
> <br/>
> <i>
> 하지만 이틀 걸렸나... <br/>
> <b style="color: red;">이 에러의 해결법을 아는 사람은 단 한 명, 단 한 개의 포스팅도 찾지 못 했다..</b><br/>
> <br/>
> </i>
> 원래 포스팅은 하지 않으려 했지만, 공식 GitHub 에도 같은 에러를 호소하는 사람들이 적지 않고,<br/>
> 이런 문제를 만나면 나처럼 삽질을 많이 할 사람들을 위해서 포스팅을 결심했다.<br/>
> <br/>
> 좋아하는 사수가 포스팅을 추천하기도 했다..ㅎ<br/>
{: .prompt-info }