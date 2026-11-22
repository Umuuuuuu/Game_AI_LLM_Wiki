# A*（A-Star）尋路演算法：NPC 如何找到通往目標的路？

## 1. Graph 與尋路問題

尋路演算法的輸入是一張由位置與連接組成的 **Graph（圖）**：

- **Node（節點）**：可到達的位置，例如格子、房間或路點。
- **Edge（邊）**：節點之間的連接，例如相鄰格子或門。
- **Movement Cost（移動成本）**：沿著某條邊移動需要多少成本。

A* 不直接理解地圖是室內、戶外或一格格的畫面。它只看 Graph 提供的節點與邊，以及移動成本。

以 `5 × 5` 的方格地圖為例：

```text
S . . # .
. . . # .
. # . # .
. # . . .
. . . G .
```

- `S` 是起點。
- `G` 是目標。
- `#` 是不能走的障礙。
- `.` 是可走的格子。

若每次只允許上下左右移動，A* 可以把每個格子視為 Node，把相鄰可走格子視為 Edge。

## 2. Frontier、Reached 與 Cost

Graph Search（圖搜尋）會維護一個待探索節點集合，Red Blob Games 稱為 **Frontier（前沿）**。一般演算法還會記錄：

- 哪些節點已經到達。
- 從起點到每個節點目前已知的成本。
- 每個節點是從哪個節點來的。

資料若以 `came_from` 表示路徑，意思是：

```text
came_from[下一個節點] = 前一個節點
```

這些資料可以作為「麵包屑」路徑，用來重建起點到目標的路線。

## 3. Breadth-First Search

Breadth-First Search（BFS，廣度優先搜尋）假設每一步成本相同。它使用一般 Queue（佇列），先探索較近的節點，再探索下一層的節點。

```text
frontier = Queue()
frontier.put(start)
reached = {start}

while frontier not empty:
    current = frontier.get()

    if current == goal:
        break

    for next in neighbors(current):
        if next not in reached:
            reached.add(next)
            frontier.put(next)
```

BFS 會從起點向各方向平均擴展。若所有移動成本都相同，它可以找到最短路徑，但不一定只探索通往目標的方向。

## 4. 記錄路徑：came_from

BFS 找到目標後，若只記錄已探索節點，仍不知道原本應走哪條路。因此演算法會記錄：

```text
came_from = {}
came_from[start] = None

while frontier not empty:
    current = frontier.get()

    if current == goal:
        break

    for next in neighbors(current):
        if next not in came_from:
            came_from[next] = current
            frontier.put(next)
```

## 5. Dijkstra’s Algorithm

當不同移動方式有不同成本時，不能只數移動了幾格。

例如：

| 地形 | 移動成本 |
|---|---:|
| 草地 | 1 |
| 水地 | 10 |
| 森林 | 5 |
| 一般方格 | 1 |

對角移動也可能比上下左右移動更昂貴。

Dijkstra’s Algorithm（Dijkstra 演算法或 Uniform Cost Search）會記錄：

- `cost_so_far[node]`：目前已知的起點到節點成本。
- `came_from[node]`：目前最佳路徑的上一個節點。
- Priority Queue：每次優先取出累積成本最低的節點。

原始資料中的核心更新公式是：

```text
new_cost = cost_so_far[current] + graph_cost(current, next)
```

只有在下面任一情況發生時，才更新節點：

```text
next 不在 cost_so_far 中
或
new_cost < cost_so_far[next]
```

演算法程式碼可簡化為：

```text
frontier = PriorityQueue()
frontier.put(start, 0)
cost_so_far[start] = 0
came_from[start] = None

while frontier not empty:
    current = frontier.get()

    if current == goal:
        break

    for next in neighbors(current):
        new_cost = cost_so_far[current] + graph.cost(current, next)

        if next not in cost_so_far or new_cost < cost_so_far[next]:
            cost_so_far[next] = new_cost
            priority = new_cost
            frontier.put(next, priority)
            came_from[next] = current
```

Priority Queue 必須讓最低優先值先被取出。這可讓演算法優先探索累積成本較低的節點。

Dijkstra 演算法會考慮所有方向的實際成本，適合找出具有可變移動成本的最短路徑。

## 6. Heuristic：估計離目標有多遠

**Heuristic（啟發式）** 是一個估計目前位置與目標位置距離的函式。它不是實際移動成本，而是用來決定探索順序。

在只能上下左右移動的四方向方格中，常用 Manhattan Distance（曼哈坦距離）：

```text
h(a, b) = |a.x - b.x| + |a.y - b.y|
```

原始資料的 Python 範例為：

```python
def heuristic(a, b):
    return abs(a.x - b.x) + abs(a.y - b.y)
```

這個估計適合四方向移動的方格。若允許對角移動，原始資料沒有直接在本文中展示對角 heuristic，不能因此自行套用未確認的公式。

## 7. 為什麼需要 Greedy Best-First Search？

Greedy Best-First Search 使用估計到目標的距離決定優先順序：

```text
priority = heuristic(goal, next)
```

它會優先探索看起來接近目標的節點，因此通常比 Dijkstra 更快找到某條路徑。但原始資料指出，在複雜地圖中，Greedy Best-First Search 找到的路徑不一定是最短路徑。

因此：

- Dijkstra：保證考慮實際成本，但可能探索較多不明確方向。
- Greedy Best-First：偏向目標方向，但可能犧牲路徑品質。

## 8. A* 的運作流程

A* 結合實際成本與目標距離估計。

兩項資訊是：

- `g(n)`：從起點移動到節點 `n` 的目前累積成本。
- `h(n)`：節點 `n` 到目標的估計成本。

原始資料支持的優先順序公式為：

```text
f(n) = g(n) + h(n)
```

Priority Queue 每次取出 `f(n)` 最小的節點。

A* 的核心程式碼為：

```text
frontier = PriorityQueue()
frontier.put(start, 0)
came_from[start] = None
cost_so_far[start] = 0

while frontier not empty:
    current = frontier.get()

    if current == goal:
        break

    for next in neighbors(current):
        new_cost = cost_so_far[current] + graph.cost(current, next)

        if next not in cost_so_far or new_cost < cost_so_far[next]:
            cost_so_far[next] = new_cost
            priority = new_cost + heuristic(goal, next)
            frontier.put(next, priority)
            came_from[next] = current
```

處理邏輯可以分成四個步驟：

1. 將起點加入 Priority Queue，優先值設為 `0`。
2. 每次取出目前估計成本最低的節點。
3. 計算鄰居的新成本；若成本更低，更新其成本與 `came_from`。
4. 找到目標後停止或繼續重建路徑。

## 9. 路徑重建

找到目標後，`came_from` 已記錄每個節點是從哪裡到達。

路徑重建的方法是：

1. 從目標節點開始。
2. 尋找目標的 `came_from` 節點。
3. 一直往前找到起點。
4. 將節點加入路徑。
5. 將路徑反轉，使順序從起點到目標。

原始資料的程式碼為：

```text
current = goal
path = []

while current != start:
    path.append(current)
    current = came_from[current]

path.append(start)
path.reverse()
```

例如，在前面的網格中，若得到：

```text
G <- C <- B <- S
```

反向重建後的路徑就是：

```text
S -> B -> C -> G
```

路徑中的節點代表位置，而每一條邊代表實際移動方式。

## 10. Heuristic 何時保證最短路徑？

原始資料指出：

- Breadth-First Search 在同一步驟成本相同時保證最短路徑。
- Dijkstra 在給定圖的移動成本下保證最短路徑。
- Greedy Best-First Search 不保證最短路徑。
- A* 在 Heuristic 不高估實際距離時，可以保證最短路徑。

Heuristic 不高估真實距離的特性稱為 **Admissible（可採納）**。原始資料沒有要求讀者自行證明此特性，只需在使用啟發式時留意它是否過度樂觀或高估真實距離。

Heuristic 越小，A* 的行為會越接近 Dijkstra；Heuristic 越大，則越接近 Greedy Best-First。

## 11. A* 的適用情境與限制

原始資料建議：

- 要找多個位置或所有位置的路徑：使用 BFS 或 Dijkstra。
- 所有移動成本相同且只找一個目標：可考慮 BFS。
- 移動成本會變動：可使用 Dijkstra 或 A*。
- 找一個目標且大多數情況需要快速決策：通常優先考慮 A*。

A* 本身不是完整的遊戲移動系統。原始資料指出，它不處理：

- 協同移動。
- 移動中的障礙物。
- 地圖變化。
- 危險區域評估。
- 編隊。
- 轉彎半徑。
- 角色大小。
- 動畫。
- Path Smoothing（路徑平滑）。

因此，A* 找到的是 Graph 上的路徑；實際角色還需要另外處理移動、轉向與碰撞。

## 12. 初學者實作建議

建立第一個 A* 範例時：

1. 用二維方格表示地圖。
2. 把每個格子視為 Node。
3. 實作上下左右鄰居的 `neighbors()` 函式。
4. 設定障礙格子不能加入鄰居。
5. 設定起點、目標與移動成本。
6. 實作 Priority Queue。
7. 設定 Manhattan Distance 作為四方向方格的 Heuristic。
8. 計算並更新 `cost_so_far` 與 `came_from`。
9. 找到目標後，從 `came_from` 重建路徑。

第一版可以先讓每格成本為 `1`，再加入草地、森林或水地等不同成本。

## 參考資料

| 資料類型 | 標題／作者／發布組織 | 網址 | 本文用途 |
|---|---|---|---|
| 開放教學文章 | *Introduction to the A* Algorithm*／Amit Patel／Red Blob Games | https://www.redblobgames.com/pathfinding/a-star/introduction.html | Graph、Breadth-First Search、Dijkstra、A*、Heuristic、Priority Queue、成本公式與路徑重建 |
