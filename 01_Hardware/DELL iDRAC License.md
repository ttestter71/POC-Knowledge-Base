---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
#  適用職能：維運 (Ops) / 開發 (Dev) / 安控 (Sec) / 企劃 (Planning)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: Dell iDRAC9 Enterprise License 匯入與匯出操作手冊
date: 2026-03-11
author: SYSadmin
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
# [SOP] Dell iDRAC 授權 (License) 綁定與匯入流程

## 1. 目的 (Objective)
說明如何將取得之 iDRAC 授權檔案（包含：**企業版 Enterprise、資料中心版 Datacenter** 之 **永久授權 (Perpetual)** 或 **試用授權 (Evaluation)**）匯入至 iDRAC 系統，以啟用進階管理功能（如：Virtual Console、Virtual Media、PCIe 散熱控制等）。

## 2. 前置準備 (Prerequisites)
1.  **授權檔案 (.xml)**：需已取得對應 Service Tag 的有效授權檔案。
    * **永久版**：需從 Dell Digital Locker 下載。
    * **試用版**：可從 Dell 官網申請後直接下載。
2.  **Service Tag**：目標伺服器的服務代碼 (例如：`ABC12D3`)。
3.  **設備歸屬確認 (建議)**：若為新購或二手設備，強烈建議先完成 **所有權轉移 (Ownership Transfer)**。這能避免因區域限制 (Region Mismatch) 導致綁定失敗。
4.  **網路連線**：操作電腦需能連線至 Dell 官網及目標伺服器的 iDRAC 管理 IP。
5.  **工具**：安裝 `racadm` 工具包或使用瀏覽器 (Chrome/Edge)。

---
## 3. 操作步驟 (Procedure)
### 第一階段：取得授權檔案 (以 Dell Digital Locker 為例)
1.  **登入平台**：前往 [Dell Digital Locker] 使用購買授權的 Email 登入。
2.  **綁定服務代碼 (Bind Service Tag)**：
    * 找到 "iDRAC Enterprise" 項目。
    * 點選 **"Bind to Service Tag"**，輸入目標伺服器的 Service Tag。
    * **注意**：此步驟不可逆，請務必確認 Tag 正確無誤。
3.  **下載授權檔案**：點選 **"Keys available for download"** 下載 `.xml` 檔案。

### 第二階段：授權匯入 (方法 A：Web UI 圖形介面)
1.  **登入 iDRAC**：使用瀏覽器開啟 iDRAC IP。
2.  **進入授權頁面**：導航至 **Configuration** > **Licenses**。
3.  **執行匯入**：點選最右側下拉選單 **"License Options"** > **"Import"**。
4.  **上傳檔案**：選擇 `.xml` 檔並點選 **Upload/Install**。

### 第三階段：授權匯入 (方法 B：CLI 指令列)
#### 情境 1：從遠端管理電腦匯入 (Remote RACADM)
~~~bash
racadm -r 192.168.1.120 -u root -p calvin license import -f C:\Downloads\ABC12D3_license.xml -c idrac.embedded.1
~~~
#### 情境 2：透過 SSH 登入 iDRAC (Local SSH)
~~~bash
ssh root@192.168.1.120
racadm license import -f tftp://<TFTP_IP>/<filename.xml> -c idrac.embedded.1
~~~

---
## 4. 驗證結果 (Verification)
1.  重新整理 **Configuration** > **Licenses** 頁面。
2.  **Status** 應顯示綠色勾勾 (OK)。
3.  **License Type** 應顯示 **"Perpetual"** (永久)。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：匯入後介面仍顯示 Datacenter (試用版)
* **原因**：系統優先顯示功能較多的試用版。
* **解法**：Enterprise 功能已啟用，可忽略，或手動 Delete 試用授權。
