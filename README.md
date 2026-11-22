# Game AI：遊戲中的 NPC 如何思考與行動？

本專案以 Game AI（遊戲人工智慧）為主題，建立 LLM Wiki 知識庫，並利用 Wiki 作為主要知識來源，生成一份可以透過瀏覽器實際瀏覽的 HTML 教學簡報。

## 1. LLM Wiki 主題與介紹

本專案探討遊戲中的 NPC（Non-Player Character，非玩家角色）如何進行行為決策、路徑搜尋與移動控制。

Wiki 涵蓋以下主題：

- Game AI 基礎概念
- FSM（Finite State Machine，有限狀態機）
- Behavior Tree（行為樹）
- A* Pathfinding（A* 尋路演算法）
- Grids and Graphs（網格與圖形）
- Steering Behaviors（移動與轉向行為）

本專案共使用五份正式原始資料來源，整理成六篇 Wiki 文章，作為後續教學簡報的知識基礎。

## 2. LLM Wiki 成果

- [原始資料與來源紀錄](sources/)
- [Wiki 知識文章](wiki_articles/)
- [Agent 專案規範](AGENTS.md)

## 3. 教學 HTML 簡報

**主題：Game AI：遊戲中的 NPC 如何思考與行動？**

本簡報共 12 頁，以繁體中文介紹 NPC 的基本概念、行為決策、尋路演算法與移動控制。

- [HTML 簡報原始檔](slides/deck.html)
- 線上瀏覽連結：待 GitHub Pages 發布後補上。

## 4. Loop / Graph 工作流程設計

本專案設計九個主要節點：

1. Wiki Retrieval：取得 Wiki 知識。
2. Evidence Cards：整理知識證據與來源。
3. Coverage Check：檢查教學主題涵蓋程度。
4. Outline：建立投影片大綱。
5. Draft：撰寫投影片內容。
6. Fidelity Check：確認內容與 Wiki 一致。
7. HTML Render：生成 HTML 投影片。
8. Render Check：檢查投影片結構與版面。
9. Final Output：輸出最終成果。

當檢查失敗時，流程會依照錯誤類型回到對應節點修正，每個檢查項目最多進行兩次修正。

詳細設計：[workflow_design.md](workflow/workflow_design.md)

## 5. 實際執行結果與檢查紀錄

### Phase 1：教學內容規劃

- 成功讀取六篇 Wiki 文章。
- 建立 19 張 Evidence Cards。
- Coverage Check 涵蓋七項必要主題。
- 產生 12 頁投影片大綱。
- 修正循環：0 次。

執行紀錄：[Phase 1](workflow/runs/phase1/)

### Phase 2：HTML 簡報生成

- 依照確認後的大綱生成 HTML。
- Fidelity Check 第一次通過。
- 產生 12 頁 HTML 投影片。
- Render Check 第一次通過。
- 修正循環：0 次。
- 實際使用瀏覽器開啟簡報確認畫面。

執行紀錄：[Phase 2](workflow/runs/phase2/)

## 6. 反思與 Workflow 調整

本專案在資料來源取得與 Wiki Retrieval 階段遇到較多問題。

例如，部分網頁雖然可以連線，卻只有前端載入框架或摘要，無法取得完整正文；另外也曾發生來源檔案名稱不一致的問題。

因此，我將來源完整性檢查提前至正式 ingest 之前，確認來源是否存在、是否能讀取完整正文，以及是否足以支持 Wiki 文章的內容。

此外，也在 `AGENTS.md` 中加入至少五份可讀取原始資料來源的規範，以及來源追溯要求。

正式執行 Phase 1 與 Phase 2 時，檢查皆在第一次通過，沒有觸發修正循環。

## 專案資料夾

```text
Game_AI_LLM_Wiki/
├── AGENTS.md
├── README.md
├── sources/
├── wiki_articles/
├── slides/
│   └── deck.html
└── workflow/
    ├── workflow_design.md
    └── runs/
        ├── phase1/
        └── phase2/
```

