---
wiki: csolua
title: 'Common'
---

該模組包含用於在{% mark color:red 客戶端 %}和{% mark color:red 伺服器 %}上一起運行的腳本的功能。
應該安裝在「 project.json 」的{% mark color:blue game %}和{% mark color:blue ui %}陣列中。

可用性：遊戲腳本、UI 腳本
訪問：唯讀

## Functions

### GetWeaponOption

{% mark color:blue function Common.GetWeaponOption (weaponid) %}

獲取武器 ID 的 [Common.WeaponOption](../common/common.weaponoption) 實例。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- weaponid: Common.WEAPON
<!-- cell -->
**返回值**
- [Common.WeaponOption](../common/common.weaponoption)
{% endgrid %}

### UseWeaponInven

{% mark color:blue function Common.UseWeaponInven (value) %}

啟用/禁用玩家的武器庫存功能。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### SetSaveCurrentWeapons

{% mark color:blue function Common.SetSaveCurrentWeapons (value) %}

玩家在死亡時保留當前裝備的武器。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### SetSaveWeaponInven

{% mark color:blue function Common.SetSaveWeaponInven (value) %}

創建新遊戲存檔時保存玩家的武器庫存內容。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### SetAutoLoad

{% mark color:blue function Common.SetAutoLoad (value) %}

啟用/禁用首次生成時自動載入玩家的遊戲存檔。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### DisableWeaponParts

{% mark color:blue function Common.DisableWeaponParts (value) %}

啟用/禁用武器的裝備部件（如火炮提升、速度、暴擊等）。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### DisableWeaponEnhance

{% mark color:blue function Common.DisableWeaponEnhance (value) %}

啟用/禁用武器的強化升級（如+3、+4、+6等）。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### DontGiveDefaultItems

{% mark color:blue function Common.DontGiveDefaultItems (value) %}

防止將預設物品（如 USP-45）提供給生成的玩家。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### DontCheckTeamKill

{% mark color:blue function Common.DontCheckTeamKill (value) %}

殺死隊友時給予正面分數。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### UseScenarioBuymenu

{% mark color:blue function Common.UseScenarioBuymenu (value) %}

將預設的死亡競賽風格武器商店選單替換為經典商店選單。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### SetNeedMoney

{% mark color:blue function Common.SetNeedMoney (value) %}

在武器商店功能表中啟用/禁用武器的價格。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}


### UseAdvancedMuzzle

{% mark color:blue function Common.UseAdvancedMuzzle (value) %}

啟用/禁用先進武器的射擊槍口。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**bool**) {% mark color:green true/false %}
{% endgrid %}

### SetMuzzleScale

{% mark color:blue function Common.SetMuzzleScale (value) %}

設置武器射擊槍口的尺寸比例。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetBloodScale

{% mark color:blue function Common.SetBloodScale (value) %}

設置怪物或玩家被擊中時噴血的大小比例。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetGunsparkScale

{% mark color:blue function Common.SetGunsparkScale (value) %}

設置武器子彈彈跳火花的擴散比例。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetHitboxScale

{% mark color:blue function Common.SetHitboxScale (value) %}

設置實體命中框的大小比例。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetMouseoverOutline

{% mark color:blue function Common.SetMouseoverOutline (value, color) %}

當玩家的滑鼠指標懸停在實體（如怪物、玩家）上時，繪製可見的輪廓。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
- color: (**color**)
{% endgrid %}

### SetUnitedPrimaryAmmoPrice

{% mark color:blue function Common.SetUnitedPrimaryAmmoPrice (value) %}

將全球價格設置為所有主要武器彈匣，每個彈匣的成本。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetUnitedSecondaryAmmoPrice

{% mark color:blue function Common.SetUnitedSecondaryAmmoPrice (value) %}

將全球價格設置為所有副武器彈匣，每個彈匣的成本。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- value: (**int**)
{% endgrid %}

### SetBuymenuWeaponList

{% mark color:blue function Common.SetBuymenuWeaponList (list) %}

在武器商店功能表中設置要購買的可用武器。

{% grid bg:box c:1 %}
<!-- cell -->
**參數:**
- list: (**table**)
{% endgrid %}

## Types

### WEAPON
{% quot 更多武器ID網站 %}

{% link https://cso-tw-goldfish.github.io/CSO-Weapon/ %}

`Common.Weapon`

允許生成和配置的武器。


|武器ID|武器名稱|
|------|-------|
|P228 | (**int**) (Pistol) 228 COMPACT.|
|Scout | (**int**) (Sniper Rifle) SCHMIDT SCOUT.|
|XM1014 | (**int**) (Shotgun) LEONE YG1265 AUTO SHOTGUN.|
|MAC10 | (**int**) (Sub-machine Gun) INGRAM MAC-10.|
|AUG	| (**int**) (Assault Rifle) BULLPUP.|
|DualBeretta	| (**int**) (Pistol) .40 DUAL ELITES.|
|FiveSeven | (**int**) (Pistol) ES FIVE-SEVEN.|
|UMP45 | (**int**) (Sub-machine Gun) K&M UMP45.|
|SG550Commando | (**int**) (Sniper Rifle) KRIEG 550 COMMANDO.|
|Galil | (**int**) (Assault Rifle) IDF DEFENDER.|
|FAMAS | (**int**) (Assault Rifle) CLARION 5.56.|
|USP45 | (**int**) (Pistol) K&M .45 TACTICAL.|
|Glock18C | (**int**) (Pistol) 9X19MM SIDEARM.|
|AWP | (**int**) (Sniper Rifle) MAGNUM SNIPER RIFLE.|
|MP5 | (**int**) (Sub-machine Gun) K&M SUB-MACHINE GUN.|
|M249 | (**int**) (Machine Gun) M249.|
|M3 | (**int**) (Shotgun) LEONE 12 GAUGE SUPER.|
|M4A1 | (**int**) (Assault Rifle) MAVERICK M4A1 CARBINE.|
|TMP | (**int**) (Sub-machine Gun) SCHMIDT MACHINE PISTOL.|
|G3SG1 | (**int**) (Sniper Rifle) D3/AU-1.|
|DesertEagle50C | (**int**) (Pistol) NIGHT HAWK .50C.|
|SG552 | (**int**) (Assault Rifle) KRIEG 552.|
|AK47 | (**int**) (Assault Rifle) CV-47.|
|P90 | (**int**) (Sub-machine Gun) ES C90.|
|SCAR | (**int**) (Assault Rifle) SCAR-L.|
|USAS12 | (**int**) (Shotgun) USAS-12.|
|QBB95 | (**int**) (Machine Gun) QBB-95.|
|MG3 | (**int**) (Machine Gun) MG3.|
|DualMP7A1 | (**int**) (Sub-machine Gun) Dual MP7A1.|
|AK47Custom | (**int**) (Assault Rifle) AK-47 60R.|
|DesertEagle50CGold | (**int**) (Pistol) Desert Eagle 50C Gold.|
|WinchesterM1887 | (**int**) (Shotgun) Winchester M1887.|
|M134Minigun | (**int**) (Machine Gun) M134 Minigun.|
|F2000 | (**int**) (Assault Rifle) F2000.|
|WinchesterM1887Gold | (**int**) (Shotgun) Winchester M1887 Gold.|
|LightningAR1 | (**int**) (Assault Rifle) Lightning AR-1.|
|M24 | (**int**) (Sniper Rifle) M24.|
|DualInfinity | (**int**) (Pistol) Dual Infinity.|
|DualInfinityCustom | (**int**) (Pistol) Dual Infinity Custom.|
|QBB95AdditionalMag | (**int**) (Machine Gun) QBB-95 EX.|
|MP7A1ExtendedMag | (**int**) (Sub-machine Gun) MP7A1 60R.|
|SawedOffM79 | (**int**) (Pistol) M79 Saw Off.|
|DualInfinityFinal | (**int**) (Pistol) Dual Infinity Final.|
|Crossbow | (**int**) (Assault Rifle) Crossbow.|
|USAS12CAMO | (**int**) (Shotgun) USAS-12 CAMO.|
|DoubleBarrelShotgun | (**int**) (Shotgun) Double Barrel Shotgun.|
|KrissSuperV | (**int**) (Sub-machine Gun) Kriss Super V.|
|TAR21 | (**int**) (Assault Rifle) TAR-21.|
|BarrettM95 | (**int**) (Sniper Rifle) Barrett M95|
|DualKriss | (**int**) (Sub-machine Gun) Dual Kriss Super V.|
|AN94 | (**int**) (Assault Rifle) AN-94.|
|M16A4 | (**int**) (Assault Rifle) M16A4.|
|P90Lapin | (**int**) (Sub-machine Gun) P90 Lapin.|
|Volcano | (**int**) (Shotgun) Volcano.|
|MG36 | (**int**) (Machine Gun) MG36.|
|Salamander | (**int**) (Equipment) Salamander.|
|LightningSG1 | (**int**) (Shotgun) Lightning SG-1.|
|Tempest | (**int**) (Sub-machine Gun) Tempest.|
|BlackDragonCannon | (**int**) (Equipment) Black Dragon Cannon.|
|TMPDragon | (**int**) (Sub-machine Gun) TMP Dragon.|
|MK48 | (**int**) (Machine Gun) MK48.|
|FNFNC | (**int**) (Assault Rifle) FN FNC.|
|L85A2 | (**int**) (Assault Rifle) L85A2.|
|AKM | (**int**) (Assault Rifle) AKM.|
|HK416 | (**int**) (Assault Rifle) HK416.|
|LightningAR2 | (**int**) (Assault Rifle) Lightning AR-2.|
|Ethereal | (**int**) (Assault Rifle) Ethereal.|
|M32MGL | (**int**) (Equipment) M32 MGL.|
|BALROGVII | (**int**) (Machine Gun) BALROG-VII.|
|OICW | (**int**) (Assault Rifle) OICW.|
|TripleBarrelShotgun | (**int**) (Shotgun) Triple Barrel Shotgun.|
|Ripper | (**int**) (Equipment) Ripper.|
|K3 | (**int**) (Machine Gun) K3.|
|Needler | (**int**) (Sub-machine Gun) Needler.|
|SKULL4 | (**int**) (Assault Rifle) SKULL-4.|
|BALROGXI | (**int**) (Shotgun) BALROG-XI.|
|AK74U | (**int**) (Assault Rifle) AK-74U.|
|PlasmaGun | (**int**) (Assault Rifle) Plasma Gun.|
|Leviathan | (**int**) (Equipment) Leviathan.|
|UTS15PinkGold | (**int**) (Shotgun) UTS-15 Pink Gold.|
|CompoundBow | (**int**) (Assault Rifle) Compound Bow.|
|ARX160 | (**int**) (Assault Rifle) ARX-160.|
|GaeBolg | (**int**) (Equipment) Gae Bolg.|
|Cyclone | (**int**) (Pistol) Cyclone.|
|SPAS12Maverick | (**int**) (Shotgun) SPAS-12 Maverick.|
|Aeolis | (**int**) (Machine Gun) Aeolis.|
|PetrolBoomer | (**int**) (Equipment) Petrol Boomer.|
|RailCannon | (**int**) (Shotgun) Rail Cannon.|
|Eruptor | (**int**) (Equipment) Erupt Cannon.|
|Slasher | (**int**) (Equipment) Slasher.|
|RPG7 | (**int**) (Equipment) RPG-7.|
|Guillotine | (**int**) (Equipment) Blood Dripper.|
|CrossbowAdvance | (**int**) (Assault Rifle) Advanced Crossbow.|
|FireVulcan | (**int**) (Shotgun) Fire Vulcan.|
|JackHammer | (**int**) (Shotgun) Jack Hammer.|
|Coilgun | (**int**) (Machine Gun) Coil Gun.|
|DualUZI | (**int**) (Sub-machine Gun) Dual UZI.|
|LaserMinigun | (**int**) (Machine Gun) Laser Minigun.|
|M4A1DarkKnight | (**int**) (Assault Rifle) M4A1 Dark Knight.|
|AK47Paladin | (**int**) (Assault Rifle) AK-47 Paladin.|
|AttackM950 | (**int**) (Pistol) Attack M950.|
|MagnumDrill | (**int**) (Equipment) Magnum Drill.|
|DesertEagleCrimsonHunter | (**int**) (Pistol) Desert Eagle Crimson Hunter.|
|FlameJackhammer | (**int**) (Shotgun) Flame Jack Hammer.|
|SG552Lycanthrope | (**int**) (Assault Rifle) SG552 Lycanthrope.|
|BroadDivine | (**int**) (Machine Gun) Broad Divine|
|PythonDesperado | (**int**) (Pistol) Python Desperado.|
|CHARGER7 | (**int**) (Machine Gun) CHARGER-7.|
|BOUNCER | (**int**) (Shotgun) BOUNCER.|
|StunRifle | (**int**) (Assault Rifle) Stun Rifle.|
|DualBerettaGunslinger | (**int**) (Pistol) Dual Beretta Gunslinger.|
|M249Phoenix | (**int**) (Machine Gun) M249 Phoenix.|
|StarChaserAR | (**int**) (Assault Rifle) Star Chaser AR.|
|M32MGLVenom | (**int**) (Equipment) M32 MGL Venom.|
|MG3CSOGSEdition | (**int**) (Machine Gun) MG3 10th Edition.|
|ThunderGhostWalker | (**int**) (Pistol) Thunder Ghost Walker.|
|Stinger | (**int**) (Equipment) Stinger.|
|InfinityLaserFist | (**int**) (Sub-machine Gun) Infinity Laser Fist.|
|ShiningHeartRod | (**int**) (Machine Gun) Shining Heart Rod.|