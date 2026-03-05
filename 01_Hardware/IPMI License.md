---
# ==========================================
#  AI 知識庫標準檔頭 (V2.0)
#  請勿隨意刪除 Key 值 (冒號前的文字)
# ==========================================
title: Supermicro IPMI OOB (SFT-OOB-LIC) 授權啟用與驗證操作手冊
tags: [Supermicro, IPMI, OOB, License, SOP, L2]
date: 2026-03-05
author: [您的名字]
# --- AI 識別與進階檢索區 (必填) ---
category: Operation
device_type: Server
vendor: Supermicro
model: [X10, X11, X12 Series]
error_code: [N/A]
severity: N/A
---
# [SOP] Supermicro IPMI OOB 授權 (License) 啟用流程
## 1. 目的 (Objective)
啟用 Supermicro 伺服器之帶外管理 (Out-of-Band, OOB) 進階功能，主要包含透過 BIOS 更新工具進行遠端 BIOS 更新、遠端 RAID 配置等核心維運功能。

## 2. 前置準備 (Prerequisites)
1.  **Node Product Key**：需具備針對該主機 BMC MAC 地址產生的 24 碼授權金鑰。
2.  **BMC MAC 地址**：可於伺服器後方標籤、BIOS 或 IPMI 登入首頁取得。
3.  **網路連線**：操作電腦需能連線至 Supermicro IPMI 管理介面。
4.  **工具**：IPMI Web UI 或 Supermicro Update Manager (SUM)。

---
## 3. 操作步驟 (Procedure)
### 第一階段：Web UI 介面啟用

1.  **登入 IPMI**：使用瀏覽器開啟 IPMI IP 並登入。
2.  **進入授權頁面**：導覽至選單列中的 **Miscellaneous** > **Activate License**。
3.  **輸入金鑰**：
    * 在 **Node Product Key** 欄位中，填入 24 碼授權金鑰（通常以 4 碼為一組，共 6 組）。
    * 點選 **Activate** 按鈕。
4.  **確認啟用**：系統將彈出視窗提示「Success」，表示授權已生效。

### 第二階段：SUM 命令行啟用 (選用)
~~~bash
# 使用 SUM 工具遠端啟用
sum -i <IPMI_IP> -u <User> -p <Pass> -c ActivateLicense --key <24_Key>
~~~

---
## 4. 驗證結果 (Verification)
1.  **檢視授權列表**：重新整理 **Miscellaneous** > **Activate License** 頁面。
2.  **Status**：確認頁面顯示該功能狀態為 **"Activated"**。
3.  **功能測試**：導航至 **Maintenance** > **BIOS Update**，確認原本的鎖定提示消失，可進入檔案上傳頁面。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：啟動失敗顯示 Key 與 MAC 不匹配
* **原因**：Supermicro OOB 授權是與 BMC MAC 地址綁定的，不同主機不可共用。
* **解法**：確認申請授權時填寫的 MAC 地址與目前硬體完全一致。
