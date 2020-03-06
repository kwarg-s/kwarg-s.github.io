---
title : "[Django] 1. 프로젝트 시작하기"
excerpt : "가상환경, 프로젝트생성, 실행하기"
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

## 가상환경 시작하기

**Windows 기준**
```bash
$pip install virtualenv
$virtualenv {myvenv}
$cd myvenv/Scripts
$activate
```

## Django 설치
```bash
$pip install Django
```

## Django 프로젝트 시작하기
```bash
$cd 작업폴더
$django-admin startproject {myproject}
$cd {myproject}
$python manage.py runserver
```

## 관리자(admin) 만들기
```bash
$python manage.py createsuperuser
```

## Reference
- https://jamanbbo.tistory.com/
- https://ssungkang.tistory.com/