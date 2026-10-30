---
last_edited: 2026-10-31
---

# Personal Codex Vault（個人 Codex 知識庫）

這是一個入門工作區，讓 Codex 能持續查閱專案、人物、技能、初始設定、定期檢查，以及寫作風格等背景資訊。

官方來源儲存庫：`jxnl/personal-monorepo-template`。

這份範本提供一個集中保存資訊的地方，讓 Codex 在行動前查閱背景，並在取得你的同意後記錄重要資訊：

- `projects/`：進行中的專案
- `experiments/`：短期探索與概念驗證
- `people/`：合作對象與代理程式（agent）
- `.codex/skills/`：此儲存庫專用的技能（skill）

這個儲存庫就是 Assistant 的共用記憶庫（vault）。初始設定應直接更新這個儲存庫；除非你明確指定其他位置，否則不應在裡面再建立一層 `vault/`，也不應另外建立 `~/vault`。

## 快速開始

按下 `Cmd+Cmd` 開啟 Codex，然後輸入：

```text
請使用 jxnl/personal-monorepo-template，幫我在 ~/vault 完成設定。
```

Codex 應複製這份範本、建立以 `~/vault` 為根目錄的 Codex 專案、開始初始設定，並將該儲存庫作為記憶庫。

## 手動設定

如果你想自行建立記憶庫：

```sh
cd ~
git clone https://github.com/jxnl/personal-monorepo-template.git vault
cd vault
rm -rf .git
git init
```

完成後，記憶庫位於：

```text
~/vault
```

這個儲存庫本身就是記憶庫，請勿在裡面再建立第二個 `vault/` 目錄。

## 設定 Codex

1. 開啟 Codex。
2. 安裝你實際會使用的外掛程式（plugin）。
3. 建立以 `~/vault` 為根目錄的新 Codex 專案。
4. 在該專案中建立新聊天。
5. 輸入：

```text
$onboard me
```

## 優先安裝的外掛程式

請在初始設定前安裝外掛程式，讓 Assistant 能讀取相關背景資訊。

優先從你平常工作的工具開始：

- Assistant
- Gmail 或 Outlook Email
- Google Calendar 或 Outlook Calendar
- Google Drive、Notion、Documents、Spreadsheets、Presentations 或 PDF
- Slack 或 Teams
- GitHub、Linear 或 Notion，用於追蹤專案
- Browser
- Chrome
- Computer Use

若工作流程需要使用 Chrome，請安裝 Chrome 外掛程式與 [Codex Chrome 擴充功能](https://chromewebstore.google.com/detail/codex/hehggadaopoacecdllhhajmbjkdcmajg)。

## 初始設定會做什麼

初始設定（onboarding）應在執行每項檢查前，先說明要檢查的內容。

它應該：

- 讀取工作區內容。
- 詢問有哪些專案，以及哪些事情最重要。
- 詢問 Codex 應該認識哪些人。
- 檢查是否缺少有用的外掛程式。
- 提議在聊天中建立自動化，執行定期檢查。
- 視需要提議設定每日動態、人物與專案追蹤，預設依你的時區在上午 9:00 與下午 4:00 進行檢查。
- 提議根據你已傳送的 Slack 訊息與電子郵件，建立 `write-like-me` 技能。
- 提議以這個儲存庫作為記憶庫，完成共用記憶設定。
- 掃描已連接的 Slack、電子郵件、行事曆、文件、專案追蹤工具與 GitHub 背景資訊後，主動提出建立或更新 `people/*.md`、專案背景資料及 `AGENTS.md` 的建議。

Assistant 在傳送訊息、變更會議、編輯共用文件、建立自動化、建立或釘選聊天、重新命名聊天、設定聊天定期循環檢查、安裝外掛程式，或寫入共用記憶前，都應先徵求你的同意。

## 技能

此儲存庫專用的技能位於 `.codex/skills/`。

以下是實用的入門技能：

- `onboarding`：首次設定。
- `assistant`：初始設定完成後，持續協助日常工作。
- `loop`：在聊天中執行定期檢查。
- `new-project`：建立專案或實驗。
- `new-person`：建立人物筆記。
- `write-like-me-bootstrap`：根據 Slack 訊息與電子郵件，建立個人寫作風格技能。

## 目錄結構

- `projects/`：長期專案。
- `experiments/`：短期探索與概念驗證。
- `people/`：人物或代理程式的筆記。
- `.codex/`：技能、外掛程式中繼資料與素材。
- `templates/`：入門範本。
- `tests/`：範本完整性檢查。
