---
name: example-task
schedule: "0 8 * * *"
enabled: false
channel: discord
prompt: |
  （這裡寫排程觸發時要 {{AI_NAME}} 做什麼的指令，寫清楚一點，因為 cron 觸發時沒有上下文）
---

# Example Task（範本）

複製這個檔改成你的任務。`enabled: false` 時不會被排程重建。

## 目標
（這個任務要達成什麼）

## 流程
1. 前置檢查（要的資料 / 權限都在嗎）
2. 派 sub-agent 做重活（`run_in_background: true`）
3. 先回一條「收到、處理中」到 channel
4. 收 sub-agent 的結構化結果 → 寫檔 / 寫雲端
5. channel 通知結果（簡短：任務名 + 結果 + 異常）
6. 寫 log 到 `logs/`

## 注意
- 排程是 session-only，session 重啟後要重建（啟動時問 {{USER_ADDRESS}}）
- 頻道 / 雲端 I/O 留 parent，不傳給 sub-agent
