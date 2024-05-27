---
wiki: csolua
title: 'GameSave API'
h1: ''
---

## 啟用遊戲保存功能

通過分配地圖的 gamesave 組索引，可以使用 GameSave 功能來支援地圖。
要禁用它，只需取消分配其遊戲保存組索引即可。


## 遊戲保存數據

遊戲儲存資料有 2 種類型：

1. 共用遊戲保存數據。
2. 玩家遊戲保存數據。

共用的遊戲存檔數據將共用到同一存檔遊戲組索引上的所有地圖。

例如，將映射{% mark color:blue A %}和映射{% mark color:blue B %}分配給保存遊戲組索引{% mark color:blue 1 %}。然後，腳本編寫器使用 Game.Rule.SetGameSave 保存地圖{% mark color:blue A %}上的資料。之後，保存的數據將共用到兩個地圖，並可以使用 Game.Rule.GetGameSave 進行檢索。

同時在玩家遊戲保存數據時，會保存在特定玩家身上，共用到同一保存遊戲組索引上的所有地圖。

例如，將映射{% mark color:blue A %}和映射{% mark color:blue B %}分配給保存遊戲組索引{% mark color:blue 1 %}。然後，玩家{% mark color:yellow Bob %}播放地圖{% mark color:blue A %}並使用 Game.Player.SetGameSave 保存數據。退出地圖{% mark color:blue A %}後，玩家"Bob"播放地圖{% mark color:blue B %}。然後，保存的數據將共用到兩個地圖，並可以使用玩家{% mark color:yellow Bob %}的Game.Player.GetGameSave進行檢索。

## 保存並載入

共用的遊戲保存數據會在遊戲開始時自動載入，並在遊戲結束時每 1 分鐘或結束時自動批量保存。

玩家遊戲保存數據可以自動載入，也可以在首次生成後按{% kbd L %}手動載入，具體取決於 Common.SetAutoLoad 設置。

如果玩家沒有載入數據，它將每 1 分鐘或在遊戲結束時自動保存並被新資料覆蓋！

## 普通玩家遊戲保存數據

玩家數據會自動保存和載入一些常用數據
以下是它們清單：
{% box %}
{% grid w:130px %}
<!-- cell -->
**health**
**maxhealth**
**armor**
**maxarmor**
**coin**
**items**
<!-- cell -->
目前血量
血量最大值
目前護甲
護甲最大值
Studio 金幣數量
Studio 物品背包清單
{% endgrid %}
{% endbox %}


由於上述數據是自動保存的，因此腳本編寫器無法使用 Game.Player.SetGameSave 方法覆蓋它們。但是，您可以使用 Game.Player.GetGameSave 方法檢索它們。