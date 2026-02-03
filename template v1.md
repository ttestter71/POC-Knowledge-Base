---
# ==========================================
#  AI 知識庫標準檔頭 (V2.0)
#  請勿隨意刪除 Key 值 (冒號前的文字)
# ==========================================
title: Dell iDRAC9 Enterprise License 匯入與匯出操作手冊
tags: [Dell, iDRAC, License, SOP, Import, RACADM, L2]
date: 2026-02-03
author: [您的名字]

# --- AI 識別與進階檢索區 (必填) ---
category: Operation         # 選項: Troubleshooting (故障排除), Operation (操作), Knowledge (知識)
device_type: Server         # 選項: Server, Network, Storage
vendor: Dell                # 選項: Dell, HP, Cisco, Fortinet
model: [PowerEdge R640, R740] # 適用機型，若通用可填 [All]
error_code: [N/A]           # 若為故障排除請填錯誤碼 (如: LIC001)，無則填 N/A
severity: N/A               # 選項: Sev1 (緊急), Sev2 (重要), Sev3 (一般), N/A (操作手冊)
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

---

## 3. 操作步驟 (Procedure)

### 第一階段：取得授權檔案 (以 Dell Digital Locker 為例)
*備註：若使用試用版 (Evaluation License)，可跳過此階段。*

1.  **登入平台**：前往 [Dell Digital Locker](https://www.dell.com/support/software/us/en/19) 使用購買授權的 Email 登入。
2.  **綁定服務代碼 (Bind Service Tag)**：
    * 找到 "iDRAC Enterprise" 項目。
    * 點選 **"Bind to Service Tag"**，輸入目標伺服器的 Service Tag。
    * **注意**：此步驟不可逆，請務必確認 Tag 正確無誤。
3.  **下載授權檔案**：點選 **"Keys available for download"** 下載 `.xml` 檔案 (建議備份至 NAS/雲端)。

### 第二階段：授權匯入 (方法 A：Web UI 圖形介面)
*適用於單機操作，最直覺的方式。*

1.  **登入 iDRAC**：使用瀏覽器開啟 iDRAC IP。
2.  **進入授權頁面**：導航至 **Configuration** > **Licenses**。
3.  **執行匯入 (關鍵步驟)**：
    * 在授權列表找到 **Device** 為 `iDRAC` 或 `iDRAC9` 的列。
    * **重要**：點選最右側下拉選單 **"License Options"** > **"Import"**。
4.  **上傳檔案**：選擇 `.xml` 檔並點選 **Upload/Install**。

### 第三階段：授權匯入 (方法 B：CLI 指令列)
*適用於自動化部署或無法存取 Web UI 時。*

#### 情境 1：從遠端管理電腦匯入 (Remote RACADM)

~~~bash
# 語法: racadm -r <IP> -u <User> -p <Pass> license import -f <XML_Path> -c idrac.embedded.1
racadm -r 192.168.1.120 -u root -p calvin license import -f C:\Downloads\ABC12D3_license.xml -c idrac.embedded.1
~~~

#### 情境 2：透過 SSH 登入 iDRAC (Local SSH)
需將 XML 放置於 TFTP/HTTP 伺服器上。

~~~bash
ssh root@192.168.1.120

# 執行匯入 (標準參數版)
racadm license import -f <filename.xml> -l <TFTP_Server_IP> -t xml -c idrac.embedded.1

# 執行匯入 (URL 直連版)
racadm license import -f tftp://<TFTP_IP>/<filename.xml> -c idrac.embedded.1
~~~

---

## 4. 驗證結果 (Verification)

### 驗證方法 A：Web UI 確認
1.  重新整理 **Configuration** > **Licenses** 頁面。
2.  **Status** 應顯示綠色勾勾 (OK)。
3.  **License Type** 應顯示 **"Perpetual"** (永久)。
4.  **功能測試**：確認 Virtual Console 可開啟。

### 驗證方法 B：CLI 確認
使用指令查詢授權狀態：

~~~bash
racadm license view
~~~

**預期輸出結果 (Expected Output):**
~~~text
Status = OK
Device = iDRAC.Embedded.1
License Description = iDRAC9 Enterprise
License Type = PERPETUAL           <-- 關鍵檢查點
Entitlement ID = xxxxxxxxxxxxx
Expiration Date = Never            <-- 確認為永久授權
~~~

## 5. 常見問題與排查 (Troubleshooting)

### 狀況：匯入後介面仍顯示 Datacenter (試用版)
* **原因**：系統同時存在試用版 (Datacenter) 與永久版 (Enterprise) 時，優先顯示功能較多的試用版。
* **解法**：Enterprise 功能已啟用，可忽略。若需強制切換，請手動 **Delete** Datacenter 試用授權。