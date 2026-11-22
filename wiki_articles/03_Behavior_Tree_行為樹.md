# Behavior Tree（行為樹）：如何組織 NPC 的決策？

## 1. 什麼是 Behavior Tree？

Behavior Tree（行為樹，BT）是一種以節點樹組織 NPC 決策與行動的方法。

Epic Games 的 Unreal Engine 文件指出，可以用 Blueprint Visual Scripting 指示角色「做某件事」，例如播放動畫、移動到特定位置或對受擊做出反應。但當希望 AI 角色自行思考並做決策時，就可以使用 Behavior Tree。

例如，一個守衛 NPC 可以依環境資訊選擇巡邏或追逐玩家。

- 如果沒有發現玩家，執行巡邏。
- 如果玩家在視野內，執行追逐。
- 如果玩家離開視野，停止追逐並恢復巡邏。

行為樹不是隨機選擇動作的模型；它會依節點的條件、順序與目前資訊決定要執行哪個分支。

## 2. Behavior Graph 與節點

在 Unreal Engine 中，Behavior Tree 是以視覺方式建立的，流程與 Blueprint 類似：在 Behavior Tree Graph 中加入節點並連接節點，使節點形成決策樹。

Epic 文件將主要節點類型分為：

| 節點類型 | 功能 |
|---|---|
| Composite（複合節點） | 組織與控制子節點的執行方式 |
| Decorator（裝飾節點） | 檢查條件，決定是否允許某個分支執行 |
| Task（工作節點） | 表示 NPC 可以執行的實際動作 |
| Service（服務節點） | 在位於某個 Composite 子樹時定期更新資訊 |

在 Unreal Engine 文件的示例中：

- 藍色節點是 Decorator。
- Decorator 會連接到 Composite 節點。
- 它會驗證 Blackboard Key 是否符合條件。
- 紫色節點是 Task，代表實際可以執行的動作，例如追逐玩家。

## 3. Blackboard：行為樹需要的資訊

Blackboard 是與 Behavior Tree 分開的資產，用來儲存 Behavior Tree 做決策時需要的資訊。資訊會儲存在稱為 **Blackboard Keys** 的欄位中。

典型流程是：

1. 建立 Blackboard。
2. 在 Blackboard 中新增所需的 Blackboard Keys。
3. 建立 Behavior Tree。
4. 指派 Blackboard 資產給 Behavior Tree。

例如，NPC 可以在 Blackboard 儲存：

- `HasLineOfSight`：是否看得見玩家。
- `TargetLocation`：玩家目前位置。
- `CanAttack`：是否可發動攻擊。

`HasLineOfSight` 只是概念性欄位名稱，實際欄位名稱與型別可依 Unreal Engine 專案設計。

Behavior Tree負責組織節點與決策邏輯；Blackboard負責提供節點需要共享的資料。兩者分工後，節點不必直接取得所有遊戲物件資料，也能透過 Blackboard Keys 判斷是否執行分支。

## 4. Unreal Engine 的執行順序

Unreal Engine 的 Behavior Tree 會由左至右、由上至下執行。

節點圖中的右上角會顯示數字化執行順序。開發者可以透過這個順序理解哪個分支先執行，以及目前執行位置。

例如，當左側分支的 Decorator 發現 `HasLineOfSight` 為真時，該分支下的 Task 可以開始追逐玩家。

簡化流程如下：

```text
開始
  |
  +-- Decorator: HasLineOfSight?
        |
        +-- 是 -> Task: Chase Player
        |
        +-- 否 -> 嘗試其他分支或恢復巡邏
```

此流程只用來說明節點條件與動作的關係，不代表 Unreal Engine 的所有節點都必定以相同方式回傳結果。

## 5. 行為樹與 Pawn、AI Controller

Unreal Engine 文件將角色執行體與控制邏輯區分為：

- **Pawn**：可由 AI 控制的角色或其他實體；它是 NPC 的「身體」。
- **AI Controller**：接管 Pawn 並指揮 Pawn 執行動作。

建立完整 AI 時，通常流程是：

1. 建立 Blackboard。
2. 建立 Behavior Tree。
3. 建立或指定 AI Controller。
4. 在 Pawn 上指定對應的 AI Controller。
5. 當 AI Controller 接管 Pawn 時，執行 Behavior Tree。

若 Pawn 需要在環境中自動尋路，需將 `NavMeshBoundsVolume` 加入關卡。

`NavMeshBoundsVolume` 是 Unreal Engine 特有的地圖導航設定；本文章只依官方文件介紹其用途，不展開其他未提供的操作細節。

## 6. Unreal Engine 與傳統行為樹的差異

### 6.1 Event-Driven（事件驅動）

Unreal Engine 的 Behavior Tree 是事件驅動（Event-Driven），而不是持續每一幀重新檢查整棵樹是否發生變化。

Behavior Tree 會被動等待事件；當相關資料變更時，再更新 Blackboard 或觸發分支。

例如，當視野事件將 `HasLineOfSight` 更新後，行為樹可以中止低優先級 Task，改為執行更左側、優先度更高的追逐分支。

這種做法有助於：

- 減少每一幀都執行整棵樹的工作。
- 更容易除錯，因為歷史紀錄主要記錄實際變化，而不是重複的相同步驟。
- 只需關注執行位置或 Blackboard 數值的變化。

### 6.2 Decorator 不作為 Leaf Node

在標準行為樹模型中，條件可能是 Leaf Node；這些條件節點只負責成功或失敗。

Unreal Engine 建議把條件放在 Decorator，而不是把條件做成 Task Leaf Node。

這有兩個主要好處：

- 行為樹介面較直觀，能清楚看見分支何時被關閉。
- 所有 Leaf Node 都是會執行動作的 Task，較容易辨認實際動作。

Decorator 也可以作為觀察者（Observer），監聽 Blackboard 或關鍵節點的變化。這是發揮 Unreal Engine 事件驅動優勢的重要機制。

## 7. 並行行為

傳統行為樹常使用 Parallel（並行）節點，同時執行所有子樹，並依預先規則處理子樹結束的情況。

Epic 文件特別說明：Parallel 不一定代表多執行緒。它可能只是概念上在同一幀或相同執行緒內執行多項工作。

Unreal Engine 使用以下方式處理類似需求：

- Simple Parallel
- Service
- Decorator 上的 Observer Abort

### 7.1 Simple Parallel

Simple Parallel 只有兩個子節點：

- 一個必須是單一 Task；可附加 Decorator。
- 另一個可以是完整子樹。

它可以表示：「執行 A 的同時，執行 B」。

例如：

```text
Simple Parallel
  主任務：Attack Enemy
  次要工作：Move Toward Enemy
```

這裡的一個 Task 是主要任務，另一個子樹則是在主要任務等待完成期間持續執行的次要工作。這種方式比完整 Parallel 節點更容易理解並進行事件驅動最佳化。

### 7.2 Service

Service 是附加在 Composite 節點上的特殊節點，例如：

- Selector
- Sequence
- Simple Parallel

Service 可以設定每隔指定秒數執行回呼，更新需要定期取得的資訊。

例如，AI 正追逐目前敵人時，Service 可以定期判斷哪個敵人最適合追逐；當目前仍位於該 Composite 子樹內，Service 就持續運作。

### 7.3 Observer Abort

Observer Abort 用來在 Decorator 監測到條件失效時中止目前工作。

Epic 文件以貓與老鼠為例：

- Sequence 包含「發出警告」與「撲向老鼠」。
- Sequence 上的 Decorator 檔查「老鼠是否可以被撲到」。
- 老鼠逃入洞口時，Decorator 發現條件不再成立。
- 設定 `Observer Aborts = Self` 後，中止目前 Sequence。

這種機制可取代傳統並行節點持續檢查條件並中止工作的需求。

## 8. Unreal Engine 行為樹的優勢

Epic 文件指出，這套並行處理方式有以下優勢：

- **Clarity（清晰度）**：使用 Services 與 Simple Parallel 可以建立較簡單、較容易閱讀的樹。
- **Ease of Debugging（容易除錯）**：較少同時執行的路徑，能更明確看出目前執行的內容。
- **Easier Optimization（較容易最佳化）**：事件驅動樹不會大量重複執行子樹，也比較容易最佳化。

這些優勢取決於行為樹是否依 Unreal Engine 的事件驅動模式設計，而不表示所有行為樹都天然具有相同效能。

## 9. 初學者設計建議

建立第一棵 Behavior Tree 時，可以依序進行：

1. 確認需要共用哪些資訊，建立 Blackboard 與 Blackboard Keys。
2. 列出 NPC 可以執行的動作，整理為 Task。
3. 找出哪些條件決定分支是否執行，使用 Decorator 表示。
4. 使用 Composite 組織子樹與節點順序。
5. 將流程由左至右、由上至下連接。
6. 使用 Service 定期更新需要同步的資訊。
7. 在條件失效時，透過 Observer Abort 中止不再需要的工作。

開始時不要一次建立所有節點。先建立「巡邏與追逐玩家」等最小範例，再逐步加入攻擊、離開視野或回到巡邏的分支。

## 10. Behavior Tree 與 FSM 的差異

本篇不依其他未列入原始文件的主張直接比較兩者。從 Epic 文件可確認：

- Behavior Tree 會以視覺節點樹組織條件與動作。
- Blackboard 儲存決策所需資料。
- Decorator 表示分支條件。
- Task 表示實際動作。
- Unreal Engine 行為樹採事件驅動執行。

Finite State Machine 則可由已 ingest 的 FSM 文章進一步比較。這份 Wiki 應在個別文章中維持各自來源依據，避免用未驗證內容混合比較。

## 參考資料

| 資料類型 | 標題／作者／發布組織 | 網址 | 本文用途 |
|---|---|---|---|
| 官方技術文件 | *Behavior Tree Overview*／Epic Games | https://dev.epicgames.com/documentation/en-us/unreal-engine/behavior-tree-in-unreal-engine---overview | Behavior Tree 基礎、Blackboard、節點類型、執行方式、事件驅動、Simple Parallel、Service 與 Observer Abort |
