---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
# ==========================================
title: HPE iLO 5 Advanced License 啟用與驗證操作手冊
date: 2026-03-11
author: SYSadmin
category: Operation
status: Done

# --- [維運/安控區] (不適用填 N/A) ---
vendor: HPE
device_type: Server
model: [ProLiant Gen10, Gen10 Plus]
error_code: N/A
severity: N/A
threat_type: N/A

# --- [開發區] (不適用填 N/A) ---
language: N/A
framework: N/A
version: N/A

# --- [企劃/專案區] (不適用填 N/A) ---
project_id: N/A
phase: N/A

# --- 檢索標籤區 ---
tags: [HPE, iLO, License, SOP, Activation, L2]
---
# [SOP] HPE iLO 5 授權啟用流程

## 1. 目的 (Objective)
啟用 HPE iLO Advanced 功能（遠端控制台、電源管理等）。

## 2. 前置準備 (Prerequisites)
1.  **25 碼授權金鑰**。
2.  **iLO 5 網路 IP**。
3.  **管理員帳號權限**。

## 3. 操作步驟 (Procedure)
### 第一階段：Web UI 介面匯入
1.  登入 iLO 5 Web 介面。
2.  導覽至 **Administration** > **Licensing**。
3.  在 **Activation Key** 欄位中輸入 25 碼金鑰。
4.  點選 **Install** 並確認跳出的合約。

### 第二階段：ilorest 工具匯入
~~~bash
ilorest login <IP> -u <User> -p <Pass>
ilorest license <Key>
~~~

## 4. 驗證結果 (Verification)
1.  確認 Licensing 頁面之 **License Type** 變更為 **iLO 5 Advanced**。
2.  測試 **Remote Console** 是否可正常開啟。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：金鑰無效
* **原因**：金鑰與硬體世代不符（如 iLO 4 金鑰用於 iLO 5）。
* **解法**：請確認金鑰支援 iLO 5 版本。
