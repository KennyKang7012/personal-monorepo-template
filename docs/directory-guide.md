---
last_edited: 2026-10-31
---

# Personal Codex Vault 目錄用途指南

本文件說明 vault 各目錄的用途與文件放置原則，供跨專案查閱。它本身屬於 `docs/` 的共用參考文件；個別專案的狀態與決策仍記錄在該專案內。

## 依據與適用範圍

- 原作者 `jxnl` 的初始 commit `54d66b6`（Initial personal monorepo template），根 README 的 Structure 段落，是下列九個目錄原始定位的來源。
- 根 AGENTS.md 補充持久文件的放置規則；本機 README 另列出 tests/ 的用途。
- 以下本機狀態於 2026-10-31 唯讀確認。目錄存在不代表其中已有實際內容或可執行功能。
- 「適合內容」是依原定位整理的實務例子，不代表原作者逐項指定了這些檔名或細分類。

## 原作者規劃的完整目錄

| 目錄 | 原始定位 | 適合內容與放置原則 | 本機狀態 |
| --- | --- | --- | --- |
| `projects/` | 長期工作（long-lived work） | 持續維護的專案入口、目標、狀態與操作說明。若程式碼在別處，可只記錄原位置與分工，不複製一套程式 | 已存在 |
| `experiments/` | 短期探索或實驗（short-lived spikes） | 可收斂、驗收或結束的調查、概念驗證及技能測試。範本命名為 `exp-<topic>-YYYY-MM-DD`；使用者指定路徑時依其要求，並記錄例外 | 已存在 |
| `archive/` | 暫停或完成的工作 | 保存已退出日常工作範圍的專案或實驗，保留結果、原位置與狀態；搬移或封存既有工作前仍須依授權執行 | 已存在 |
| `docs/` | 整個 repository 共用的參考文件（repo-level references） | 共用目錄指南、跨專案操作說明、規範解釋及參考索引。個別專案的進度與專屬計畫仍放在對應專案或實驗內 | 已存在；本文件建立前僅有 `.gitkeep` |
| `outputs/` | 不是正式依據的生成產物（generated artifacts that are not source of truth） | 可重新產生的匯出檔、呈現版本與預覽。例如由正式資料生成的 HTML 或 PDF。不能只在這裡保存唯一決策或唯一來源 | 已存在 |
| `people/` | 關於人與 Agent 的持久協作筆記 | 公開安全的角色、合作偏好與交接方式；沿用人物範本，不保存憑證或私人個資 | 已存在 |
| `.codex/` | repo-local skills、插件 metadata 與資產 | 可重用的 Agent 流程及其必要資源。Skills 入口為 `.codex/skills/<skill-name>/SKILL.md`，在 repo 內使用不等於安裝到全域環境 | 已存在 |
| `templates/` | 新工作使用的起始範本 | 新專案、實驗及 Agent 指引的可重用骨架；實際工作填寫在各自目錄，不把專案進度寫回範本 | 已存在 |
| `scripts/` | 輔助腳本（helper scripts） | repository 共用的可執行輔助程式。只服務單一 skill 的腳本應隨該 skill 保存，依最接近用途的本機規則維護 | 根目錄目前不存在；此為原作者預留定位，未因建立本指南而新增 |

原作者對 `docs/` 的原文是：`docs/ holds repo-level references.` 原始 Structure 沒有再細分 docs 子目錄，也沒有提供內容範例。

## 原始清單以外的本機目錄

| 目錄 | 用途 | 來源與狀態 |
| --- | --- | --- |
| `tests/` | 驗證範本及 repo 規則是否正常，例如 Markdown metadata、skill 結構與輔助腳本行為 | 現行根 README 的 Structure 列有此目錄，本機已存在；不是上述原始九項之一 |
| `.github/` | GitHub repository 的自動化設定；目前有模板封裝工作流程 `.github/workflows/package.yml` | 本機已存在，依實際檔案確認；不是工作計畫或一般參考文件的保存區，本次未執行或修改其工作流程 |

`.git/` 是 Git 自身的版本管理資料，不是工作文件的保存位置，不應手動放入專案資料。`.gitkeep` 則是讓 Git 保留空目錄的佔位檔，不是功能設定。

## 持久文件的放置方式

以下依根 AGENTS.md 的 Durable State 規則整理：

| 內容 | 保存位置 |
| --- | --- |
| 專案目前狀態、操作入口與近期下一步 | 對應專案的 README.md |
| 長期目標 | 對應工作的 GOAL.md |
| 已完成工作、驗證結果與限制 | 對應工作的 RESULT.md |
| 人與 Agent 的協作脈絡 | `people/*.md` |
| 跨專案發現與導覽 | repo 層級文件，例如根 README.md；較完整的共用參考說明可放 `docs/` |
| Agent 必須遵守的本機工作規則 | 最接近工作目錄的 AGENTS.md，與根規則共同適用 |

根 README 適合作為導覽；本指南負責完整解釋目錄，不需要把相同內容再複製到 README。

## 以目前玉山競賽為例

- [專案入口](../projects/2026-esun-ai-challenge/README.md)：保存目標、Claude Code／Codex 分工、程式與資料路徑、既有排程現況。
- [獨立技能驗證計畫](../experiments/esun-skill-validation/README.md)：保存短期無人看守驗證的安排、隔離界線及驗收條件。該路徑經使用者同意，沿用現有入口。
- [下載 skill](../.codex/skills/esun-datapack-guarded-download/SKILL.md) 與 [截圖 skill](../.codex/skills/esun-screenshot-archive/SKILL.md)：保存可重複使用的流程，不因維護 skills 就改動原 Project 排程。
- 本目錄指南放在 `docs/`，因為它適用整個 vault，不只適用競賽專案。
- 正式程式碼與資料依使用者安排維持原位；本指南不授權搬移、複製或刪除既有內容。

## 使用界線

目錄分類不等於操作授權。新增排程、改動原 Project 設定、外部帳號操作、搬移重要資料或公開發布，仍依使用者當次授權及相關 AGENTS.md 執行。敏感資料不得因為目錄是本機路徑就寫入文件或 Git。

本次僅建立此指南，沒有改寫 README.md、搬動既有資料或變更兩個原排程。
