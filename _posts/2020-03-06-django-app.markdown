---
title : "[Django] 2. 새로운 앱 만들기"
excerpt : "app, settings, html, views, urls"
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

## 1. 앱 만들기
```bash
$python manage.py startapp {myapp}
```

### 폴더구조
```
myproject
ㄴmyproject
  ㄴsettings.py
  ㄴurls.py
  ㄴ...
ㄴmyapp
  ㄴmigrations
  ㄴtemplates(직접생성)
  ㄴadmin.py
  ㄴviews.py
  ㄴmodels.py
  ㄴ...
ㄴmanage.py
```

## 2. settings.py에 앱 추가
```python
INSTALLED_APPS+=['myapp.apps.MyappConfig']
```

## 3. templates에 html 생성  

**myapp/templates/index.html**  
```html
<h1>hello world</h1>
```

## 3. views.py를 통해 동작하게 하기

**myapp/views.py**
```python
from django.shortcuts import render
def index(request):
  return render(request,'index.html')
```

## 4. url 추가하기

**myproject/urls.py**  
```python
import myapp.views
urlpatterns+=[path('',myapp.views.index, name='index')]
```

## 5. models.py에 db 추가하기

**myapp/models.py**
```python
from django.db import models
class Mydb(models.Model):
  db_id=models.IntegerField(primary_key=True)
  title=models.CharField(max_length=200)
  content=models.TextField()
  pic=models.ImageField(upload_to="image")

  def __str__(self):
    return self.title
```

## 6. media 파일(이미지 등) 저장 경로 설정하기 

**media 파일**: FileField를 상속받은 필드로서, ImageField가 속해있습니다.

**myproject/settings.py**
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

**myproject/urls.py**
```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```  

## 7. html 파일에 DB 데이터 넣기 

**myapp/templates/index.html** 
```html
{%for data in mydb.all%}
  {% if mydb.pic %}
  <p>
  {{data.title}}<br>
  <img src="{{data.pic.url}}">
  <p>
  {%endif%}
{%endfor%}
```


## Reference
- https://ssungkang.tistory.com/entry/Django-media-%ED%8C%8C%EC%9D%BC-%EC%97%85%EB%A1%9C%EB%93%9C%ED%95%98%EA%B8%B0 
- https://ssungkang.tistory.com/entry/Django-05-queryset-%EA%B3%BC-method?category=320582 
