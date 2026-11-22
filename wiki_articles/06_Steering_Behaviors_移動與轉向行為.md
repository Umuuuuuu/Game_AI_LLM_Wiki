# Steering Behaviors（移動與轉向行為）：NPC 如何即時移動？

## 1. 什麼是 Steering Behavior？

Steering Behavior（轉向行為）是讓自主角色（Autonomous Character）在世界中以自然、即興方式移動的方法。

論文中的 Steering 位於三層行為架構的中間：

```text
Action Selection（目標與策略）
        ↓
Steering（分解目標、產生轉向）
        ↓
Locomotion（把控制訊號轉成身體移動）
```

- **Action Selection**：決定現在要做什麼，例如追捕逃跑的 NPC。
- **Steering**：把目標分解成簡單子目標，例如接近目標、閃避障礙。
- **Locomotion**：根據轉向控制，實際移動角色。

這種分層讓 Steering Behavior 可以不綁定在特定移動方式。無論角色是雙腳、人、車、飛行器或船隻，只要 Locomotion 能接收相同形式的轉向控制，目標與轉向邏輯仍可重用。

## 2. Steering 與 Path-Finding 的差異

論文明確將 Path-Finding 與 Steering 作為相關但不同的問題：

- **Path-Finding**：在節點或格線網路上尋找路線，例如 A* 或 Dijkstra。
- **Steering**：沿著路線或直接朝目標移動，並即時處理轉彎、障礙與群體互動。

可用開車作比喻：

- 路徑演算法像是提供「應從哪條道路前往目的地」的書面指示。
- Steering 類似實際開車時轉方向盤、加速、煞車及閃避障礙。

對 NPC 而言，一條路徑可以作為 Steering 的目標規格，但 Steering 還需要處理：

- 角色的速度與最大轉彎能力。
- 移動中的障礙物。
- 角色之間的碰撞。
- 是否保持在路徑附近。
- 是否減速並抵達目標。

## 3. 簡單車輛模型

論文使用一個簡化的 Point-Mass Vehicle（質點車輛）模型，讓 Steering 的說明有共同基礎。

主要參數包括：

- `position`：位置向量。
- `velocity`：速度向量。
- `max_force`：最大轉向力。
- `max_speed`：最高速度。
- `orientation`：角色的前方、側向與上方方向。
- `mass`：質量，用於計算加速度。

論文中的更新流程為：

```text
steering_force = truncate(steering_direction, max_force)
acceleration = steering_force / mass
velocity = truncate(velocity + acceleration, max_speed)
position = position + velocity
```

其中特定的轉向力會受到 `max_force` 限制，新速度會受到 `max_speed` 限制。

這個模型假設角色的速度方向與角色朝向對齊，因此無法模擬打滑、旋轉或側滑等所有現實效果。遊戲中可使用較真實或較受限的 Locomotion 模型替代。

## 4. Seek：朝固定目標移動

**Seek** 是朝指定位置移動的基本轉向行為。它會讓角色的速度方向指向目標。

論文把目標方向視為 Desired Velocity（期望速度）：

```text
desired_velocity = normalize(position_of_target - position_of_character) * max_speed
steering = desired_velocity - current_velocity
```

NPC 例子：

- 敵人朝玩家的目前位置移動。
- 機器人朝地圖上的充電點移動。
- 角色朝路徑上的下一個位置移動。

Seek 與重力等吸引力不同：重力可能讓物體繞目標運轉，而純粹 Seek 通常會直接對準目標方向。

如果角色一直使用 Seek，可能越過目標，再轉回來接近目標，類似飛蛾繞燈。

## 5. Flee：遠離固定位置

**Flee** 是 Seek 的反向行為：期望速度指向遠離目標的方向。

NPC 例子：

- 角色被爆炸中心嚇到後遠離爆炸位置。
- 敵人離開危險區域。
- 動物逃離固定威脅位置。

論文特別區分 Flee 與 Obstacle Avoidance：

- Flee 會持續遠離指定位置，即使角色與牆壁平行也可能轉離牆壁。
- Obstacle Avoidance 只有在障礙物位於角色前方且構成威脅時才會修正方向。

## 6. Pursuit：追逐移動目標

**Pursuit** 與 Seek 相似，但目標本身會移動。

為了追上移動目標，角色需要預測目標未來位置，而不是只朝目前位置 Seek。

論文提出一種簡單預測方式：

```text
future_position = target_position + target_velocity * prediction_time
```

預測時間 `T` 可以簡單估計為：

```text
T = distance / turning_parameter
```

論文建議：

- 角色距離目標較遠時，較大的 `T` 可以較有效。
- 角色接近目標時，較小的 `T` 更合適。
- 較複雜的方法也可考慮追逐者與目標的相對朝向。

之後，Pursuit 對預測位置套用 Seek。

NPC 例子：

- 追逐正在移動的玩家。
- 導彈預測飛機未來位置。
- 守衛追捕逃跑的敵人。

## 7. Evasion：躲避移動目標

**Evasion** 是 Pursuit 的相反行為：

- Pursuit 預測目標未來位置後使用 Seek。
- Evasion 預測目標未來位置後使用 Flee。

論文提到，Evasion 可以刻意保持不完全最優或不可預測，以避免追逐者只要預測出固定逃跑方向就成功攔截。

NPC 例子：

- 逃跑角色在時間夠用時預測追逐者可能到達的位置。
- 輕量、自動規劃逃離正在接近的敵人。

## 8. Arrival：接近後減速並停止

**Arrival** 在角色距離目標較遠時與 Seek 相同，但接近目標時會逐漸減速，最後停在目標位置。

原始論文公式為：

```text
target_offset = target - position
distance = length(target_offset)
ramped_speed = max_speed * (distance / slowing_distance)
clipped_speed = minimum(ramped_speed, max_speed)
desired_velocity = (clipped_speed / distance) * target_offset
steering = desired_velocity - velocity
```

其中：

- `slowing_distance`：開始減速的距離參數。
- `ramped_speed`：依距離縮減的期望速度。
- `clipped_speed`：不超過最大速度的期望速度。

NPC 例子：

- 車輛開到停車位後自然停止。
- 守衛走到巡邏點後停下。
- 遊戲角色走向互動點並停在指定位置。

## 9. Obstacle Avoidance：避開前方障礙

**Obstacle Avoidance** 讓角色在擁擠環境中繞開障礙物。

論文將角色與障礙物簡化為 Sphere（球形），並在角色前方建立一個預測障礙區域。障礙物是否構成威脅取決於：

- 障礙物是否在角色前方。
- 障礙物是否與角色前方的預測區域相交。
- 障礙物是否在角色可考慮的預測距離內。

如無威脅，返回 Zero 或 Null Steering，表示目前不需修正方向。

論文指出，Obstacle Avoidance 通常只關心位於角色與目標之間的障礙物。例如飛機不需要避開機場後方的山，只需要避開兩者之間的山。

NPC 例子：

- NPC 朝玩家移動時避開牆角與大型障礙。
- 車輛沿直線行進，只有前方有障礙時才修正方向。

## 10. Wander：自然持續遊走

**Wander** 是持續產生不規則方向的隨機轉向。

論文不建議每幀直接產生完全隨機的轉向，因為這會造成 Twitchy（抖動）且不連續的動作。

較好的方式是保留上一幀轉向方向，再加入小幅隨機位移，讓轉向逐步變化。

論文提到另一種實作方式是：

- 在角色前方放置一個球面。
- 每幀對球面上的轉向目標加入隨機位移。
- 將結果限制在球面上。
- 大圓半徑決定 Wander 強度。
- 小圓位移決定 Wander 速度或變化率。

Perlin Noise 也是論文提到的替代方向產生方式。

NPC 例子：

- 野外動物在區域內持續遊走。
- 未發現玩家時，守衛在巡邏區域內自然移動。

## 11. Path Following：沿預定路徑移動

**Path Following** 讓角色沿著預定路徑移動，例如道路、走廊或隧道。

論文將路徑理想化為：

- **Spine**：路徑中心線，可由樣條曲線或折線表示。
- **Radius**：角色允許偏離中心線的半徑。

與嚴格沿鐵軌移動不同，Path Following 允許角色在路徑半徑內自由偏離。

核心流程：

1. 根據角色速度預測未來位置。
2. 找到未來位置在中心線上的最近投影點。
3. 若投影距離小於路徑半徑，表示角色正沿路徑移動，不需修正。
4. 若偏離過遠，使用 Seek 朝投影點或調整後的目標移動。

NPC 例子：

- 守衛沿走廊巡邏。
- 車輛沿道路中心線行駛，但可輕微偏向不同車道位置。
- NPC沿預先規劃路線移動。

## 12. Flocking：群體行為

論文的 Flocking（群體移動）由以下三種基本群體行為組成：

### Separation（分離）

讓角色與附近角色保持適當距離，避免擁擠。

論文對每個鄰近角色計算排斥力，再把所有排斥力相加。

### Cohesion（凝聚）

讓角色靠向附近角色形成群體。

可用方法：

- 計算附近角色的平均位置。
- 朝平均位置產生轉向。
- 或將平均位置作為 Seek 的目標。

### Alignment（對齊）

讓角色與附近角色調整相近的方向或速度。

論文會：

- 找出鄰居速度或前方方向。
- 計算平均速度或平均朝向。
- 用平均結果減去角色目前速度或朝向，得到轉向。

Flocking 將 Separation、Cohesion 和 Alignment 結合，形成鳥群、魚群、獸群或隊伍的群體移動。

論文指出，鄰居範圍由：

- **Distance**：兩個角色多近算鄰居。
- **Angle**：角色的視野角度。

決定。

## 13. 行為組合與優先順序

複雜 NPC 很少只執行一種 Steering Behavior。

論文將組合分成兩種：

### 離散切換

當情況變化時，切換到另一種行為模式。例如大群鹿原本在吃草，發現狼靠近後切換為逃跑。

這種切換發生在較高層的 Action Selection。

### 混合（Blending）

同時使用多種轉向，例如角色逃離狼群時混合：

- Evasion：遠離狼。
- Obstacle Avoidance：避開樹木。

論文列出三種主要組合方式：

- 線性相加：計算各行為的轉向向量並加總，可使用權重。
- 計算載入與動量：每幀只選一個行為計算，利用動量低通濾波來融合。
- 優先順序：先檢查最高優先行為是否產生非零轉向，再依序嘗試低優先行為。

例如優先順序可設為：

```text
1. Obstacle Avoidance
2. Evasion
3. Path Following
4. Wander
```

這些方法是論文提到的實作選項，不代表所有遊戲都應使用相同權重或優先順序。

## 14. 初學者 NPC 移動例子

可以把一個 NPC 的移動拆成：

```text
離 NPC 太遠且尚無威脅 -> Wander
發現玩家 -> Pursuit
玩家太靠近或需逃離 -> Evasion
前方有障礙 -> Obstacle Avoidance
需回到巡邏路線 -> Path Following
到達巡邏點 -> Arrival
```

實際使用時仍需要由上層決策選擇或混合行為。例如高優先的障礙避免可在追逐時仍然作用。

## 15. 初學者設計建議

實作第一個 Steering Behavior 時：

1. 建立簡單的 2D 位置與速度向量。
2. 設定 `max_speed` 與 `max_force`。
3. 先實作 Seek。
4. 再加入 Arrival，確認角色可減速並停止。
5. 加入 Flee 與 Obstacle Avoidance。
6. 當目標移動時改用 Pursuit／Evasion。
7. 當需要沿路線移動時加入 Path Following。
8. 最後再加入 Separation、Cohesion 與 Alignment 等群體行為。

不要一開始就混合所有行為。先確認每個行為單獨產生的轉向，再測試優先順序或權重組合。

## 參考資料

| 資料類型 | 標題／作者／發布組織 | 網址 | 本文用途 |
|---|---|---|---|
| GDC 1999 原始論文 | *Steering Behaviors For Autonomous Characters*／Craig W. Reynolds／Sony Computer Entertainment America | https://www.red3d.com/cwr/steer/gdc99/ | 三層行為架構、車輛模型、個體與群體 Steering Behaviors、行為組合與結論 |
| PDF 論文 | *Steering Behaviors For Autonomous Characters*／Craig W. Reynolds | https://www.red3d.com/cwr/papers/1999/gdc99steer.pdf | GDC 1999 論文原始 PDF |
