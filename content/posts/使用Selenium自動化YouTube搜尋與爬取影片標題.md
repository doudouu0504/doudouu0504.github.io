+++
date = '2025-02-01T00:16:10+08:00'
draft = false
title = '使用 Selenium 自動化 YouTube 搜尋與爬取影片標題'
tags = ['Python','Selenium','爬蟲']
categories = ['軟體教學','Selenium','爬蟲','Python']
+++

內容提及: 爬取影片標題的方式。

在這篇文章中，我將分享如何使用 `Selenium` 自動搜尋 YouTube 影片，並抓取搜尋結果的影片標題。這對於數據分析、內容整理，甚至是自動化工具開發都相當有幫助。

<!--more-->

---

## 一、環境準備

如果你想瞭解如何安裝 Selenium 以及下載對應的 ChromeDriver，可以參考我的另一篇文章：[使用 Poetry 安裝 Selenium 並下載 ChromeDriver](https://doudouu0504.github.io/posts/%E5%AE%89%E8%A3%9Dselenium%E4%B8%A6%E4%B8%8B%E8%BC%89chromedriver/)。

## 二、程式碼解析

### 2.1 完整程式碼

以下是完整的 Python 程式碼，負責開啟 YouTube，輸入關鍵字 "比特幣"，並列出搜尋結果中的影片標題。

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.keys import Keys
import selenium
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 設定 ChromeDriver 路徑
PATH = "/Users/doudouu0504/Desktop/chromedriver-mac-arm64/chromedriver"

# 建立 WebDriver 物件
if selenium.__version__.startswith("4"):
    driver = webdriver.Chrome(service=Service(PATH))
else:
    driver = webdriver.Chrome(executable_path=PATH)

# 開啟 YouTube
driver.get("https://www.youtube.com/")

# 找到搜尋框並輸入關鍵字
search = driver.find_element(By.NAME, "search_query")
search.send_keys("比特幣")
search.send_keys(Keys.RETURN)

# 等待搜尋結果載入
WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, "container")))

# 取得影片標題
titles = driver.find_elements(By.ID, "video-title")
for title in titles:
    print(title.text)

# 保持瀏覽器開啟，等待使用者按 Enter 鍵結束
input("按 Enter 鍵結束程式並關閉瀏覽器...")
driver.quit()
```

## 三、程式碼說明

### 3.1 啟動 WebDriver

程式會根據 Selenium 版本來決定 `webdriver.Chrome()` 的呼叫方式，確保相容性。

### 3.2 開啟 YouTube 並搜尋關鍵字

使用 `driver.get()` 方法開啟 YouTube，然後透過 `find_element(By.NAME, "search_query")` 找到搜尋框，並輸入 "比特幣"，最後使用 `send_keys(Keys.RETURN)` 執行搜尋。

### 3.3 等待搜尋結果載入

使用 `WebDriverWait` 確保搜尋結果已載入，避免 `find_elements` 執行時找不到目標元素。

- `WebDriverWait(driver, 10)`: 設定等待時間最長 10 秒。
- `until(EC.presence_of_element_located((By.ID, "container")))`: 直到 ID 為 `container` 的元素出現才繼續執行。
- 其他條件如：
  - `visibility_of_element_located(locator)`: 確保元素可見。
  - `element_to_be_clickable(locator)`: 確保元素可被點擊。

### 3.4 取得影片標題

使用 `find_elements(By.ID, "video-title")` 找到所有影片標題並印出。

## 四、注意事項

### 4.1 ChromeDriver 版本

請確保 Chrome 瀏覽器與 `chromedriver` 版本相符，否則可能會出錯。

### 4.2 YouTube 限制

YouTube 可能會檢測機器人行為，建議適當調整 `WebDriverWait` 來模擬人類操作。

## 五、結論

這篇文章介紹了如何使用 `Selenium` 自動化 YouTube 搜尋與爬取影片標題的方式，適合用於研究熱門關鍵字或是建立個人化的影片推薦系統。
