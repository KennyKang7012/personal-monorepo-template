---
last_edited: 2026-11-04
---

# claw-init — Codex 版本

透過問答建立個人助理的專案指令、身分、語氣、工作區記憶、角色工作說明與選用任務規格。入口與完整輸出對照見 [SKILL.md](SKILL.md)。

## 使用

本 vault 依專案慣例將技能保留在 `.codex/skills/claw-init/`；目前主機已提供此技能。一般 Codex 環境的官方 repo 技能探索位置為 `.agents/skills/`，不要假設所有環境都會自動探索 `.codex/skills/`，也不要為此自動建立重複副本。

若技能已出現在清單中：

```text
使用 $claw-init，幫我在指定的工作目錄初始化個人助理，先檢查既有檔案再問答。
```

未被探索時直接指定：

```text
請讀取本專案 .codex/skills/claw-init/SKILL.md，依流程在我指定的目錄初始化助理。
```

已有 vault 時先規劃合併；不會把本技能資料夾當成新工作區。單純要求修改技能或分析相容性，不會觸發建站。

## 從 Claude Code 轉換的差異

| 上游做法 | 此版本 |
| --- | --- |
| `CLAUDE.md` | `AGENTS.md`，明確讀取身分與記憶檔 |
| `.claude/settings.json` | `WORKSPACE.md` 協作約定，不變更主機權限 |
| `.claude/agents/*.md` 與固定模型／工具 | `agents/*.md` 一般角色提示，依可用能力與授權決定是否委派 |
| CronCreate 與每週刪除重建管家 | 任務規格與 Codex 自動化分離，不自動註冊或刷新排程 |
| Discord 等頻道假設 | 依實際可用連接工具與授權處理 |

角色範本保留記憶整理員、研究員兩種職責。任務範本保留通用任務與選用每晚記憶整理。初始化只準備檔案；排程、頻道、常駐執行與無人值守成果需分別驗證。

## 來源與授權

改編自 [Wcc723/claude-claw-init](https://github.com/Wcc723/claude-claw-init)，基準版本 `f171f0bd896f7b340f76097f8dc53386204b57ca`。保留原始 [MIT LICENSE](LICENSE)。此目錄是本地 Codex 改編版，不再與上游逐位元相同。

查核日期：2026-11-04。設計依據：[技能格式與探索](https://learn.chatgpt.com/docs/build-skills)、[AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)、[排程任務](https://learn.chatgpt.com/docs/automations?surface=app)。
