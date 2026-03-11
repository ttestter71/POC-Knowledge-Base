---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
# ==========================================
title: Supermicro IPMI OOB (SFT-OOB-LIC) 授權啟用與驗證操作手冊
date: 2026-03-11
author: SYSadmin
category: Operation
status: Done

# --- [維運/安控區] (不適用填 N/A) ---
vendor: Supermicro
device_type: Server
model: [X10, X11, X12 Series]
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
tags: [Supermicro, IPMI, OOB, License, SOP, L2]
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
