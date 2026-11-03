---
name: claw-init
description: 問答式將 Codex 工作目錄初始化為 OpenClaw 風格個人助理（初始化龍蝦、bootstrap openclaw）。建立專案指令、身分、記憶、角色工作說明與選用任務規格；修改技能包或單純分析時不執行初始化。
last_edited: 2026-11-04
---

# claw-init — Codex 個人助理初始化

保留上游的問答建站流程，改用 Codex 的 `AGENTS.md` 與可攜的 Markdown 工作說明。產出是工作脈絡與任務規格；不會自動變成常駐服務、接通外部頻道或啟用排程。

## 先確認目標與範圍

1. 辨識使用者要「初始化助理」、「分析」或「修改技能包」。後兩者只處理指定內容，不進入建站問答。
2. 初始化時確認目標的絕對路徑；使用者已明確指定就沿用，不要把技能所在資料夾當成輸出位置。
3. 閱讀目標適用的 `AGENTS.md`、`AGENTS.override.md`、README 與 Git 狀態，檢查下表所有目標是否存在。遇到 override 時說明新 `AGENTS.md` 可能不會被載入，不自行刪除 override。
4. 已有 vault 或助理結構時沿用既有路徑，提出逐檔合併方案與 diff，再取得核准；不整檔覆蓋、不建立第二套 vault，不因沿用模板而重寫既有規則。
5. 寫入授權只涵蓋已確認的檔案；Git init、commit、push、權限設定、外部訊息與實際排程要分別依使用者授權處理。技能不能擴大主機提供的存取權限。

## 問答

一次詢問 1～3 題，沿用本次對話已確認的答案，只補缺漏。互動工具依目前環境可用能力選擇；外部頻道無法顯示選項時改用文字問句，仍須保留必要的核准流程。

- 使用者：稱呼、名字或暱稱、主要工作脈絡。
- 助理：名字、定位、語氣、吐槽尺度、emoji 與禁用詞。
- 工作方式：Codex app／CLI／IDE 或已連接的其他介面、可處理的目錄、一般操作與需先確認的範圍、日期使用的時區。這些是協作約定，不是沙箱設定。
- 記憶：允許保存哪些摘要、哪些內容不要保存；區分本工作區記憶與平台管理的記憶。
- 選用每晚記憶任務：是否建立規格；若要，詢問確切時間與時區。預設只準備規格，不建立自動化。時間依使用者答案，不自行偏移分鐘。

複述答案、預計新增／合併的檔案與不會自動啟用的功能，確認後再產生。不要沿用上游範例人物。敏感資料不寫進 repo；只保留經核准的安全儲存位置說明。

## 產出對照

所有模板路徑皆相對於本技能目錄；輸出路徑相對於已確認的目標工作區。

| 模板 | 輸出 | 用途 |
| --- | --- | --- |
| [templates/AGENTS.md](templates/AGENTS.md) | `AGENTS.md` | Codex 專案指令與脈絡讀取入口 |
| [templates/IDENTITY.md](templates/IDENTITY.md) | `IDENTITY.md` | 助理定位 |
| [templates/SOUL.md](templates/SOUL.md) | `SOUL.md` | 語氣約定 |
| [templates/USER.md](templates/USER.md) | `USER.md` | 核准保存的使用者偏好 |
| [templates/MEMORY.md](templates/MEMORY.md) | `MEMORY.md` | 工作區長期記憶索引 |
| [templates/WORKSPACE.md](templates/WORKSPACE.md) | `WORKSPACE.md` | 工作目錄與授權邊界，非執行設定 |
| [templates/agent-memory-keeper.md](templates/agent-memory-keeper.md) | `agents/memory-keeper.md` | 記憶整理角色工作說明 |
| [templates/agent-researcher.md](templates/agent-researcher.md) | `agents/researcher.md` | 研究角色工作說明 |
| [templates/task-example.md](templates/task-example.md) | `tasks/_TEMPLATE.md` | 手動／自動化共用任務規格範本 |
| [templates/task-memory-nightly.md](templates/task-memory-nightly.md) | `tasks/memory-nightly.md` | 選用的每晚記憶任務規格 |
| [templates/gitignore](templates/gitignore) | `.gitignore` | 僅合併必要的暫存忽略規則 |

另建立 `memory/YYYY-MM-DD.md`（建站紀錄）、`artifacts/.gitkeep`、`logs/.gitkeep`。日期使用已確認的使用者時區。若既有專案有其他歸宿，就調整模板中的路徑與連結，一致沿用既有結構。

### 填值規則

- `AI_NAME`、`USER_NAME`、`USER_ADDRESS`、`USER_CONTEXT`、`MAIN_CHANNEL`：使用者答案；不願提供真名時用核准的稱呼。
- `AI_IDENTITY_DESC`：依助理定位寫 1～2 句。
- `SOUL_SUMMARY`、`TONE_RULES`、`HARD_STYLE_RULES`、`EMOJI_RULE`：整理語氣、吐槽、禁用詞與 emoji 答案，不自行增加人格偏好。
- `WORK_DIRS`、`APPROVAL_BOUNDARY`、`MEMORY_SCOPE`：以條列文字記錄已核准的目錄、操作與保存範圍。
- `TIMEZONE`：工作區採用的使用者時區，優先沿用已確認的環境資訊，否則詢問。
- `TODAY`：使用者時區的 `YYYY-MM-DD`；每日紀錄格式見主指令模板。
- `MEMORY_TIME_HUMAN`：僅在建立每晚任務時使用，記載確切時間。

讀取需要的模板，把 `{{變數}}` 全部替換。YAML 純量中的使用者文字需正確引用與跳脫，避免名字中的冒號、引號或換行破壞格式。建站紀錄只寫已完成的事；未啟用排程就記「僅準備任務規格」。已存在的每日紀錄只合併新增內容，不覆寫。

## 角色與排程的 Codex 邊界

- `agents/*.md` 是給 Codex 讀取的角色提示，不是自動註冊的 sub-agent。先讀相關角色檔，可由主 agent 直接完成；只有環境提供委派能力，且使用者或適用指令授權時才委派。不臆造模型、工具名稱或背景執行參數。
- `tasks/*.md` 的 frontmatter 是本技能自訂的任務規格，Codex 不會自動掃描後啟用。規格檔與排程服務各自記錄預期內容與實際狀態，修改檔案不代表更新服務。
- 使用者另行要求啟用時，先準備可審閱的 prompt、工作區、時間、時區、寫入與通知範圍。依當前環境探索 `automation_update`，讀取實際 schema，先查是否已有相同排程，再操作指定項目；不改無關排程。
- 沒有自動化工具時保留手動規格並說明限制，不自行改用 OS cron、背景 daemon 或其他服務。CLI／IDE 可準備與手動驗證任務，不能假定具備 app 排程管理能力。
- 不保留上游的七天刷新管家；不刪除或重建所有排程。設定成功、手動執行通過、無人值守執行通過是三種不同證據。
- 外部頻道須先確認連接工具與收件對象、取得傳送授權。僅選擇主要介面不代表建立或授權外部訊息串接。
- 不產生權限設定檔；若另有設定需求，先查當前官方文件並檢查使用者設定，再提出最小改動。

## 驗證與回報

檢查輸出：UTF-8、YAML 可解析、無未填變數、所有相對連結可解析、目標檔與核准清單相符；檢查 diff 與 Git 狀態，確認沒有改動主機設定、未授權檔案或既有排程。指令若被 override 遮蔽需明確回報。

先回報完成內容與檔案，再列出實際驗證、未驗證能力與下一步。建議從目標工作區開啟新 Codex 聊天，請它摘要載入的指令與助理定位，作為下一個人工驗收；未做這一步就不宣稱啟動驗收完成。
