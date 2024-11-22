+++
date = '2024-11-22T10:41:17+08:00'
draft = false
title = '使用Django做一個頁面'
+++

<img src="/images/article/django.jpg" alt="Forest" width="500px">
<br>
<p style="color:"><strong>內容提及:</strong>安裝django、前期的MTV之TV的實作練習</p>
用 django 框架，做一個 about 頁面。

<!--more-->
<h3>前情提要:</h3>
<strong>◎面試常見問題</strong><br>
<strong>一、MVC（Model-View-Controller）:</strong>
<br>
M （Model，模型）：封裝與資料庫的訪問，處理對資料庫的增刪改查（CRUD）。<br>
V （View，視圖）：負責（1）處理業務邏輯、封裝結果；（2）生成頁面展現給使用者。<br>
C （Controller，控制器）：對 Request、Response 進行處理、處理 Model 與 View 的交互。
<br><br>
<strong>二、MTV（Model-Template-View）:</strong><br>
M （Model，模型）： 同上 Model。<br>
T （Template，模板）：同上的 View （2）生成頁面展現給使用者的部分，也就是我們看見的前端 HTML、CSS、JavaScript 的部分。<br>
V （Views，視圖）：同上的 View （1）處理業務邏輯、封裝結果的部分，負責處理 URL 與 callback 函式之間的關係，每一個 view 都代表一個簡單的 Python function。<br>
<br>
C （Controller，控制器）的部分如果要對應的話為 Django 本身。
MTV：Model(溝通),Template(畫面),View(流程及邏輯控制)

<p>
<strong>◎ 如何用虛擬環境Poetry安裝Django，參考網址：</strong>
<a href="https://builtwithdjango.com/blog/basic-django-setup" target="blank">點擊連結</a>
</p>
<p>
◎ 做的資料夾（app），一定要輸入在根目錄下，setting 的 INSTALLED_APPS 裡面，要把新增的 app 寫入。
<br>
(ex: 不管靜態或動態頁面的名稱，像是下面的 pages)
</p>
<p>
◎ 下面描述的『外面的 templates/shared/layout.html』，一定要輸入名稱在根目錄下，setting 的 TEMPLATES 裡面的 DIRS 裡面才讀取得到。
（ex: "DIRS": ["templates"],）
</p>

<h3>🪐 開始做一個 about 頁面：</h3>
<p>
1、在虛擬環境下下載 django 套件:
<a href="https://www.djangoproject.com/download/" target="blank">點擊連結</a>
</p>
<p>
2、下載完後，建立一個根目錄，<a href="https://docs.djangoproject.com/en/5.1/ref/django-admin/" target="_blank">參考網址</a>，輸入:

```py
django-admin startproject pydev .
```

(空格點很重要！pydev 是自己取的)

<br>
接著在pydev底下的 urls 做路徑的東西:

</p>

```py
from django.contrib import admin
from django.urls import path, include
from django.http import HttpResponse

def aboot(requests):
return HttpResponse("hi")

urlpatterns = [
path("admin/", admin.site.urls),
path("", include("pages.urls")),
]
# 前面是網址的路徑，後面是渲染的東西
# include 把 url 是要引入其他 urls，增加樹枝

```

3、用系統創一個資料夾（動態頁面），<a href="https://docs.djangoproject.com/en/5.1/ref/django-admin/" target="_blank">參考網址</a>，輸入：

```py
python manage.py startapp resumes
```

或是可以自己創一個資料夾（靜態頁面），像是：<br>
建立一個 pages（靜態頁面），內容要建有資料夾 ：templates/pages、urls.py、views.py

4、在 pages 的 urls.py 底下，設置路徑連接到流程控制邏輯 views：

```py
urls.py

from django.urls import path
from pages.views import about

# 同層要加.

urlpatterns = [
path("about/", about),
# 這個 app 應用程式有很多的路徑，這個路徑要執行邏輯是 views

]
```

5、在 pages 的流程控制邏輯 views 內容裡面放：

```py
pages/views.py

from django.shortcuts import render #去 import 這個方法

def about(request):
return render(request, "pages/about.html")

# 任何一個網址都有 request 請求一個畫面
```

上述可以上網查詢關鍵字 :django view render 找如何使用這個方法，<a href="https://docs.djangoproject.com/en/5.1/intro/tutorial03/" target="_blank">參考連結</a>。

6、在 pages 的要找的 templates 底下創一個 pages 資料夾，建立一個 about.html 並輸入內文。<br>
(內容繼承了 pages 外面也建立一個叫 templates/shared/layout.html 的資料夾，並且可以更改內文)
<br>
裡面的 pages/templates/pages/about.html：

```py
about.html

{% extends "shared/layout.html" %}
{% block "content" %}

<h1>about</h1>
{% endblock %}
```

外面的 templates/shared/layout.html：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    {% block "content" %}{% endblock %}
  </body>
</html>
```

◎ 最後看跑頁面看畫面，在 REPL(終端機)下輸入:

```py
python manage.py runserver
```

就成功了！

◎django 的註解：

```py
{% comment %}
我是註解
{% endcomment %}
```

django 的使用的語法並不是「真的神社語法（jinja2）」，只是長得很像，用法也差不多。 Django 自己的語言叫做 Django Template Language (DTL)，如果你想改用 jinja2 也可以的，但要另外設置。
