+++
date = '2025-01-31T21:22:41+08:00'
draft = false
title = '使用 Poetry 安裝 Selenium 並下載 ChromeDriver'
tags = ['Python','Selenium','Poetry']
categories = ['軟體教學','Selenium','爬蟲','Python']
+++

內容提及: 安裝 Selenium、ChromeDriver，並嘗試抓取網頁標題。

在這篇文章中，我將介紹如何使用 Poetry 來管理 Python 環境，並安裝 Selenium 以進行網頁自動化測試。此外，我們也會下載並設定 ChromeDriver，使其能夠與 Selenium 搭配運行。

<!--more-->

---

## 一、安裝與初始化 Poetry

### 1.1 安裝 Poetry（若尚未安裝）

如果你還沒有安裝 Poetry，可以使用以下指令來安裝：

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

安裝完成後，使用以下指令確認是否成功安裝：

```bash
poetry --version
```

### 1.2 初始化 Poetry 專案

使用以下指令來初始化一個新的 Poetry 專案：

```bash
poetry init
```

這個指令會引導你輸入一些專案資訊（名稱、版本、描述等），你可以根據需求填寫，或直接按 `Enter` 跳過。

---

## 二、啟動虛擬環境並安裝 Selenium

### 2.1 啟動虛擬環境

在 Poetry 初始化完成後，使用以下指令進入虛擬環境：

```bash
poetry shell
```

### 2.2 安裝 Selenium

安裝 Selenium 並將其記錄到 `pyproject.toml` 內，確保環境的可重現性：

```bash
poetry add selenium
```

---

## 三、下載並設定 ChromeDriver

### 3.1 確認 Chrome 版本

1. 開啟 Chrome 瀏覽器。
2. 在網址列輸入：
   ```
   chrome://settings/help
   ```
3. 記下 Chrome 的版本號，例如 `120.0.6099.129`。

### 3.2 下載 ChromeDriver

前往 [Chrome for Testing](https://googlechromelabs.github.io/chrome-for-testing/) 官方網站，下載與你的 Chrome 版本相符的 `chromedriver`。

- **Apple Silicon (M1/M2/M3)**：下載 `chromedriver-mac-arm64.zip`
- **Intel Mac**：下載 `chromedriver-mac-x64.zip`

### 3.3 解壓縮並設定 ChromeDriver

下載後，打開終端機並輸入以下指令來解壓縮：

```bash
cd ~/Downloads
unzip chromedriver-mac-arm64.zip -d ~/Desktop/
```

確保它有執行權限：

```bash
chmod +x ~/Desktop/chromedriver
```

---

## 四、撰寫 Selenium 測試程式

### 4.1 開啟瀏覽器

建立一個 `practice.py`，並寫入以下程式碼來開啟一個空白的 Chrome 瀏覽器：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import selenium

PATH = "/Users/doudouu0504/Desktop/chromedriver"

driver = webdriver.Chrome(service=Service(PATH))

driver.get("about:blank")
input("按 Enter 鍵以結束程式並關閉瀏覽器...")#這樣頁面就不會一閃而過
driver.quit()
```

### 4.2 執行測試

執行程式：

```bash
python practice.py
```

如果沒有錯誤，則表示你的 Selenium 和 ChromeDriver 都已成功安裝！

> &#x20;請記得要在終端機按下 Enter 鍵，不然會一直開著瀏覽器。

---

## 五、抓取網頁標題

### 5.1 撰寫抓取標題程式碼

如果你想要抓取某個網頁的標題，例如 Dcard 首頁，可以使用以下程式碼：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import selenium

PATH = "/Users/doudouu0504/Desktop/chromedriver"

driver = webdriver.Chrome(service=Service(PATH))

driver.get("https://www.dcard.tw/f")
print(driver.title)

# input("按 Enter 鍵以結束程式並關閉瀏覽器...")
driver.quit()
```

### 5.2 執行並驗證

執行後，終端機應該會顯示 Dcard 的標題。

> **提示**：`input("按 Enter 鍵以結束程式並關閉瀏覽器...")` 這一行可以選擇性保留，當你希望手動關閉瀏覽器時才使用。

---

## 六、結論

透過 Poetry，我們可以方便地管理 Python 專案的環境與依賴，而 Selenium 則讓我們能夠輕鬆地自動化操作網頁。此外，我們學習了如何下載並解壓縮 ChromeDriver 到桌面，並使用 Selenium 來抓取網頁標題。

希望這篇文章能幫助你順利完成 Selenium 的安裝與測試！
