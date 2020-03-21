---
title : "[Django] 3. 장고에 Bootstrap 디자인 적용하기"
excerpt : ""
category :
  - django
tag :
  - django
use_math : true
author_profile : true
header:
  teaser : /assets/images/main.jpg
  overlay_image : /assets/images/main.jpg
  overlay_filter: 0.1
---

## 1. Bootstrap 템플릿 다운 받기

<a href="https://startbootstrap.com/">여기</a>에서 다운받고, 압축을 해제합니다. 

## 2. static 폴더 만들기
myapp하위에 static폴더 생성
```
myproject
ㄴmyproject
  ㄴsettings.py
  ㄴurls.py
  ㄴ...
ㄴmyapp
  ㄴmigrations
  ㄴtemplates(직접생성)
  ㄴstatic(직접생성)
  ㄴadmin.py
  ㄴviews.py
  ㄴmodels.py
  ㄴ...
ㄴmanage.py
```
myapp/static/ 폴더 안에 압축해제한 파일 중 **``index.html을 제외한 모든 파일``**을 붙여넣기합니다.

<center><img src="../assets/img/django/bootstrap.png"></center>

## 3. index.html 내용 붙여넣고, 소스 추가하기

myapp/templates의 원하는 html 파일에 index.html 내용을 붙여넣기 합니다. 상단부에는 다음 내용을 추가해주세요. 
**myapp/templates/index.html**
```
{% raw %} {% load static %} {% endraw %}
```

settings에는 다음 내용을 추가해주세요.  
**myproject/settings.py**
```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

## 4. index.html에서 경로 수정하기

그리고 경로를 수정하여야 합니다. 상단부에 있는 css파일의 링크와 하단부에 있는 javascript파일의 링크를 모두 수정해주셔야 합니다. 웹사이트 주소를 참조하는 부분은 제외하고, 내부 폴더의 파일을 참조하는 부분을 변경해야합니다. 아래와 같은 부분이 변경해야할 부분입니다.    
**myapp/templates/index.html**
```html
<!-- 상단부: css -->
  <!-- Custom fonts for this theme -->
  <link href="vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">

  <!-- Theme CSS -->
  <link href="css/freelancer.min.css" rel="stylesheet">

<!-- 하단부: javascript -->
  <!-- Bootstrap core JavaScript -->
  <script src="vendor/jquery/jquery.min.js"></script>
  <script src="vendor/bootstrap/js/bootstrap.bundle.min.js"></script>

  <!-- Plugin JavaScript -->
  <script src="vendor/jquery-easing/jquery.easing.min.js"></script>

  <!-- Contact Form JavaScript -->
  <script src="js/jqBootstrapValidation.js"></script>
  <script src="js/contact_me.js"></script>

  <!-- Custom scripts for this template -->
  <script src="js/freelancer.min.js"></script>
```

위 부분을 아래와 같이 수정해주세요. 링크의 앞부분에는 {% raw %}'{% static{% endraw %}을, 뒵부분에는 %}'를 붙여줍니다.  
**myapp/templates/index.html**
```html
{% raw %}<!-- 상단부: css -->
  <!-- Custom fonts for this theme -->
  <link href='{% static "vendor/fontawesome-free/css/all.min.css" %}' rel="stylesheet" type="text/css">
 
  <!-- Theme CSS -->
  <link href='{% static "css/freelancer.min.css" %}' rel="stylesheet">

<!-- 하단부: javascript -->
  <!-- Bootstrap core JavaScript -->
  <script src='{% static "vendor/jquery/jquery.min.js" %}'></script>
  <script src='{% static "vendor/bootstrap/js/bootstrap.bundle.min.js" %}'></script>

  <!-- Plugin JavaScript -->
  <script src='{% static "vendor/jquery-easing/jquery.easing.min.js" %}'></script>

  <!-- Contact Form JavaScript -->
  <script src='{% static "js/jqBootstrapValidation.js" %}'></script>
  <script src='{% static "js/contact_me.js" %}'></script>

  <!-- Custom scripts for this template -->
  <script src='{% static "js/freelancer.min.js" %}' ></script>
  {% endraw %}
```

## 5. runserver

runserver를 통해 적용된 디자인을 확인해보세요. 


## Reference
- https://idlecomputer.tistory.com/91
