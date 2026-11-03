---
name: memory-nightly
schedule: "{{MEMORY_CRON}}"
enabled: {{MEMORY_SCHEDULE_ENABLED}}
channel: {{MAIN_CHANNEL}}
prompt: |
  現在是每晚的記憶整理時間。整理「今天」這一天的紀錄：
  1. 派 memory-keeper sub-agent（run_in_background: true）讀今天的 logs/、session 紀錄、git log，
     整合寫進 memory/YYYY-MM-DD.md（以 {{AI_NAME}} 第一人稱），重大事件補進 MEMORY.md 索引。
  2. 收到 memory-keeper 的結構化結果後，到 {{MAIN_CHANNEL}} 回報一句（今天整理了幾筆 / 有無重點 / 有無異常）。
  3. 寫一行 log 到 logs/。
  找不到當天紀錄就讓 memory-keeper 寫「今天無事發生」，不要硬編。
---

# memory-nightly — 每晚記憶整理排程

每天 {{MEMORY_TIME_HUMAN}} 自動整理當天記憶。這是 claw-init 內建的記憶排程，由 `memory-keeper` sub-agent 執行實際整理。

## 目標
把 {{USER_ADDRESS}} 當天發生的事，整理留存成 `memory/YYYY-MM-DD.md`，重大事件補進 `MEMORY.md`，讓 {{AI_NAME}} 的記憶能跨 session 延續。

## 流程
1. 前置檢查（今天的 `logs/` / session 紀錄 / `git log` 在不在）
2. 派 **memory-keeper** sub-agent 做整理（`run_in_background: true`）— 不另開新 agent，記憶整理本來就是它的職責
3. 先回一條「收到、開始整理今天的記憶」到 {{MAIN_CHANNEL}}
4. 收 memory-keeper 的結構化結果 → 確認 `memory/YYYY-MM-DD.md` 已寫入
5. {{MAIN_CHANNEL}} 回報結果（簡短：日期 + 幾筆重點 + 有無異常）
6. 寫 log 到 `logs/`

## 排程注意（重要）
- 排程是 session-only，且 **CronCreate 的遞迴排程 7 天後會自動過期**（最後觸發一次就被刪）
- 所以這個排程靠兩道機制維持，細節見 `CLAUDE.md` 的「Task 與排程設計原則」：
  1. **啟動時**：{{AI_NAME}} 檢查 `CronList`，缺這個排程就提醒 {{USER_ADDRESS}} 是否重建
  2. **週末管家**：每週末刪除並重建所有 enabled 排程（含管家自己），繞過 7 天上限
- 頻道 / 雲端 I/O 留 parent，不傳給 sub-agent
- 改時間：改本檔 `schedule:`（cron 格式，本地時區），下次重建時生效
