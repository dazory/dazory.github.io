---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[TechBlog-01] Jekyll 블로그 개설하기'
excerpt: "[기술블로그 운영하기] Jekyll을 Github pages에 호스팅하는 방법"
date: 2021-07-06
last_modified_at: 2021-09-26
categories:
  - etc-blog
tags: 
   - [blog]

use_math: true
comments: true
share : false
---

<br>

# 배경

**기술블로그**란 <span style="background-color:rgba(255, 255, 102, .5)">개발에 대한 내용이 중점인 블로그</span>로, [당근마켓](https://medium.com/daangn), [카카오](https://tech.kakao.com/blog/), [쿠팡](https://medium.com/coupang-tech) 등 여러 기업에서 최신 기술에 대한 정보를 제공하고, 기술적 역량을 어필하여 더 좋은 개발자를 채용하기 위한 목적에서 운영중이다. 기업뿐만 아니라 여러 개발자들도 <span style="background-color:rgba(255, 255, 102, .5)">학습을 정리하거나 기록하기 위한 목적</span>으로 운영하고 있는데, 나 역시 이러한 목적에서 기술블로그를 시작하고자 한다. 이번 포스팅에서는 <span style="background-color:rgba(255, 255, 102, .5)">몇 가지 블로그 플랫폼에 대해 소개하고, 그 중에서 내가 사용할 GitHub page에 대한 상세한 사용방법을 소개한다</span>.


<br>
<br>

# 블로그 플랫폼

기술블로그를 시작할 때 사용할 수 있는 몇가지 블로그 플랫폼은 다음과 같다. <span style="background-color:rgba(255, 255, 102, .5)">각 블로그 플랫폼이 가진 특징 및 장점과 단점을 비교해본다.</span>

## 플랫폼 비교

### 네이버 블로그

네이버 블로그는 개인적으로 너무너무 제약이 많고 그래서 불편하다고 생각한다. 기술블로그용 플랫폼으로는 적합하지 않은 느낌..

<b>장점</b>

* 개설과 블로그 디자인이 직관적이므로 일반인 사용자도 **쉽게 사용**할 수 있다.

<b>단점</b>

* 파워블로그와 같이 **상업적 성격**을 띠는 블로그가 많아짐에 따라 네이버 검색 순위에서 밀리는 경우가 많아졌다.
* **커스터마이징에 한계**가 있다.

<b>관련 블로그</b>

* 티몬 개발 블로그 : [https://blog.naver.com/tmondev](https://blog.naver.com/tmondev)

[네이버 블로그 바로가기](https://section.blog.naver.com/BlogHome.nhn?directoryNo=0&currentPage=1&groupId=0)

<br>

### 티스토리(Tistory)

티스토리를 기술 블로그로 활용하는 분들도 많고, 적절히 커스텀할 수 도 있어서 나쁘지 않다. 다만 에디터보단 마크다운 문법이 더 익숙하다면 비추한다 (적용된 모습을 확인하면서 작업할 수 없기때문에 불편하다). 나 역시 에디터가 불편하다고 느껴서 티스토리를 잠깐 사용하다가 말았다.

<b>장점</b>

* 개설과 블로그 디자인이 **쉽고** 네이버블로그보다 **디자인이 심플**하다.
* 커스터마이징이 쉬워 **여러 API를 적용**할 수 있다. **HTML/CSS 수정이 가능**하다.
* **비밀글** 기능이 있어서 민감한 내용을 포함한 글은 숨길 수 있다.

<b>단점</b>

   * **글쓰기 도구에 한계**가 있다. (글 편집이 너무 어렵다😢)

<b>관련 블로그</b>

* 카카오 엔터프라이즈 기술블로그 : [https://tech-kakaoenterprise.tistory.com/](https://tech-kakaoenterprise.tistory.com/)

[티스토리 바로가기](https://www.tistory.com/)

<br>

### Medium

최근에 알게 된 블로그 플랫폼이다. 사용해보진 않았지만, 개발블로그로서 필요한 기능은 그래도 갖추고 있는 것 같다. 메디움은 인터페이스가 굉장히 깔끔한데 이게 장점이자 단점이다. 현재 많은 기업들이 이 플랫폼을 사용하는 걸로 봐서 괜찮은 선택지 중 하나라고 생각한다.

<b>장점</b>

* Publication 기능이 있어서, 어딘가에 소속되어 글을 쓸 수 있다.
* 인터페이스가 굉장히 깔끔하다.

<b>단점</b>

* 코드 하이라이팅 기능이 없다. (GitHub Gist로 해결할 순 있을 것 같다.)
* 카테고리 기능이 없다.

<b>관련 블로그</b>

* Coupang Tech : [https://medium.com/coupang-tech](https://medium.com/coupang-tech)
* Airbnb Tech : [https://medium.com/airbnb-engineering](https://medium.com/airbnb-engineering)
* Netflix Tech : [https://netflixtechblog.com/](https://netflixtechblog.com/)
* Google Play : [https://medium.com/googleplaydev](https://medium.com/googleplaydev)
* 당근마켓 : [https://medium.com/daangn](https://medium.com/daangn)


[Medium 바로가기](https://medium.com/coupang-tech)

<br>

### GitHub Page

내가 활용하고 있는 블로그 플랫폼이다. 초반에 환경 구축하는 게 조금 까다롭긴 하지만 한 번 익숙해지면 이것만큼 편한 것도 없다고 생각한다. 나는 로컬PC에서 [Typo](https://typora.io/) 프로그램으로 글을 작성 및 관리하고, Visual Studio를 이용하여 GitHub에 업로드하고있다. 물론 애초에 블로그를 위한 서비스는 아니기때문에 불편한 점도 꽤 있긴 하다.

<b>장점</b>

* **커스터마이징**이 매우 자유롭다.

<b>단점</b>

* 일반인이 **입문하기 어려우며** 프로그래밍 지식이 부족하다면 기능이 매우 제한된다.
* 이미지를 일일이 GitHub에 push해주어야 한다. (폴더에 불필요한 이미지가 포함되면 일일이 삭제해주어야하므로 약간 불편하다.)
* 블로그에 커스텀 및 글 **업로드 반응이 느리다.**

<b>관련 블로그</b>

* 우아한형제들 기술 블로그 : [https://techblog.woowahan.com/](https://techblog.woowahan.com/)
* spoqa 기술 블로그 : [https://spoqa.github.io/](https://spoqa.github.io/)
* 마켓컬리 기술 블로그 : [https://helloworld.kurly.com/](https://helloworld.kurly.com/)
* 올리브영 기술 블로그 : [https://tech.oliveyoung.co.kr/](https://tech.oliveyoung.co.kr/)

[Github Page 바로가기](https://pages.github.com/)

<br>
<br>

# GitHub Page

나는 글이 길어져도 글의 전체 구조를 쉽게 파악할 수 있도록 **table of contents 기능**이 너무 갖고싶었으나 네이버 블로그는 지원하지 않았다. Tistory는 HTML/CSS 편집을 통해 해당 기능을 넣을 수 있었으나, 글편집기는 너무 불편하다고 느껴졌다 ( 코드블록 하이라이트를 하려면 별도로 플러그인을 설치해주어야 한다. 또한 에디터가 사용하기에 너무 불편하다.). 따라서 <span style="background-color:rgba(255, 255, 102, .5)">자유로운 커스터마이징이 가능한 GitHub Page를 이용하여 블로그를 개설</span>하였다. 

![table of contents example](https://mmistakes.github.io/minimal-mistakes/assets/images/mm-toc-helper-example.jpg)<span style="color:gray">*Fig 1. minimal-mistakes의 table of content 이미지. 오른쪽 리스트와 같이 본문에 붙어서 독자가 읽는 위치를 알려주는 기능 및 바로가기 기능을 한다.*</span>

<div class="callout">네이버 < 티스토리 < 깃헙페이지 순으로 커스터마이징이 자유롭다. 티스토리도 웬만한 커스텀은 가능하므로 좀 더 직관적인 글쓰기를 하고싶다면 티스토리도 추천한다.</div>

<br>

GitHub Pages는 단순히 파일들을 호스팅하여 사이트를 생성해주는 역할만을 한다. 직접 zero부터 시작해서 웹페이지를 생성하여 GitHub Page에 호스팅할 수도 있지만 각종 에러에 시달려야 할 수도 있다. 따라서 <span style="background-color:rgba(255, 255, 102, .5)">나는 **Jekyll**이라는 정적 사이트 생성기를 이용하여 블로그를 개설할 것이다.</span>

<div class="callout"> 직접 처음부터 끝까지 웹페이지를 만드는 것은 매우 비효율적이다. 웹개발에 뜻이 있다면 말리지는 않겠지만, 웹개발 고수가 아니라면 차라리 Jekyll과 같이 테마를 다운받아 시작해서 전체적인 사이트 구조를 파악하고 하나씩 커스텀해 나가는 방식을 추천한다.</div>

<br>

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

#### 블로그 초기 세팅

[여기](https://syki66.github.io/blog/2020/04/12/minimal-mistakes-theme.html)를 참고하여 초기 세팅을 할 수 있다.

<br>

#### 디렉토리 설명

주요 디렉토리 구조는 다음과 같다.

* `_data` 

  데이터를 저장하는 것과 관련된 폴더이다. 기술블로그인 경우, 멤버들의 정보를 저장하기도 하고,  navigation 등을 위해 사용되기도 한다. 여기에 저장된 데이터를 필요로하는 페이지에서 불러와 사용된다. 어떤 데이터를 여러 페이지에서 사용하고싶고, 이후에 수정을 한 곳에서 하고싶은 경우에 활용할 수 있다.

  (사용예시)

  1. /_data/members.yml (출처: [올리브영 기술블로그](https://tech.oliveyoung.co.kr/about/))

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926221905840.png" alt="image-20210926221905840" style="width:600px;" />

     이런식으로 data를 저장하면, 아래와 같이 다양한 페이지에서 불러와 사용할 수 있다.

     * 개발자 소개 페이지

       <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926222138698.png" alt="image-20210926222138698" style="width:500px;" />

     * 포스팅

       <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926222055373.png" alt="image-20210926222055373" style="width:500px;" />

  2. /_data/navigation.yml

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926222314140.png" alt="image-20210926222314140" style="width:400px;" />

     위와 같이 저장된 navigation정보는 아래와 같이 메인 페이지에서 불러올 수 있다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926222409490.png" alt="image-20210926222409490" style="width:600px;" />

  <br>

* `_includes`

  페이지 렌더링이나 여러 보이지 않는 기능들과 관련된 폴더이다. 주로 전반적인 페이지 모두에게 적용되는 코드들을 넣어서 관리한다.

  (사용예시)

  1. /_includes/head.html

     head.html은 모든 페이지의 head 태그 부분에 들어가는 코드를 담는다. 아래와 같이 에서 .css파일을 link하여 페이지를 렌더링할 수 있다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926222844353.png" alt="image-20210926222844353" style="width:700px;" />

  2. /\_includes/nav_list_main, /\_includes/nav_list

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926223133060.png" alt="image-20210926223133060" style="width:300px;" />

     블로그 왼쪽에 위와 같이 항상 붙어있는 카테고리 항목을 볼 수 있는데, 이 역시 _includes 폴더에 navigation list를 담아서 생성했다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926223321037.png" alt="image-20210926223321037" style="width:700px;" />

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926223038459.png" alt="image-20210926223038459" style="width:700px;" />

     [공부하는 식빵맘](https://ansohxxn.github.io/blog/category/) 블로그를 참고하여 만들었다.

  <br>

* `_layouts`

  포스팅의 레이아웃을 관리하는 폴더이다. 다양한 레이아웃을 만들어놓고 상황에 따라 적용할 수 있다.

  (사용예시)

  1. _layouts/post-with-comments.html

     minimal-mistakes의 기본 layout인 archive.html을 조금 수정하여 댓글 기능이 들어가도록 만든 레이아웃이다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926223750261.png" alt="image-20210926223750261" style="width:500px;" />

     아래에서 더 자세히 설명할 거지만, _posts나 _pages에서 이 레이아웃들을 가져와 사용할 수 있다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926223917687.png" alt="image-20210926223917687" style="width:400px;" />

  <br>

* `_pages`

  사이트의 각 페이지를 관리하는 폴더이다.

  (사용예시)

  1. _pages/about.md

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926224128550.png" alt="image-20210926224128550" style="width:500px;" />

     위와 같은 about 페이지를 markdown언어로 작성하여 페이지를 구성할 수 있다.

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926224039292.png" alt="image-20210926224039292" style="width:500px;" />

  2. _pages/404.md

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926224326082.png" alt="image-20210926224326082" style="width:500px;" />

     404 페이지 역시 여기에서 작성할 수 있다.

  <br>

* `_posts`

  포스팅하는 글을 관리하는 폴더이다. 아래 이미지와 같이 .md로 작성된 다양한 포스트들을 관리하고있다.

  <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926224434765.png" alt="image-20210926224434765" style="width:300px;" />

  (사용예시)

  1. _posts/2021-08-08-cs-algorithm-algo03.md

     <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926224515392.png" alt="image-20210926224515392" style="width:600px;" />

     _posts의 .md파일들은 대략적으로 두 부분으로 구성된다. 위 이미지에서 1~20번 라인은 해당 페이지의 속성에 관한 코드이고, 21번 라인 이후부터는 글 내용에 관한 코드이다. 

     1~20번 라인을 통해 해당 글의 레이아웃, 제목, 날짜 등의 속성을 적용할 수 있다.

  <br>

* `_sass`

  minimal-mistakes 테마에 관한 .scss 파일들이 들어있다. 나는 무서워서 잘 건드리진 않는다.

  <br>

* `assets`

  .css나 .js 파일을 관리할 수 있다. main.scss나 _main.js는 건드리지 않는 것이 좋고, 수정이 필요하다면 custom.css나 custom.js라는 파일을 따로 만들어서 _includes/head.html에서 링크걸어 사용하는 것이 좋다.

  <br>

* `files`

  블로그에서 사용되는 이미지들을 저장하는 폴더로 사용중이다. 

  <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926225100799.png" alt="image-20210926225100799" style="width:400px;" />

  포스트별로 폴더를 만들어서 이미지를 저장하고, 포스트에서 불러와 사용한다. Typora의 기능중에 복사한 이미지의 경로를 환경설정할 수 있는데, 이를 이용하여 아래와 같이 typora에서 작성한 글에 관련된 이미지들을 편리하게 관리하고 있다.

  <img src="/files/2021-07-06-etc-blog-TechBlog01/image-20210926225335081.png" alt="image-20210926225335081" style="width:600px;" />

<br>

#### 각종 기능 추가하기

[공부하는 식빵맘](https://ansohxxn.github.io/categories/blog) 블로그에 가면 카테고리기능, 스크롤바 커스텀, 댓글기능 등 다양한 기능에 관한 설명을 확인할 수 있다.

<br>


## Tips

### 1. markdown 편집

VScode에서 markdown을 편집하는건 너무 힘들다. 사용하기 좋은 몇 가지 markdown 편집기가 있다.

1. **Notion**

   notion에서 글을 작성하면 기본적으로 markdown 기반으로 작성된다. 이후 [Export]-markdown을 하여 `.md`파일을 생성할 수 있다. 나는 간혹 레이아웃이 깨지는 경우가 있어서 잘 사용하지 않는다.

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

   
<br>
<br>
