+++
date = '2025-01-08T00:12:15+08:00'
draft = false
title = 'Django 忘記密碼教學：使用 Gmail SMTP 完整配置指南'
tags = ['Django','Python','Django郵件教學','忘記密碼功能']

+++

本文將詳細講解如何在 Django 中實現忘記密碼功能，並完整演示 Gmail SMTP 配置的每一步驟，幫助您快速部署信件服務。這個功能會利用 Gmail 作為信件伺服，並使用 Google 應用專用密碼進行伺服器帳號的登入串接。

<!--more-->

---

# SMTP 解說

此篇文章是使用 SMTP（Simple Mail Transfer Protocol）來實現電子郵件功能的應用。SMTP 是一種傳送電子郵件的通訊協議。在這個實現中，我們使用了 Gmail 的 SMTP 服務，通過 Django 的內建郵件後端將密碼重置相關的電子郵件發送給用戶。
具體來說，這裡的設定包括：

- 使用 Gmail 的 SMTP 服務器 (smtp.gmail.com)。
- 啟用 TLS 加密（在 587 端口上）來保護傳輸過程。
- 使用 Google 的應用專用密碼（EMAIL_HOST_PASSWORD）來進行身份驗證。

SMTP 是這個功能的基礎協議，而 Google 應用專用密碼確保了 Gmail 賬戶的安全性，因為它允許您在不暴露主密碼的情況下為特定應用生成專用密碼。

---

# 一、Django 忘記密碼功能完整教學（包含 SMTP 配置指南）

### 1.1 設定 Google 應用專用密碼

在使用 Gmail 發送信件前，您需要在 Google 帳號中生成應用專用密碼：

1. 前往 [Google 安全設定](https://myaccount.google.com/security) > 啟用 "電子郵件的入口"功能。
2. 完成驗證後，向下捲動到 "應用程式密碼" 區域，點選 "生成密碼"。（也可以在搜尋欄搜尋關鍵字）
3. 選擇應用類型為 "電子郵件" 和製備字軟體為 "同步電子郵件" 。
4. 輸入應用程式名稱，會生成一組密碼請記住它。
5. 將生成的密碼填入 `.env` 檔案進行版控。

### 1.2 專案設置

在 settings.py 配置 Django 的 Gmail SMTP 信件服務：

```python
import os
from dotenv import load_dotenv
# 設置 Gmail 發送信件
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackend"  # 使用 SMTP 服務發送郵件
EMAIL_HOST = "smtp.gmail.com"  # 指定 Gmail SMTP 服務器
EMAIL_PORT = 587  # Gmail SMTP 使用的端口
EMAIL_USE_TLS = True  # 啟用 TLS 加密
EMAIL_USE_SSL = False  # 不使用 SSL 加密
EMAIL_HOST_USER = os.getenv("EMAIL_HOST_USER")  # Gmail 地址
EMAIL_HOST_PASSWORD = os.getenv("EMAIL_HOST_PASSWORD")  # Gmail 密碼
DEFAULT_FROM_EMAIL = "三合平台 <youremail@gmail.com>"

# 指定基礎域名
# 替換 `DEFAULT_DOMAIN` 和 `PROTOCOL` 為您的開發環境
DEFAULT_DOMAIN = os.getenv("DEFAULT_DOMAIN", "localhost:8000")
PROTOCOL = os.getenv("PROTOCOL", "http")
LANGUAGE_CODE = "zh-hant" #設置為中文
```

- `EMAIL_HOST_USER` 與 `EMAIL_HOST_PASSWORD`已經接受版本控制，.env 內容前者為您的 Email，後者為 Gmail 提供給您的密碼。
- `DEFAULT_DOMAIN`與`PROTOCOL`，在測試環境下，基礎設置，.env 內容前者為 127.0.0.1:8000，後者為 http。
  > .env 檔案的範例：
  > EMAIL_HOST_USER=your_email@gmail.com
  > EMAIL_HOST_PASSWORD=your_app_password
  > DEFAULT_DOMAIN=127.0.0.1:8000

---

# 二、使用路徑與設計觀念

忘記密碼功能依賴 Django 的內建功能，包括了以下路由和觀念實現：

### 2.2 配置 URLs

在 `users/urls.py` 中配置相關路由：

```python
from .views import (
    CustomPasswordResetView,
    CustomPasswordResetDoneView,
    CustomPasswordResetConfirmView,
    CustomPasswordResetCompleteView,
)

urlpatterns = [
    path("password_reset/", CustomPasswordResetView.as_view(), name="password_reset"),
    path("password_reset_done/", CustomPasswordResetDoneView.as_view(), name="password_reset_done"),
    path("reset/<uidb64>/<token>/", CustomPasswordResetConfirmView.as_view(), name="password_reset_confirm"),
    path("reset_done/", CustomPasswordResetCompleteView.as_view(), name="password_reset_complete"),
]
```

### 2.3 根目錄的 URL 配置：為 Django 忘記密碼功能設置路徑

在根目錄`core/urls.py` 中增加路徑：

```python
path("users/", include("users.urls", namespace="users")),
```

---

# 三、定義流程控制 View

在 users/views.py 執行，以下自定義的 View 內容：

#### 3.1 CustomPasswordResetView

```python
from django.conf import settings
from django.contrib.auth import views as auth_views
from django.utils.encoding import force_bytes
from django.contrib.auth.tokens import default_token_generator
from django.core.mail import EmailMessage
from django.template.loader import render_to_string
from django.utils.http import urlsafe_base64_encode

class CustomPasswordResetView(auth_views.PasswordResetView):
    template_name = "users/password_reset.html"
    email_template_name = "users/password_reset_email.html"
    subject_template_name = "users/password_reset_subject.txt" #自定義郵件標題模板
    success_url = "/users/password_reset_done/"

    #覆蓋郵件發送邏輯，避免重複發送郵件
    #目的為，可以使用HTML格式寄Email
    def form_valid(self, form):
        email = form.cleaned_data["email"]
        for user in form.get_users(email):
            context = {
                "email": email,
                "domain": settings.DEFAULT_DOMAIN,
                "protocol": settings.PROTOCOL,
                "uid": urlsafe_base64_encode(force_bytes(user.pk)),
                "token": default_token_generator.make_token(user),
            }
            subject = render_to_string(self.subject_template_name, context).strip()
            html_message = render_to_string(self.email_template_name, context)

            # 發送郵件
            email_msg = EmailMessage(
                subject=subject,
                body=html_message,
                from_email=settings.DEFAULT_FROM_EMAIL,
                to=[email],
            )
            email_msg.content_subtype = "html" #設置內容類型為 HTML
            email_msg.send()
         # 不再調用的郵件發送邏輯
        return super().form_valid(form)
```

#### 3.2 CustomPasswordResetDoneView

```python
class CustomPasswordResetDoneView(auth_views.PasswordResetDoneView):
    template_name = "users/password_reset_done.html"
```

#### 3.3 CustomPasswordResetConfirmView

```python
class CustomPasswordResetConfirmView(auth_views.PasswordResetConfirmView):
    template_name = "users/password_reset_confirm.html"
    success_url = "/users/reset_done/"
```

#### 3.4 CustomPasswordResetCompleteView

```python
class CustomPasswordResetCompleteView(auth_views.PasswordResetCompleteView):
    template_name = "users/password_reset_complete.html"
```

---

# 四、信件樣板

在 users/templates/users 底下，設置以下所有 html 信件樣板

### 4.1 點選忘記密碼第一步

此為用戶忘記密碼寄件前的說明，讓用戶可以輸入自己的 Email，使得後台可以去做發信的作業。
在`password_reset.html`底下輸入：

```html
<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <div class="w-full max-w-md p-8 bg-white rounded-lg shadow-md">
    <h2 class="mb-4 text-2xl font-semibold text-center text-gray-700">
      忘記密碼？
    </h2>
    <p class="mb-6 text-center text-gray-500">
      請輸入您的電子郵件地址，我們將向您發送重置密碼的指示。
    </p>
    <form method="post" class="space-y-6">
      {% csrf_token %}
      <div class="mt-1">{{ form.as_p }}</div>
      <button
        type="submit"
        class="w-full px-4 py-2 text-white bg-blue-500 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
      >
        發送重置連結
      </button>
    </form>
    <p class="mt-4 text-sm text-center text-gray-600">
      記得密碼了嗎？
      <a href="{% url 'users:login' %}" class="text-blue-500 hover:underline"
        >回到登入頁面</a
      >
    </p>
  </div>
</div>
```

### 4.2 發送信件告知

此為告知用戶發送信件了。
在`password_reset_done.html`輸入：

```html
<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <div class="w-full max-w-md p-8 bg-white rounded-lg shadow-md">
    <h2 class="mb-4 text-2xl font-semibold text-center text-gray-700">
      密碼重置連結已發送
    </h2>
    <p class="mb-6 text-center text-gray-500">
      我們已經將重置密碼的指示發送到您的電子郵件地址，請檢查您的信箱。
    </p>
    <a
      href="{% url 'users:login' %}"
      class="block w-full px-4 py-2 text-center text-white bg-blue-500 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
    >
      返回登入頁面
    </a>
  </div>
</div>
```

### 4.3 發送的信件內容

用戶會收到的新建內容。
在`password_reset_done.html`輸入：

```html
<p>親愛的用戶，您好！這裡是三合接案平台。</p>

<p>
  我們收到您重置密碼的申請，為了確保您的帳號安全，請點擊以下連結完成密碼重置：
</p>

<p>
  <a
    href="{{ protocol }}://{{ domain }}{% url 'users:password_reset_confirm' uidb64=uid token=token %}"
    style="color: #007bff; text-decoration: none; font-weight: bold"
  >
    重置密碼
  </a>
</p>

<p>請注意：該連結僅在一段時間內有效，過期後將無法使用。</p>

<p>如果您並未發起此操作，請忽略此郵件，無需進行任何操作。</p>

<p>
  感謝您使用「三合」接案平台！如有任何疑問或需要幫助，請隨時聯繫我們的客服團隊，我們將竭誠為您服務。
</p>

<p>祝順利！<br />三合團隊</p>
```

### 4.4 重置密碼

用戶點擊信件內容的「 **重置密碼** 」後，會需要輸入新的密碼做確認。
在`password_reset_confirm.html`輸入：

```html
<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <div class="w-full max-w-md p-8 bg-white rounded-lg shadow-md">
    <h2 class="mb-4 text-2xl font-semibold text-center text-gray-700">
      重置您的密碼
    </h2>
    <form method="post" class="space-y-6">
      {% csrf_token %}
      <div class="space-y-4">
        <div>
          <label
            for="id_new_password1"
            class="block text-sm font-medium text-gray-700 break-words"
          >
            新密碼：</label
          >
          <input
            type="password"
            name="new_password1"
            id="id_new_password1"
            class="block w-full px-3 py-2 mt-1 text-gray-700 placeholder-gray-400 bg-white border border-        gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
            placeholder="請輸入新密碼"
          />
        </div>
        <div>
          <label
            for="id_new_password2"
            class="block text-sm font-medium text-gray-700 break-words"
          >
            新密碼確認：</label
          >
          <input
            type="password"
            name="new_password2"
            id="id_new_password2"
            class="block w-full px-3 py-2 mt-1 text-gray-700 placeholder-gray-400 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
            placeholder="請再次輸入新密碼"
          />
        </div>
      </div>
      <button
        type="submit"
        class="w-full px-4 py-2 text-white bg-blue-500 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
      >
        修改密碼
      </button>
    </form>
    <p class="mt-4 text-sm text-center text-gray-600">
      想起密碼了嗎？
      <a href="{% url 'users:login' %}" class="text-blue-500 hover:underline">
        返回登入頁面
      </a>
    </p>
  </div>
</div>
```

### 4.4 重置密碼

最後完成重置密碼後，會有的完成頁面，增加用戶體驗。
在`password_reset_complete.html`輸入：

```html
<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <div class="w-full max-w-md p-8 bg-white rounded-lg shadow-md">
    <h1 class="mb-6 text-2xl font-semibold text-center text-gray-700">
      密碼重置成功
    </h1>
    <p class="mb-6 text-center text-gray-500">
      您的密碼已經成功重置，您現在可以使用新密碼！
    </p>
    <div class="text-center">
      <a
        href="{% url 'users:login' %}"
        class="inline-block px-6 py-2 text-white bg-blue-500 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
      >
        返回登入頁面
      </a>
    </div>
  </div>
</div>
```

### 4.5 信件標題

最後，若是想增加用戶體驗，也可以新增標題。
在`password_reset_subject.txt`輸入：
三合接案平台：密碼通知

---

# 五、主要結構清單

```plaintext
users/
  ├── urls.py
  ├── views.py
  ├── templates/
  │   ├── users/
  │       ├── password_reset.html
  │       ├── password_reset_done.html
  │       ├── password_reset_confirm.html
  │       ├── password_reset_complete.html
  │       ├── password_reset_subject.txt

```

---

# 六、結論

本文完整展示了 Django 忘記密碼功能的實現過程，從 Gmail SMTP 配置到路由設置與信件模板編寫，幫助您快速完成相關功能開發。如需進一步了解，請參考「Django 郵件發送指南」或官方文檔。<br>
<br>
**參考文檔**：[Django 官方文檔](https://docs.djangoproject.com/en/5.1/topics/email/)
