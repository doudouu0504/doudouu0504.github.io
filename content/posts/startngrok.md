+++
date = '2025-01-01T19:04:47+08:00'
draft = false
title = '新手必備：Ngrok 詳盡教學指南'
tags = ['Ngrok','軟體教學','Django']
categories = ['軟體教學','Django']
summary = '學習如何使用 Ngrok 分享本地服務到外部網絡，適合新手的一站式教學。'
+++

<img src="/images/article/startngrok.jpg" alt="Forest" width="600px">
<br>
<p style="color:"><strong>內容提及:</strong>Ngrok好用上手的工具</p>
這篇教學針對新手，將以簡單易懂的方式，教您如何在 Mac 的 VSCode 終端機中，使用 Ngrok 分享 Django 專案。

<!--more-->

Ngrok 是一個非常實用的工具，可以快速將您本地的服務分享給外部網絡。本文也會提供可能遇到的問題及解決方法，讓您能順利完成操作。

# 一、什麼是 Ngrok？

1. Ngrok 是什麼？

   Ngrok 是一個工具，可以將內網服務分享為外部可訪問的網址。它特別適合用於：

   - 測試 API：無需將程式部署到線上環境。

   - 展示本地專案給團隊或客戶。

   - 收到 Webhook 回調訊息。

2. 主要功能簡介

- 內網穿透：快速將本地服務分享為一個可訪問的網址。

- 即時流量監控：檢視請求的詳細資訊。

- 高級功能（付費版）：包括自訂域名與 TLS 支援。

# 二、安裝和設置 Ngrok

1.  安裝 Ngrok

    #### (a) 安裝 Homebrew（如果尚未安裝）

    - 在 VSCode 的終端機中執行以下指令來安裝 Homebrew(輸入全部)：

      ```py
      /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
      ```

    - 驗證 Homebrew 是否安裝成功：
      ```py
      brew --version
      ```

    #### (b) 使用 Homebrew 安裝 Ngrok

    - 安裝 Ngrok：

      ```py
      brew install ngrok
      ```

    - 驗證安裝是否成功：
      ```py
      ngrok version
      ```
      如果顯示版本資訊，表示安裝成功。

2.  註冊和認證 Ngrok

    #### (a) 註冊帳戶

    - 前往 Ngrok 官網 註冊一個免費帳戶。

    #### (b) 獲取認證碼

    - 登入帳戶後，在控制台找到您的 authtoken。

    #### (c) 在終端機中認證

    - 在 VSCode 終端機中執行以下指令：

      ```py
      ngrok authtoken <您的認證碼>
      ```

      認證成功後，Ngrok 將可以正常使用。

# 三、啟動 Ngrok（以 Django 為例）

1. 啟動 Django 開發伺服器

   - 進入您的 Django 專案目錄，啟動伺服器：
     ```py
     python manage.py runserver
     ```
   - Ngrok 啟動後，您會看到類似以下的輸出：預設伺服器會在 http://127.0.0.1:8000 運行。

2. 啟動 Ngrok

   - 在另一個終端機視窗中，執行以下指令，將 Django 開發伺服器分享至外部：

     ```py
      ngrok http 8000
     ```

   - Ngrok 啟動後，您會看到類似以下的輸出：

     ```py
     Forwarding                    https://abc123.ngrok.io -> http://127.0.0.1:8000
     ```

3. 複製上述第二點您出現的網址<span style="background:	#D0D0D0"> https://abc123.ngrok.io </span>，並在瀏覽器中訪問您的專案。

4. 成功分享後，團隊或客戶可通過外部網址直接訪問您的專案。

# 四、常見問題與解決方法

1. 常見問題

   - 無法分享網址： 確保 Django 伺服器已成功啟動，並確認埠號配置正確。
   - 認證失敗： 確認您輸入的 authtoken 正確無誤。

2. 解決方法
   - 重啟服務： 重新啟動 Django 開發伺服器和 Ngrok。
   - 升級版本： 確保使用的是最新版本的 Ngrok。
   - 檢查網路連線： 確保您的網路穩定，避免因網路問題導致 Ngrok 無法啟動。

# 五、進階功能介紹

1. 自訂域名（付費功能）

   如果您使用付費版 Ngrok，可以設置自訂域名：

   ```py
   ngrok http -hostname=custom.yourdomain.com 8000
   ```

2. 即時流量監控
   Ngrok 提供一個本地監控介面，幫助您檢視請求的詳細資料：

   - 在瀏覽器中訪問：

     ```py
     http://127.0.0.1:4040
     ```

   - 您可以看到所有進入 Ngrok 隧道的請求細節，包括 Headers 和 Payload。

# 六、Django 相關操作建議

1. 允許外部連接：

   - 在<span style="background:	#D0D0D0"> settings.py </span>中設置<span style="background:	#D0D0D0"> ALLOWED_HOSTS </span>，例如：

     ```py
     ALLOWED_HOSTS = ['127.0.0.1', '.ngrok.io']
     ```

2. 調整 Debug 模式：
   - 確保在開發環境下 <span style="background:	#D0D0D0"> DEBUG = True</span>，以便正常使用 Ngrok。

# 七、補充工具與比較

1. LocalTunnel

   - 無需註冊即可使用，但穩定性稍差。

2. Expose

   - 支援自訂域名和加密，但需要額外配置。

3. Ngrok

   - 使用最簡單，功能強大，適合大部分使用場景。
