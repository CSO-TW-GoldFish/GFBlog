---
wiki: csolua
title: '團隊死鬥 Game'
h1: ''
---

```lua
-- 團隊死亡競賽範例(game)

local TDM = Game.Rule
TDM.name = "팀데스매치"
TDM.desc = "스크립트로 만드는 팀데 모드"

-- 地圖可以被破壞
TDM.breakable = true

-- 目標分數
local MaxKill = Game.SyncValue.Create("MaxKill")
MaxKill.value = 30

-- 團隊得分
local Score = {}
Score[Game.TEAM.CT] = Game.SyncValue.Create("ScoreCT")
Score[Game.TEAM.CT].value = 0
Score[Game.TEAM.TR] = Game.SyncValue.Create("ScoreTR")
Score[Game.TEAM.TR].value = 0

function TDM:OnPlayerSpawn(player)
    player:ShowBuymenu()
end

function TDM:OnPlayerKilled(victim, killer)
    -- 自殺、跌倒等。
    if killer == nil then
        return
    end

    -- 殺死玩家的隊伍得 1 分
    local killer_team = killer.team
    local point = Score[killer_team]
    point.value = point.value + 1

    -- 如果你超過了目標，你就贏了！
    if (point.value >= MaxKill.value) then
        self:Win(killer_team)
    end
end
```