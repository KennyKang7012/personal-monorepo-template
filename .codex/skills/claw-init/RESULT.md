---
last_edited: 2026-11-04
---

# Codex 轉換與驗證紀錄

原版基準已保存在 `codex/claw-init-codex` 分支的 `fc5da81`，來源為 Wcc723/claude-claw-init 的 `f171f0bd896f7b340f76097f8dc53386204b57ca`。MIT LICENSE 保持不變。

## 完成範圍

- 技能入口改成 Codex 問答流程，觸發語句集中於 description，保留本專案要求的 `last_edited`。
- `templates/CLAUDE.md` 改為 `templates/AGENTS.md`，由主指令明確讀取身分、語氣、使用者與記憶文件。
- `.claude/settings.json` 模板改為 `WORKSPACE.md` 協作約定，不產生或變更主機權限設定。
- 兩份 agent 模板改為可由主 agent 直接使用的角色工作說明；無固定 Claude 模型、工具清單或背景執行參數，不宣稱自動註冊 Codex agent。
- task 模板改成自訂任務規格，保留手動執行與另行授權的 Codex 自動化設定流程。移除每週刪除重建排程管家，區分規格、設定與實際執行證據。
- 加入 `agents/openai.yaml` UI metadata，更新包內 README 與上層技能索引，保留使用者指定的 `.codex/skills/` 路徑並說明官方 `.agents/skills/` 探索位置。

## 驗證

- `PYTHONDONTWRITEBYTECODE=1 uv run --offline --with pytest --with pyyaml pytest -q tests/test_skills.py`：3 passed。
- 通用 `quick_validate.py` 直接檢查原檔：拒絕 `last_edited`；該欄位由本專案 `.codex/AGENTS.md` 與既有測試要求，未為通用驗證器移除。
- 僅在暫存副本移除 `last_edited`，再呼叫同一通用驗證器：通過。這代表其餘通用格式通過，不代表原檔直接通過該驗證器。
- 隔離暫存目錄填值演練：未選記憶任務的 10 份、選記憶任務的 11 份模板均成功填值，YAML 可解析、無未填變數，生成後的記憶索引連結可解析。此為機械式模板演練，並非 Codex 自主建站行為驗收。
- 檢查包內 UTF-8／LF、實際文件連結、UI YAML 與模板中的 Claude 專屬執行依賴；模板內連到未來輸出位置的連結以生成後結果查核。
- `git diff --check` 檢查已追蹤變更；新檔另讀取檢查，不把此命令當成新檔的完整驗證。

## 未執行與下一步

本次僅修改技能包與索引，未在 vault 根目錄執行建站、未變更主機設定、未建立或修改真實排程、未委派 agent 或發送外部訊息。依使用者授權，轉換結果與本紀錄一併納入後續本機提交；基準 commit 不包含後續轉換，未執行 push。

下一步可在使用者指定的獨立測試目錄，以新 Codex 聊天實際走一次問答與生成，核對載入的 AGENTS 指令、保留既有檔案的行為，以及選用任務不會自動啟用。實際自動化啟用與無人值守驗收需另外授權與執行。
