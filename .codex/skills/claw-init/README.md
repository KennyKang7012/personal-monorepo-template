# claw-init 🦞

> 把一個（通常是空的）Claude Code 工作目錄，**問答式**初始化成一隻 OpenClaw 風格的個人助理（社長口中的「龍蝦」）。

這是一個 [Claude Code](https://claude.com/claude-code) **Skill**。OpenClaw 的概念是：把 Claude Code 跑成 24/7 的個人助理，接 Discord / Telegram / LINE 等頻道，靠 `CLAUDE.md` / `IDENTITY.md` / `SOUL.md` / `USER.md` / `MEMORY.md` 這幾個身份檔，把通用的 Claude 變成「某個人的專屬助理」。`claw-init` 就是那個引導建站流程。

## 安裝

把整包丟進你專案的 skills 目錄：

```bash
git clone https://github.com/Wcc723/claude-claw-init.git
mkdir -p <你的專案>/.claude/skills
cp -R claude-claw-init <你的專案>/.claude/skills/claw-init
```

之後在那個專案裡對 Claude Code 說「**初始化龍蝦**」「**claw-init**」「**bootstrap openclaw**」其中一句即可觸發。

## 跑完會長出什麼

寫進**目前工作目錄（CWD）**，不是這個 skill 所在的 repo：

| 路徑 | 內容 |
|---|---|
| `CLAUDE.md` | 啟動流程 + 運行期互動規則 + 人格 + 記憶維護 + task 設計原則 + 頻道規則 + 安全守則 |
| `IDENTITY.md` | AI 是誰 |
| `SOUL.md` | AI 怎麼說話（語氣規則） |
| `USER.md` | 在幫誰（名字、稱呼、脈絡） |
| `MEMORY.md` | 長期記憶索引（骨架） |
| `memory/<今天>.md` | 第一篇記憶（建站紀錄） |
| `.claude/settings.json` | 資料夾範圍 + 封鎖危險指令 |
| `tasks/_TEMPLATE.md` | 排程任務範本 |
| `tasks/memory-nightly.md`、`tasks/schedule-keeper.md` | 每晚記憶整理排程（**選用**，問答決定） |
| `.claude/agents/memory-keeper.md`、`.claude/agents/researcher.md` | 兩隻基本 sub-agent |
| `.gitignore`、`artifacts/`、`logs/` | 暫存與 log 歸宿 |

## 結構

```
.
├── SKILL.md          # Skill 主體（觸發詞、問答流程、模板填值規則）
└── templates/        # 各身份檔 / agent / task 的模板（用 {{變數}} 佔位）
```

## 授權

MIT
