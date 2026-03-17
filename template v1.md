---
title: TEMP v2

---

---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
#  適用職能：維運 (Ops) / 開發 (Dev) / 安控 (Sec) / 企劃 (Planning)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: TMP
date: 2026-03-17
author: 仁嘉
category: Operation # 四擇一：Operation / Development / Security / Planning
status: Done      # 選項：Draft / Review / Done

# --- [維運/安控區] (不適用填 N/A) ---
vendor: Dell        # 廠牌名稱 (如: Dell, HPE, Cisco, Fortinet)
device_type: Server # 設備類型 (如: Server, Network, Storage)
model: [PowerEdge R640, R740] # 具體型號 (如: R640, Gen10)
error_code: N/A    # 錯誤代碼 (僅故障排除必填，其餘填 N/A)
severity: N/A      # 嚴重程度 (如: Sev1 緊急, Sev2 重要, Sev3 一般, N/A)
threat_type: N/A   # 資安威脅類型 (安控專用，如: Phishing, CVE)

# --- [開發區] (不適用填 N/A) ---
language: N/A      # 程式語言 (如: Python, Java, Shell)
framework: N/A     # 開發框架 (如: SpringBoot, React)
version: N/A       # 版本號 (如: v1.0.2)

# --- [企劃/專案區] (不適用填 N/A) ---
project_id: N/A    # 專案編號 (如: KMS-2026)
phase: N/A         # 專案階段 (如: Design, UAT, Production)

# --- 檢索標籤區 ---
tags: [Dell, iDRAC, License, SOP, Import, RACADM, L2] # 關鍵技術標籤集
---
# Dell License 匯入流程

## 1. 目的 (Objective)
說明如何將取得之 iDRAC 授權檔案。

## 2. 前置準備 (Prerequisites)
1.  **授權檔案 (.xml)**：需已取得對應 Service Tag 的有效授權檔案。
3.  **設備歸屬確認 (建議)**：建議先完成 **所有權轉移 (Ownership Transfer)**。避免區域限制 (Region Mismatch) 導致綁定失敗。
4.  **網路連線**：操作電腦需能連線至 Dell 官網及目標伺服器的 iDRAC 管理 IP。
5.  **工具**：使用瀏覽器 (Chrome/Edge)。

---
## 3. 操作步驟 (Procedure)
### 第一階段：取得授權檔案 (以 Dell Digital Locker 為例)
1.  **登入平台**：前往 [Dell Digital Locker] 
2.  **綁定服務代碼 (Bind Service Tag)**：
    * 找到 "iDRAC Enterprise" 項目。
    * 點選 **"Bind to Service Tag"**，輸入Service Tag。
    * **注意**：此步驟不可逆，請務必確認 Tag 正確無誤。
3.  **下載授權檔案**：

### 第二階段：授權匯入 (方法 A：Web UI 圖形介面)
1.  **登入 iDRAC**：
2.  進入授權頁面：
3.  **執行匯入**：
4.  **上傳檔案**：

### 第三階段：授權匯入
#### 透過 SSH 登入 iDRAC (Local SSH)
~~~bash
ssh root@192.168.1.120
racadm license import -f tftp://<TFTP_IP>/<filename.xml> -c idrac.embedded.1
~~~

---
## 4. 驗證結果 (Verification)
1.  重新整理 頁面。
2.  **Status** 應顯示綠色勾勾 (OK)。
3.  **License Type** 應顯示 **"Perpetual"** 。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：
* **原因**：。
* **解法**：。
---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
#  適用職能：維運 (Ops) / 開發 (Dev) / 安控 (Sec) / 企劃 (Planning)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: TMP
date: 2026-03-17
author: 仁嘉
category: Operation # 四擇一：Operation / Development / Security / Planning
status: Done      # 選項：Draft / Review / Done

# --- [維運/安控區] (不適用填 N/A) ---
vendor: Dell        # 廠牌名稱 (如: Dell, HPE, Cisco, Fortinet)
device_type: Server # 設備類型 (如: Server, Network, Storage)
model: [PowerEdge R640, R740] # 具體型號 (如: R640, Gen10)
error_code: N/A    # 錯誤代碼 (僅故障排除必填，其餘填 N/A)
severity: N/A      # 嚴重程度 (如: Sev1 緊急, Sev2 重要, Sev3 一般, N/A)
threat_type: N/A   # 資安威脅類型 (安控專用，如: Phishing, CVE)

# --- [開發區] (不適用填 N/A) ---
language: N/A      # 程式語言 (如: Python, Java, Shell)
framework: N/A     # 開發框架 (如: SpringBoot, React)
version: N/A       # 版本號 (如: v1.0.2)

# --- [企劃/專案區] (不適用填 N/A) ---
project_id: N/A    # 專案編號 (如: KMS-2026)
phase: N/A         # 專案階段 (如: Design, UAT, Production)

# --- 檢索標籤區 ---
tags: [Dell, iDRAC, License, SOP, Import, RACADM, L2] # 關鍵技術標籤集
---
# Dell License 匯入流程

## 1. 目的 (Objective)
說明如何將取得之 iDRAC 授權檔案。

## 2. 前置準備 (Prerequisites)
1.  **授權檔案 (.xml)**：需已取得對應 Service Tag 的有效授權檔案。
3.  **設備歸屬確認 (建議)**：建議先完成 **所有權轉移 (Ownership Transfer)**。避免區域限制 (Region Mismatch) 導致綁定失敗。
4.  **網路連線**：操作電腦需能連線至 Dell 官網及目標伺服器的 iDRAC 管理 IP。
5.  **工具**：使用瀏覽器 (Chrome/Edge)。

---
## 3. 操作步驟 (Procedure)
### 第一階段：取得授權檔案 (以 Dell Digital Locker 為例)
1.  **登入平台**：前往 [Dell Digital Locker] 
2.  **綁定服務代碼 (Bind Service Tag)**：
    * 找到 "iDRAC Enterprise" 項目。
    * 點選 **"Bind to Service Tag"**，輸入Service Tag。
    * **注意**：此步驟不可逆，請務必確認 Tag 正確無誤。
3.  **下載授權檔案**：

### 第二階段：授權匯入 (方法 A：Web UI 圖形介面)
1.  **登入 iDRAC**：
2.  進入授權頁面：
3.  **執行匯入**：
4.  **上傳檔案**：

### 第三階段：授權匯入
#### 透過 SSH 登入 iDRAC (Local SSH)
~~~bash
ssh root@192.168.1.120
racadm license import -f tftp://<TFTP_IP>/<filename.xml> -c idrac.embedded.1
~~~

---
## 4. 驗證結果 (Verification)
1.  重新整理 頁面。
2.  **Status** 應顯示綠色勾勾 (OK)。
3.  **License Type** 應顯示 **"Perpetual"** 。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：
* **原因**：。
* **解法**：。
