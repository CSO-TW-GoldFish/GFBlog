---
wiki: csolua
title: '介紹'
references: 
    - '[CSO LUA API](https://cso.dn.nexoncdn.co.kr/vxlman/api/index.html)'
---

## Modules 模組

- Common
    - 該模組包含用於在{% mark color:red 客戶端 %}和{% mark color:red 伺服器 %}上一起運行的腳本的功能。
    - 應該安裝在「 project.json 」的{% mark color:blue game %}和{% mark color:blue ui %}陣列中。
- Game
    - 此模組包含僅在{% mark color:red 伺服器 %}上運行的腳本的功能。
    - 應安裝在「 project.json 」的{% mark color:blue game %}陣列中。
- UI
    - 此模組包含僅在{% mark color:red 用戶端 %}上運行的腳本的功能。
    - 應安裝在「 project.json 」的{% mark color:blue ui %}陣列中。

## Classes 類別

### Common

- Common.WeaponOption
    - 通用武器選項類。
    - 這將影響所有具有給定武器ID的生成武器。
    - 例如，如果您修改了 AK-47 的 WeaponOption，遊戲中使用的所有 AK-47 都將反映修改後的統計數據。

### Game

{% grid %}
<!-- cell -->
- Game.Entity
- Game.EntityBlock
- Game.Rule
- Game.Monster
- Game.Player
- Game.SyncValue
- Game.Weapon
<!-- cell -->
遊戲實體的基礎類
Studio 裝置方塊類
遊戲規則類
怪物類
玩家類
數據同步類
武器類
{% endgrid %}

### UI

{% grid %}
<!-- cell -->
- UI.SyncValue
- UI.Box
- UI.Text
- UI.Event
<!-- cell -->
數據同步類
矩形UI類
事件回調
文字UI類
{% endgrid %}

## Topics 主題

- 更新日誌
- 遊戲存檔

## Examples 範例

- scenariot_common.lua
- scenariot_game.lua
- scenariot_ui.lua
- tdm_game.lua
- tdm_ui.lua
