+++
date = '2025-01-31T21:22:41+08:00'
draft = false
title = '使用 Poetry 安裝 Selenium 並下載 ChromeDriver'
tags = ['Python','Selenium','Poetry']

+++

內容提及: 安裝 Selenium、ChromeDriver，並嘗試抓取網頁標題。

在這篇文章中，我將介紹如何使用 Poetry 來管理 Python 環境，並安裝 Selenium 以進行網頁自動化測試。此外，我們也會下載並設定 ChromeDriver，使其能夠與 Selenium 搭配運行。

<!--more-->

---

## **► 步驟 1：初始化 Poetry 專案**

Poetry 是一個強大的 Python 依賴管理工具，能夠幫助我們建立和管理虛擬環境。

### **1.1 安裝 Poetry（若尚未安裝）**

如果你還沒有安裝 Poetry，可以使用以下指令來安裝：

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

安裝完成後，使用以下指令確認是否成功安裝：

```bash
poetry --version
```

### **1.2 初始化專案**

接著，我們需要初始化一個新的 Poetry 專案。

```bash
poetry init
```

這個指令會引導你輸入一些專案資訊（名稱、版本、描述等），你可以根據需求填寫，或直接按 `Enter` 跳過。

---

## **► 步驟 2：啟動虛擬環境並安裝 Selenium**

在 Poetry 初始化完成後，使用以下指令進入虛擬環境：

```bash
poetry shell
```

然後安裝 Selenium：

```bash
poetry add selenium
```

這樣就會安裝 Selenium 並將其記錄在 `pyproject.toml` 內，確保環境的可重現性。

---

## **► 步驟 3：下載並解壓縮 ChromeDriver**

Selenium 需要使用 ChromeDriver 來操作 Google Chrome，因此我們需要下載與 Chrome 瀏覽器匹配的版本。

### **3.1 確認 Chrome 版本**

1. 開啟 Chrome 瀏覽器。
2. 在網址列輸入：
   ```
   chrome://settings/help
   ```
3. 記下 Chrome 的版本號，例如 `120.0.6099.129`。

### **3.2 下載 ChromeDriver**

前往 [Chrome for Testing](https://googlechromelabs.github.io/chrome-for-testing/) 官方網站，下載與你的 Chrome 版本相符的 `chromedriver`。

如果你使用 macOS，請確認你的架構：

- **Apple Silicon (M1/M2/M3)**：下載 `chromedriver-mac-arm64.zip`
- **Intel Mac**：下載 `chromedriver-mac-x64.zip`

### **3.3 解壓縮 ChromeDriver 並放置在桌面**

下載後，打開終端機並輸入以下指令來解壓縮：<br>
（也可自行解壓縮後放在桌面）

```bash
cd ~/Downloads
unzip chromedriver-mac-arm64.zip -d ~/Desktop/
```

這樣 `chromedriver` 會被解壓到桌面。

確保它有執行權限：<br>
（也可打開解壓縮的資料夾，並且點擊名稱為 chromedriver ）

```bash
chmod +x ~/Desktop/chromedriver
```

注意：Windows 用戶可以將 chromedriver.exe 放在專案目錄 或 C:/Program Files/ChromeDriver 之類的地方

---

## **► 步驟 4：撰寫 Selenium 測試程式**

建立一個 `practice.py`，並寫入以下程式碼來開啟一個空白的 Chrome 瀏覽器：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import selenium

PATH = "/Users/doudouu0504/Desktop/chromedriver"

if selenium.__version__.startswith("4"):
    driver = webdriver.Chrome(service=Service(PATH))
else:
    driver = webdriver.Chrome(executable_path=PATH)

driver.get("about:blank")

input("按 Enter 鍵以結束程式並關閉瀏覽器...")#這樣頁面就不會一閃而過
driver.quit()
```

執行程式：

```bash
python practice.py
```

如果沒有錯誤，則表示你的 Selenium 和 ChromeDriver 都已成功安裝！

> 請記得要在終端機按下 Enter 鍵，不然會一直開著瀏覽器。

---

## **► 步驟 5：抓取網頁標題**

如果你想要抓取某個網頁的標題，例如 Dcard 首頁，可以使用以下程式碼：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import selenium

PATH = "/Users/doudouu0504/Desktop/chromedriver"

if selenium.__version__.startswith("4"):
    driver = webdriver.Chrome(service=Service(PATH))
else:
    driver = webdriver.Chrome(executable_path=PATH)

driver.get("https://www.dcard.tw/f")
print(driver.title)

# input("按 Enter 鍵以結束程式並關閉瀏覽器...")
driver.quit()
```

這樣就可以成功抓取 Dcard 首頁的標題，並顯示在終端機中。

> 自行選擇是否要 input 那一行，留著代表頁面你還要開著，等在終端機按下 Enter 後會關閉。反之則是抓完標題後，瀏覽器就會自動關閉。

---

## **結論**

透過 Poetry，我們可以方便地管理 Python 專案的環境與依賴，而 Selenium 則讓我們能夠輕鬆地自動化操作網頁。此外，我們學習了如何下載並解壓縮 ChromeDriver 到桌面，並使用 Selenium 來抓取網頁標題，希望這篇文章能幫助你順利完成 Selenium 的安裝與測試！
