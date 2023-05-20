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
><br/>
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