---
# ==========================================
#  AI 知識庫標準檔頭 (V2.4 - 2026/03/19)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: HP iLO License 匯入流程 # hackmd檔案的標題
date: 2026-03-19
author: 仁嘉
category: Operation # 四擇一：Operation / Development / Security / Planning
status: Done       # 選項：Draft / Review / Done

# --- [維運/安控區] (不適用填 [N/A]) ---
vendor: [HPE]                 # 廠牌名稱 (支援多值，如: [Dell, HPE, Fortinet])
device_type: [Server]         # 設備類型 (支援多值，如: [Server, Network])
model: [ProLiant]             # 具體型號 (支援多值填寫)
target_env: [iLO]             # 目標操作環境/介面 (如: [Linux], [Windows], [iDRAC])
required_privilege: [Administrator] # 執行所需權限 (如: [root], [Administrator], [User])
error_code: [N/A]             # 錯誤代碼 (僅故障排除必填，其餘填 [N/A])
severity: [N/A]               # 嚴重程度 (單一或多值，如: [Sev2], [N/A])
threat_type: [N/A]            # 資安威脅類型 (安控專用，如: [Phishing], [N/A])

# --- [開發區] (不適用填 N/A) ---
language: N/A      # 程式語言 (如: Python, Java, Shell)
framework: N/A     # 開發框架 (如: SpringBoot, React)
version: N/A       # 版本號 (如: v1.0.2)

# --- [企劃/專案區] (不適用填 N/A) ---
project_id: N/A    # 專案編號 (如: KMS-2026)
phase: N/A         # 專案階段 (如: Design, UAT, Production)

# --- 檢索標籤區 ---
tags: [HPE, iLO, License, SOP, Import] # 關鍵技術標籤集
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
