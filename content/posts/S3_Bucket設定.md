+++
date = '2025-01-25T13:32:50+08:00'
draft = false
title = 'S3 Bucket 設定詳細指南與建議'
tags = ['Django','AWS']
+++

內容提及: AWS S3 Bucket 的設定。

本文將逐步解析 AWS S3 Bucket 的設定選項，並針對常見情境提供最佳化建議，幫助您更快上手並避免踩雷！

<!--more-->

---

## ► 一、 General Configuration

### 1.1 General purpose vs. Directory

- **建議選擇 General purpose（預設）**
  - 適用於大部分情境，可儲存任何類型的物件（圖片、影片、文件等）。
  - **Directory** 通常用於大數據管理，對一般應用來說不必要。

### 1.2 Bucket Name

- 命名需**全球唯一**，建議使用簡單有意義的名稱，例如：
  - `my-app-user-avatars`
  - `project-name-profile-images`
- **命名規則**：
  - 僅可使用小寫字母、數字、破折號（-）。
  - 不能以破折號開頭或結尾。

### 1.3 Copy Settings from Existing Bucket

- **建議略過**：僅當有需要複製其他 Bucket 設定時使用。

---

## ► 二、 Object Ownership

### 2.1 ACLs Disabled (Recommended)

- **建議選擇 ACLs disabled**：
  - 物件的所有權由 Bucket 擁有者（您）全權控制。
  - 更安全且更簡單，適合絕大多數情境。

### 2.2 ACLs Enabled

- **不建議使用**：為舊版存取控制方式，僅在需要對每個物件進行細粒度設定時考慮。

---

## ► 三、 Block Public Access Settings

### 3.1 建議保持 `Block all public access` 開啟（預設）

- 防止未經授權的公開訪問，確保安全性。
- 若需公開某些物件，可在**上傳時單獨設定公開權限**，無需整個 Bucket 公開。

---

## ► 四、 Bucket Versioning

### 4.1 Disable 或 Enable？

- **建議選擇 Disable**（預設）：
  - 僅保留最新版本的檔案，節省空間。
- **啟用 Versioning** 的情境：
  - 若處理的是重要且頻繁變動的檔案，可考慮啟用，以保留歷史版本。

---

## ► 五、 Tags（可選）

- 可加入標籤來組織或管理資源，例如：
  - `Key: Environment, Value: Production`
  - `Key: Project, Value: MyApp`
- **建議：非必要時可略過**。

---

## ► 六、 Default Encryption

AWS 提供三種伺服器端加密選項：

### 6.1 SSE-S3（推薦）

- **建議選擇 SSE-S3**（預設）：
  - S3 自動管理加密金鑰，安全且方便，適用於一般應用。

### 6.2 SSE-KMS

- 使用 AWS Key Management Service（KMS）管理金鑰。
- 適用於對安全性要求極高的資料（如金融或醫療數據）。
- 需額外設定，並增加少量成本。

### 6.3 DSSE-KMS

- 雙層加密，適用於需符合嚴格合規需求的情境（如 GDPR、HIPAA）。

---

## ► 七、 總結建議

| 設定項目                  | 建議選擇                | 理由           |
| ------------------------- | ----------------------- | -------------- |
| **General Configuration** | General purpose         | 通用用途       |
| **Bucket Name**           | 自行命名                | 必須全球唯一   |
| **Copy Settings**         | 略過                    | 當前情境不需要 |
| **Object Ownership**      | ACLs disabled (推薦)    | 簡單且安全     |
| **Block Public Access**   | Block all public access | 預設最安全     |
| **Bucket Versioning**     | Disable                 | 節省空間       |
| **Tags**                  | 可略過                  | 非必要         |
| **Default Encryption**    | SSE-S3                  | 簡單、符合需求 |

---
