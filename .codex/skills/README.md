---
last_edited: 2026-10-31
---

# 技能包索引

本專案目前包含 **18 個技能包**。技能包是給 Codex 使用的工作說明書，定義適用情境、處理步驟與驗證方式；每包的入口都是 `SKILL.md`。

這些技能放在專案內直接使用，不代表已安裝到全域環境。點選下表的技能名稱，可以查看完整說明。

## 功能與使用範例

下表範例是可輸入給 Codex 的提示詞，不是終端機指令。

| 類別 | 技能包 | 功能 | 使用範例 |
| --- | --- | --- | --- |
| 玉山競賽 | [esun-datapack-guarded-download](esun-datapack-guarded-download/SKILL.md) | 依官方交易日下載 DataPack，避免重複下載或覆寫，核對 JSON 與來源檔案。 | 使用 esun-datapack-guarded-download，依官方顯示日期下載並驗證 DataPack。 |
| 玉山競賽 | [esun-screenshot-archive](esun-screenshot-archive/SKILL.md) | 備存儀表板頁面、日期／組別／分頁與完整歷史回看，產生索引及驗證紀錄。 | 使用 esun-screenshot-archive，依當次授權範圍截取並備存晚間儀表板資訊。 |
| 助理 | [onboarding](onboarding/SKILL.md) | 首次設定工作助理，了解專案、優先順序、合作對象、連接服務與追蹤需求。 | 使用 onboarding，幫我完成這個 vault 的初始設定。 |
| 助理 | [assistant](assistant/SKILL.md) | 日常工作支援：整理待辦、準備會議、追蹤承諾與撰寫回覆草稿。 | 使用 assistant，幫我整理今天需要注意的工作與待回覆事項。 |
| 助理 | [write-like-me-bootstrap](write-like-me-bootstrap/SKILL.md) | 分析本人撰寫的 Slack、Email，建立個人寫作風格技能。 | 使用 write-like-me-bootstrap，分析我的寄件內容，先提供風格預覽。 |
| 自動追蹤 | [loop](loop/SKILL.md) | 在目前聊天建立定期檢查，設定檢查頻率與停止條件。 | 使用 loop，每 30 分鐘檢查這個 PR 的 CI，完成或失敗時通知我。 |
| 長期目標 | [ultragoal](ultragoal/SKILL.md) | 設計或啟動持續執行的目標，定義完成條件、驗證方法與進度紀錄。 | 使用 ultragoal，先設計這個重構工作的目標與驗收條件，暫不啟動。 |
| 任務交接 | [task-handoff](task-handoff/SKILL.md) | 產生八段交接文件，或在新 Session 核對檔案、Git 狀態、驗證紀錄與授權後接手。 | 使用 task-handoff，將目前任務存成專案的 Handoff.md，並附上新 Session 提示詞。 |
| 建立資料 | [new-project](new-project/SKILL.md) | 建立長期專案或短期實驗目錄，產生 README 與相關專案指引。 | 使用 new-project，建立讀書筆記工具專案，先準備文件骨架。 |
| 建立資料 | [new-person](new-person/SKILL.md) | 依範本建立或更新合作對象筆記，保存角色與合作偏好等非敏感資訊。 | 使用 new-person，建立 Alex 的合作筆記，角色是前端工程師。 |
| 程式審查 | [audit-ai-code](audit-ai-code/SKILL.md) | 檢查一般程式或後端程式的重複邏輯、過度設計、錯誤處理、測試與安全問題。 | 使用 audit-ai-code，審查目前 diff，只列問題，先不要修改。 |
| 前端審查 | [audit-ai-frontend](audit-ai-frontend/SKILL.md) | 檢查前端程式與畫面，包括元件拆分、重複樣式、手機版顯示、無障礙與設計一致性。 | 使用 audit-ai-frontend，檢查這個頁面的手機版與元件重用問題。 |
| 文字審查 | [audit-ai-writing](audit-ai-writing/SKILL.md) | 檢查空泛內容、過重的 AI 語氣、錯誤引用、格式與殘留標記。 | 使用 audit-ai-writing，檢查這篇 README 的贅詞與引用問題。 |
| HTML 產出 | [simple-html-artifact](simple-html-artifact/SKILL.md) | 製作容易閱讀的單一 HTML 檔案，適合報告、比較表、說明頁與資訊儀表板。 | 使用 simple-html-artifact，把這份研究整理成單檔 HTML 報告。 |
| Git 提交 | [gh-commit](gh-commit/SKILL.md) | 檢查目前變更，依修改目的拆成清楚、範圍合理的 commits。 | 使用 gh-commit，把目前變更依功能與文件拆開提交，不要 push。 |
| PR 審查處理 | [gh-address-comments](gh-address-comments/SKILL.md) | 讀取目前分支 PR 的審查留言，整理待處理項目，再修正指定內容。 | 使用 gh-address-comments，先列出這個 PR 尚待處理的留言。 |
| CI 除錯 | [gh-fix-ci](gh-fix-ci/SKILL.md) | 讀取 GitHub Actions 失敗紀錄，分析原因並提出修復計畫，確認後修改。 | 使用 gh-fix-ci，分析這個 PR 的失敗檢查，先給我修復計畫。 |
| 提交與開 PR | [yeet](yeet/SKILL.md) | 一次完成暫存、commit、push，以及建立草稿 PR。 | 使用 yeet，將本次變更提交、push，並建立草稿 PR。 |

## 怎麼提出需求

說明「技能名稱、處理範圍、預期結果」即可。例如：

```text
使用 audit-ai-code，審查 projects/example-project/ 的 Python 程式，
只分析、不修改，列出最重要的 5 個問題。
```

如果技能尚未出現在目前環境的技能清單，可以明確指定專案內的說明檔：

```text
請先讀取 .codex/skills/audit-ai-code/SKILL.md，
依照其中流程審查目前 diff，只分析、不修改。
```

## 使用前要知道

- 任務交接：`task-handoff` 會先區分 vault 入口與外部實作、資料位置，沿用既有交接檔名，引用 README、RESULT 等正式紀錄；不會因為交接而搬移程式或資料。
- 外部服務：Email、Slack、GitHub 等相關流程需要可用的連接工具與帳號權限；GitHub 技能可能需要已登入的 `gh` 或 `oai_gh`。
- 定期檢查與長期目標：`loop` 是隔一段時間再檢查；`ultragoal` 是持續推進到符合驗收條件。實際執行仍需環境提供相應工具。
- 個人寫作風格：`write-like-me-bootstrap` 用來建立風格技能，預設產出 `.codex/skills/write-like-me/`。目前索引中的 18 包尚不包含這個產出；寫入前會先提供預覽並取得確認，不保存原始私人訊息。
- Git 與公開操作：`gh-commit` 處理本機提交；`yeet` 包含 push 與建立 PR，只有明確授權整套流程時才使用。其他技能不會因為被選用，就自動取得 push、merge 或部署授權。
- 隱私：公開範本中不要放入私人身分、財務、帳號專屬資料或憑證；個人專用內容應另行在自己的私有環境管理。

本索引整理各技能文件定義的用途，不代表已逐一驗證外部連線、自動化或完整執行流程。
