---
title : "깃허브 블로그 (4) : CSS/Font 추가하기"
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


### 1. css/에 .css 파일을 추가합니다. 

- css/style.css을 다음처럼 추가해주세요.

```css
/* style.css */
p, form {
    font-family: 'Nanum Gothic', serif;
    font-size:20px;
  }

.footer-text {
    font-family: 'Jeju Gothic', serif;
    font-size: 16px;
  }
```

---

### 2. _layouts에서 적용합니다.

- _layouts/home.html에 적용해주면 됩니다.

```html
<link rel="stylesheet" href="../css/font-awesome.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/earlyaccess/jejugothic.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/earlyaccess/nanumgothic.css">
```


---



### Reference 

- <a href="http://alex.devpools.kr/2017/03/16/jekyll%EB%A1%9C-github-page-%EB%A7%8C%EB%93%A4%EA%B8%B0/"> 참고사이트 </a>