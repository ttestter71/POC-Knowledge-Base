---
# ==========================================
#  AI 知識庫標準檔頭 (V2.4 - 2026/03/19)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: Supermicro IPMI License 匯入流程 # hackmd檔案的標題
date: 2026-03-19
author: 仁嘉
category: Operation # 四擇一：Operation / Development / Security / Planning
status: Done       # 選項：Draft / Review / Done

# --- [維運/安控區] (不適用填 [N/A]) ---
vendor: [Supermicro]          # 廠牌名稱 (支援多值，如: [Dell, HPE, Fortinet])
device_type: [Server]         # 設備類型 (支援多值，如: [Server, Network])
model: [N/A]                  # 具體型號 (支援多值填寫)
target_env: [IPMI]            # 目標操作環境/介面 (如: [Linux], [Windows], [iDRAC])
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
tags: [Supermicro, IPMI, License, SOP, Import] # 關鍵技術標籤集
---
# [SOP] Supermicro IPMI OOB 授權啟用流程

## 1. 目的 (Objective)
啟用 Supermicro 伺服器之遠端 BIOS 更新及 RAID 設定功能。

## 2. 前置準備 (Prerequisites)
1.  **24 碼 OOB 授權金鑰**。
2.  **目標伺服器之 BMC MAC 地址**。
3.  **IPMI Web 連線**。

## 3. 操作步驟 (Procedure)
### 第一階段：Web UI 啟用
1.  登入 Supermicro IPMI。
2.  導覽至 **Miscellaneous** > **Activate License**。
3.  輸入 24 碼 Node Product Key。
4.  點選 **Activate** 並確認 "Success" 視窗。

### 第二階段：SUM 工具啟用
~~~bash
sum -i <IP> -u <User> -p <Pass> -c ActivateLicense --key <Key>
~~~

## 4. 驗證結果 (Verification)
1.  確認頁面狀態顯示為 **Activated**。
2.  進入 **Maintenance** > **BIOS Update** 確認功能解鎖。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：MAC 地址不匹配
* **原因**：OOB 授權與 MAC 綁定，輸入錯誤金鑰將導致失敗。
* **解法**：核對金鑰對應的 MAC 是否與硬體一致。
