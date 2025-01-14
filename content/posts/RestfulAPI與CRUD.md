+++
date = '2024-11-23T13:08:55+08:00'
draft = false
title = 'Restful API與CRUD'
tags = ['軟體教學']
categories = ['軟體教學','Django']
+++

<img src="/images/article/RestfulAPI與CRUD.jpg" alt="Forest" width="600px">
<br>
<p style="color:"><strong>內容提及:</strong>關於Restful API、CRUD</p>

<!--more-->

# 一、RESTful API 介紹

RESTful API 是一種遵循 REST (Representational State Transfer) 架構風格的 Web API 設計方式。REST 的核心思想是通過統一的資源接口（URI）來操作資源，並使用標準的 HTTP 協議（如 GET、POST、PUT、DELETE）進行通信。
<br>
RESTful API 被廣泛應用於「前後端分離」架構中，使得前端（如 Web 或手機應用）能與後端服務通過標準化接口進行通信。

# 二、RESTful API 的六大設計原則

REST 的架構風格基於以下六個核心原則：
<br>
1、統一接口 (Uniform Interface)

<ul>
    <li>每個資源都有統一的 URI (資源路徑)。</li>
    <li>HTTP 方法（GET、POST、PUT、DELETE 等）用於執行操作。</li>
    <li>資源的表述 (Representation) 通常是 JSON 或 XML 格式。</li>
</ul>
2、無狀態性 (Statelessness)
<ul>
    <li>每個請求都是獨立的，服務端不保存客戶端的狀態信息。</li>
    <li>需要的上下文信息（如認證令牌）必須包含在每次請求中。</li>
</ul>
3、可緩存性 (Cacheability)
<ul>
    <li>客戶端或中間代理（如 CDN）可以緩存服務端的響應，提升性能。</li>
    <li>API 應設計適當的緩存策略，例如使用 HTTP 的 Cache-Control 標頭。</li>
</ul>
4、分層系統 (Layered System)
<ul>
    <li>客戶端無需直接與服務器交互，而是可以通過中間層（如代理或負載均衡器）進行通信。</li>
    <li>系統分層提高了可擴展性和安全性。</li>
</ul>
5、按需代碼 (Code on Demand, 可選)
<ul>
    <li>API 可以返回執行代碼（如 JavaScript）給客戶端執行，但這並非必須。</li>
</ul>
6、客戶端-服務器分離 (Client-Server Separation)
<ul>
    <li>客戶端與服務器的責任是分離的。</li>
    <li>客戶端只需關注用戶界面與交互；服務器則負責數據存儲和邏輯處理。</li>
</ul>
<br>

# 三、HTTP 方法與資源操作

HTTP 方法被用來對資源執行操作，具體對應如下：

```py
/HTTP方法/ /動作/         /用途/
GET       讀取           獲取資源或資源列表。
POST      創建           在資源集合下創建一個新資源。
PUT       更新（整體替換） 更新資源，會覆蓋整個對象。
PATCH     更新（部分更新） 僅更新資源中的部分字段。
DELETE    刪除           刪除指定資源。
```

# 四、HTTP 狀態碼

RESTful API 使用標準的 HTTP 狀態碼來表示操作結果。例如：

```py
/狀態碼/    /含義/
200        成功 (GET、PUT、DELETE 成功)。
201        創建成功 (POST 成功)。
204        無內容 (DELETE 成功且無返回內容)。
400        壞請求(例如，缺少必須的參數)。
401        未授權(認證失敗)。
404        找不到資源。
500        服務器內部錯誤。
```

# 五、CRUD

CRUD 是針對數據操作的基本概念，通常在內部系統（如後端數據庫操作、應用邏輯層）中使用。CRUD 通常用於開發人員直接處理數據時的核心功能。以下影片為我的 CRUD 分享，使用 poetry 環境、Django 框架、Python 操作。

<iframe width="560" height="315" src="https://www.youtube.com/embed/jhw5rzoMCPM?si=vCFTmhpr7AOW4cN7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3>用途</h3>
數據庫操作：<br>
1、CRUD 是關於如何和數據庫交互的核心概念。例如：
<ul>
<li>INSERT INTO 對應 Create。</li>
<li>SELECT 對應 Read。</li>
<li>UPDATE 對應 Update。</li>
<li>DELETE 對應 Delete。</li>
</ul>
使用 SQL 或 ORM（如 Sequelize、Django ORM）時，CRUD 操作是開發的基礎。

2、後端應用邏輯：

<ul>
<li>在系統內部，CRUD 操作用於處理數據的增刪改查。</li>
<li>例如，後端應用邏輯處理「用戶管理」，就會對用戶數據進行 CRUD 操作。</li>
</ul>

# 六、RESTful API 的設計與 CRUD 的對應

| HTTP 方法 | RESTful API 功能     | 對應 CRUD 動作 | 範例                         |
| --------- | -------------------- | -------------- | ---------------------------- |
| POST      | 新增一個資源         | Create         | 新增一筆產品資料             |
| GET       | 讀取資源(單個或多個) | Read           | 獲取所有用戶的列表或某個用戶 |
| PUT       | 更新整個資源         | Update         | 修改用戶資料                 |
| PATCH     | 部分更新資源         | Update         | 更新用戶的部分資料           |
| DELETE    | 刪除一個資源         | Delete         | 刪除一筆產品資料             |

<h3><span style="background-color:#9B90C2;">差異點:</span></h3>

| 特性     | CRUD                     | RESTful API                      |
| -------- | ------------------------ | -------------------------------- |
| 概念     | 描述數據操作的基本功能   | 是設計 Web API 的一種架構風格    |
| 技術     | 與數據庫操作密切相關     | 使用 HTTP 方法來實現 CRUD        |
| 應用     | 一般用於系統內部邏輯     | 一般用於服務端和客戶端之間的通信 |
| 方式     | 不依賴於特定的協議或技術 | 嚴格遵守 REST 原則，基於 HTTP    |
| 數據資源 | 以數據庫的數據為核心     | 強調資源的概念，資源以 URI 表示  |

# 七、總結

<ul>
<li>CRUD 是數據層的基礎操作，通常用於內部的數據邏輯中，例如處理數據庫或文件系統中的數據。</li>
<li>RESTful API 是接口層的設計風格，旨在將後端資源暴露給外部（如前端、第三方應用、其他服務）以進行交互。</li>
</ul>
