---
name: claw-init
description: 把一個空的 Claude Code 工作目錄問答式初始化成 OpenClaw 風的個人助理（龍蝦）。逐題問新人「怎麼稱呼你 / AI 叫什麼 / 什麼語氣 / 主要頻道 / 權限尺度 / 要不要每晚記憶排程」，然後生出 CLAUDE.md、IDENTITY.md、SOUL.md、USER.md、MEMORY.md、memory/ 今日篇、.claude/settings.json（危險指令封鎖、資料夾範圍；權限模式 defaultMode 不寫專案層級、改問是否加到使用者層級）、tasks/ 範本與設計原則、選用的每晚記憶排程（memory-nightly + schedule-keeper 兩個 task 檔，含啟動提醒，靠週末管家繞過 cron 7 天過期）、.claude/agents/ 兩隻基本 agent（memory-keeper + researcher）、logs/。
triggers_zh:
  - 初始化龍蝦
  - 建站
  - 把 claude code 變成個人助理
  - openclaw 初始化
  - claw-init
triggers_en:
  - init my assistant
  - bootstrap openclaw
  - set up personal assistant
  - claw-init
---

# claw-init — OpenClaw 風個人助理建站

把一個（通常是空的）Claude Code 工作目錄，問答式初始化成一隻 OpenClaw 風格的個人助理（社長口中的「龍蝦」）。

OpenClaw 的概念：把 Claude Code 跑成 24/7 的個人助理，接 Discord / Telegram / LINE 等頻道，靠 `CLAUDE.md` / `IDENTITY.md` / `SOUL.md` / `USER.md` / `MEMORY.md` 這幾個身份檔，把通用 Claude 變成「某個人的專屬助理」。這個 Skill 就是那個引導建站流程。

---

## 這個 Skill 跑完會長出什麼

| 路徑 | 內容 | 模板 |
|---|---|---|
| `CLAUDE.md` | 啟動流程 + 運行期互動規則 + 人格 + 記憶維護 + task 設計原則 + 頻道規則 + 安全守則 + 編制 | `templates/CLAUDE.md` |
| `IDENTITY.md` | AI 是誰 | `templates/IDENTITY.md` |
| `SOUL.md` | AI 怎麼說話（語氣規則） | `templates/SOUL.md` |
| `USER.md` | 在幫誰（名字、稱呼、脈絡） | `templates/USER.md` |
| `MEMORY.md` | 長期記憶索引（骨架） | `templates/MEMORY.md` |
| `memory/<今天>.md` | 第一篇記憶（建站紀錄） | 執行期直接寫，無模板 |
| `.claude/settings.json` | 資料夾範圍 + 封鎖危險指令（**不含 `defaultMode`**，專案層級無效） | `templates/settings.json` |
| `tasks/_TEMPLATE.md` | 排程任務範本 | `templates/task-example.md` |
| `tasks/memory-nightly.md` | 每晚記憶整理排程（**選用**，問答決定要不要建） | `templates/task-memory-nightly.md` |
| `tasks/schedule-keeper.md` | 週末排程管家，刪除+重建所有排程繞過 cron 7 天過期（**選用**，跟記憶排程一起建） | `templates/task-schedule-keeper.md` |
| `.claude/agents/memory-keeper.md` | 記憶整理員（也是 memory-nightly 排程實際派的 sub-agent） | `templates/agent-memory-keeper.md` |
| `.claude/agents/researcher.md` | 通用研究員 | `templates/agent-researcher.md` |
| `.gitignore` | 忽略 artifacts / log / 暫存 | `templates/gitignore` |
| `artifacts/.gitkeep` | 暫存產物歸宿 | 執行期建立 |
| `logs/.gitkeep` | log 資料夾 | 執行期建立 |

全部寫進**目前工作目錄（CWD）**，不是這個 Skill 所在的 repo。

---

## Hard rules（不可違反）

1. **問答在終端機逐題進行**：一次問 1~3 題，等使用者用文字回答再往下。這是建站階段、使用者就坐在電腦前，可以用選項 UI；但**養成用文字問句的習慣**，因為產出的助理之後在頻道上不能用選項面板（見下方第 5 點，這條也會寫進產出的 CLAUDE.md）。
2. **不要硬編使用者資料**：所有人名、稱呼、語氣都來自問答答案，不要套用任何既有人物（不要出現「伊茲」「社長 Casper」這類本 repo 的範例人物）。
3. **機敏資訊先問再存**：問答過程若使用者主動丟 token / API key / webhook URL，**不要寫進任何 repo 內檔案**，先問「這個要存哪？」（建議 `~/.config/<name>/` 或 keychain），這座站的檔案只記「放在哪」的 reference。
4. **不覆蓋既有檔案而不確認**：Step 0 先檢查 CWD 是否已建過站，已有就先問要不要覆蓋。
5. **產出 CLAUDE.md 必含「運行期禁止選項」規則**：模板已內建，不要刪。

---

## 流程

### Step 0 — 前置檢查

1. 跑 `pwd` 確認 CWD，跟使用者確認「就在這個資料夾建站對嗎？」
2. 跑 `ls -la` 看是不是空的、是不是 git repo
3. 若已存在 `CLAUDE.md` / `IDENTITY.md` / `SOUL.md` 任何一個 → 提醒使用者「這個資料夾好像已經建過站，要覆蓋重建嗎？」，得到肯定才繼續
4. 若不是 git repo → 建議「要不要 `git init` 讓記憶檔有版控備份？」（OpenClaw 建議把 workspace 當 private git repo）。問一下，不要自己 init

### Step 1 — 問答

逐題問，蒐集下列答案。題目用使用者的語言、口語問，不要一次倒一大串。

**關於使用者**
- 我該怎麼稱呼你？（名字 / 暱稱 / 稱謂，例如「老闆」「直接叫名字」） → `USER_ADDRESS`、`USER_NAME`
- 你平常在忙什麼？（工作 / 專案脈絡，讓助理知道幫忙的方向） → `USER_CONTEXT`

**關於 AI 自己**
- 你想叫我什麼名字？ → `AI_NAME`
- 你想要我是什麼個性？分開問或一起問：語氣（正經 / 輕鬆 / 毒舌）、要不要吐槽、emoji 用不用、有沒有禁用的符號或詞 → `TONE`、`SNARK`、`EMOJI`、`BANNED`

**關於運行**
- 你主要會從哪裡跟我講話？（純終端機 / Discord / Telegram / LINE / 其他） → `MAIN_CHANNEL`
- 你要讓我能讀寫哪些資料夾？（工作目錄範圍，給絕對路徑或 `~/...`） → `WORK_DIRS`
- 權限尺度：要我自動執行多數操作、少打斷你嗎？還是每步都先問？ → `PERMISSION_MODE`
  - 「自動、少打斷」→ `DEFAULT_MODE = auto`（Claude Code 的 auto mode：自動核准工具呼叫、背景跑安全檢查）
  - 「每步先問」→ `DEFAULT_MODE = default`
  - （若使用者要更猛可提 `bypassPermissions`，但要講清楚風險，預設不推）
  - ⚠️ **這個答案不會寫進專案層級的 `.claude/settings.json`**：`permissions.defaultMode` 寫在 checked-in 的專案設定**不會生效**（Claude Code 不讓 repo 設定自動放寬權限模式，是安全設計）。先記著 `DEFAULT_MODE`，到 Step 3 再問使用者要不要加進**使用者層級** `~/.claude/settings.json`

**關於記憶排程（選用）**
- 要不要幫你建一個「每晚自動整理當天記憶」的排程？（會派 `memory-keeper` 整理當天紀錄寫進 `memory/YYYY-MM-DD.md`） → `MEMORY_SCHEDULE`
  - 要 → `MEMORY_SCHEDULE = yes`，再問「幾點跑？（預設晚上 11 點）」→ `MEMORY_TIME`
  - 不要 → `MEMORY_SCHEDULE = no`，**不生成** `tasks/memory-nightly.md`
- 解釋給使用者聽：這排程不是馬上開跑，建站只是寫好任務規格；之後在頻道 / 終端機跑起來時，{{AI_NAME}} 會主動提醒要不要啟用（排程是 session-only，且 cron 7 天會過期，所以靠啟動提醒 + 週末管家維持，這套規則已寫進產出的 CLAUDE.md）

收齊後跟使用者複述一遍答案，確認沒問題再生成。

### Step 2 — 生成檔案

先建目錄骨架：`mkdir -p .claude/agents tasks memory logs artifacts`，再逐一讀 `templates/` 下對應模板，把 `{{...}}` 佔位符換成答案，寫到上表的目標路徑。

注意幾個易踩點：
- 兩隻 agent 模板叫 `agent-memory-keeper.md` / `agent-researcher.md`，但**目標檔名要去掉 `agent-` 前綴**：寫成 `.claude/agents/memory-keeper.md` / `.claude/agents/researcher.md`
- `templates/gitignore`（無開頭點）要寫成目標 `.gitignore`
- `{{TODAY}}` 同時出現在 `memory/<TODAY>.md` 的**檔名**跟 `MEMORY.md` 的**內文**，兩處都要填
- `task-memory-nightly.md` / `task-schedule-keeper.md` 是**選用且成對**：只有 `MEMORY_SCHEDULE = yes` 才生成，目標寫成 `tasks/memory-nightly.md` / `tasks/schedule-keeper.md`（去掉 `task-` 前綴）。使用者答 no 就兩個都不寫
- **管家要跟記憶排程一起生成**：`tasks/schedule-keeper.md` 是讓排程繞過 cron 7 天過期的基礎建設，它自己也是 `enabled: true` 的 task（靠「掃 tasks/ 重建全部、自己也在裡面」自我延續）。只建 memory-nightly 不建 keeper 的話，排程 7 天後會一起死掉

佔位符對照：

- `{{AI_NAME}}` ← AI_NAME
- `{{USER_ADDRESS}}` ← USER_ADDRESS
- `{{USER_NAME}}` ← USER_NAME
- `{{USER_CONTEXT}}` ← USER_CONTEXT
- `{{MAIN_CHANNEL}}` ← MAIN_CHANNEL
- `{{TODAY}}` ← 今天日期（從你的 context 取，格式 YYYY-MM-DD）
- `{{AI_IDENTITY_DESC}}` ← 你依答案寫 1~2 句「這隻 AI 是什麼定位」
- `{{SOUL_SUMMARY}}` ← 你把語氣答案濃縮成 2~3 條重點
- `{{TONE_RULES}}` / `{{HARD_STYLE_RULES}}` / `{{EMOJI_RULE}}` ← 你依 TONE / SNARK / BANNED / EMOJI 答案寫進 SOUL.md。建議分配：TONE → `TONE_RULES`；BANNED（禁用符號/詞）→ `HARD_STYLE_RULES`；SNARK（吐槽尺度）→ `TONE_RULES` 與 `HARD_STYLE_RULES` 都可帶一句；EMOJI → `EMOJI_RULE`
- `{{ADDITIONAL_DIRS}}` ← 把 WORK_DIRS 寫成 JSON 字串陣列，例如 `["~/Projects", "~/Desktop"]`
- （`DEFAULT_MODE` **不再是模板佔位符**，`settings.json` 已不含它。它只是 Step 1 收集到的答案，留到 Step 3 問使用者要不要寫進使用者層級設定）
- `{{MEMORY_CRON}}`（僅 `task-memory-nightly.md`）← 把 MEMORY_TIME 轉成 5 欄位 cron（本地時區）。預設晚上 11 點 → `7 23 * * *`（**刻意避開整點分鐘 :00 / :30**，否則全球排程會擠在同一刻）
- `{{MEMORY_SCHEDULE_ENABLED}}`（僅 `task-memory-nightly.md`）← 填 `true`（因為只有使用者選 yes 才生成這檔）
- `{{MEMORY_TIME_HUMAN}}`（僅 `task-memory-nightly.md`）← MEMORY_TIME 的人話版，例如「晚上 11 點」

額外動作：
- `memory/<TODAY>.md`：直接寫第一篇，內容是「今天用 claw-init 建站、AI_NAME 上線、做了什麼設定」，格式照 CLAUDE.md 規定（# 日期 / ## 今天做了什麼 / ## 重要決定 / ## 待辦）
- `logs/.gitkeep` / `artifacts/.gitkeep`：建立空檔讓資料夾進版控（`artifacts/` 是暫存產物歸宿，內容被 `.gitignore` 忽略、只留 .gitkeep）
- `settings.json`：`deny` 固定那串危險指令不要動；只填 `additionalDirectories`（依 WORK_DIRS）。**不要寫 `defaultMode`**——專案層級無效，模板也已移除它
- `tasks/_TEMPLATE.md`：模板的 `channel: discord` 是佔位範例，**保持原樣即可**（它只是給新人複製的範本、`enabled: false`），新人之後自己改成實際頻道。不用依 `MAIN_CHANNEL` 改它
- `tasks/memory-nightly.md` + `tasks/schedule-keeper.md`（**僅 MEMORY_SCHEDULE = yes 才寫，兩個一起寫**）：
  - 讀 `templates/task-memory-nightly.md` → 填 `{{MEMORY_CRON}}` / `{{MEMORY_SCHEDULE_ENABLED}}` / `{{MEMORY_TIME_HUMAN}}` / `{{MAIN_CHANNEL}}` / `{{AI_NAME}}` / `{{USER_ADDRESS}}`
  - 讀 `templates/task-schedule-keeper.md` → 填 `{{MEMORY_SCHEDULE_ENABLED}}`（管家的 enabled 跟記憶排程同步，都是 `true`）/ `{{MAIN_CHANNEL}}` / `{{AI_NAME}}`
  - 兩者的 `channel:` 跟 `_TEMPLATE.md` 不同，**要**依 `MAIN_CHANNEL` 填（這是真的要跑的排程，不是範本）
  - 生成後在 Step 3 收尾提醒：排程不會自動開跑，下次 {{AI_NAME}} 啟動時會問要不要 `CronCreate` 啟用（含管家）

### Step 3 — 收尾

1. 列出實際建了哪些檔（用相對路徑）

2. **權限模式（選用，寫使用者層級）**——依 Step 1 收集的 `DEFAULT_MODE` 處理：
   - 先說明：`permissions.defaultMode` 寫在這座站的專案 `.claude/settings.json` **不會生效**（Claude Code 安全設計，不讓 checked-in 設定自動放寬權限），所以剛剛沒寫進去
   - 問使用者：「要不要把 `defaultMode: <值>` 加進你的**使用者層級**設定 `~/.claude/settings.json`？這會讓你**之後所有專案**預設用這個權限模式」
   - 說明該模式意義 + 影響範圍：
     - `auto`／`acceptEdits` = 自動核准多數工具呼叫、少打斷你
     - `default` = 每個動作都先問你（最保守）
     - `bypassPermissions` = 幾乎全放行、**風險高**，只在你完全信任的環境用
   - 使用者說**要**才動手，且務必安全寫入：
     1. 讀現有 `~/.claude/settings.json`（不存在就當 `{}`）
     2. **只 merge 進 `permissions.defaultMode`，不要覆蓋任何既有設定**（先解析 JSON 再改，不要整檔重寫）
     3. 寫回後把改動的 `permissions` 區塊 before/after 給使用者看，確認沒動到別的
   - 使用者說**不要** → 跳過，告訴他「之後想開可自己加到 `~/.claude/settings.json`」

3. 提示下一步，但**不要自動執行**：
   - 重啟 session（或重開 Claude Code）讓 `CLAUDE.md` 生效
   - 若要接頻道（Discord / Telegram / LINE），去跑對應的 channel 設定
   - 若有建記憶排程：說明它現在是 `enabled: true` 的規格檔但**還沒實際排上**，下次 {{AI_NAME}} 啟動時會主動問要不要 `CronCreate` 啟用（含週末管家），排程是 session-only、cron 7 天會過期，靠這套機制維持
   - 建議 `git add -A && git commit`（**問過再做**，commit 是動版控、先確認）

4. 用一句該助理的語氣，跟使用者打個招呼收尾

---

## 設計理念附註

- 這套結構照 OpenClaw 的身份檔模型（CLAUDE / IDENTITY / SOUL / USER / MEMORY）做的精簡可成長版
- 產出的 `CLAUDE.md` 已內建「運行期（在頻道上跑時）禁止 plan mode / 選項面板」的規則，因為選項面板會卡住外部通訊工具
- 兩隻基本 agent（memory-keeper + researcher）是最泛用的拆分：記憶 + 情報。使用者之後可自行加 agent、改名、加排程任務
- 記憶排程（memory-nightly）刻意設計成**選用 + 規格與執行分離**：`tasks/memory-nightly.md` 只是規格，真正的 cron 由執行期的 CLAUDE.md 規則用 `CronCreate` 建。因為 cron 排程是 session-only 且 7 天過期，所以靠「啟動提醒 + 排程全空時主動問 + 週末管家自我重建」三道機制維持，不讓排程在使用者不知情下默默跑或默默死掉
- `permissions.defaultMode` **刻意不寫專案層級**：checked-in 的專案設定放寬權限模式不會生效（Claude Code 安全設計，避免 clone repo 的人被自動放權）。這種「會放寬權限」的設定本來就該在**使用者層級**、由本人明確開啟，所以 claw-init 改成收尾時問、merge 進 `~/.claude/settings.json`，而非塞進站內 repo
