# Evidence Cards

## EC-01：Game AI 的定義與目的

- **概念名稱：** Game AI（遊戲人工智慧）
- **解釋：** Game AI 控制遊戲中的角色、敵人或其他物件，使其能依環境與條件選擇行動。它不一定追求像人一樣聰明，主要目的是增加挑戰性、真實感、遊戲體驗與難度控制。
- **對應 Wiki：** [01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md)
- **原始來源：** 目前這篇基礎文章引用的外部來源及原始內容未列入目前正式五份來源之一；本張卡僅用於投影片的基礎概述，後續 Draft 不應將其視為新形式的證據來源。
- **重要術語：** Game AI、NPC、Challenge、Realism

## EC-02：Sense / Think / Act

- **概念名稱：** Sense / Think / Act
- **解釋：** AI 分別取得環境資訊、判斷應採取的行動，以及執行決策；環境變化後可進入下一輪。
- **對應 Wiki：** [01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md)
- **原始來源：** 尚未由正式五份原始來源逐段證實；僅作基礎流程說明，後續不得新增未支持的實作細節。
- **重要術語：** Sense（感知）、Think（決策）、Act（行動）

## EC-03：FSM 核心結構

- **概念名稱：** Finite State Machine（FSM，有限狀態機）
- **解釋：** FSM 有固定狀態、目前單一狀態、輸入或事件，以及每個狀態的轉換。若目前狀態沒有定義對應轉換，輸入可被忽略。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- **原始來源：** [Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)，完整原始檔：[game-programming-patterns-state.html](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/sources/game-programming-patterns-state.html)
- **重要術語：** State、Input、Event、Transition

## EC-04：FSM NPC 狀態例子

- **概念名稱：** Patrol / Chase / Attack
- **解釋：** NPC 可把巡邏、追逐與攻擊拆成不同狀態；發現玩家、進入攻擊範圍或失去目標會觸發轉換。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- **原始來源：** Game Programming Patterns 的 FSM 狀態與轉換原理；巡邏、追逐、攻擊是 Wiki 依此原理建立的教學例子，不視為原始資料逐字案例。
- **重要術語：** Patrol（巡邏）、Chase（追逐）、Attack（攻擊）

## EC-05：Enum 與 Switch FSM

- **概念名稱：** Enum／Switch 實作
- **解釋：** 最簡單的 FSM 可用一個 enum 表示狀態，並在 switch 中處理目前狀態的輸入與轉換。對較小問題是簡單有效的方式。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- **原始來源：** [Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)
- **重要術語：** Enum、Switch、State-dependent behavior

## EC-06：State Pattern 與 Entry／Exit Action

- **概念名稱：** State Pattern（狀態模式）
- **解釋：** 每個狀態可由獨立物件處理輸入與更新，主物件委派到目前狀態物件。Entry Action 在進入時執行，Exit Action 在離開時執行。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- **原始來源：** [Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)
- **重要術語：** State Pattern、Entry Action、Exit Action、Delegation

## EC-07：FSM 限制與進階結構

- **概念名稱：** FSM 的組合爆炸與進階結構
- **解釋：** 独立能力組合會使單一 FSM 狀態數量快速增加；可使用並行狀態機、階層式狀態機或堆疊式狀態機處理特定需求。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)
- **原始來源：** [Bob Nystrom — State](https://gameprogrammingpatterns.com/state.html)
- **重要術語：** Concurrent FSM、Hierarchical State Machine、Pushdown Automaton

## EC-08：Behavior Tree 基礎

- **概念名稱：** Behavior Tree（行為樹）
- **解釋：** 行為樹以視覺節點樹組織 NPC 汳策與動作；可依條件選擇分支並執行對應 Task。
- **對應 Wiki：** [03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)
- **原始來源：** [Epic Games — Behavior Tree Overview](https://dev.epicgames.com/documentation/en-us/unreal-engine/behavior-tree-in-unreal-engine---overview)
- **重要術語：** Behavior Tree、Composite、Decorator、Task

## EC-09：Blackboard

- **概念名稱：** Blackboard（黑板）
- **解釋：** Blackboard 是獨立資產，儲存 Behavior Tree 做決策所需資訊；欄位稱為 Blackboard Keys。
- **對應 Wiki：** [03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)
- **原始來源：** Epic Games 官方文件
- **重要術語：** Blackboard、Blackboard Key

## EC-10：Event-Driven Behavior Tree

- **概念名稱：** Event-Driven（事件驅動）
- **解釋：** Unreal Engine 的行為樹不是持續每幀重複檢查整棵樹，而是等待相關事件並更新 Blackboard 或改變執行分支。
- **對應 Wiki：** [03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)
- **原始來源：** Epic Games 官方文件
- **重要術語：** Event-Driven、Observer Abort、Service

## EC-11：Graph 基礎

- **概念名稱：** Graph（圖）
- **解釋：** 圖由節點集合與節點間的邊集合組成。尋路演算法只需知道位置、連接及移動成本，不必知道圖的版面外觀。
- **對應 Wiki：** [05_Grids_and_Graphs_網格與圖形.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/05_Grids_and_Graphs_網格與圖形.md)
- **原始來源：** [Red Blob Games — Grids and Graphs](https://www.redblobgames.com/pathfinding/grids/graphs.html)
- **重要術語：** Node、Edge、Vertex、Weighted Directed Graph

## EC-12：Grid 轉換成 Graph

- **概念名稱：** Grid 轉成 Graph
- **解釋：** 可走格子是節點，相鄰可走格子間是邊；四方向或八方向決定鄰居集合。障礙可移除節點、移除邊或設為無限權重。
- **對應 Wiki：** [05_Grids_and_Graphs_網格與圖形.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/05_Grids_and_Graphs_網格與圖形.md)
- **原始來源：** Red Blob Games — Grids and Graphs
- **重要術語：** Neighbors、Four-way movement、Eight-way movement

## EC-13：Breadth-First、Dijkstra 與 A*

- **概念名稱：** 圖搜尋演算法演進
- **解釋：** BFS 假設每步成本相同；Dijkstra 考慮不同移動成本；A* 結合已知成本與目標方向估計。
- **對應 Wiki：** [04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md)
- **原始來源：** [Red Blob Games — Introduction to the A* Algorithm](https://www.redblobgames.com/pathfinding/a-star/introduction.html)
- **重要術語：** Breadth-First Search、Dijkstra’s Algorithm、A*、Priority Queue

## EC-14：A* 公式與路徑重建

- **概念名稱：** A* 優先值與 `came_from`
- **解釋：** 優先值為 `f(n) = g(n) + h(n)`；`g(n)` 是起點累積成本，`h(n)` 是估計到目標距離。找到目標後以 `came_from` 從目標往回重建再反轉路徑。
- **對應 Wiki：** [04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md)
- **原始來源：** Red Blob Games — Introduction to the A* Algorithm
- **重要術語：** Heuristic、`cost_so_far`、`came_from`、Manhattan Distance

## EC-15：Steering 三層架構

- **概念名稱：** Action Selection / Steering / Locomotion
- **解釋：** 高層選擇目標，中層把目標拆成轉向行為，低層把控制訊號轉成角色實際移動。
- **對應 Wiki：** [06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- **原始來源：** [Craig W. Reynolds — Steering Behaviors For Autonomous Characters](https://www.red3d.com/cwr/steer/gdc99/)
- **重要術語：** Action Selection、Steering、Locomotion

## EC-16：Seek、Flee、Pursuit、Evasion、Arrival

- **概念名稱：** 個體目標導向行為
- **解釋：** Seek 朝固定目標、Flee 遠離固定目標、Pursuit 預測移動目標、Evasion 遠離預測位置、Arrival 接近後減速停止。
- **對應 Wiki：** [06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- **原始來源：** Craig Reynolds GDC 1999 論文完整 HTML／PDF
- **重要術語：** Seek、Flee、Pursuit、Evasion、Arrival

## EC-17：Wander、Obstacle Avoidance、Path Following

- **概念名稱：** 環境導向與路徑行為
- **解釋：** Wander 持續自然遊走；Obstacle Avoidance 警覺前方威脅；Path Following 預測位置並偏向預定路徑中心線。
- **對應 Wiki：** [06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- **原始來源：** Craig Reynolds GDC 1999 論文完整 HTML／PDF
- **重要術語：** Wander、Obstacle Avoidance、Path Following、Spine、Radius

## EC-18：Flocking 與行為組合

- **概念名稱：** 群體行為與混合
- **解釋：** Flocking 由 Separation、Cohesion、Alignment 組成。複雜角色可離散切換或混合多種轉向，也可使用優先順序。
- **對應 Wiki：** [06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- **原始來源：** Craig Reynolds GDC 1999 論文完整 HTML／PDF
- **重要術語：** Separation、Cohesion、Alignment、Flocking、Prioritized Behavior

## EC-19：方法用途與差異

- **概念名稱：** FSM、Behavior Tree、A*、Steering Behaviors 的分工
- **解釋：** FSM 管理明確狀態切換；Behavior Tree組織條件與動作；A* 在圖上找到路徑；Steering 將目標或路徑轉為即時移動與轉向。
- **對應 Wiki：** [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md)、[03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md)、[04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md)、[06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md)
- **原始來源：** 四份正式來源與對應 Wiki；此張卡是教學整合，不加入來源未明示的新比較數據。
- **重要術語：** State、Decision Tree、Pathfinding、Steering
