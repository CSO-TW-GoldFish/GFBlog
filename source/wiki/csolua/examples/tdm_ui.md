---
wiki: csolua
title: '團隊死鬥 UI'
h1: ''
---

```lua
-- 團隊死亡競賽範例(ui)

-- 需要在伺服器上同步的分數變數
ScoreCT = UI.SyncValue.Create("ScoreCT")
ScoreTR = UI.SyncValue.Create("ScoreTR")
MaxKill = UI.SyncValue.Create("MaxKill")

-- 創建記分牌
screen = UI.ScreenSize()
center = {x = screen.width / 2, y = screen.height / 2}

scoreBG = UI.Box.Create()
scoreBG:Set({x = center.x - 100, y = 0, width = 200, height = 50, r = 255, g = 255, b = 255, a = 150})

goalBG = UI.Box.Create()
goalBG:Set({x = center.x - 50, y = 0, width = 100, height = 50, r = 40, g = 40, b = 40, a = 150})

goalLabel = UI.Text.Create()
goalLabel:Set({text='00', font='large', align='center', x = center.x - 50, y = 10, width = 100, height = 50, r = 80, g = 255, b = 80})

ctLabel = UI.Text.Create()
ctLabel:Set({text='00', font='medium', align='left', x = center.x - 95, y = 20, width = 50, height = 50, r = 80, g = 80, b = 255})

trLabel = UI.Text.Create()
trLabel:Set({text='00', font='medium', align='right', x = center.x + 45, y = 20, width = 50, height = 50, r = 255, g = 80, b = 80})

-- 每次同步變數時，記分板都會更新。
function ScoreCT:OnSync()
    local str = string.format("%02d", self.value)
    ctLabel:Set({text = str})
end

function ScoreTR:OnSync()
    local str = string.format("%02d", self.value)
    trLabel:Set({text = str})
end

function MaxKill:OnSync()
    local str = string.format("%02d", self.value)
    goalLabel:Set({text = str})
end
```