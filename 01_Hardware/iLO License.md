---
# ==========================================
#  AI 知識庫標準檔頭 (V2.0)
#  請勿隨意刪除 Key 值 (冒號前的文字)
# ==========================================
title: HPE iLO 5 Advanced License 啟用與驗證操作手冊
tags: [HPE, iLO, License, SOP, Activation, L2]
date: 2026-03-05
author: [您的名字]
# --- AI 識別與進階檢索區 (必填) ---
category: Operation         # 選項: Troubleshooting (故障排除), Operation (操作), Knowledge (知識)
device_type: Server         # 選項: Server, Network, Storage
vendor: HPE                 # 選項: Dell, HP, Cisco, Fortinet
model: [ProLiant Gen10, Gen10 Plus] # 適用機型
error_code: [N/A]
severity: N/A
---
# [SOP] HPE iLO 5 授權 (License) 啟用流程
## 1. 目的 (Objective)
說明如何將 HPE iLO Advanced 授權金鑰匯入至 iLO 5 管理晶片中，以解鎖進階功能（如：遠端控制台 KVM、伺服器電源管理、進階資安監視等）。

## 2. 前置準備 (Prerequisites)
1.  **授權金鑰 (Entitlement Order)**：需具備 25 碼之授權啟用金鑰 (例如：`XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`)。
2.  **iLO IP 地址**：目標伺服器的 iLO 5 專用管理網路 IP。
3.  **管理帳號**：具備「Configure iLO Settings」權限之帳號（預設為 Administrator）。
4.  **網路連線**：操作電腦需能透過瀏覽器存取 iLO Web 介面。

---
## 3. 操作步驟 (Procedure)
### 第一階段：Web UI 圖形介面匯入

1.  **登入 iLO 5**：使用瀏覽器開啟 iLO IP，輸入帳號密碼登入。
2.  **進入授權頁面**：在左側導覽列中，依序點選 **Administration** > **Licensing**。
3.  **輸入金鑰**：
    * 在 **Activation Key** 區塊的五個欄位中，依序填入 25 碼金鑰。
    * 確認金鑰填寫無誤後，點選 **Install** 按鈕。
4.  **確認合約**：若系統跳出 EULA 授權合約聲明，請點選 **OK** 或 **I Agree** 完成安裝。

### 第二階段：XML / Scripting 批次匯入 (選用)
*適用於需要透過腳本自動化部署多台伺服器時，可搭配 HPE RESTful Interface Tool (ilorest) 執行。*
~~~bash
ilorest login <iLO_IP> -u <User> -p <Password>
ilorest license <Activation_Key>
~~~

---
## 4. 驗證結果 (Verification)
1.  **檢查授權狀態**：回到 **Administration** > **Licensing** 頁面。
2.  **License Type**：確認目前顯示為 **"iLO 5 Advanced"** 或對應之授權等級。
3.  **功能確認**：導航至 **Remote Console & Media** > **Web Console**，確認可成功開啟整合式遠端主控台 (IRC)。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：輸入金鑰後顯示 "Invalid License Key"
* **原因**：金鑰輸入錯誤、金鑰已過期（若為試用版），或該金鑰不適用於 iLO 5 版本。
* **解法**：重新檢查 25 碼金鑰字元，確認無誤後再次輸入；確認金鑰與硬體世代 (Gen10) 匹配。
