---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[TechBlog-01] Jekyll 블로그 개설하기'
excerpt: "[기술블로그 운영하기] Jekyll을 Github pages에 호스팅하는 방법"
date: 2021-07-06
last_modified_at: 2021-07-13
categories:
  - etc-blog
tags: 
   - [blog]

use_math: true
comments: true
share : false
sidebar_main: true
---

<br>

# 배경

**기술블로그**란 <span style="background-color:rgba(255, 255, 102, .5)">개발에 대한 내용이 중점인 블로그</span>로, [당근마켓](https://medium.com/daangn), [카카오](https://tech.kakao.com/blog/), [쿠팡](https://medium.com/coupang-tech) 등 여러 기업에서 최신 기술에 대한 정보를 제공하고, 기술적 역량을 어필하여 더 좋은 개발자를 채용하기 위한 목적에서 운영중이다. 기업뿐만 아니라 여러 개발자들도 <span style="background-color:rgba(255, 255, 102, .5)">학습을 정리하거나 기록하기 위한 목적</span>으로 운영하고 있는데, 나 역시 이러한 목적에서 기술블로그를 시작하고자 한다. 이번 포스팅에서는 <span style="background-color:rgba(255, 255, 102, .5)">몇 가지 블로그 플랫폼에 대해 소개하고, 그 중에서 내가 사용할 GitHub page에 대한 상세한 사용방법을 소개한다</span>.


<br>
<br>

# 블로그 플랫폼

기술블로그를 시작할 때 사용할 수 있는 몇가지 블로그 플랫폼은 다음과 같다. <span style="background-color:rgba(255, 255, 102, .5)">각 블로그 플랫폼이 가진 특징 및 장점과 단점을 비교해본다.</span>

## 플랫폼 비교

1. **네이버 블로그**

   장점

    * 개설과 블로그 디자인이 직관적이므로 일반인 사용자도 **쉽게 사용**할 수 있다.

   단점

   * 파워블로그와 같이 **상업적 성격**을 띠는 블로그가 많아짐에 따라 네이버 검색 순위에서 밀리는 경우가 많아졌다.

   * **커스터마이징에 한계**가 있다.

2. **티스토리(Tistory)**

   장점

   * 개설과 블로그 디자인이 **쉽고** 네이버블로그보다 **디자인이 심플**하다.
   * 커스터마이징이 쉬워 **여러 API를 적용**할 수 있다. **HTML/CSS 수정이 가능**하다.

   단점

   * **글쓰기 도구에 한계**가 있다. (글 편집이 너무 어렵다😢)

3. **GitHub Page**

   장점

   * **커스터마이징**이 매우 자유롭다.

   단점

   * 일반인이 **입문하기 어려우며** 프로그래밍 지식이 부족하다면 기능이 매우 제한된다.

## 결론

나는 글이 길어져도 글의 전체 구조를 쉽게 파악할 수 있도록 **table of contents 기능**이 너무 갖고싶었으나 네이버 블로그는 지원하지 않았다. Tistory는 HTML/CSS 편집을 통해 해당 기능을 넣을 수 있었으나, 글편집기는 너무 불편하다고 느껴졌다 ( 코드블록 하이라이트를 하려면 별도로 플러그인을 설치해주어야 한다. 또한 에디터가 사용하기에 너무 불편하다.). 따라서 <span style="background-color:rgba(255, 255, 102, .5)">자유로운 커스터마이징이 가능한 GitHub Page를 이용하여 블로그를 개설</span>하였다. 

![table of contents example](https://mmistakes.github.io/minimal-mistakes/assets/images/mm-toc-helper-example.jpg)<span style="color:gray">*Fig 1. minimal-mistakes의 table of content 이미지. 오른쪽 리스트와 같이 본문에 붙어서 독자가 읽는 위치를 알려주는 기능 및 바로가기 기능을 한다.*</span>

> 네이버 < 티스토리 < 깃헙페이지 순으로 커스터마이징이 자유롭다. 티스토리도 웬만한 커스텀은 가능하므로 좀 더 직관적인 글쓰기를 하고싶다면 티스토리도 추천한다.


<br>
<br>


# GitHub Page

GitHub Pages는 단순히 파일들을 호스팅하여 사이트를 생성해주는 역할만을 한다. 직접 zero부터 시작해서 웹페이지를 생성하여 GitHub Page에 호스팅할 수도 있지만 각종 에러에 시달려야 할 수도 있다. 따라서 <span style="background-color:rgba(255, 255, 102, .5)">나는 **Jekyll**이라는 정적 사이트 생성기를 이용하여 블로그를 개설할 것이다.</span>

> 직접 처음부터 끝까지 웹페이지를 만드는 것은 매우 비효율적이다. 웹개발에 뜻이 있다면 말리지는 않겠지만, 웹개발 고수가 아니라면 차라리 Jekyll과 같이 테마를 다운받아 시작해서 전체적인 사이트 구조를 파악하고 하나씩 커스텀해 나가는 방식을 추천한다.

## 간단한 블로그 개설 튜토리얼

본 튜토리얼은 Windows 개발환경에서 진행되었음을 알린다.

### 1. 개발 환경세팅

1. **Git 설치**

   [git 공식 사이트](https://git-scm.com/)에 접속하여 본인 개발환경에 맞게 git을 다운받는다. 필자는 Widnows용 설치파일을 다운로드받았다. 기본 세팅 그대로 Next 버튼을 클릭하여 설치를 완료한다.

2. **Ruby 설치**

   [Ruby 설치 페이지](https://www.ruby-lang.org/ko/)에 접속하여 본인 개발환경에 맞게 Ruby를 다운받는다. 필자는 Windows환경이므로 [RubyInstaller](https://rubyinstaller.org/)를 사용하여 WITH DEVKIT 버전을 다운로드하였다. 기본 세팅 그대로 설치를 완료하면 된다.

   끝까지 Next를 클릭하면 아래와 같이 커맨드창이 나오는데, 3을 입력하여 설치를 마치며 된다.

   <img src="/files/image-20210704110358262.png" alt="image-20210704110358262" style="zoom:89%;"/><span style="color:gray">*Fig 2. RubyInstaller2 예시 이미지.*</span>

   

   설치 완료 후, 설치된 `Start Command Prompt with Ruby` 프로그램을 실행한 뒤 명령창에 다음과 같이 입력하여 인코딩을 부여해준다. 명령어 오류가 발생하지 않도록 해주는 명령어이다.

   ```ruby
   chcp 65001
   ```

   이후 아래 명령어를 통해 미리 jekyll과 bundler를 다운로드받아주었다.

   ```ruby
   gem install jekyll
   gem install bundler
   ```

3. **Visual Studio Code 설치**

   [Visual Studio Code 홈페이지](https://code.visualstudio.com/)에서 본인 개발환경에 맞게 VSCode를 다운받는다. 


<br>
<br>

### 2. Jekyll 환경 구축

1. **GitHub pages 생성**

   먼저 [GitHub](https://github.com/)에 접속하고 로그인한다. Create New repository를 클릭하여 Repository name값을 `본인이름.github.io`로 세팅하여 레포지토리를 생성한다.

   <img src="/files/image-20210704111604606.png" alt="image-20210704111604606" style="zoom:89%;"/>
   <span style="color:gray">*Fig 3. Repository name 예시 이미지. 나는 기존에 같은 이름의 repository가 존재하므로 이미 존재한다는 경고창이 뜬다.*</span>

2. **Git clone**

   나는 VSCode terminal 환경에서 작업하기위해 아래와 같이 VScode를 실행하고 Git Bash 터미널을 열어주었다.

   <img src="/files/image-20210704111917003.png" alt="image-20210704111917003" style="zoom:89%;"/><span style="color:gray">*Fig 4. VScode에서 Git Bash terminal을 여는 방법 이미지.*</span>

   다음의 명령어를 통해 본인이 생성한 repository를 로컬PC로 clone한다.

   ```bash
   git init
   git clone "https://github.com/dazory/dazory.github.io"
   ```   

3. **Jekyll 설치**

   >  [Jekyll 홈페이지](https://jekyllrb.com/)에 접속하면 여러 정적 사이트 테마를 확인할 수 있다. 필자는 가장 심플한 디자인인 [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)을 사용하기로 하였다.

   > 안정성이 좋은 release 버전으로 다운받기로 한다.

   해당 테마를 fork한 뒤, 압축을 풀고 위에서 생성했던 repository에 옮겨준다.

   이후 아래와 같이 불필요한 파일을 삭제해준다.

   * `.editorconfig`
   * `.gitattributes`
   * `.github` : github관련 항목을 내부에 배치하는 데에 사용되는 컨벤션 폴더
   * `/docs` : demo를 위한 파일이 들어있음
   * `/test` : test를 위한 파일이 들어있음. test를 하고싶다면 `http://localhost:4000/test/`에 접속하면 됨
   * `CHANGELOG.md`  : version이 업데이트되면서 수정된 내용이 담긴 마크다운 파일
   * `README.md` : 설명서
   * `screenshot-layouts.png` : README.md 파일에 이미지를 추가하기 위한 파일일 뿐
   * `screenshot.png` : README.md 파일에 이미지를 추가하기 위한 파일일 뿐

### 3. Customize

1. 블로그 초기 세팅

   [여기](https://syki66.github.io/blog/2020/04/12/minimal-mistakes-theme.html)를 참고하여 초기 세팅을 할 수 있다.



## Tips

### 1. markdown 편집

VScode에서 markdown을 편집하는건 너무 힘들다. 사용하기 좋은 몇 가지 markdown 편집기가 있다.

1. **Notion**

   notion에서 글을 작성하면 기본적으로 markdown 기반으로 작성된다. 이후 [Export]-markdown을 하여 `.md`파일을 생성할 수 있다.

2. **Typora**

   이 글 역시 [Typora](https://typora.io/)를 이용하여 편집되었다. notion은 지원하는 여러 기능이 추가로 있어서 export하여 그대로 git에 업로드하면 깨지는 경우가 있었다. 하지만 typora는 지원하는 기능이 정말 markdown으로 한정되어있어 깨질 염려가 없다.



### 2. 로컬에서 jekyll blog 확인해보기

매번 Github에 호스팅한 뒤 잘 적용이 되었는지를 확인하는 건 매우 번거롭다. 또한 어떤 에러가 발생할지 모르기때문에 반드시 로컬에서 확인을 하고 호스팅해주어야 한다. 이때 사용할 수 있는 명령어는 다음과 같다.

```bash
jekyll serve --port 8001
```

8001말고 다른 port number를 사용해도 괜찮지만, 이미 사용중인 port number는 피하도록한다.

이후 [localhost:8001](http://localhost:8001/)에 접속하여 github pages에 호스팅된 결과를 시뮬레이션해볼 수 있다.


<br>
<br>


# References

1. 개발 환경세팅 : https://murra.tistory.com/160#%EB%AA%A9%EC%B0%A8

2. Jekyll 블로그 생성 및 git에 업로드 : https://2ssue.github.io/blog/make_jekyll_blog/

3. 블로그 초기환경 설정 : https://syki66.github.io/blog/2020/04/12/minimal-mistakes-theme.html

4. `_post`폴더에 글 등록 : https://devinlife.com/howto%20github%20pages/first-post/

5. notion에서 export markdown : https://swieeft.github.io/2020/03/02/NotionToGithubioPorting.html

   


