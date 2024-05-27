---
wiki: csolua
title: '啟示錄TX Common'
h1: ''
---

```lua
--[[ 修改遊戲選項 ]]
Common.UseWeaponInven(true) -- 使用武器庫存功能
Common.SetSaveCurrentWeapons(true) -- 設定保存目前裝備的武器
Common.SetSaveWeaponInven(true) -- 設定保存武器庫存內容（必須先設定UseWeaponInven）
Common.SetAutoLoad(true) -- 自動載入已儲存的訊息
Common.DisableWeaponParts(true) -- 停用武器零件功能
Common.DisableWeaponEnhance(true) -- 停用武器增強功能
Common.DontGiveDefaultItems(true) -- 遊戲開始時不提供預設武器。
Common.DontCheckTeamKill(true) -- 即使有團隊擊殺，也會被視為普通擊殺。
Common.UseScenarioBuymenu(true) -- 讓商店使用場景商店櫥窗
Common.SetNeedMoney(true) -- 設定購買槍枝時的金錢需求。
Common.UseAdvancedMuzzle(true) -- 開火時以新形狀繪製槍口（忽略比例）
Common.SetMuzzleScale(1.0) -- 設定射擊時的槍口尺寸
Common.SetBloodScale(2) -- 修改命中時血量效果的大小
Common.SetGunsparkScale(10) -- 修改子彈擊中牆壁等時的效果大小。
Common.SetHitboxScale(2.5) -- 修改碰撞箱大小
Common.SetMouseoverOutline(true, {r = 255, g = 0, b = 0}) -- 當您將滑鼠停留在實體（例如怪物）上時，讓輪廓可見。
Common.SetUnitedPrimaryAmmoPrice(50) -- 將所有主要武器的每個彈匣的價格統一為此值。
Common.SetUnitedSecondaryAmmoPrice(0) -- 將所有輔助武器的每個彈匣的價格統一為此值。


--[[ 公共常數聲明 ]]


-- 比較實際值時使用
EPSILON = 0.00001

-- 從 UI 發送到game的訊號
SignalToGame = {
	openWeaponInven = 1,
}

-- 訊號從game發送到 UI
SignalToUI = {
	reloadStarted = 1,
	reloadFinished = 2
}

-- 遊戲中使用的武器類別
WeaponGrade = {
	normal = 1,
	rare = 2,
	unique = 3,
	legend = 4,
	END = 4
}

-- 商店武器列表
BuymenuWeaponList =	{
	Common.WEAPON.P228,
	Common.WEAPON.DualBeretta,
	Common.WEAPON.FiveSeven,
	Common.WEAPON.Glock18C,
	Common.WEAPON.USP45,
	Common.WEAPON.DesertEagle50C,
	Common.WEAPON.DualInfinity,
	Common.WEAPON.Galil,
	Common.WEAPON.FAMAS,
	Common.WEAPON.M4A1,
	Common.WEAPON.AK47,
	Common.WEAPON.OICW,
	Common.WEAPON.MAC10,
	Common.WEAPON.UMP45,
	Common.WEAPON.MP5,
	Common.WEAPON.TMP,
	Common.WEAPON.P90,
	Common.WEAPON.MP7A1ExtendedMag,
	Common.WEAPON.Needler,
	Common.WEAPON.M3,
	Common.WEAPON.XM1014,
	Common.WEAPON.DoubleBarrelShotgun,
	Common.WEAPON.WinchesterM1887,
	Common.WEAPON.USAS12,
	Common.WEAPON.FireVulcan,
	Common.WEAPON.M249,
	Common.WEAPON.MG3,
	Common.WEAPON.M134Minigun,
	Common.WEAPON.K3,
	Common.WEAPON.QBB95,
	Common.WEAPON.M32MGL,
	Common.WEAPON.Leviathan,
	Common.WEAPON.Salamander,
	Common.WEAPON.RPG7
}

-- 設定商店武器清單（需要UseScenarioBuymenu設定）
Common.SetBuymenuWeaponList(BuymenuWeaponList)

-- 遊戲中使用的所有武器列表
WeaponList = {
	-- 副武器列表
	Common.WEAPON.P228,
	Common.WEAPON.DualBeretta,
	Common.WEAPON.FiveSeven,
	Common.WEAPON.Glock18C,
	Common.WEAPON.USP45,
	Common.WEAPON.DesertEagle50C,
	Common.WEAPON.DualInfinity,
	Common.WEAPON.DualInfinityCustom,
	Common.WEAPON.DualInfinityFinal,
	Common.WEAPON.SawedOffM79,
	Common.WEAPON.Cyclone,
	Common.WEAPON.AttackM950,
	Common.WEAPON.DesertEagle50CGold,
	Common.WEAPON.ThunderGhostWalker,
	Common.WEAPON.PythonDesperado,
	Common.WEAPON.DesertEagleCrimsonHunter,
	Common.WEAPON.DualBerettaGunslinger,
	-- 步槍清單
	Common.WEAPON.Galil,
	Common.WEAPON.FAMAS,
	Common.WEAPON.M4A1,
	Common.WEAPON.SG552,
	Common.WEAPON.AK47,
	Common.WEAPON.AUG,
	Common.WEAPON.AN94,
	Common.WEAPON.M16A4,
	Common.WEAPON.AK47Custom,
	Common.WEAPON.HK416,
	Common.WEAPON.AK74U,
	Common.WEAPON.AKM,
	Common.WEAPON.L85A2,
	Common.WEAPON.FNFNC,
	Common.WEAPON.TAR21,
	Common.WEAPON.SCAR,
	Common.WEAPON.SKULL4,
	Common.WEAPON.OICW,
	Common.WEAPON.PlasmaGun,
	Common.WEAPON.StunRifle,
	Common.WEAPON.StarChaserAR,
	Common.WEAPON.CompoundBow,
	Common.WEAPON.LightningAR2,
	Common.WEAPON.Ethereal,
	Common.WEAPON.LightningAR1,
	Common.WEAPON.F2000,
	Common.WEAPON.Crossbow,
	Common.WEAPON.CrossbowAdvance,
	Common.WEAPON.M4A1DarkKnight,
	Common.WEAPON.AK47Paladin,
	-- 衝鋒槍列表
	Common.WEAPON.MAC10,
	Common.WEAPON.UMP45,
	Common.WEAPON.MP5,
	Common.WEAPON.TMP,
	Common.WEAPON.P90,
	Common.WEAPON.MP7A1ExtendedMag,
	Common.WEAPON.DualKriss,
	Common.WEAPON.KrissSuperV,
	Common.WEAPON.DualMP7A1,
	Common.WEAPON.Tempest,
	Common.WEAPON.TMPDragon,
	Common.WEAPON.P90Lapin,
	Common.WEAPON.DualUZI,
	Common.WEAPON.Needler,
	Common.WEAPON.InfinityLaserFist,
	-- 霰彈槍清單
	Common.WEAPON.M3,
	Common.WEAPON.XM1014,
	Common.WEAPON.DoubleBarrelShotgun,
	Common.WEAPON.WinchesterM1887,
	Common.WEAPON.USAS12,
	Common.WEAPON.JackHammer,
	Common.WEAPON.TripleBarrelShotgun,
	Common.WEAPON.SPAS12Maverick,
	Common.WEAPON.FireVulcan,
	Common.WEAPON.BALROGXI,
	Common.WEAPON.BOUNCER,
	Common.WEAPON.FlameJackhammer,
	Common.WEAPON.RailCannon,
	Common.WEAPON.LightningSG1,
	Common.WEAPON.USAS12CAMO,
	Common.WEAPON.WinchesterM1887Gold,
	Common.WEAPON.UTS15PinkGold,
	Common.WEAPON.Volcano,
	-- 機槍清單
	Common.WEAPON.M249,
	Common.WEAPON.MG3,
	Common.WEAPON.M134Minigun,
	Common.WEAPON.MG36,
	Common.WEAPON.MK48,
	Common.WEAPON.K3,
	Common.WEAPON.QBB95,
	Common.WEAPON.QBB95AdditionalMag,
	Common.WEAPON.BALROGVII,
	Common.WEAPON.MG3CSOGSEdition,
	Common.WEAPON.CHARGER7,
	Common.WEAPON.ShiningHeartRod,
	Common.WEAPON.Coilgun,
	Common.WEAPON.Aeolis,
	Common.WEAPON.BroadDivine,
	Common.WEAPON.LaserMinigun,
	Common.WEAPON.M249Phoenix,
	-- 裝備武器清單
	Common.WEAPON.M32MGL,
	Common.WEAPON.PetrolBoomer,
	Common.WEAPON.Slasher,
	Common.WEAPON.Eruptor,
	Common.WEAPON.Leviathan,
	Common.WEAPON.Salamander,
	Common.WEAPON.RPG7,
	Common.WEAPON.M32MGLVenom,
	Common.WEAPON.Stinger,
	Common.WEAPON.MagnumDrill,
	Common.WEAPON.GaeBolg,
	Common.WEAPON.Ripper,
	Common.WEAPON.BlackDragonCannon,
	Common.WEAPON.Guillotine
}


--[[ 修改每種武器類型的屬性 ]]


-- 次要武器屬性
option = Common.GetWeaponOption(Common.WEAPON.P228)
option.price = 100 -- 武器購買價格
option.damage = 1.0 -- 當與 Weapon 類別一起設定時，兩者都會相乘。
option.penetration = 1.0 -- 滲透率
option.rangemod = 1.0 -- 基於距離的傷害衰減率
option.cycletime = 1.0 -- 開火率
option.reloadtime = 1.0 -- 重新載入速度
option.accuracy = 1.0 -- 準確度
option.spread = 1.0 -- 執行操作時準確度降低的程度
option:SetBulletColor({r = 255, g = 255, b = 50}); -- 開槍時，會出現指定顏色的射擊路徑。
option.user.grade = WeaponGrade.normal -- 預先定義每種武器類型的最低等級
option.user.level = 1 -- 購買武器時的等級限制

-- 武器屬性詳細設定：武器id、價格、等級、可用等級、R、G、B
function SetOption(weaponid, price, grade, level, red, green, blue)
	option = Common.GetWeaponOption(weaponid)
	option.price = price
	option.user.grade = grade
	option.user.level = level

	if red ~= nil then
		option:SetBulletColor({r = red, g = green, b = blue});
	end
end

SetOption(Common.WEAPON.DualBeretta,		100, WeaponGrade.normal, 3, 255, 255, 50)
SetOption(Common.WEAPON.FiveSeven,			100, WeaponGrade.normal, 1, 255, 255, 50)
SetOption(Common.WEAPON.Glock18C,			100, WeaponGrade.normal, 1, 255, 255, 50)
SetOption(Common.WEAPON.USP45,				100, WeaponGrade.normal, 1, 255, 255, 50)
SetOption(Common.WEAPON.DesertEagle50C,		500, WeaponGrade.normal, 5, 255, 255, 50)
SetOption(Common.WEAPON.DualInfinity,		500, WeaponGrade.normal, 3, 255, 255, 50)
SetOption(Common.WEAPON.DualInfinityCustom,	800, WeaponGrade.normal, 4, 255, 255, 50)
SetOption(Common.WEAPON.DualInfinityFinal,	1500, WeaponGrade.normal, 7, 255, 255, 50)
SetOption(Common.WEAPON.SawedOffM79,		1000, WeaponGrade.normal, 5)
SetOption(Common.WEAPON.Cyclone,			2000, WeaponGrade.unique, 8)
SetOption(Common.WEAPON.AttackM950,			700, WeaponGrade.unique, 5, 255, 255, 50)
SetOption(Common.WEAPON.DesertEagle50CGold,	700, WeaponGrade.unique, 5, 255, 255, 50)
SetOption(Common.WEAPON.ThunderGhostWalker,			5000, WeaponGrade.legend, 7)
SetOption(Common.WEAPON.PythonDesperado,			8000, WeaponGrade.legend, 15, 255, 255, 50)
SetOption(Common.WEAPON.DesertEagleCrimsonHunter,	7000, WeaponGrade.legend, 12, 255, 255, 50)
SetOption(Common.WEAPON.DualBerettaGunslinger,		20000, WeaponGrade.legend, 30, 255, 255, 50)

-- 步槍屬性
SetOption(Common.WEAPON.Galil,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.FAMAS,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.M4A1,		700, WeaponGrade.normal, 5, 255, 128, 0)
SetOption(Common.WEAPON.SG552,		700, WeaponGrade.normal, 5, 255, 128, 0)
SetOption(Common.WEAPON.AK47,		700, WeaponGrade.normal, 5, 255, 128, 0)
SetOption(Common.WEAPON.AUG,		700, WeaponGrade.normal, 5, 255, 128, 0)
SetOption(Common.WEAPON.AN94,		500, WeaponGrade.normal, 1, 255, 128, 0)
SetOption(Common.WEAPON.M16A4,		500, WeaponGrade.normal, 1, 255, 128, 0)
SetOption(Common.WEAPON.AK47Custom,	15000, WeaponGrade.normal, 20, 255, 128, 0)
SetOption(Common.WEAPON.HK416,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.AK74U,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.AKM,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.L85A2,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.FNFNC,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.TAR21,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.SCAR,		500, WeaponGrade.normal, 2, 255, 128, 0)
SetOption(Common.WEAPON.SKULL4,				5000, WeaponGrade.rare, 10, 255, 128, 0)
SetOption(Common.WEAPON.OICW,				1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.PlasmaGun,			1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.StunRifle,			7000, WeaponGrade.unique, 15, 255, 128, 0)
SetOption(Common.WEAPON.StarChaserAR,		8000, WeaponGrade.unique, 20, 255, 128, 0)
SetOption(Common.WEAPON.CompoundBow,		1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.LightningAR2,		1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.Ethereal,			1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.LightningAR1,		1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.F2000,				1000, WeaponGrade.unique, 5, 255, 128, 0)
SetOption(Common.WEAPON.Crossbow,			1000, WeaponGrade.legend, 8, 255, 128, 0)
SetOption(Common.WEAPON.CrossbowAdvance,	2600, WeaponGrade.legend, 8, 255, 128, 0)
SetOption(Common.WEAPON.M4A1DarkKnight,		20000, WeaponGrade.legend, 25, 255, 128, 0)
SetOption(Common.WEAPON.AK47Paladin,		20000, WeaponGrade.legend, 25, 255, 128, 0)

-- 衝鋒槍屬性
SetOption(Common.WEAPON.MAC10,					300, WeaponGrade.normal, 2, 128, 255, 255)
SetOption(Common.WEAPON.UMP45,					300, WeaponGrade.normal, 2, 128, 255, 255)
SetOption(Common.WEAPON.MP5,					300, WeaponGrade.normal, 3, 128, 255, 255)
SetOption(Common.WEAPON.TMP,					300, WeaponGrade.normal, 1, 128, 255, 255)
SetOption(Common.WEAPON.P90,					300, WeaponGrade.normal, 1, 128, 255, 255)
SetOption(Common.WEAPON.MP7A1ExtendedMag,		2000, WeaponGrade.normal, 10, 128, 255, 255)
SetOption(Common.WEAPON.DualKriss,				500, WeaponGrade.normal, 5, 128, 255, 255)
SetOption(Common.WEAPON.KrissSuperV,			300, WeaponGrade.normal, 3, 128, 255, 255)
SetOption(Common.WEAPON.DualMP7A1,				500, WeaponGrade.normal, 5, 128, 255, 255)
SetOption(Common.WEAPON.Tempest,				1000, WeaponGrade.unique, 6, 128, 255, 255)
SetOption(Common.WEAPON.TMPDragon,				800, WeaponGrade.unique, 3, 128, 255, 255)
SetOption(Common.WEAPON.P90Lapin,				800, WeaponGrade.unique, 3, 128, 255, 255)
SetOption(Common.WEAPON.DualUZI,				1200, WeaponGrade.unique, 4, 128, 255, 255)
SetOption(Common.WEAPON.Needler,				1000, WeaponGrade.unique, 3, 128, 255, 255)
SetOption(Common.WEAPON.InfinityLaserFist,		20000, WeaponGrade.legend, 25)

-- 霰彈槍屬性
SetOption(Common.WEAPON.M3,						300, WeaponGrade.normal, 3, 50, 255, 50)
SetOption(Common.WEAPON.XM1014,					500, WeaponGrade.normal, 4, 50, 255, 50)
SetOption(Common.WEAPON.DoubleBarrelShotgun,	200, WeaponGrade.normal, 1, 50, 255, 50)
SetOption(Common.WEAPON.WinchesterM1887,		500, WeaponGrade.normal, 4, 50, 255, 50)
SetOption(Common.WEAPON.USAS12,					700, WeaponGrade.normal, 5, 50, 255, 50)
SetOption(Common.WEAPON.JackHammer,				500, WeaponGrade.normal, 3, 50, 255, 50)
SetOption(Common.WEAPON.TripleBarrelShotgun,	600, WeaponGrade.normal, 4, 50, 255, 50)
SetOption(Common.WEAPON.SPAS12Maverick,			1200, WeaponGrade.rare, 7, 50, 255, 50)
SetOption(Common.WEAPON.FireVulcan,				3000, WeaponGrade.rare, 5, 50, 255, 50)
SetOption(Common.WEAPON.BALROGXI,				10000, WeaponGrade.rare, 12, 50, 255, 50)
SetOption(Common.WEAPON.BOUNCER,				20000, WeaponGrade.unique, 14)
SetOption(Common.WEAPON.FlameJackhammer,		3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.RailCannon,				3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.LightningSG1,			3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.USAS12CAMO,				3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.WinchesterM1887Gold,	3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.UTS15PinkGold,			3000, WeaponGrade.unique, 5, 50, 255, 50)
SetOption(Common.WEAPON.Volcano,				15000, WeaponGrade.legend, 14, 50, 255, 50)


-- 機槍屬性
SetOption(Common.WEAPON.M249,			600, WeaponGrade.normal, 3, 255, 50, 255)
SetOption(Common.WEAPON.MG3,			1500, WeaponGrade.normal, 7, 255, 50, 255)
SetOption(Common.WEAPON.M134Minigun,	1000, WeaponGrade.normal, 3, 255, 50, 255)
SetOption(Common.WEAPON.MG36,			1000, WeaponGrade.normal, 5, 255, 50, 255)
SetOption(Common.WEAPON.MK48,			1000, WeaponGrade.normal, 5, 255, 50, 255)
SetOption(Common.WEAPON.K3,				600, WeaponGrade.normal, 5, 255, 50, 255)
SetOption(Common.WEAPON.QBB95,			800, WeaponGrade.normal, 7, 255, 50, 255)
SetOption(Common.WEAPON.QBB95AdditionalMag,		1000, WeaponGrade.normal, 7, 255, 50, 255)
SetOption(Common.WEAPON.BALROGVII,				2500, WeaponGrade.rare, 7, 255, 50, 255)
SetOption(Common.WEAPON.MG3CSOGSEdition,		2500, WeaponGrade.rare, 7, 255, 50, 255)
SetOption(Common.WEAPON.CHARGER7,				5000, WeaponGrade.rare, 7, 255, 50, 255)
SetOption(Common.WEAPON.ShiningHeartRod,		8000, WeaponGrade.unique, 12, 255, 50, 255)
SetOption(Common.WEAPON.Coilgun,				5000, WeaponGrade.unique, 7, 255, 50, 255)
SetOption(Common.WEAPON.Aeolis,					5000, WeaponGrade.unique, 7, 255, 50, 255)
SetOption(Common.WEAPON.BroadDivine,			7000, WeaponGrade.unique, 10, 255, 50, 255)
SetOption(Common.WEAPON.LaserMinigun,			7000, WeaponGrade.unique, 10)
SetOption(Common.WEAPON.M249Phoenix,			30000, WeaponGrade.legend, 25, 255, 50, 255)


-- 裝備武器屬性
SetOption(Common.WEAPON.M32MGL,			3000, WeaponGrade.normal, 5)
SetOption(Common.WEAPON.PetrolBoomer,	3000, WeaponGrade.normal, 5)
SetOption(Common.WEAPON.Slasher,		3000, WeaponGrade.normal, 5)
SetOption(Common.WEAPON.Eruptor,		300, WeaponGrade.normal, 1)
SetOption(Common.WEAPON.Leviathan,		2000, WeaponGrade.normal, 7)
SetOption(Common.WEAPON.Salamander,		2000, WeaponGrade.normal, 7)
SetOption(Common.WEAPON.RPG7,			1200, WeaponGrade.normal, 3)
SetOption(Common.WEAPON.M32MGLVenom,	5000, WeaponGrade.unique, 15)
SetOption(Common.WEAPON.Stinger,		3000, WeaponGrade.unique, 3)
SetOption(Common.WEAPON.MagnumDrill,	20000, WeaponGrade.legend, 25, 50, 50, 255)
SetOption(Common.WEAPON.GaeBolg,		10000, WeaponGrade.legend, 15)
SetOption(Common.WEAPON.Ripper,			15000, WeaponGrade.legend, 20)
SetOption(Common.WEAPON.BlackDragonCannon,		10000, WeaponGrade.legend, 7)
SetOption(Common.WEAPON.Guillotine,		10000, WeaponGrade.legend, 10)
```