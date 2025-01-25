+++
date = '2025-01-20T17:28:08+08:00'
draft = false
title = '深入理解 HTML 表單的 multipart/form-data'
tags = ['Django']
+++

內容提及: 探索 multipart/form-data 的作用及其在 Django 文件上傳中的應用。

在開發 Web 應用程式時，處理表單提交是不可避免的一部分。而當表單需要支持文件上傳時，`enctype="multipart/form-data"` 成為了一個必不可少的屬性。在本文中，我們將深入了解 `multipart/form-data` 的作用及其在 Django 文件上傳中的應用。

<!--more-->

# **一、什麼是 `multipart/form-data`？**

在 HTML 表單中，`enctype="multipart/form-data"` 是一種表單編碼類型，指定了表單數據應該如何被編碼並傳送到伺服器。

### 1.1 為什麼需要它？

當表單包含文件（如圖片或 PDF）或其他二進制數據時，需要使用 `multipart/form-data`，否則文件數據無法正確傳輸到伺服器。

# **二、表單編碼類型對比**

1. **`application/x-www-form-urlencoded`**

   - 預設的表單編碼類型。
   - 適用於普通表單數據（如文字輸入框和選擇框）。
   - 數據以鍵值對形式（如 `key=value&key2=value2`）進行編碼。

2. **`multipart/form-data`**

   - 專門用於包含文件的表單。
   - 將表單數據分隔成多部分，每部分都有自己的 HTTP 標頭來描述數據類型。

3. **`text/plain`**
   - 很少使用，數據以未編碼的純文本形式傳送。

---

# **三、`multipart/form-data` 的作用**

當表單包含文件上傳時，每個字段（包括文件數據）需要被單獨處理和編碼，而不只是單一的鍵值對格式。

`multipart/form-data` 可以：

- 將文字字段與文件內容分開編碼。
- 使用邊界（boundary）標記不同字段的開始和結束。
- 正確傳遞二進制文件內容。

---

# **四、在 Django 中的應用**

### 4.1 HTML 表單結構

當需要在 Django 中實現文件上傳功能時，HTML 表單需要包含以下屬性：

- **`method="POST"`**：表單必須使用 POST 方法提交數據（因為 GET 方法無法傳送文件）。
- **`enctype="multipart/form-data"`**：指定表單以多部分格式進行編碼，允許文件數據上傳。

以下是一個 HTML 表單的範例：

```html
<form method="POST" enctype="multipart/form-data">
  {% csrf_token %} {{ form.as_p }}
  <button type="submit">上傳</button>
</form>
```

### 4.2. Django 模型與表單示例

#### 模型

定義一個模型來處理圖片上傳：

```python
from django.db import models

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    avatar = models.ImageField(upload_to='avatars/')
```

#### 表單

```python
from django import forms
from .models import Profile

class ProfileForm(forms.ModelForm):
    class Meta:
        model = Profile
        fields = ['avatar']
```

#### 視圖

```python
from django.shortcuts import render
from .forms import ProfileForm

def update_profile(request):
    if request.method == 'POST':
        form = ProfileForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
    else:
        form = ProfileForm()

    return render(request, 'profile_update.html', {'form': form})
```

### 4.3 請求數據結構

當使用 `multipart/form-data` 提交表單時，請求的內容是分段的。例如：

```
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryAbCdEfGhIjKl
```

每個段包含：

1. **字段名稱和文件名**（如果有）。
2. **數據類型**。
3. **字段值或文件內容**。

---

## **總結**

- **`enctype="multipart/form-data"` 是文件上傳表單的必要屬性。**
- Django 提供了簡單的模型和表單結構來處理文件上傳。
- 配置 `request.FILES`，並正確存儲文件到伺服器或雲端（如 AWS S3），是實現文件上傳功能的核心。

如果你的項目需要文件上傳功能，以上的結構和配置可以幫助你快速搭建穩定的上傳系統。
