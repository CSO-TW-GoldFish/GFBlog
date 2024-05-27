---
wiki: csolua
title: 'API 更新日誌'
h1: ''
---

## 1.5 - 2019-11-08 21:27:50

T-Scenario主要更新

**添加**

- Common模組，在遊戲和UI腳本上運行。
- Common.WeaponOption 類，用於修改武器性能和統計資訊。
- Game.Entity 衍生遊戲實體類別的基底類別。
- Game.Monster 用於處理怪物實體的類，繼承自 Game.Entity 基類。
- Game.Weapon 類，用於處理玩家武器實體。
- Game.GetEntity 方法檢索具有給定實體索引的實體實例。
- Game.RandomInt 和 Game.RandomFloat 方法用於隨機數生成器。
- Game.KillAllMonsters 方法，殺死所有怪物。
- Game.ENTITYTYPE、Game.MONSTERTYPE、Game.RENDERFX、Game.WEAPONCOLOR Game.THIRDPERSON_FIXED_PLANE表。
- Game.Rule.OnRoundStartFinished 事件，在回合凍結時間結束時調用。
- Game.Rule.OnPlayerJoiningSpawn 事件，在玩家首次生成時調用。
- Game.Rule.OnLoadGameSave 事件，在載入/導入遊戲保存數據時調用。
- Game.Rule.OnClearGameSave 事件，在清除遊戲保存數據時調用。
- Game.Rule.OnTakeDamage 事件，在實體受到傷害時調用。
- Game.Rule.OnKilled 事件，在實體被殺死時調用。
- Game.Rule.CanBuyWeapon 事件，當玩家即將在武器商店功能表中購買武器時調用。
- Game.Rule.CanHaveWeaponInHand 事件，在玩家即將拾取武器實體時調用。
- Game.Rule.OnGetWeapon 事件，在玩家拾取武器實體時調用。
- Game.Rule.OnSwitchWeapon 事件，當玩家將當前武器切換到另一種武器時調用。
- Game.Rule.PostFireWeapon 事件，在玩家發射當前武器時調用。
- Game.Rule.OnReload 事件，在玩家重新載入武器彈藥時調用。
- Game.Rule.PostFireWeapon 事件，在玩家完成重新裝填武器彈藥時調用。
- Game.Player.gravity 字段，用於更改玩家的重力。
- Game.Player.infiniteclip 字段，用於啟用/禁用無限彈藥供應。
- 使用 Game.Player.SetFirstPersonView、Game.Player.SetThirdPersonView 和 Game.Player.SetThirdPersonFixedView 方法更改玩家的攝像機透視圖。
- Game.Player.SetThirdPersonFixedPlane 方法，用於在使用固定的第三人稱攝像機時更改滑鼠指標光線投射。
- 新的武器庫存系統，可以保存遊戲，引入Game.Player.GetWeaponInvenList，Game.Player.SetWeaponInvenLockedUI，Game.Player.ShowWeaponInven，Game.Player.ToggleWeaponInven和Game.Player.ClearWeaponInven方法。
- 使用 Game.Player.GetPrimaryWeapon 和 Game.Player.GetSecondaryWeapon 方法檢索玩家的當前武器。
- 使用 Game.Player.SetBuymenuLockedUI 方法啟動殭屍場景樣式的 UI。
- Game.Player.SetBuymenuLockedUI 方法自定義武器購物功能表外觀。
- 使用 Game.Player.Signal 方法和 UI 從伺服器發送 UI 信號。Event.OnSignal 事件。
- 新的鍵輸入事件，UI。Event.OnKeyDown 和 UI。Event.OnKeyUp。

**改變**
- 由於新添加了 Game.Entity 基類，Game.Player 的某些欄位將移動到該基類，並從該基類繼承。

## 1.4

實現了遊戲保存 API。

**添加**

- Game.Rule.CanSave 方法，用於檢查當前地圖是否支持遊戲保存。
- 使用 Game.Rule.SetGameSave 和 Game.Rule.GetGameSave 方法載入和保存共享遊戲保存數據。
- 使用 Game.Player.SetGameSave 和 Game.Player.GetGameSave 方法載入和保存玩家的遊戲保存數據。
- 使用 Game.Rule.OnReceiveGameSave 事件掛接遊戲保存載入事件。
- 使用 Game.Rule.OnGameSave 事件掛接遊戲保存事件。

## 1.3

**添加**

- 將 aksha 殭屍模型添加到 Game.MODEL 表 （Game.MODEL.AKSHA_ZOMBIE & Game.MODEL.AKSHA_ZOMBIE_HOST） 中。

**固定**

- 修復了UI中的拼寫錯誤。SyncValue.value。

## 1.2

**添加**

- 使用 Game.Rule.OnRoundStart 和 UI 挂鉤新一輪開始的事件。Event.OnRoundStart 事件。

**固定**

- 修復了更改 Game.Player.team 字段並調用 Game.Player.Win 方法後，腳本在下一輪開始時重新啟動的問題。

## 1.1 添加

- Game.SetTrigger 方法，用於（取消）啟動“設備控制腳本”塊。
- Game.GetTriggerEntity 和 Game.GetScriptCaller，用於“Function Caller”塊調用的全域函數。
- Game.WEAPONTYPE和Game.HITBOX表。
- 通過 Game.Player.team 欄位更改玩家的團隊。
- 通過 Game.Player.flinch 欄位更改玩家受到傷害時的剛度。
- 通過 Game.Player.knockback 字段更改玩家受到傷害時的擊退。
- 通過 Game.Player.model 欄位更改玩家的模型。
- 使用 Game.Rule.OnPlayerAttack 事件挂鉤玩家的承受傷害事件。
- 使用UI在UI腳本中檢索玩家的實體索引。PlayerIndex 方法。
- 通過UI檢索UI物件的配置。Box.Get 和 UI。Text.Get 方法。
- 使用 Game.GetTime 和 UI 檢索引擎時間。GetTime 方法。
- `Function Caller`塊，用於調用在遊戲腳本中聲明的全域函數。

**改變**

- 在 Game.Rule.OnPlayerKilled 事件上添加了“weapontype”和“hitbox”參數。
- 在Game.Rule.Win和Game.Player.Win方法上添加了“exit”參數，以在下一輪開始時終止遊戲。

**固定**

- 玩家可以在 Game.Rule.OnPlayerKilled 事件中使用 Game.Player.Respawn 方法以程式設計方式正確重生。

### 1.0

- 首次發佈。