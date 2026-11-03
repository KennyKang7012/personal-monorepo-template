---
name: schedule-keeper
schedule: "13 23 * * 0"
enabled: {{MEMORY_SCHEDULE_ENABLED}}
channel: {{MAIN_CHANNEL}}
prompt: |
  現在是每週的排程刷新時間，目的是繞過 cron 排程「7 天自動過期」的上限。
  1. 跑 CronList 看目前有哪些排程在跑。
  2. CronDelete 把現有的 task 排程全部刪掉（**包含本排程 schedule-keeper 自己**）。
  3. 重新掃 tasks/*.md，對每個 enabled: true 的任務（**含本檔 schedule-keeper.md 自己**），
     用 CronCreate 依其 frontmatter 的 schedule 重新排上。
  4. 到 {{MAIN_CHANNEL}} 回報一句：刷新了哪幾個排程、下次刷新時間。
  關鍵：本任務一定要把自己也重建，否則下週就不會再刷新、所有排程會在 7 天後一起過期。
---

# schedule-keeper — 週末排程管家

每週日深夜把所有 enabled 排程刪掉重建，繞過 `CronCreate` 遞迴排程「7 天自動過期」的上限。這是讓 {{AI_NAME}} 的排程能長期存活的基礎建設。

## 為什麼需要它
`CronCreate` 的遞迴排程 7 天後會自動過期（最後觸發一次就被刪）。對 24/7 不重啟的 {{AI_NAME}} 來說，沒有人重啟 session 去觸發「啟動時重建」，排程就會在第 7 天悄悄死掉。這隻管家負責在過期前主動刷新。

## 自我延續原理
管家本身也是 `enabled: true` 的 task，所以它執行「掃 tasks/ → 重建全部 enabled」時，**會把自己一起重建**（拿到全新的 7 天視窗）。因為遞迴排程是「過期前最後一次觸發才被刪」，管家總會在被刪前那一刻把自己重新排上 → 永遠自我延續，不需要特例處理。

## 流程
1. `CronList` 看現況
2. `CronDelete` 清掉所有現有 task 排程（含自己）
3. 掃 `tasks/*.md`，對每個 `enabled: true` 的任務（含自己）`CronCreate` 重建
4. {{MAIN_CHANNEL}} 回報刷新結果

## 注意
- `tasks/*.md` 是排程的事實來源；管家只負責把它「同步」成實際的 cron job
- 改刷新時間：改本檔 `schedule:`（預設 `13 23 * * 0` = 週日 23:13，避開整點）
- 頻道 I/O 留 parent
