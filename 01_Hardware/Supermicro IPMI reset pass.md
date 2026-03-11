---
# ==========================================
#  AI 知識庫標準檔頭 (V2.3 - 2026/03/11 註解強化版)
# ==========================================
title: Supermicro IPMI 管理密碼重置操作手冊 (IPMICFG)
date: 2026-03-11
author: SYSadmin
category: Troubleshooting
status: Done

# --- [維運/安控區] (不適用填 N/A) ---
vendor: Supermicro
device_type: Server
model: [All Series]
error_code: Login Failed
severity: Sev2
threat_type: N/A

# --- [開發區] (不適用填 N/A) ---
language: N/A
framework: N/A
version: N/A

# --- [企劃/專案區] (不適用填 N/A) ---
project_id: N/A
phase: N/A

# --- 檢索標籤區 ---
tags: [Supermicro, IPMI, Password, Reset, Security, SOP, L2]
---
# [SOP] Supermicro IPMI 管理密碼重置流程

## 1. 目的 (Objective)
當 Web 密碼遺失時，透過作業系統端強制重置 IPMI 密碼。

## 2. 前置準備 (Prerequisites)
1.  **OS 權限**：具備 root 或 Administrator 權限。
2.  **工具程式**：Supermicro IPMICFG 執行檔。
3.  **驅動程式**：確認 OS 已載入 IPMI 驅動。

## 3. 操作步驟 (Procedure)
1.  開啟管理員權限終端機。
2.  執行查詢使用者列表：
    ~~~bash
    ./IPMICFG-Linux -user list
    ~~~
3.  執行重置（預設 ADMIN ID 為 2）：
    ~~~bash
    ./IPMICFG-Linux -user setpwd 2 <新密碼>
    ~~~

## 4. 驗證結果 (Verification)
1.  開啟瀏覽器使用新密碼登入 IPMI IP。
2.  確認可進入 Dashboard。

## 5. 常見問題與排查 (Troubleshooting)
### 狀況：Driver not found
* **原因**：核心未掛載 ipmi_devintf。
* **解法**：執行 `modprobe ipmi_devintf`。
