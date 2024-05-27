---
menu_id: blog
title: CSO LUA 小技巧
date: 2024-05-04 06:24:22
categories: CSO LUA
tags: [CSO LUA, CSO LUA小技巧]
---
## 玩家座標的使用

這可能是你的寫法，看起來沒毛病，但這會讓程式碼很攏長。

```lua=
x = {}
y = {}
z = {}

function Game.Rule:OnPlayerConnect(player)
   x[player.index] = player.position.x
   y[player.index] = player.position.y
   z[player.index] = player.position.z
end
```

這是比較理想的寫法，一個變數直接儲存XYZ。

```lua=
playerPosition = {}

function Game.Rule:OnPlayerConnect(player)
    playerPosition[player.index] = player.position
end
```

舉個例子，像是KZ的存讀點功能。

```lua=
playerPosition = {}

function Game.Rule:OnPlayerSignal(player, signal)
    if signal == 1 then
        -- 當玩家按下1，儲存座標
        playerPosition[player.index] = player.position
    
    elseif signal == 2 then
        -- 當玩家按下2，讀取座標
        player.position = playerPosition[player.index]
    end
end
```

## 大量且重複性高的EntityBlock創建

這我很常用在KZ裡面的跳板，10個關卡，每一關可能有10個板子，那就要創建100個座標和函式。

```lua=
playerPosition = {}

block_1 = Game.EntityBlock.Create({x = 1, y = 1, z = 1})
function block_1:OnTouch(player)
     playerPosition[player.index] = player.position
end

block_2 = Game.EntityBlock.Create({x = 2, y = 2, z = 2})
function block_2:OnTouch(player)
     playerPosition[player.index] = player.position
end

block_3 = Game.EntityBlock.Create({x = 3, y = 3, z = 3})
function block_3:OnTouch(player)
     playerPosition[player.index] = player.position
end

...無限循環
```

只要把座標全部都彙整進一個變數，利用迴圈來跑就不用再寫無意義的函式。

```lua=
blocks = {
    {x = 1, y = 1, z = 1},
    {x = 2, y = 2, z = 2},
    {x = 3, y = 3, z = 3}
}

for k,v in pairs(blocks) do
    local block = Game.EntityBlock.Create(v)
    function block:OnTouch(player)
         playerPosition[player.index] = player.position
    end
end
```

## 動態玩家排名系統

直接利用Lua內建函式`table.sort`幫我們處立即可。

```lua
-- list: 欲排序的表(table)
-- comp: 自定義排序方式(function)
table.sort(list [, comp])
```

```lua=
playerRank = {
    {player = player, time = 15}, -- 玩家一
    {player = player, time = 17}, -- 玩家二
    {player = player, time = 30}, -- 玩家三
    {player = player, time = 10}, -- 玩家四
    {player = player, time = 13}, -- 玩家五
}

-- comp: 自訂義排序，將表裡面的time屬性進行比較，時間快的排越前面。
function sort(a, b)
    return a.time < b.time
end

-- 執行排序功能
table.sort(playerRank, sort)

--[[
結果:
    playerRank = {玩家四, 玩家五, 玩家一, 玩家二, 玩家三}
]]
```

## 多個玩家的SyncValue優化

想要把Game多個玩家的資料傳送到UI，你可能會這麼做。
這也沒毛病，但如果寫到24人，實在是太攏長了。

```lua=
-- game.lua
InitialLevel = 1

sync_PlayerLevel_1 = Game.SyncValue.Create("sync_PlayerLevel.1")
sync_PlayerLevel_2 = Game.SyncValue.Create("sync_PlayerLevel.2")
sync_PlayerLevel_3 = Game.SyncValue.Create("sync_PlayerLevel.3")

function Game.Rule:OnPlayerConnect(player)
    if player.index == 1 then
        sync_PlayerLevel_1.value = InitialLevel
    elseif player.index == 2 then
        sync_PlayerLevel_2.value = InitialLevel
    elseif player.index == 3 then
        sync_PlayerLevel_3.value = InitialLevel
    end
end
```

```lua=
-- ui.lua
sync_PlayerLevel_1 = UI.SyncValue.Create("sync_PlayerLevel.1")
function sync_PlayerLevel_1:OnSync()
   -- do something 
end
sync_PlayerLevel_2 = UI.SyncValue.Create("sync_PlayerLevel.2")
function sync_PlayerLevel_2:OnSync()
   -- do something 
end
sync_PlayerLevel_3 = UI.SyncValue.Create("sync_PlayerLevel.3")
function sync_PlayerLevel_3:OnSync()
   -- do something 
end
```

只要改成這樣，程式碼就會變得簡潔、易懂。

```lua=
-- game.lua
InitialLevel = 1
sync_PlayerLevel = {}

for i = 1, 24 do
    sync_PlayerLevel[i] = Game.SyncValue.Create("sync_PlayerLevel." .. i)
end

function Game.Rule:OnPlayerConnect(player)
    sync_PlayerLevel[player.index].value = InitialLevel
end
```

```lua=
-- ui.lua
sync_PlayerLevel = UI.SyncValue.Create("sync_PlayerLevel." .. UI.PlayerIndex())
function sync_PlayerLevel:OnSync()
   -- do something 
end
```

## 數字轉貨幣格式

> [來源](https://gist.github.com/Ivaar/a95d9e565bd9a0e2036179c0c8cdee58#file-currencies-lua-L18)
```lua=
function comma_value(n)
    local left,num,right = string.match(n,'^([^%d]*%d)(%d*)(.-)$')
    return left..(num:reverse():gsub('(%d%d%d)','%1,'):reverse())..right
end

print(comma_value(10000000))   --    10,000,000
print(comma_value("10000000")) --    10,000,000
print(comma_value(123456789))  --   123,456,789
print(comma_value(1234567890)) -- 1,234,567,890
```

## 秒數轉換時間格式

這個可以轉換成: 小時:分鐘:秒 `00:00:00`

```lua=
function SecondsToTime(sec)
    local hour    = math.floor(sec / 3600)
    local minute  = math.floor(sec / 60) - (hour * 60)
    local seconds = math.floor(sec % 60)
    return string.format("%02d:%02d:%02d", hour, minute, seconds)
end

print(SecondsToTime(0))    -- 00:00:00
print(SecondsToTime(100))  -- 00:01:40
print(SecondsToTime(1280)) -- 00:21:20
print(SecondsToTime(3600)) -- 01:00:00
```

這個可以轉換成: 分鐘:秒.毫秒 `00:00.00`

```lua=
function SecondsToTime(sec)
    local minute  = math.floor(sec / 60)
    local seconds = math.floor(sec % 60)
    return string.format("%02d:%05.2f", minute, seconds)
end

print(SecondsToTime(0.661))     -- 00:00:00.66
print(SecondsToTime(100.185))   -- 00:01:40.19
print(SecondsToTime(1280.1698)) -- 00:21:20.17
print(SecondsToTime(3600.9541)) -- 01:00:00.95
```

## UI漸層顏色

```lua
-- colorA:   (table)A顏色 {r = 255, g = 255, b = 255, a = 255}
-- colorB:   (table)B顏色 {r = 255, g =   0, b = 255, a = 255}
-- boxCount: (number)產生A~B之間的漸變顏色數量
GetGradient(colorA, colorB, boxCount)
```

```lua=
function GetGradient(colorA, colorB, boxCount)
    colorA.a = colorA.a or 255
    colorB.a = colorB.a or 255

    local temp = {}
    for i = 1, boxCount do
        local rAvg = colorA.r + math.floor((colorB.r - colorA.r) * i / boxCount)
        local gAvg = colorA.g + math.floor((colorB.g - colorA.g) * i / boxCount)
        local bAvg = colorA.b + math.floor((colorB.b - colorA.b) * i / boxCount)
        local aAvg = colorA.a + math.floor((colorB.a - colorA.a) * i / boxCount)
        table.insert(temp, {r = rAvg, g = gAvg, b = bAvg, a = aAvg})
    end
    return temp
end

print(GetGradient({r=255,g=255,b=255}, {r=255,g=0,b=255}, 6))
--[[
output
    {
        {r=255,g=212,b=255},{r=255,g=170,b=255},{r=255,g=127,b=255},
        {r=255,g= 85,b=255},{r=255,g= 42,b=255},{r=255,g=  0,b=255}
    }
]]
```