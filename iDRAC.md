---
# 這是給 AI 看的身分證 (YAML Metadata)
type: 標準作業程序 (SOP)
device: Dell PowerEdge Server
client: Internal_Lab
tags: [Dell, iDRAC, License, Import, Dell_DDL, CLI, RACADM, Perpetual, Evaluation]
author: System_Engineer
status: Done
---

# [SOP] Dell iDRAC 授權 (License) 綁定與匯入流程

## 1. 問題描述

### 需求背景
使用者需將取得之 iDRAC 授權檔案（包含：**企業版 Enterprise、資料中心版 Datacenter** 之 **永久授權 (Perpetual)** 或 **試用授權 (Evaluation)**）匯入至 iDRAC 系統，以啟用進階管理功能（如：Virtual Console、Virtual Media、PCIe 散熱控制等）。

### 前置準備
1.  **授權檔案 (.xml)**：需已取得對應 Service Tag 的有效授權檔案。
    * **永久版**：需從 Dell Digital Locker 下載。
    * **試用版**：可從 Dell 官網申請後直接下載。
2.  **Service Tag**：目標伺服器的服務代碼 (例如：`ABC12D3`)。
3.  **設備歸屬確認 (建議)**：若為新購或二手設備，強烈建議先完成 **所有權轉移 (Ownership Transfer)**。這能避免因區域限制 (Region Mismatch) 導致綁定失敗，並確保未來可順利下載原廠韌體與管理資產。
4.  **網路連線**：操作電腦需能連線至 Dell 官網及目標伺服器的 iDRAC 管理 IP。

---

## 2. 排查過程 (執行步驟)

### 第一階段：取得授權檔案 (以 Dell Digital Locker 為例)

*備註：若您使用的是試用版 (Evaluation License)，可跳過此階段，直接進行第二階段的匯入。*

在將永久授權匯入伺服器前，必須先在 Dell 雲端後台完成「綁定」動作。

1.  **登入平台**
    * 前往 [Dell Digital Locker](https://www.dell.com/support/software/us/en/19)。
    * 使用購買授權的 Email 登入。

2.  **搜尋產品**
    * 在產品列表中找到 **"iDRAC Enterprise"** 或 **"iDRAC Datacenter"** 項目。
    * 點擊產品名稱進入詳細資訊頁面。

3.  **綁定服務代碼 (Bind Service Tag)**
    * 找到 **"Bind to Service Tag"** (綁定至服務代碼) 選項。
    * 輸入目標伺服器的 Service Tag (例如：`ABC12D3`)。
    * **注意**：此步驟不可逆，請務必確認 Tag 正確無誤。

4.  **下載授權檔案**
    * 綁定完成後，點擊 **"Keys available for download"**。
    * 下載格式為 `.xml` 的授權檔案 (例如：`ABC12D3_license.xml`)。
    * **建議**：將此檔案備份至安全位置 (NAS/雲端)，未來更換主機板時需重新匯入。

---

### 第二階段：授權匯入 (方法 A：Web UI 圖形介面)

適用於單機操作，最直覺的方式。

1.  **登入 iDRAC**
    * 使用瀏覽器開啟 iDRAC IP，輸入帳號密碼登入。

2.  **進入授權管理頁面**
    * 導航至 **Configuration (配置)** > **Licenses (許可證)**。

3.  **執行匯入操作 (關鍵步驟)**
    * 在授權列表表格中，找到 **Device** 為 `iDRAC` 或 `iDRAC9` 的那一列。
    * **重要**：點擊該列最右側的 **"License Options" (許可選項)** 下拉選單。
    * 選擇 **"Import" (導入/匯入)**。
    * *(註：Dell UI 設計將匯入藏在下拉選單中，而非頂部按鈕)*。

4.  **上傳檔案**
    * 在彈出視窗中瀏覽並選擇剛才下載的 `.xml` 檔案。
    * 點擊 **Upload/Install**。
    * 系統提示 "License imported successfully" 即完成。

--- 

### 第三階段：授權匯入 (方法 B：CLI 指令列)

適用於自動化部署或無法存取 Web UI 時，使用 `racadm` 工具。

#### 情境 1：從遠端管理電腦匯入 (Remote RACADM)
需先在管理電腦上安裝 Dell RACADM 工具。

* **語法**：
    ```bash
    racadm -r <iDRAC_IP> -u <Username> -p <Password> license import -f <Path_to_XML_File> -c idrac.embedded.1
    ```
* **範例**：
    ```bash
    racadm -r 192.168.1.120 -u root -p calvin license import -f C:\Downloads\ABC12D3_license.xml -c idrac.embedded.1
    ```

#### 情境 2：透過 SSH 登入 iDRAC (Local SSH)
需將 XML 檔案放置於 TFTP/FTP/HTTP/NFS 伺服器上，適合無法從本機直接傳檔時使用。
*注意：已登入 SSH 狀態下，無需再次輸入 `-u` 與 `-p` 參數。*

1.  **SSH 連線**：`ssh root@<iDRAC_IP>`
2.  **執行匯入指令**：
    ```bash
    # 【新手推薦】標準參數版 (參數明確，易於除錯)
    racadm license import -f <filename.xml> -l <TFTP_Server_IP> -t xml -c idrac.embedded.1

    # 【進階用戶】URL 直連版 (語法簡潔，自動化常用)
    racadm license import -f tftp://<TFTP_IP>/<filename.xml> -c idrac.embedded.1
    ```

---

## 3. 解決方案 (驗證結果)

完成上述匯入步驟後，請執行以下檢查以確保授權生效。

### 驗證方法 A：Web UI 確認
1.  重新整理 **Configuration** > **Licenses** 頁面。
2.  檢查表格中是否出現 **"Enterprise"** 或 **"Datacenter"** 字樣。
3.  **狀態檢查**：
    * **Status**：顯示綠色勾勾 (OK)。
    * **License Type**：顯示 **"Perpetual"** (永久) 或 **"Evaluation"** (試用)。
4.  **功能測試**：確認 Dashboard 首頁的 **Virtual Console** 是否已解鎖並可開啟預覽畫面。

### 驗證方法 B：CLI 確認
使用 SSH 登入 iDRAC 或透過遠端 racadm 執行查詢。

* **指令**：
    ```bash
    racadm license view
    ```
* **預期輸出結果** (關鍵在於 Expiration Date)：
    ```text
    Status = OK
    Device = iDRAC.Embedded.1
    License Description = iDRAC9 Enterprise
    License Type = PERPETUAL
    Entitlement ID = xxxxxxxxxxxxx
    Expiration Date = Never  <-- 確認此欄位以驗證永久授權
    ```

### 常見問題排除
* **狀況**：匯入 Enterprise 後，介面左上角仍顯示 Datacenter。
* **原因**：系統同時存在 Datacenter (試用版) 與 Enterprise (永久版) 時，會優先顯示功能較多的 Datacenter 版。
* **解法**：
    1.  **功能確認**：Enterprise 的所有功能目前皆已啟用，不受顯示名稱影響。
    2.  **處理方式**：可忽略等待試用過期，或手動在 Licenses 頁面刪除 (Delete) Datacenter 試用授權，介面即會切換回 Enterprise。

![Flabbycatty](https://hackmd.io/_uploads/HkybRZ9UZg.png)
