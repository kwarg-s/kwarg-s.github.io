---
layout: post
title:  "깃허브 블로그 생성하기"
date:   2019-12-17 14:07:15 +0900
comments: true
categories: github-blog
permalink: '/github-blog/create-blog'

---

---

### 1. Github에 사이트에 가입하고 repository를 생성해주세요

- <a href="https://github.com/">Github</a>에 로그인해주세요
- 우측 상단에서 ``New repository``를 선택하여 새 repository를 만들어 줍시다.
<br>
<img src="../assets/img/create-blog1.png" style="width:300px">

 

- ``Repository name``은 반드시 ``github아이디.github.io``로 지정하여야 하며, ``Public``을 선택하여야 합니다. 

<img src="../assets/img/create-blog2.png" style="width:800px">

---

### 2. Jekyll 테마 적용해보기

- Jekyll 테마를 적용하기 위해서는 ruby를 설치해야 합니다. 
- 윈도우 : <a href="http://rubyinstaller.org/downloads/">여기</a>에 접속해서 ``Ruby+Devkit``을 설치합니다.
- Jekyll과 Bundler를 설치합니다.
```bash
$gem install jekyll bundler
```
- 이제부터 Jekyll 테마를 적용해보겠습니다. ``minimal-mistakes``라는 테마를 적용해볼건데요, 테마를 제공해주는 github 주소는 <a href="https://github.com/mmistakes/minimal-mistakes">이곳</a>입니다. 
- ``Clone or download``버튼을 눌러서 링크를 복사합니다.
<img src="../assets/img/create-blog3.png" style="width:800px">

- 로컬 폴더에 가서 ``git clone 복사한 링크``를 실행하면 폴더에 테마의 코드가 복사됩니다.
```bash
$git clone https://github.com/mmistakes/minimal-mistakes.git
```

---

### 3. 내 로컬 폴더에 git 연동하기

- 로컬 폴더 위치에서 console창을 켜서 다음을 실행해주면, 내 폴더는 방금 만든 repository와 연동이 됩니다.
```bash
$git init
$git remote add origin https://github.com/내ID/내ID.github.io.git
```
- 코드를 add, commit, push하면 github에 적용이 됩니다. 
```bash
$git add .
$git commit -m "first commit"
$git push origin master
```
- ``http://내id.github.io``에 접속해서 내 블로그가 뜨는지 확인해봅니다.


---



### Reference 

- Null