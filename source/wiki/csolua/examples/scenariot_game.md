---
wiki: csolua
title: '啟示錄TX Game'
h1: ''
---

```lua
--[[ 事先定義game中所需的常數和變數 ]]
players = {}
difficultySelectPlayer = 0 -- 玩家索引選擇或選擇難度級別

-- 設定武器和怪物能力等時所使用的標準等級。
MonsterLevel = 1 -- 怪物等級
LEVEL_MIN = 1 -- 最低玩家等級
LEVEL_MAX = 99 -- 玩家最高等級
WEAPONLEVEL_MAX = 60 -- 武器最大等級
mapDifficulty = SignalToGame.difficulty0 -- 目前地圖難度

-- 武器和怪物的各種能力是根據其等級乘以以下數值來使用的。
LevelRatio = {
	1.000,
	1.050,
	1.103,
	1.158,
	1.216,
	1.276,
	1.340,
	1.407,
	1.477,
	1.551,
	1.629,
	1.710,
	1.796,
	1.886,
	1.980,
	2.079,
	2.183,
	2.292,
	2.407,
	2.527,
	2.653,
	2.786,
	2.925,
	3.072,
	3.225,
	3.386,
	3.556,
	3.733,
	3.920,
	4.116,
	4.322,
	4.538,
	4.765,
	5.003,
	5.253,
	5.516,
	5.792,
	6.081,
	6.385,
	6.705,
	7.040,
	7.392,
	7.762,
	8.150,
	8.557,
	8.985,
	9.434,
	9.906,
	10.401,
	10.921,
	11.467,
	12.041,
	12.643,
	13.275,
	13.939,
	14.636,
	15.367,
	16.136,
	16.943,
	17.790,
	18.679,
	19.613,
	20.594,
	21.623,
	22.705,
	23.840,
	25.032,
	26.283,
	27.598,
	28.978,
	30.426,
	31.948,
	33.545,
	35.222,
	36.984,
	38.833,
	40.774,
	42.813,
	44.954,
	47.201,
	49.561,
	52.040,
	54.641,
	57.374,
	60.242,
	63.254,
	66.417,
	69.738,
	73.225,
	76.886,
	80.730,
	84.767,
	89.005,
	93.455,
	98.128,
	103.035,
	108.186,
	113.596,
	119.276,
}

-- 武器等級判定機率
WeaponGradeProb = {
	1.0,
	0.2,
	0.05,
	0.005
}

-- 按武器類別劃分的基本傷害
WeaponGradeDamage = {
	1.0,
	1.25,
	1.5,
	2.0
}

-- 武器隨機傷害（如果是0.15，則在0.85到1.15之間隨機產生）
WeaponRandomDamage = 0.15

-- 按武器等級劃分的傷害
WeaponLevelDamage = 1.0

-- 保存遊戲中使用的怪物類型
MonsterTypes = {
	Game.MONSTERTYPE.NORMAL0,
	Game.MONSTERTYPE.NORMAL1,
	Game.MONSTERTYPE.NORMAL2,
	Game.MONSTERTYPE.NORMAL3,
	Game.MONSTERTYPE.NORMAL4,
	Game.MONSTERTYPE.NORMAL5,
	Game.MONSTERTYPE.NORMAL6,
	Game.MONSTERTYPE.RUNNER0,
	Game.MONSTERTYPE.RUNNER1,
	Game.MONSTERTYPE.RUNNER2,
	Game.MONSTERTYPE.RUNNER3,
	Game.MONSTERTYPE.RUNNER4,
	Game.MONSTERTYPE.RUNNER5,
	Game.MONSTERTYPE.RUNNER6,
	Game.MONSTERTYPE.HEAVY1,
	Game.MONSTERTYPE.HEAVY2,
	Game.MONSTERTYPE.A101AR,
	Game.MONSTERTYPE.A104RL,
}

-- 遊戲中使用的怪物類別的定義
MonsterGrade = {
	normal = 1,
	rare = 2,
	unique = 3,
	legend = 4,
	END = 4
}

-- 統計每個怪物組（組）是否死亡的表格
monsterGroupCnt = {}

-- 怪物波管理表（與怪物生成器類似的行為）
monsterWaveCnt = {}
monsterWavePosition = {}

-- 是否對付wave怪
WaveFuncState = {
	enable = 1,  -- 啟用wave
	disable = 3, -- 禁用wave
}
monsterWaveFuncState = WaveFuncState.enable

-- 怪物等級判定機率
MonsterGradeProb = {
	1.0,
	0.3,
	0.1,
	0.01
}

-- 不同怪物等級的武器掉落機率（最大 1.0）
WeaponDropProb = {
	0.035,
	0.08,
	0.08,
	1
}

-- 為每個怪物等級設定數值
MonsterLevelVar = {
	normal =	{hpMin = 30, hpMax = 60, armorMin = 5, armorMax = 30, damageMin = 10, damageMax = 20, coinMin = 1, coinMax = 5},
	runner =	{hpMin = 20, hpMax = 50, armorMin = 0, armorMax = 20, damageMin = 10, damageMax = 30, coinMin = 3, coinMax = 8},
	heavy =		{hpMin = 150, hpMax = 250, armorMin = 20, armorMax = 55, damageMin = 20, damageMax = 40, coinMin = 10, coinMax = 15},
	a101ar =	{hpMin = 150, hpMax = 250, armorMin = 30, armorMax = 50, damageMin = 4, damageMax = 4, coinMin = 20, coinMax = 25},
	a104rl =	{hpMin = 150, hpMax = 250, armorMin = 30, armorMax = 50, damageMin = 17, damageMax = 17, coinMin = 20, coinMax = 25},
	etc =		{hpMin = 20, hpMax = 50, armorMin = 0, armorMax = 20, damageMin = 10, damageMax = 35, coinMin = 3, coinMax = 8},
}

-- 基於怪物生命值的經驗值（exp == hp * MonsterExpMult）
MonsterExpMult = 0.6

-- 設定每個等級的最高等級和所需經驗
PlayerRequireExp = {
	87,
	360,
	864,
	1584,
	2625,
	3996,
	5586,
	7680,
	10206,
	13200,
	17061,
	21168,
	25857,
	31752,
	38475,
	45312,
	53754,
	64152,
	74727,
	87600,
	100548,
	116160,
	133308,
	152064,
	174375,
	196716,
	223074,
	251664,
	285099,
	318600,
	357492,
	402432,
	447579,
	499392,
	554925,
	618192,
	685869,
	758100,
	839592,
	926400,
	1023729,
	1127196,
	1236981,
	1359072,
	1494450,
	1637784,
	1795917,
	1969920,
	2153697,
	2355000,
	2574990,
	2806752,
	3067428,
	3341736,
	3639075,
	3960768,
	4308174,
	4682688,
	5096184,
	5529600,
	5994531,
	6504048,
	7060851,
	7643136,
	8276775,
	8964648,
	9696240,
	10487232,
	11340702,
	12245100,
	13232625,
	14292288,
	15427455,
	16641564,
	17955000,
	19355376,
	20864151,
	22486464,
	24208839,
	26073600,
	28067958,
	30217656,
	32509191,
	34948368,
	37584450,
	40404348,
	43415784,
	46626624,
	50092404,
	53775900,
	57735132,
	61956480,
	66476214,
	71306520,
	76486875,
	82003968,
	87898878,
	94215240,
	100000000
}


--[[ 怪物相關的用戶自訂函數 ]]

-- 根據等級設定怪物能力
function SetMonsterAttribute(monster, grade)

	if monster == nil then
		return
	end

	monster.user.level = monsterLevel
	monster.user.grade = grade

	monster.applyKnockback = true -- 使怪物能夠受到擊退
	monster.canJump = false -- 防止怪物跳躍
	monster.viewDistance = 12 -- 發現敵人的視野範圍

	local levelVar
	if Game.MONSTERTYPE.NORMAL0 <= monster.type and monster.type <= Game.MONSTERTYPE.NORMAL6 then
		levelVar = MonsterLevelVar.normal
	elseif Game.MONSTERTYPE.RUNNER0 <= monster.type and monster.type <= Game.MONSTERTYPE.RUNNER6 then
		levelVar = MonsterLevelVar.runner
	elseif Game.MONSTERTYPE.HEAVY1 <= monster.type and monster.type <= Game.MONSTERTYPE.HEAVY2 then
		levelVar = MonsterLevelVar.heavy
	elseif monster.type == Game.MONSTERTYPE.A101AR then
		levelVar = MonsterLevelVar.a101ar
	elseif monster.type == Game.MONSTERTYPE.A104RL then
		levelVar = MonsterLevelVar.a104rl
	else
		levelVar = MonsterLevelVar.etc
	end

	-- 傷害比其他值更漸進
	local damageMult = ((LevelRatio[monsterLevel] - 1.0) * 0.5) + 1.0
	monster.damage = math.floor(Game.RandomInt(levelVar.damageMin, levelVar.damageMax) * damageMult)

	monster.health = math.floor(Game.RandomInt(levelVar.hpMin, levelVar.hpMax) * LevelRatio[monsterLevel])
	monster.coin = math.floor(Game.RandomInt(levelVar.coinMin, levelVar.coinMax) * LevelRatio[monsterLevel])
	monster.user.exp = math.floor(monster.health * MonsterExpMult) -- 玩家捕捉該怪物時將獲得的經驗值

	-- 根據確定的等級設定顏色和能力
	if grade == MonsterGrade.rare then
		monster:SetRenderFX(Game.RENDERFX.GLOWSHELL)
		monster:SetRenderColor({r = 0, g = 30, b = 255})
		monster.health = math.floor(monster.health * 3.0)
		monster.damage = monster.damage * 1.5
		monster.speed = 1.5
		monster.user.exp = math.floor(monster.user.exp * 1.5)
	elseif grade == MonsterGrade.unique then
		monster:SetRenderFX(Game.RENDERFX.GLOWSHELL)
		monster:SetRenderColor({r = 255, g = 30, b = 30})
		monster.health = math.floor(monster.health * 5.0)
		monster.damage = monster.damage * 2.0
		monster.speed = 1.5
		monster.user.exp = math.floor(monster.user.exp * 3.0)
	elseif grade == MonsterGrade.legend then
		monster:SetRenderFX(Game.RENDERFX.GLOWSHELL)
		monster:SetRenderColor({r = 255, g = 255, b = 100})
		monster.health = math.floor(monster.health * 12.0)
		monster.damage = monster.damage * 2.0
		monster.speed = 2.5
		monster.user.exp = math.floor(monster.user.exp * 10.0)
	end
end

-- 在指定地點創造一群怪物
function CreateMonsters(type, num, pos, groupid, grade)

	if grade == nil then
		grade = MonsterGrade.normal
	end

	result = {}

	-- 創造num數量的怪物
	for i = 1, num do
		monster = Game.Monster.Create(type, pos)
		if monster then
			-- 依怪物等級設定能力
			SetMonsterAttribute(monster, grade)

			-- 儲存怪物所屬的組號，並將此組號加一
			monster.user.groupid = groupid
			if monsterGroupCnt[groupid] then
				monsterGroupCnt[groupid] = monsterGroupCnt[groupid] + 1
			else
				monsterGroupCnt[groupid] = 1;
			end

			-- 保存結果
			table.insert(result, monster)
		end
	end

	return result
end

-- 當所有怪物都死掉時所呼叫的函數
function OnMonsterKilled(monster)
    if monster.user.waveFunc then
		if monsterWaveFuncState == WaveFuncState.enable then
			monster.user.waveFunc(true, monster.user.waveFuncArg)
		else
			monster.user.waveFunc = nil
		end
    end

    -- boss團死了
    if monster.user.groupid == 8055 then
        monsterWaveFuncState = WaveFuncState.disable
        difficultySelectPlayer = 0
        Game.KillAllMonsters() -- 下次更新後殺死所有怪物
    end

    Game.SetTrigger('OnMonsterKilled' ..  monster.user.groupid, true)
end


--[[ 武器相關自訂函數 ]]


-- 設定所有武器的最低基礎統計數據
function SetWeaponAttributeDefault(weapon)
	if weapon == nil then
		return
	end

	-- 副武器有無限彈匣
	if weapon:GetWeaponType() == Game.WEAPONTYPE.PISTOL then
		weapon.infiniteclip = true
	else
		weapon:AddClip1(3) -- 提供3個標準彈匣
	end

	-- 基本水平
	if weapon.user.level == nil then
		weapon.user.level = Common.GetWeaponOption(weapon.weaponid).user.level
	end

	-- 基本水平
	weapon.user.grade = WeaponGrade.normal
	weapon.color = Game.WEAPONCOLOR.WHITE
end

-- 依等級設定武器能力
function SetWeaponAttribute(weapon, level)

	if weapon == nil then
		return
	end

	-- 基本能力設定
	SetWeaponAttributeDefault(weapon)

	-- 等級設定
	weapon.user.level = level

	-- 取得目前武器的WeaponOption和等級。
	local option = Common.GetWeaponOption(weapon.weaponid)
	local grade = option.user.grade

	-- 按等級決定出現機率
	local weightMax = 0.0
	for i = grade, WeaponGrade.END do
		weightMax = weightMax + WeaponGradeProb[i]
	end

	local weight = Game.RandomFloat(0.0, weightMax)

	local weightSum = 0.0
	for i = grade, WeaponGrade.END do
		weightSum = weightSum + WeaponGradeProb[i]
		if weight <= weightSum  then
			grade = i
			break
		end
	end

	-- 按等級、等級和隨機進行傷害計算
	weapon.damage = WeaponLevelDamage * LevelRatio[level]
	weapon.damage = weapon.damage * (WeaponGradeDamage[grade] + Game.RandomFloat(-WeaponRandomDamage, WeaponRandomDamage))

	-- 隨機能力的最大數量
	local maxAttrNum = 0

	-- 根據確定的等級設定能力的顏色和數量
	if grade == WeaponGrade.normal then
		weapon.color = Game.WEAPONCOLOR.WHITE
		maxAttrNum = 0
	elseif grade == WeaponGrade.rare then
		weapon.color = Game.WEAPONCOLOR.BLUE
		maxAttrNum = 1
	elseif grade == WeaponGrade.unique then
		weapon.color = Game.WEAPONCOLOR.RED
		maxAttrNum = 2
	elseif grade == WeaponGrade.legend then
		weapon.color = Game.WEAPONCOLOR.ORANGE
		maxAttrNum = 3
	end

	-- 防止重複出現的能力
	local attrDuplicateCheck = {}
	if weapon:GetWeaponType() == Game.WEAPONTYPE.PISTOL then
		attrDuplicateCheck[5] = true
	end

	-- 能力分數判定
	local attrNum = Game.RandomInt(0, maxAttrNum)
	for i = 1, attrNum do
		local attrType = Game.RandomInt(1, 5) -- 從speed到infiniteclip的 5 種類型
		if attrDuplicateCheck[attrType] then
			-- 如果能力重疊的話
			i = i - 1
		else
			attrDuplicateCheck[attrType] = true

			if attrType == 1 then
				weapon.speed = Game.RandomFloat(1.2, 1.3)
			elseif attrType == 2 then
				weapon.knockback = Game.RandomFloat(1.2, 2.0)
				weapon.flinch = Game.RandomFloat(1.2, 2.0)
			elseif attrType == 3 then
				weapon.criticalrate = Game.RandomFloat(0.03, 0.2)
				weapon.criticaldamage = Game.RandomFloat(1.5, 2.5)
			elseif attrType == 4 then
				weapon.bloodsucking = Game.RandomFloat(0.01, 0.03)
			elseif attrType == 5 then
				weapon.infiniteclip = true
			end
		end
	end
end

-- 隨機化武器等級（使用怪物等級）
function GetWeaponRandomLevel(level)

	local minLevel = level - 5
	local maxLevel = level + 3

	if minLevel < LEVEL_MIN then
		minLevel = LEVEL_MIN
	end
	if maxLevel > WEAPONLEVEL_MAX then
		maxLevel = WEAPONLEVEL_MAX
	end

	return Game.RandomInt(minLevel, maxLevel)
end

-- 隨機武器生成（使用等級、位置）
function CreateWeapon(level, pos)

	-- 隨機決定武器類型（使用武器等級）
	local weightMax = 0.0
	local list = {}
	for i = 1, #WeaponList do
		local weaponOption = Common.GetWeaponOption(WeaponList[i])
		if weaponOption.user.level <= level then
			table.insert(list, weaponOption)
			weightMax = weightMax + (LevelRatio[weaponOption.user.level] * WeaponGradeProb[weaponOption.user.grade])
		end
	end

	local type = 0
	local weightSum = 0.0
	local weight = Game.RandomFloat(0.0, weightMax)
	for i = 1, #list do
		weightSum = weightSum + (LevelRatio[list[i].user.level] * WeaponGradeProb[list[i].user.grade])
		if weight <= weightSum  then
			type = list[i].weaponid
			break
		end
	end

	if type == 0 then
		return nil
	end

	local weapon = Game.Weapon.CreateAndDrop(type, pos)
	if weapon then
		SetWeaponAttribute(weapon, level)
	end

	return weapon
end


--[[ 與玩家等級相關的自訂功能 ]]


-- 當玩家升級時
function OnLevelUp(player)

	-- 增加耐力
	player.maxhealth = math.floor(100 * LevelRatio[player.user.level])
	player.health = player.maxhealth

	-- 在普通難度下，怪物等級會根據玩家等級而變化
	if mapDifficulty == SignalToGame.difficulty0 then
		if player.user.level > monsterLevel then
			monsterLevel = player.user.level
			if monsterLevel > 30 then
				monsterLevel = 30
			end
		end
	end

	-- 比較 WeaponOption 和等級並顯示 UI 鎖定顯示（商店櫥窗）
	for i = 1, #BuymenuWeaponList do
		local option = Common.GetWeaponOption(BuymenuWeaponList[i])
		if option then
			player:SetBuymenuLockedUI(BuymenuWeaponList[i], option.user.level > player.user.level, option.user.level)
		end
	end

	-- 比較武器和等級並顯示 UI 鎖定顯示（武器庫存視窗）
	local invenWeapons = player:GetWeaponInvenList()
	for i = 1, #invenWeapons do
		player:SetWeaponInvenLockedUI(invenWeapons[i], invenWeapons[i].user.level > player.user.level, invenWeapons[i].user.level)
	end
end

-- 根據玩家目前等級和經驗值計算經驗百分比
function CalcExpRate(level, exp)
	return exp / PlayerRequireExp[level]
end

-- 給予玩家經驗值
function AddExp(player, exp)

	local pu = player.user

	-- 如果您已達到滿級，請跳過
	if pu.level >= LEVEL_MAX then
		return
	end

	pu.exp = pu.exp + exp

	 -- 升級
	if pu.exp > PlayerRequireExp[pu.level] then
		pu.level = pu.level + 1

		if pu.level >= LEVEL_MAX then
			pu.exp = 0
		else
			pu.exp = pu.exp - PlayerRequireExp[pu.level - 1]
		end

		OnLevelUp(player)
	end

	-- 顯示和更新等級/經驗 UI
	pu.expRate = CalcExpRate(pu.level, pu.exp)
	player:SetLevelUI(pu.level, pu.expRate)
end


--[[ 工作室呼叫功能（武器、怪物相關） ]]


-- 字串分割函數
function splitstr_tonumber(inputstr)
	local t = {}
	for str in string.gmatch(inputstr, "([^,]*)") do
		table.insert(t, tonumber(str))
	end
	return t
end

-- 指定每個怪物組的 AttackTo 位置
MonsterAttackPos = {

	-- CreateDefaultMonsters
	[1] = {x = -12, y = 100, z = 1},
	[2] = {x = -9, y = 98, z = 1},
	[3] = {x = 14, y = 77, z = 1},
	[7] = {x = 80, y = 26, z = 1},
	[9] = {x = 81, y = 22, z = 1},
	[11] = {x = 81, y = 24, z = 1},
	[13] = {x = 36, y = -14, z = 1},
	[14] = {x = 35, y = -11, z = 1},
	[17] = {x = 34, y = -32, z = -3},
	[18] = {x = 35, y = -40, z = -3},
	[8055] = {x = 25, y = -31, z = -3},

	-- CreateWaveMonsters
	[1000] = {x = -4, y = 72, z = 1},

	-- CreateSpecialMonsters
	[10000] = {x = -12, y = 108, z = 1},
}

-- 怪物等級的確定
function GetRandomGrade(min, max)
	if min == max then
		return min
	end

	local grade = min

	local weightMax = 0.0
	for i = min, max do
		weightMax = weightMax + MonsterGradeProb[i]
	end

	local weight = Game.RandomFloat(0.0, weightMax)

	local weightSum = 0.0
	for i = min, max do
		weightSum = weightSum + MonsterGradeProb[i]
		if weight <= weightSum  then
			return i
		end
	end

	return min
end

function CreateDefaultMonsters(callerOn, arg)

	if callerOn == nil or callerOn == false then
		return
	end

	local args = splitstr_tonumber(arg) -- 拆分 arg 字串並將其儲存在數字數組中。
	local type = MonsterTypes[args[1]] -- 怪物類型
	local num = args[2] -- 召喚怪物數量
	local groupid = args[3] -- 怪物群組ID
	local grade = GetRandomGrade(args[4], args[5]) -- 怪物等級

	-- Game.GetScriptCaller()：取得呼叫函數的腳本函數呼叫VoxelEntity。
	local monsters = CreateMonsters(type, num, Game.GetScriptCaller().position, groupid, grade)

	-- AttackTo 需要指定地點的怪物群
	for i = 1, #monsters do
		if MonsterAttackPos[groupid] then
			monsters[i]:AttackTo(MonsterAttackPos[groupid]) -- 移動到指定座標時進行攻擊
		end
	end
end

-- 怪物wave成功能
function CreateWaveMonsters(callerOn, arg)
	if callerOn == nil or callerOn == false then
		return
	end

	local args = splitstr_tonumber(arg)
	local type = MonsterTypes[args[1]]
	local num = args[2]
	local groupid = args[3]
	local grade = GetRandomGrade(args[4], args[5])
	local waveCnt = args[6] -- 產生的wave數

	if monsterWaveCnt[groupid] then
		monsterWaveCnt[groupid] = monsterWaveCnt[groupid] + 1
		if monsterWaveCnt[groupid] > waveCnt then
			if monsterWaveCnt[groupid] == waveCnt + 1 then
				Game.SetTrigger('OnWaveEnded' ..  groupid, true)
			end

			return
		end
	else
		monsterWaveCnt[groupid] = 1
	end

	if Game.GetScriptCaller() then
		monsterWavePosition[groupid] = Game.GetScriptCaller().position
	end

	local monsters = CreateMonsters(type, num, monsterWavePosition[groupid], groupid, grade)
	for i = 1, #monsters do
		monsters[i].user.waveFunc = CreateWaveMonsters
		monsters[i].user.waveFuncArg = arg

		if MonsterAttackPos[groupid] then
			monsters[i]:AttackTo(MonsterAttackPos[groupid]) -- 移動到指定座標時進行攻擊
		end
	end
end

-- 特定物品掉落怪物創建功能
function CreateSpecialMonsters(callerOn, arg)
	if callerOn then

		local args = splitstr_tonumber(arg)
		local type = MonsterTypes[args[1]]
		local num = args[2]
		local groupid = args[3]
		local grade = GetRandomGrade(args[4], args[5])

		local monsters = CreateMonsters(type, num, Game.GetScriptCaller().position, groupid, grade)
		for i = 1, #monsters do
			monsters[i].user.specialWeaponDrop = groupid -- 指定特定物品掉落

			if MonsterAttackPos[groupid] then
				monsters[i]:AttackTo(MonsterAttackPos[groupid]) -- 移動到指定座標時進行攻擊
			end
		end
	end
end

-- 特定物品掉落指定功能
function CreateSpecialWeapon(specialWeaponDrop, position)

	if specialWeaponDrop == 10000 then
		local weapon = Game.Weapon.CreateAndDrop(Common.WEAPON.P90, position)
		SetWeaponAttributeDefault(weapon)
	elseif specialWeaponDrop == 10001 then
		local weapon = Game.Weapon.CreateAndDrop(Common.WEAPON.M134Minigun, position)
		SetWeaponAttributeDefault(weapon)
	end
end

-- 創建放置在地圖上的基本武器
function CreateDefaultWeapon()
    local weapon = Game.Weapon.CreateAndDrop(Common.WEAPON.USP45, {x = -6, y = 150, z = 1})
	SetWeaponAttributeDefault(weapon)
end

-- 隨機武器生成
function CreateRandomWeapon(callerOn, arg)
	if callerOn then
		CreateWeapon(tonumber(arg), Game.GetScriptCaller().position)
	end
end


--[[ 事件函數 ]]


-- 當玩家在選擇職業後首次生成時調用的函數
function Game.Rule:OnPlayerJoiningSpawn(player)

	-- 儲存到玩家數組
	players[player.index] = player

	-- 將相機改為第三人稱，將輸入法改為基於滑鼠指標
	player:SetThirdPersonFixedView(-45, 53, 100, 250) -- yaw, pitch, minDist, maxDist

	-- ThirdPersonFixedView 修改滑鼠指標光線投射位置的計算方式
	player:SetThirdPersonFixedPlane(Game.THIRDPERSON_FIXED_PLANE.GROUND)

	-- 基礎水準、經驗
	player.user.level = 1
	player.user.exp = 0
	player.user.expRate = 0

	-- UI按級別初始化
	OnLevelUp(player) -- 武器鎖UI初始化目的
	player:SetLevelUI(player.user.level, player.user.expRate) -- 等級/經驗 UI 設定

	-- 團隊設定
	player.team = Game.TEAM.CT
end

-- entity 當（怪物、玩家等）受到傷害時呼叫的函數。
function Game.Rule:OnTakeDamage(victim, attacker, damage, weapontype, hitbox)
	if attacker == nil or victim == nil then return end

	if victim:IsMonster() then
		victim = victim:ToMonster()
		victim:ShowOverheadDamage(damage, 0) -- 傷害顯示在頭頂上方。 如果第二個參數（玩家索引）為 0，則會顯示給所有人。
	end
end

-- entity死亡時呼叫的函數
function Game.Rule:OnKilled(victim, killer)
	if victim == nil or killer == nil then
		return
	end

	-- 當玩家死亡時使他復活
	if victim:IsPlayer() then

		victim = victim:ToPlayer()

		-- 如果難度不正常，經驗值減少10%。
		if mapDifficulty ~= SignalToGame.difficulty0 then
			AddExp(victim, -math.floor(PlayerRequireExp[victim.user.level] / 10.0))
		end

		if victim.user.spawnable == true then
			victim:Respawn()
		end

		-- 隱藏重新載入 UI
		victim:Signal(SignalToUI.reloadFinished)
	end

	-- 當怪物死亡時
	if victim:IsMonster() then

		victim = victim:ToMonster()

		-- 如果killer是玩家，則會給予經驗值。
		if killer:IsPlayer() then
			killer = killer:ToPlayer()
			AddExp(killer, victim.user.exp)
		end

		-- 為怪物製作指定武器或根據怪物等級檢查武器掉落機率來掉落武器。
		if victim.user.specialWeaponDrop then
			CreateSpecialWeapon(victim.user.specialWeaponDrop, victim.position)
		else
			weight = Game.RandomFloat(0.0, 1.0)
			if weight <= WeaponDropProb[victim.user.grade] then
				CreateWeapon(GetWeaponRandomLevel(victim.user.level), victim.position)
			end
		end

		-- 檢查怪物數量，如果達到 0，則在 OnMonsterKilled 函數中傳送到工作室。
		monsterGroupCnt[victim.user.groupid] = monsterGroupCnt[victim.user.groupid] - 1
		if monsterGroupCnt[victim.user.groupid] <= 0 then
			monsterGroupCnt[victim.user.groupid] = nil
			OnMonsterKilled(victim)
		end
	end
end

-- 偵測武器是否可以購買的功能
-- 未包含在腳本武器清單中的武器不會被呼叫。
function Game.Rule:CanBuyWeapon(player, weaponid)
	local weaponOption = Common.GetWeaponOption(weaponid)
	return weaponOption.user.level <= player.user.level
end

-- 偵測武器是否可以手持的功能
-- 未包含在腳本武器清單中的武器不會被呼叫。
-- 如果從未使用過 Weapon 類，則 Weapon 參數將作為 nil 傳遞。
function Game.Rule:CanHaveWeaponInHand(player, weaponid, weapon)
	local weaponOptionCheck = Common.GetWeaponOption(weaponid).user.level <= player.user.level
	local weaponCheck = weapon == nil or weapon.user.level == nil or weapon.user.level <= player.user.level

	return weaponOptionCheck and weaponCheck
end

-- 取得武器時呼叫的函數
-- 未包含在腳本武器清單中的武器不會被呼叫。
-- 如果從未使用過 Weapon 類，則 Weapon 參數將作為 nil 傳遞。
function Game.Rule:OnGetWeapon(player, weaponid, weapon)
	if weapon == nil then
		return
	end

	-- 基本能力設定
	if weapon.user.level == nil then
		SetWeaponAttributeDefault(weapon)
	end

	-- 鎖定的使用者介面更新
	player:SetWeaponInvenLockedUI(weapon, weapon.user.level > player.user.level, weapon.user.level)
end

-- 用於傳送重裝時間的變數
reloadTimeSync = Game.SyncValue.Create("reloadTime")

-- 重新載入時呼叫的函數
-- 如果 Weapon 類別從未使用過或不是腳本武器，則 Weapon 參數將作為 nil 傳遞。
function Game.Rule:OnReload(player, weapon, time)
	-- 顯示重新載入 UI
	player:Signal(SignalToUI.reloadStarted)
	-- 交付重裝時間
	reloadTimeSync.value = time
end

-- 載入後呼叫的函數
-- 如果 Weapon 類別從未使用過或不是腳本武器，則 Weapon 參數將作為 nil 傳遞。
function Game.Rule:OnReloadFinished(player, weapon)
	player:Signal(SignalToUI.reloadFinished) -- 隱藏重新載入 UI
end

-- 當玩家切換武器時呼叫的函數
function Game.Rule:OnSwitchWeapon(player)
	player:Signal(SignalToUI.reloadFinished) -- 隱藏重新載入 UI
end

-- 啟動後呼叫的函數
-- 如果 Weapon 類別從未使用過或不是腳本武器，則 Weapon 參數將作為 nil 傳遞。
function Game.Rule:PostFireWeapon(player, weapon, time)

	-- 僅當武器的射擊間隔超過 1 秒時才會顯示裝彈 UI。
	if time > 1.0 then
		-- 顯示重新載入 UI
		player:Signal(SignalToUI.reloadStarted)
		-- 交付重裝時間
		reloadTimeSync.value = time
	end
end

-- 當玩家拔出武器時呼叫的函數
function Game.Rule:OnDeployWeapon(player, weapon)
	player:Signal(SignalToUI.reloadFinished) -- 隱藏重新載入 UI
end

-- 保存每個玩家的保存訊息
function Game.Rule:OnGameSave(player)
	if player == nil then
		return
	end

	-- 保存等級和經驗值
	player:SetGameSave('level', player.user.level)
	player:SetGameSave('exp', player.user.exp)

	-- 保存您目前持有的武器的等級
	local primaryWeapon = player:GetPrimaryWeapon()
	local secondaryWeapon = player:GetSecondaryWeapon()

	if primaryWeapon then
		player:SetGameSave('wpn_lv_pri', primaryWeapon.user.level)
	else
		player:SetGameSave('wpn_lv_pri', 0)
	end

	if secondaryWeapon then
		player:SetGameSave('wpn_lv_sec', secondaryWeapon.user.level)
	else
		player:SetGameSave('wpn_lv_sec', 0)
	end

	-- 保存庫存武器等級
	local invenWeapons = player:GetWeaponInvenList()
	for i = 1, #invenWeapons do
		player:SetGameSave('wpn_lv_inven' .. i, invenWeapons[i].user.level)
	end
end

function DoubleToInt(number)
	return math.floor(math.abs(number + EPSILON))
end

-- 載入每個玩家的保存訊息
function Game.Rule:OnLoadGameSave(player)
	if player == nil then
		return
	end

	-- 等級、經驗負荷
	player.user.level = DoubleToInt(player:GetGameSave('level'))
	player.user.exp = DoubleToInt(player:GetGameSave('exp'))

	if player.user.level == nil then
		player.user.level = 1
	end
	if player.user.exp == nil then
		player.user.exp = 0
	end

	player.user.expRate = CalcExpRate(player.user.level, player.user.exp)

	-- 載入你目前持有的武器
	local primaryWeapon = player:GetPrimaryWeapon()
	local secondaryWeapon = player:GetSecondaryWeapon()

	if primaryWeapon then
		primaryWeapon.user.level = DoubleToInt(player:GetGameSave('wpn_lv_pri'))
	end

	if secondaryWeapon then
		secondaryWeapon.user.level = DoubleToInt(player:GetGameSave('wpn_lv_sec'))
	end

	-- 載入庫存武器等級
	local invenWeapons = player:GetWeaponInvenList()
	for i = 1, #invenWeapons do
		invenWeapons[i].user.level = DoubleToInt(player:GetGameSave('wpn_lv_inven' .. i))
	end

	-- UI按級別初始化
	OnLevelUp(player) -- 武器鎖UI初始化目的
	player:SetLevelUI(player.user.level, player.user.expRate) -- 等級/經驗 UI 設定
end

-- 重置每個玩家的保存訊息
function Game.Rule:OnClearGameSave(player)
	player.user.level = 1
	player.user.exp = 0
	player.user.expRate = 0
	player:RemoveWeapon()
	player:ClearWeaponInven()

	-- UI按級別初始化
	OnLevelUp(player) -- 武器鎖UI初始化目的
	player:SetLevelUI(player.user.level, player.user.expRate) -- 等級/經驗 UI 設定
end


--[[ 透過UI傳輸訊息 ]]


function Game.Rule:OnPlayerSignal(player, signal)

	if signal == SignalToGame.openWeaponInven then
		player:ToggleWeaponInven()
	end
end

-- 打開商店（呼叫Studio的函數）
function ShowBuymenu(callerOn)
	local triggerEnt = Game.GetTriggerEntity()
	if triggerEnt and triggerEnt:IsPlayer() then
		triggerEnt = triggerEnt:ToPlayer()
		triggerEnt:ShowBuymenu()
	end
end
```