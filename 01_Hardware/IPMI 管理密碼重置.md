---
# ==========================================
#  AI 知識庫標準檔頭 (V2.0)
#  請勿隨意刪除 Key 值 (冒號前的文字)
# ==========================================
title: Supermicro IPMI 管理密碼重置操作手冊 (IPMICFG)
tags: [Supermicro, IPMI, Password, Reset, Security, SOP, L2]
date: 2026-03-05
author: [您的名字]
# --- AI 識別與進階檢索區 (必填) ---
category: Troubleshooting
device_type: Server
vendor: Supermicro
model: [All Series]
error_code: [Login Failed]
severity: Sev2
---
# [SOP] Supermicro IPMI 管理密碼重置流程
## 1. 目的 (Objective)
當管理員忘記 IPMI Web 介面之登入密碼，或因資安策略需在作業系統 (OS) 端強制變更 IPMI 帳號密碼時，使用本 SOP 進行重置。

## 2. 前置準備 (Prerequisites)
1.  **作業系統權限**：需具備進入該伺服器 OS (Windows/Linux) 的 Administrator 或 root 權限。
2.  **工具程式**：需下載並解壓縮 Supermicro **IPMICFG** 實體工具程式。
3.  **驅動程式**：確認系統已安裝 IPMI 驅動（Linux 通常為 `ipmi_si` 或 `ipmi_devintf` 模組）。

---
## 3. 操作步驟 (Procedure)
### 第一階段：查詢使用者資訊

1.  **開啟終端機**：以管理員權限開啟 Command Prompt (Windows) 或 Terminal (Linux)。
2.  **導航至工具目錄**：使用 `cd` 指令進入 IPMICFG 所在資料夾。
3.  **查詢列表**：輸入以下指令查看目前 IPMI 的使用者清單與對應 ID：
~~~bash
# Linux 範例
./IPMICFG-Linux -user list
# Windows 範例
IPMICFG-Win.exe -user list
~~~
*備註：預設的 Administrator 帳號 ID 通常為 2。*

### 第二階段：執行密碼重置
1.  **設定新密碼**：使用以下指令變更特定 ID 的密碼：
~~~bash
# 格式：IPMICFG -user setpwd <User_ID> <New_Password>
./IPMICFG-Linux -user setpwd 2 MyNewPassword123!
~~~

---
## 4. 驗證結果 (Verification)
1.  **Web 登入測試**：開啟瀏覽器並輸入伺服器 IPMI IP。
2.  **憑證測試**：輸入帳號 (例如: ADMIN) 與剛剛設定的新密碼。
3.  **確認成功**：確認能成功進入 IPMI Dashboard 頁面。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：執行指令顯示 "Driver not found"
* **原因**：作業系統層級未載入 IPMI 驅動程式。
* **解法 (Linux)**：執行 `modprobe ipmi_devintf` 與 `modprobe ipmi_si` 後重試。
* **解法 (Windows)**：確認裝置管理員中「系統裝置」下的「Microsoft 通用 IPMI 相容裝置」運作正常。
