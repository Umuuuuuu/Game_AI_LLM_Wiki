# AGENTS.md

## 專案目標

建立「Game AI：遊戲中的 NPC 如何思考與行動？」LLM Wiki，作為後續初學者教學 HTML 簡報的知識庫。

未來生成簡報前，Wiki 內容應能獨立閱讀、便於查找，並提供足夠的概念、來源與實作脈絡。

## 讀者與語言

- 主要讀者：有程式基礎、第一次學習 Game AI 的初學者。
- 使用繁體中文撰寫。
- 技術術語中英文並列，例如「有限狀態機（Finite State Machine，FSM）」。
- 解說應由概念到例子，由淺入深，避免未定義專有名詞。
- 程式片段應註明 Unity 版本或相關 API，若未測試實際版本則明確說明。

## 內容範圍

Wiki 應涵蓋：

- Game AI 基礎與 Sense／Think／Act 決策循環。
- 有限狀態機（Finite State Machine，FSM）。
- 行為樹（Behavior Tree）。
- 尋路與導航，例如 A*。
- 移動與轉向行為（Steering Behaviors）。
- 決策與感知系統，包括視野、記憶與黑板。
- 可執行的 Unity C# 實作範例。
- 遊戲設計與難度平衡。

除非需求明確變更，不得隨意擴張到另一個主題或跳過初學者理解所需的前置概念。

## 資料來源與引用

優先使用官方文件與權威技術資料。

可補充使用：

- 學術論文與教科書。
- 知名且內容可靠的開放教學文章與部落格。
- 具有授權或公開散布的原始碼與範例。

不得捏造來源、版本、API 行為或統計資訊。

## 原始來源與知識庫收錄規範

- 本專案至少必須收錄並使用五份可實際讀取完整正文的原始資料來源。
- 每份來源都必須有對應的來源紀錄，記錄至少包含：
  - 標題。
  - 作者或發布組織。
  - 原始 URL。
  - 原始檔案或資料的專案內位置。
  - 收錄用途，例如作為概念定義、演算法說明或實作範例依據。
- Wiki 文章中的事實性內容、公式、行為定義及技術主張都必須能追溯到已收錄的原始來源，不得虛構引用或未經證實的來源關係。
- 完整正文、資源頁、摘要及僅可取得的外部連結是不同層級的資料：
  - 完整正文是可支撐技術主張的主要來源。
  - 資源頁或摘要只能記錄為資源索引，不能僅憑摘要宣稱已讀取完整正文。
  - 若來源無法讀取、正文不完整，或僅取得摘要，不得將該來源視為已完成收錄或用於支持無法驗證的主張。
- 任何替代來源、補充來源或來源移除都應同步更新來源紀錄；不得只依據網址或檔名聲稱已取得原文。

### 目前採用的五份正式來源

1. **Bob Nystrom — Game Programming Patterns: State**
   - 原始 URL：https://gameprogrammingpatterns.com/state.html
   - 完整原始檔：[sources/game-programming-patterns-state.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/game-programming-patterns-state.html)
   - 對應來源紀錄：[sources/06_game-programming-patterns-state-source.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/06_game-programming-patterns-state-source.md)
   - 收錄用途：FSM 定義、enum 與 switch 實作、State Pattern、階層式與堆疊式狀態機，以及 FSM 的適用限制。

2. **Epic Games — Behavior Tree in Unreal Engine: Overview**
   - 原始 URL：https://dev.epicgames.com/documentation/en-us/unreal-engine/behavior-tree-in-unreal-engine---overview
   - 完整原始檔：[sources/epic-behavior-tree-overview.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/epic-behavior-tree-overview.html)
   - 對應來源紀錄：[sources/02_epic-behavior-tree-source.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/02_epic-behavior-tree-source.md)
   - 收錄用途：Behavior Tree、Blackboard、Decorators、Composites、Tasks、Services、Event-Driven 行為及 Unreal Engine 並行節點。

3. **Red Blob Games — Introduction to the A* Algorithm**
   - 原始 URL：https://www.redblobgames.com/pathfinding/a-star/introduction.html
   - 完整原始檔：[sources/redblob-a-star-introduction.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/redblob-a-star-introduction.html)
   - 對應來源紀錄：[sources/03_redblob-a-star-source.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/03_redblob-a-star-source.md)
   - 收錄用途：Graph、Breadth-First Search、Dijkstra、A*、Heuristic、Priority Queue、成本公式與路徑重建。

4. **Red Blob Games — Grids and Graphs**
   - 原始 URL：https://www.redblobgames.com/pathfinding/grids/graphs.html
   - 完整原始檔：[sources/redblob-grids-graphs.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/redblob-grids-graphs.html)
   - 對應來源紀錄：[sources/04_redblob-grids-graphs-source.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/04_redblob-grids-graphs-source.md)
   - 收錄用途：Grid 與 Graph 的表示、Node／Edge／Neighbor、Directed／Undirected／Weighted Graph，以及障礙物處理。

5. **Craig W. Reynolds — Steering Behaviors For Autonomous Characters**
   - 原始資源頁 URL：https://www.red3d.com/cwr/steer/
   - 完整論文 HTML URL：https://www.red3d.com/cwr/steer/gdc99/
   - 完整論文 PDF URL：https://www.red3d.com/cwr/papers/1999/gdc99steer.pdf
   - 資源頁原始檔：[sources/craig-reynolds-steering-behaviors.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/craig-reynolds-steering-behaviors.html)
   - 完整論文 HTML：[sources/craig-reynolds-gdc99steer.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/craig-reynolds-gdc99steer.html)
   - 完整論文 PDF：[sources/craig-reynolds-gdc99steer.pdf](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/craig-reynolds-gdc99steer.pdf)
   - 對應來源紀錄：[sources/05_craig-reynolds-steering-source.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/05_craig-reynolds-steering-source.md)
   - 收錄用途：三層行為架構、簡單車輛模型、個體與群體 Steering Behaviors，以及行為組合方式。
   - 資料區分：`craig-reynolds-steering-behaviors.html` 是論文相關資源頁索引；只有 `craig-reynolds-gdc99steer.html` 或 `craig-reynolds-gdc99steer.pdf` 可作為完整論文正文依據。不得將資源頁內容當作完整論文正文。

每篇 Markdown 文章只需在文末列出「參考資料」表，不要求逐段尾註。

參考資料至少包含：

- 標題。
- 作者或發布組織。
- 發布或更新日期（有提供時）。
- 完整連結。
- 本文用途簡述（例如概念定義、API 說明或範例參考）。

官方來源與第三方教學來源發生衝突時，優先官方或學術來源，並應在文章中清楚說明差異。

## Markdown 撰寫規範

- 文章存放於 `wiki_articles/`。
- Markdown 檔名使用數字主題編號與中文名稱，例如 `01_Game_AI_基礎概念.md`。
- 每篇應包含標題、引言、條列式說明、適當的圖表或文字示意、總結，以及參考資料。
- 使用標準 Markdown，不使用未提供渲染支援的特殊語法。
- 程式碼區塊必須標註語言，例如 ` ```csharp `。
- 圖片若需要暫存，存放於 `sources/` 或適當的既有資料夾，不得自行建立未經確認的新目錄。
- 所有內部連結應指向專案內實際存在的檔案。

## Unity C# 範例規範

- 範例主要使用 Unity 與 C#。
- 優先提供可直接理解的最小完整範例，不先假設讀者熟悉大型專案架構。
- 範例應說明需要的 Unity 物件、Component、欄位與執行步驟。
- 每個範例至少說明：
  - 目的。
  - 前置設定。
  - 核心邏輯。
  - 預期行為。
  - 常見錯誤或限制。
- 修改既有範例時，應保留原本行為並避免引入未說明的套件或依賴。
- 未經實際執行驗證的範例不得聲稱「可執行」或「已測試」。

## 內容檢查規則

生成或修改文章後，至少檢查：

- 詞彙與中英文術語是否一致。
- 官方文件、學術資料或教學來源是否支持文中的主張。
- C# 與 Unity 程式碼的語法、邏輯、前置設定及預期結果是否一致。
- 範例是否符合初學者難度。
- Markdown 標題層級、表格、程式碼區塊與連結是否有效。
- 參考資料是否完整且可開啟。
- 是否需要明確標註假設、限制或版本差異。

人工審閱是最終定稿前的必要步驟；AI 檔案檢查不能替代人工確認。

## 未來 HTML 簡報需求

簡報尚未指定框架，生成前應比較適合初學者教學的框架與呈現方式。

簡報密度依主題難度彈性安排，不預設固定張數。

簡報應：

- 每一張只聚焦一個核心概念。
- 先有文字解釋，再有圖示或程式片段。
- 保持與 Wiki 術語、結構與來源一致。
- 使用繁體中文並保留英文術語。
- 支援鍵盤操作、可讀的字型與對比，以及合理的手機或投影片縮放。

## 資料夾用途

- `wiki_articles/`：正式 Wiki Markdown 文章。
- `sources/`：來源資料、下載的參考檔案或經確認可保存的附加素材。
- `slides/`：未來 HTML 簡報及其資源。
- `workflow/`：生成、檢查與審閱流程相關資料。

## 工作方式

建立或修改內容前，應先確認需求與本規範的交集。

除非使用者明確要求，內容工作只能涵蓋 AGENTS.md 或當前指定的文章，不得修改、刪除或建立無關檔案。
