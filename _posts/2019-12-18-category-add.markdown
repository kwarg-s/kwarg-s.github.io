---
layout: post
title:  "카테고리 추가하기"
date:   2019-12-18 12:08:15 +0900
categories: github-blog
permalink: '/github-blog/category-add'
---


---

### 1. 파일 추가

- _config.yml에 카테고리 리스트를 추가해주세요.

```yml
category_list: [github-blog]
```

- category 모음 페이지의 레이아웃을 설정해야 합니다.
    - category/Category.html을 생성합니다.
    - /categorylist 링크로 들어가면 category 모음 페이지를 볼 수 있게 됩니다. 

```html
---
title: Category
layout: default
permalink: "/categorylist"
---
<div class="category-wrap">
    <h1 class="category-title"><a class="catea" href="/category/github-blog">
    Github blog 제작하기</a></h1>
    <ul class="posts-list">
        <li>
            <h3>
                <a href="/github-blog/disqus">Disqus 추가하기</a>
                <!-- <small>31 Mar 2017</small> -->
            </h3>
        </li>
        <li>
            <h3>
                <a href="/github-blog/category-add">카테고리 추가하기</a>
                <!-- <small>31 Mar 2017</small> -->
            </h3>
        </li>
        <li>
            <h3>
                <a href="/github-blog/pages-add">상단바 메뉴 추가하기</a>
                <!-- <small>31 Mar 2017</small> -->
            </h3>
        </li>
        <li>
            <h3>
                <a href="/github-blog/design-add">CSS/Font 추가하기</a>
                <!-- <small>31 Mar 2017</small> -->
            </h3>
        </li>
    </ul>
</div>
```


---

### 2. 포스팅시

- _posts/markdown 파일에서 상단에 필요한 정보를 기입해주세요 (categories, permalink) 

```html
<!-- 2019-12-18-category-add.markdown -->
---
layout: post
title:  "카테고리 추가하기"
date:   2019-12-18 12:08:15 +0900
categories: github-blog
permalink: '/github-blog/category-add'
---
```


---



### Reference 

- -