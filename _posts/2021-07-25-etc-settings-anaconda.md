---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[Settings] Anaconda 가상환경 생성 및 VSCode 연동 방법'
excerpt: "[Anaconda] 기본적인 명령어와 VScode에 연동하는 방법에 대해 알아본다."
date: 2021-07-25
last_modified_at: 2021-07-25
categories:
  - etc-settings
tags: 
   - [settings, anaconda, vscode]

use_math: false
comments: true
share : false
---


2021.07.25 최초 작성

---

<br>
# Anaconda 설정

아나콘다는 [여기](https://www.anaconda.com/products/individual#download-section)에서 다운로드받을 수 있다. (전부 Next 클릭)



## 세팅

**아나콘다 버전 확인**

```shell
conda --version
```

<br>

**아나콘다 업데이트**

```shell
conda update conda
```





## 가상환경 생성

**가상환경 생성**

* 문법:

  ```shell
  conda create --name 가상환경명 설치할패키지
  ```

* 예시: project01라는 가상환경에 python 3.5 버전을 설치하는 경우

  ```shell
  conda create --name project01 python=3.5
  ```

  <div class="callout" style="background-color:#E9E9E9; color:#808080; border-radius:10px; padding:5px 15px;">
  나는 보통 가상환경명을 프로젝트명으로 한다. 그래야 프로젝트 단위로 관리하기 쉽기 때문이다.
  </div>





<br>





## 가상환경 활성화 및 비활성화

**가상환경 리스트 확인**

```shell
conda info --envs
```

<br>

**가상환경 활성화**

* 문법:

  ```shell
  activate 가상환경명
  ```

* 예시: project01라는 가상환경을 활성화하는 경우

  ```shell
  activate project01
  ```

<br>

**가상환경 비활성화**

* 문법:

  ```shell
  deactivate 가상환경명
  ```

* 예시: project01라는 가상환경을 비활성화하는 경우

  ```shell
  deactivate project01
  ```





<br>
<br>





# VScode에 Anaconda 연동

## 1. VScode 설치

[VSCode 홈페이지](https://code.visualstudio.com/download)에서 설치



<br>





## 2. Extensions

Extensions에서 `Python`과 `Code Runner`를 설치한 뒤, VScode 재실행





<br>





## 3. Interpreter 선택

`ctrl+shift+p` 누른 뒤, `Python: Select Interpreter` 선택

이후 원하는 아나콘다 가상환경 선택



<br>
<br>



# 기타 명령어

## Python

**패키지 버전 변경**

* 예시: python을 3.5가 아닌 3.7.0버전으로 변경하고 싶은 경우

  ```shell
  conda install pyton=3.7.0
  ```



**라이브러리 설치**

* 문법:

  ```sh
  pip install 설치할라이브러리
  ```

* 예시: 

  matplotlib을 pip 패키지로 설치하려는 경우

  ```shell
  pip install matplotlib
  ```

  numpy를 pip 패키지로 설치하려는 경우

  ```shell
  pip install numpy
  ```

  
<br>


---


<br>


