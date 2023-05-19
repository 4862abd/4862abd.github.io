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
> 로 변경만 해주면 내가 진행하려는 <b>.zip 파일을 GitHub와 연동한 직후의 상태(2 - 6번)</b>와 동일해진다.<br/>
> 혹시 Fork를 딴 후, 진행상황을 모르겠다면 2 - 6번 부터 방법을 진행하면 된다.<br/>
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

## ● 시작
<br/>

### 1. Repository 생성, Git 연결

<!-- Create new Repository -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/f87a05c8-882d-44fa-948d-db485733e8f7)
<br/>

> <b>본인아이디.github.io</b><br/>
> 1 - 1. 위의 형태로 Repository 명을 생성해준다.<br/>
> 그리고 공개 타입을 <b>Public</b>으로 설정해야한다.<br/>
{: .prompt-tip }

---
<br/>

<!-- Git bash 실행 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/8b1c4381-4940-409e-b92b-2ca76c95efaa)
<br/>

> 1 - 2. 그리고 블로그의 소스를 관리할 폴더를 하나 만들어서 <b>Git bash</b>를 실행한다.<br/>
{: .prompt-tip }

---
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

> 1 - 3. 우선 global 옵션으로 본인의 GitHub 아이디와 이메일을 등록한다.<br/>
> global로 지정하지 않으면 추후 Git 작업을 할 때마다 등록을 해야할 수 있다.<br/>
{: .prompt-tip }

---
<br/>

<!-- Git bash 실행 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/bfcd0b22-b06e-4041-90c8-7745ede7a3dd)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/04b0b4d1-7931-4fc9-b6b1-4937c05f258f)
<br/>
<br/>

```bash
---
git clone https://github.com/유저 아이디/유저 아이디.github.io.git
exit
---
```

> 1 - 4. 그 후, 아까 새로 생성한 Repository의 url을 이용해서 아까 만든 빈 폴더에 git clone을 해준다.<br/>
> 정상적으로 clone이 되었다면 exit 명령어를 통해 bash를 종료한다.<br/>
{: .prompt-tip }

---
<br/>

### 2. 초기 설정, Git commit & push

<!-- .git 폴더 확인 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/825a0639-c981-4ba0-9655-a2321daed3e6)
<br/>
<br/>

> 2 - 1. 위의 과정을 거치면 <b>본인 아이디.github.io 의 이름을 가진 현재 폴더</b>에 아무 변화도 없어 보인다.<br/>
> 하지만 숨김 파일 보기 설정을 키는 순간 .git 폴더가 생긴 것을 확인할 수 있다.<br/>
{: .prompt-tip }

---
<br/>

<!-- 압축 해제 파일 붙여넣기 -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/f9a22a07-8250-4919-963c-8c0488dddfce)
<br/>
<br/>

```bash
---
git clone https://github.com/유저 아이디/유저 아이디.github.io.git
exit
---
```

> 2 - 2. 이제 아까 압축을 풀었던 jekyll-theme-chirpy-master.zip 폴더 안의 파일과 폴더들을<br/>
> .git 폴더가 있는 본인 아이디.github.io 폴더 안에 복사 붙여넣기 하자.<br/>
> 그러면 .git이라는 폴더가 같이 있는 블로그 소스의 폴더가 완성된다.<br/>
{: .prompt-tip }

---
<br/>

<!-- git add . -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/6070eb5c-c381-4433-ad51-af7a9b9727ff)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/41488b8a-60c3-45ac-9916-104f9bb45eb5)
<br/>
<br/>

```bash
---
git add .
---
```

> 2 - 3. 이제 해당 폴더 내의 파일들을 Git에 commit & push 할 차례이다.<br/>
> git bash를 다시 실행하자.<br/>
> 그 후, bash 에<br/>
> <b>git add .</b><br/>
> 를 입력한다.<br/>
> 우리는 이미 Git 의 연동에 대한 정보를 가지고 있으므로 바로 commit & push를 진행할 수 있는 상태이다.<br/>
> git add . 가 완료되면 사진과 같은 로그가 쭉 남는다.<br/>
{: .prompt-tip }

---
<br/>

<!-- git status -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/b3db959e-7d28-48f1-85ca-71b01da5941e)
<br/>
<br/>

```bash
---
git status
---
```

> 2 - 4. git add .를 통해 파일과 폴더들이 commit이 될 준비를 마쳤으니<br/>
> <b>git status</b><br/>
> 를 입력해서 파일들이 초록색으로 변한 것을 확인한다.<br/>
> 초록색으로 표시되는 폴더, 파일은 정상적으로 대기 상태에 올랐다는 것을 확인한다.<br/>
> 만약 빨간색으로 표시가 된다면 git add . 를 안했거나 깃에 연결이 되어있지 않는 등 문제가 발생한 것이다.<br/>
{: .prompt-tip }

---
<br/>

<!-- git commit -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/af756989-ca55-41ae-9167-fda635809602)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/41a8d2c9-4501-42fa-b430-5bf8b4bdd782)
<br/>
<br/>

```bash
---
git commit -m "커밋 메세지"
---
```

> 2 - 5. 이제 commit을 해보자.<br/>
> -m 은 커밋 메세지를 남기는 옵션이다.<br/>
> 작성자 같은 경우는 처음 블로그의 소스를 commit 하는 것이니 [SYSTEM] INIT 이라는 메세지를 남겼다.<br/>
> commit 이 진행되면서 이렇게 쭉 로그가 출력된다.
{: .prompt-tip }

---
<br/>

<!-- git push -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/4674c658-562d-4778-815e-594b89be9850)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/f70dc63e-c88d-481d-8ef9-59720441cf5b)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/b42dcc29-4a3f-439a-8f50-9e1811e548ba)
<br/>
<br/>

```bash
---
git push origin main
---
```

> 2 - 6. commit을 했으니 push를 해야 한다.<br/>
> 위의 push 명령어를 이용하고 origin의 main이라는 브랜치로 push를 진행한다.<br/>
> 마찬가지로 로그가 쭈욱 남는 것을 확인할 수 있다.<br/>
> 이제 Github의 해당 Repository로 들어가면 코드가 추가된 것을 확인할 수 있다.<br/>
{: .prompt-tip }

<br/>
<details>
    <summary><b>이 부분에서 Fork 한 사람들과 차이점이 발생할 수 있다.</b></summary>
        <br/>
        &nbsp;&nbsp;&nbsp;&nbsp;위의 작업을 마치면 Fork를 뜬 Repository의 상태와 동일하게 된다.<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;<b>하지만 다른점이 한 개 있는데 Fork를 뜬 Repository는 default 브랜치가 master로 잡힐 것이다.</b><br/>
        &nbsp;&nbsp;&nbsp;&nbsp;해당 테마에서 기본 브랜치를 master로 잡아뒀기 때문이다.<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;해결법: <a href="https://4862abd.github.io/posts/create-git-blog-2" target="_blank">Fork와 .zip의 차이점</a>
</details>

---
<br/>

### 3. 블로그 가동
#### 작성자 같은 경우, 소스를 편하게 관리하기 위해 VSCode를 이용하기로 했습니다.
<br/>

<!-- Check GitHub, Delete etc. files -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/12ce7f40-2dba-4743-86ab-7623e7c75218)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/a656da91-0bfe-49a5-b886-09906cc86879)
<br/>
<br/>

> 3 - 1. GitHub에 파일이 정상적으로 올라간 것을 확인한다.<br/>
> 이 중에서 배포를 위해 지워야 할 파일이 몇 있다.<br/>
> <br/>
> <b>commitlint.yml</b><br/>
> <b>page-deploy.yml.hook</b><br/>
> <br/>
> .github - workflows 안에 있는 위 두 파일을 남기고 .github 내의 모든 폴더, 파일을 지운다.<br/>
> 그리고 page-deploy.yml.hook 이라는 파일의 명에서 .hook 부분을 지워서<br/>
> <b>page-deploy.yml</b><br/>
> 로 만든다.<br/>
{: .prompt-tip }

<br/>
<br/>

<!-- Check deleted files -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/422ec085-3ff4-48cb-9ad8-a3523c1c7ffd)
<br/>
<br/>

> 그러면 이렇게 <b>.github - workflows - commitlint.yml, pages-deplay.yml 만 남아있는 상태가 된다.</b><br/>
{: .prompt-tip }

---
<br/>

<!-- bundle install -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/9fba819e-6bcc-44a3-b833-21733e166930)
<br/>
<br/>

```shell
---
bundle install
---
```

> 3 - 2. 그 후, 위의 명령어를 입력합니다.<br/>
> <b>대부분의 포스팅에서 저 명령어를 입력하기 이전에 tools/... 이러한 명령어를 입력하라고 한다.</b><br/>
> 이는 초기 설정에 필요 없는 파일을 날리는 명령어라고 하는데,<br/>
> <b>굳이 하지 않아도 문제 없다.</b><br/>
> <br/>
> build나 deploy가 꼬이는 문제는 어떡하냐고 묻는다면..<br/>
> 요새 버전에는 해당 파일들이 없는 경우도 있고 포함 되어 있는 .gitignore 작성이 정말 잘 되어 있어서 꼬일 수 있는 파일은 애초에 Git에 올리질 못 한다.<br/>
> <br/>
> 하지만 혹시라도 찝찝한 사람들을 위해 지워야 할 파일 목록은 남겨 두겠다.<br/>
> 지워야 할 파일 목록:<br/>
> 1. .travis.yml<br/>
> 2. docs 폴더<br/>
> 3. _posts 폴더 하위의 파일들<br/>
> <br/>
>
> 우선 작성자는 1, 2 번의 파일, 폴더가 존재하지도 않았다.<br/>
{: .prompt-tip }

---
<br/>

<!-- bundle install -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/9fba819e-6bcc-44a3-b833-21733e166930)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/22633a45-d076-4e3f-b567-267e68e4c93f)
<br/>
<br/>

```shell
---
bundle install
---
```

> 3 - 3. 그 후, 위의 명령어를 입력한다.<br/>
> 여러 파일이 생성되는 것을 확인할 수 있다.<br/>
> ex)<br/>
> _site 폴더<br/>
> Gemfile.lock 등<br/>
{: .prompt-tip }

---
<br/>

<!-- jekyll serve -->
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/3a04e61a-d568-4a75-ba20-3893a742272e)
![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/cc938f88-2759-48ad-a8fb-8f41f247bd34)
<br/>
<br/>

```shell
---
jekyll serve
---
```

> 3 - 4. 위의 명령어를 입력하여 서버를 가동하고<br/>
> 두 번째 사진 같이 로그가 나온다면 성공이다.<br/>
> 로그를 보면 http://127.0.0.1:4000 으로 접속이 가능하다고 하며, 콘솔에서 ctrl + c 를 눌러서 중지할 수 있다고 한다.<br/>
> 그리고 다들 아시겠지만 127.0.0.1 은 localhost 로 대체가 가능하다.<br/>
><br/>
> 접속 해보자.<br/>
{: .prompt-tip }

![image](https://github.com/cotes2020/jekyll-theme-chirpy/assets/77370682/9cb02c2e-90cc-4351-9c3d-6a0a977cb256)
<br/>
<br/>

> 자, 이제 블로그 가동을 마쳤다!<br/>
> 나온 산출물을 Git에 commit & push (2 - [3, 4, 5, 6] 확인) 하고 이 과정은 마무리 한다.<br/>
{: .prompt-tip }

### 4. 자동 배포 오류 해결..
#### 내 불행은 여기서 멈추질 못 했다..

내가 접한 에러: [**깃 블로그 개설하기(Chirpy theme) - 오류 해결**](https://4862abd.github.io/posts/create-git-blog-3_error)


<br/>
