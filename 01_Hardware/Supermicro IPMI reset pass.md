---
# ==========================================
#  AI 知識庫標準檔頭 (V2.4 - 2026/03/19)
#  所有欄位請務必保留，不適用者請填寫 N/A
# ==========================================
title: Supermicro IPMI 密碼重置流程 # hackmd檔案的標題
date: 2026-03-19
author: 仁嘉
category: Operation # 四擇一：Operation / Development / Security / Planning
status: Done       # 選項：Draft / Review / Done

# --- [維運/安控區] (不適用填 [N/A]) ---
vendor: [Supermicro]          # 廠牌名稱 (支援多值，如: [Dell, HPE, Fortinet])
device_type: [Server]         # 設備類型 (支援多值，如: [Server, Network])
model: [N/A]                  # 具體型號 (支援多值填寫)
target_env: [Linux]           # 目標操作環境/介面 (如: [Linux], [Windows], [iDRAC])
required_privilege: [root]    # 執行所需權限 (如: [root], [Administrator], [User])
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
tags: [Supermicro, IPMI, Password, IPMICFG, Reset] # 關鍵技術標籤集
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
