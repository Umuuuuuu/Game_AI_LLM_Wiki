# Coverage Check

## 結果

**通過：** 7 項必要主題均已由 Wiki 擷取並找到 Evidence Card。

## 涵蓋矩陣

| 必要主題 | 涵蓋狀態 | Evidence Cards | Wiki 依據 |
|---|---|---|---|
| Game AI 基本概念 | 通過 | EC-01、EC-02 | [01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md) |
| FSM | 通過 | EC-03 至 EC-07 | [02_FSM_有限狀態機.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/02_FSM_有限狀態機.md) |
| Behavior Tree | 通過 | EC-08 至 EC-10 | [03_Behavior_Tree_行為樹.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/03_Behavior_Tree_行為樹.md) |
| Grids and Graphs | 通過 | EC-11、EC-12 | [05_Grids_and_Graphs_網格與圖形.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/05_Grids_and_Graphs_網格與圖形.md) |
| A* Pathfinding | 通過 | EC-13、EC-14 | [04_A_Star_A星尋路演算法.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/04_A_Star_A星尋路演算法.md) |
| Steering Behaviors | 通過 | EC-15 至 EC-18 | [06_Steering_Behaviors_移動與轉向行為.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/06_Steering_Behaviors_移動與轉向行為.md) |
| 各方法用途與差異 | 通過 | EC-19 | 五篇主題 Wiki；以文章明示內容為基礎 |

## 教學順序檢查

建議順序為：

1. Game AI 基礎與 Sense／Think／Act。
2. FSM：最直觀的決策狀態管理。
3. Behavior Tree：以條件與動作組織更複雜決策。
4. Graph 與 Grid：理解尋路資料結構。
5. A*：從地圖圖形找到路徑。
6. Steering Behaviors：沿目標或路徑即時移動。
7. 方法比較與一個 NPC 完整例子。

此順序符合投影片初學者的由概念到執行脈絡。

## 缺口檢查

沒有技術缺口。以下兩項不列為資料缺口：

- [01_Game_AI_基礎概念.md](/Users/chung/Documents/vscode/Game_AI_LLM_Wiki/wiki_articles/01_Game_AI_基礎概念.md) 的 Sense／Think／Act 使用既有 Wiki，但它的基礎來源未納入目前五份正式來源。
- 各方法的詳細比較不使用未經正式來源支持的效能數據或實際引擎基準。

## 修正情況

- 未發現重大缺口，不需回到 Wiki Retrieval 或 Evidence Cards。
- 未發現本次需要的修正。
- 修正次數：0。
