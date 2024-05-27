---
wiki: csolua
title: '啟示錄TX UI'
h1: ''
---

```lua
screen = UI.ScreenSize()
center = {x = screen.width / 2, y = screen.height / 2}

--[[ 顯示庫存密鑰 ]]
weaponInvenKeyText = UI.Text.Create()
weaponInvenKeyText:Set({text = '인벤토리 : [B]', font='small', align='right', x = center.x + 80, y = screen.height - 25, width = 185, height = 30, r = 255, g = 200, b = 0, a = 100})

--[[ 載入中 ui ]]

reloadingInit = false
reloadingBG = UI.Box.Create()
reloadingGauge = UI.Box.Create()
reloadingText = UI.Text.Create()
reloadingBG:Hide()
reloadingGauge:Hide()
reloadingText:Hide()

reloadTime = 0 -- 重新載入所需時間
reloadStartTime = 0 -- 重新載入開始時間

reloadTimeSync = UI.SyncValue.Create("reloadTime")
function reloadTimeSync:OnSync()
	reloadTime = self.value
end

function UI.Event:OnUpdate(time)
	if reloadingBG:IsVisible() and reloadTime > EPSILON then
		if reloadStartTime < EPSILON then
			reloadStartTime = time
		else
			-- 透過計算自載入開始以來經過的時間來變更儀表的寬度。
			local diff = time - reloadStartTime
			local ratio = diff / reloadTime
			if ratio > 1 then
				reloadingBG:Hide()
				reloadingGauge:Hide()
				reloadingText:Hide()
				reloadingGauge:Set({width = 0})
				reloadTime = 0
			else
				reloadingGauge:Set({width = 100 * ratio})
			end
		end
	end
end


--[[ 事件函數 ]]


-- 按鍵時的事件函數
function UI.Event:OnKeyDown(inputs)
	if inputs[UI.KEY.B] then
		UI.Signal(SignalToGame.openWeaponInven)
	end
end

-- 透過game傳遞訊息
function UI.Event:OnSignal(signal)
	if signal == SignalToUI.reloadStarted then

		-- 第一次打開時填寫資料
		if reloadingInit == false then
			reloadingInit = true
			reloadingBG:Set({x = center.x - 50, y = center.y - 20, width = 100, height = 27, r = 25, g = 25, b = 25, a = 220})
			reloadingGauge:Set({x = center.x - 50, y = center.y - 20, width = 100, height = 27, r = 100, g = 120, b = 150, a = 220})
			reloadingText:Set({text = '장전중', font='small', align='center', x = center.x - 50, y = center.y - 4, width = 100, height = 20, r = 255, g = 255, b = 255})
		end

		reloadingBG:Show()
		reloadingGauge:Show()
		reloadingText:Show()
		reloadingGauge:Set({width = 0})
		reloadStartTime = 0
		reloadTime = 0

	elseif signal == SignalToUI.reloadFinished then
		reloadingBG:Hide()
		reloadingGauge:Hide()
		reloadingText:Hide()
		reloadingGauge:Set({width = 0})
		reloadTime = 0
	end
end
```