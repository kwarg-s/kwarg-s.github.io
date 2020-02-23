---
title : "깃허브 블로그 (1) : 깃허브 블로그 생성하기"
category :
  - github-blog
tag :
  - github
  - blog
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/subin.jpg
  overlay_image : /assets/images/category/subin.jpg
  overlay_filter: 0.1
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

### 3. 로컬에서 블로그 수정하기
- 매번 github에 코드를 push해서 내 블로그를 확인하는 건 되게 번거로운 일인데요, 로컬에서 실시간으로 수정 상황을 확인하고 나서 push하는 것이 편합니다. 실시간으로 수정 상황은 ``$jekyll server``명령어를 통해 가능해요.
- git폴더 안에서 아까 jekyll을 설치했었죠
```bash
$gem install jekyll bundler
```
- 그런데 여기서 ``$jekyll serve``를 실행했을 때 다음과 같은 에러 메시지가 뜰 수 있습니다.
> Could not find public_suffix-4.0.1 in any of the sources (Bundler::GemNotFound)
- 이 경우에는 다음과 같이 설치를 해줘야합니다.
```bash
$gem install public_suffix --version 4.0.1
$bundle update
```
- ``$jekyll -v``을 통해 jekyll이 제대로 설치되었는지 확인해봅시다.
```bash
$jekyll -v
```
> jekyll 4.0.0
- 그럼 이제 ``$jekyll serve``를 실행시키면 로컬 주소 ``127.0.0.1:4000``에서 나의 블로그의 수정 상황을 실시간으로 확인할 수 있습니다. 
```bash
$jekyll serve
```
> Server address: http://127.0.0.1:4000/ 
> Server running... press ctrl-c to stop.

### Reference 
- https://17billion.github.io/jekyll/install/2017/05/27/install-a-jekyll.html
- https://jekyllrb-ko.github.io/docs/windows/