# 投影片內容大綱

主題：Game AI：遊戲中的 NPC 如何思考與行動？

共 12 頁，繁體中文，面向有程式基礎但第一次學習 Game AI 的讀者。

## 第 1 頁：NPC 真的會「思考」嗎？

- 教學重點：
  - Game AI 是讓角色依環境與條件選擇行動的技術。
  - NPC 的巡邏、追逐與攻擊可由規則與演算法組織。
  - 本簡報會依核心技術拆解 NPC 行為。
- 預計例子或圖示：
  - 守衛 NPC 在巡邏、發現玩家、追逐與攻擊間切換。
- Wiki 文章：[01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md)
- Evidence Card：EC-01
- 正式來源：五份正式來源清單；本頁不引入新數據。

## 第 2 頁：Sense / Think / Act 決策循環

- 教學重點：
  - Sense：取得玩家位置、距離或視野。
  - Think：依目前資料判斷行動。
  - Act：巡邏、追逐、攻擊或逃跑。
- 預計例子或圖示：
  - 三階段循環圖，箭頭回到 Sense。
- Wiki 文章：[01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md)
- Evidence Card：EC-02
- 正式來源：基礎文章；不使用未核實來源增加技術主張。

## 第 3 頁：用 FSM 管理明確行為狀態

- 教學重點：
  - FSM 有固定 State、單一目前 State、Input／Event 與 Transition。
  - 一個 NPC 在同一時間只處於一個狀態。
  - 狀態對應巡邏、追逐、攻擊等行為。
- 預計例子或圖示：
  - Patrol → Chase → Attack 狀態圖。
- Wiki 文章：[02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- Evidence Card：EC-03、EC-04
- 正式來源：[Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)

## 第 4 頁：FSM 的簡單實作與進階封裝

- 教學重點：
  - enum／switch 可集中處理目前狀態。
  - 旗標組合會產生無效組合與維護成本。
  - State Pattern、Entry Action 與 Exit Action 分別負責物件封裝與進出動作。
- 預計例子或圖示：
  - Flags vs. State 切換示意；PatrolState／ChaseState／AttackState 結構圖。
- Wiki 文章：[02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- Evidence Card：EC-05、EC-06
- 正式來源：[Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)

## 第 5 頁：FSM 的限制與替代方向

- 教學重點：
  - 能力組合會造成狀態數量快速增加。
  - 可考慮 Concurrent、Hierarchical 或 Pushdown FSM。
  - 行為複雜時可評估 Behavior Tree 或 Planning System。
- 預計例子或圖示：
  - n × m 狀態組合與分拆成兩台 FSM 的比較圖。
- Wiki 文章：[02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- Evidence Card：EC-07
- 正式來源：[Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)

## 第 6 頁：用 Behavior Tree 組織決策

- 教學重點：
  - Behavior Tree 由視覺節點形成決策樹。
  - Composite 組織分支，Decorator 檢查條件，Task 執行動作。
  - 左至右、由上至下的基本執行脈絡。
- 預計例子或圖示：
  - 有視野時追逐，無視野時巡邏的簡單節點樹。
- Wiki 文章：[03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)
- Evidence Card：EC-08
- 正式來源：[Epic Games — Behavior Tree Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/behavior-tree-in-unreal-engine---overview)

## 第 7 頁：Blackboard 與 Unreal Engine 執行架構

- 教學重點：
  - Blackboard 保存 Behavior Tree 使用的共享資訊。
  - Pawn 是角色或實體，AI Controller 指揮 Pawn 執行 Behaviour Tree。
  - Event-Driven 只在相關事件變化時更新分支。
- 預計例子或圖示：
  - AI Controller → Behavior Tree → Blackboard → Task 的架構圖。
- Wiki 文章：[03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)
- Evidence Card：EC-09、EC-10
- 正式來源：[Epic Games — Behavior Tree Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/behavior-tree-in-unreal-engine---overview)

## 第 8 頁：從格線地圖到 Graph

- 教學重點：
  - Node 是位置，Edge 是連接，Weight 是移動成本。
  - 格線的可走格子可轉成節點。
  - 四方向或八方向決定鄰居。
- 預計例子或圖示：
  - 3×3 地圖、障礙格與相鄰節點連線圖。
- Wiki 文章：[05_Grids_and_Graphs_網格與圖形.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/05_Grids_and_Graphs_網格與圖形.md)
- Evidence Card：EC-11、EC-12
- 正式來源：[Red Blob Games — Grids and Graphs](https://www.redblobgames.com/pathfinding/grids/graphs.html)

## 第 9 頁：Graph 的邊與障礙物

- 教學重點：
  - Undirected、Directed、Multigraph 與 Weighted Graph 的差異。
  - 障礙物可移除節點、移除邊或使用 Infinity 權重。
- 預計例子或圖示：
  - 單向門、道路成本與障礙格示意。
- Wiki 文章：[05_Grids_and_Graphs_網格與圖形.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/05_Grids_and_Graphs_網格與圖形.md)
- Evidence Card：EC-11、EC-12
- 正式來源：[Red Blob Games — Grids and Graphs](https://www.redblobgames.com/pathfinding/grids/graphs.html)

## 第 10 頁：A* 如何找到路徑？

- 教學重點：
  - BFS、Dijkstra 與 A* 的角色差異。
  - Priority Queue 優先處理低成本節點。
  - A* 優先值是 `f(n) = g(n) + h(n)`。
- 預計例子或圖示：
  - 5×5 網格、起點、目標與障礙；三種演算法探索範圍比較。
- Wiki 文章：[04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md)
- Evidence Card：EC-13、EC-14
- 正式來源：[Red Blob Games — Introduction to the A* Algorithm](https://www.redblobgames.com/pathfinding/a-star/introduction.html)

## 第 11 頁：路徑重建與 Steering 的銜接

- 教學重點：
  - A* 使用 `came_from` 記錄上一個節點。
  - 從目標往回追蹤，再反轉為起點到目標的路徑。
  - A* 找路徑，Steering 負責角色沿路轉向與避開即時障礙。
- 預計例子或圖示：
  - 麵包屑箭頭圖；路徑點轉成角色移動向量。
- Wiki 文章：[04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md)、[06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- Evidence Card：EC-14、EC-15
- 正式來源：[Red Blob Games — A*](https://www.redblobgames.com/pathfinding/a-star/introduction.html)、[Craig Reynolds GDC 1999 論文](https://www.red3d.com/cwr/steer/gdc99/)

## 第 12 頁：Steering Behaviors 與完整 NPC

- 教學重點：
  - Seek、Flee、Pursuit、Evasion、Arrival。
  - Wander、Obstacle Avoidance、Path Following。
  - Flocking 的 Separation、Cohesion、Alignment。
  - 整合一個巡邏、追逐、避障與回到路線的 NPC 例子。
- 預計例子或圖示：
  - 多種行為切換或混合流程圖；一張 NPC 行為決策圖。
- Wiki 文章：[06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- Evidence Card：EC-16 至 EC-19
- 正式來源：[Craig Reynolds GDC 1999 論文](https://www.red3d.com/cwr/steer/gdc99/)
