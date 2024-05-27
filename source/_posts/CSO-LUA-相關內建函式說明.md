---
menu_id: blog
title: CSO LUA 相關內建函式說明
date: 2024-05-04 19:52:42
categories: CSO LUA
tags: [CSO LUA, CSO LUA小技巧]
---

## 目錄

- 關鍵字
- 數學運算子
- 字串相接運算子
- 邏輯運算子
- 關係運算子
- bitwise 位元運算子
- Lua Functions

## 關鍵字

```lua
and       break     do        else      elseif
end       false     for       function  if
in        local     nil       not       or
repeat    return    then      true      until     while
```

## 數學運算子

```lua
+  -  *  /  %  ^
```

```lua=
1 + 7  -- 輸出: 8
2 - 8  -- 輸出: -6
3 * 9  -- 輸出: 27
4 / 10 -- 輸出: 0.4
5 % 11 -- 輸出: 5
6 ^ 12 -- 輸出: 2176782336
```

## 字串相接運算子

```lua
..
```

```lua=
str = "Hello" .. "World" -- str = Hello World
```

## 邏輯運算子

```lua
and  -- 且
or   -- 或
not  -- 相反
```

```lua=
 true and  true -- 輸出: true
 true and false -- 輸出: false
 true or   true -- 輸出: true
 true or  false -- 輸出: true
false or  false -- 輸出: false
not true        -- 輸出: false
```

## 關係運算子


```lua
==  -- 相等
~=  -- 不相等

>   -- 大於
<   -- 小於

>=  -- 大於等於
<=  -- 小於等於
```

```lua=
111 == 111 -- 輸出:  true
222 ~= 222 -- 輸出: false
300 >  400 -- 輸出: false
300 <  400 -- 輸出:  true
400 >= 400 -- 輸出:  true
400 >= 401 -- 輸出: false
400 <= 400 -- 輸出:  true
400 <= 399 -- 輸出: false
```

## bitwise 位元運算子

```lua
& | ~ << >>
```

**AND**
```lua
3 & 5  -- 輸出: 1

算法:
     00000011
  &  00000101
-------------
     00000001
```

**OR**
```lua
3 | 5  -- 輸出: 7

算法:
     00000011
  |  00000101
-------------
     00000111
```

**XOR**
```lua
3 ~ 5  -- 輸出: 6

算法:
     00000011
  ~  00000101
-------------
     00000110
```

**Right Shift**
```lua
7 >> 1  -- 輸出: 3

算法:
0000 0111 -- 原本
0000 0011 -- 結果
```

**Left Shift**
```lua
7 << 1  -- 輸出: 14

算法:
0000 0111 -- 原本
0000 1110 -- 結果
```

**NOT**
```lua
~7    -- 輸出: -8
~10   -- 輸出: -11
~999  -- 輸出: -1000
```

## Lua Functions

**tonumber**： 轉 **整數** 資料型態
```lua
tonumber("12as") -- nil
tonumber("12")   -- 12
tonumber(123.3)  -- 123.3
tonumber(1230.0) -- 1230
tonumber(1230)   -- 1230
```

**tostring**： 轉 **字串** 資料型態
```lua
tostring("fafsaf!$") -- "fafsaf!$"
tostring(5606)       -- "5606"
tostring(125.15665)  -- "125.15665"
```

**type**： 識別 資料型態
```lua
a, b, c, d, e, f, g = 1, 2.3, "4", true, nil, {}, function() end
type(a) -- number
type(b) -- number
type(c) -- string
type(d) -- boolean
type(e) -- nil
type(f) -- table
type(g) -- function
```

**assert**： 檢查 異常錯誤，會導致Script錯誤
```lua
a, b, c, d, e, f, g = 1, 2.3, "4", true, nil, {}, function() end
a = 1; assert(a == 1) -- 自訂的錯誤訊息，不一定要
a = 2; assert(a == 1, "變數a只能是1") -- Script錯誤，控制台會拋出你自訂的錯誤訊息。
```

**error**： 異常錯誤訊息，會導致Script錯誤
```lua
pos = {x = 1, y = 1, z = 1}
if pos.x == 1 then
	error("座標X錯誤")
end
```

**setmetatable**： 元表，這有點難，可以做成類似class
```lua
setmetatable()
```
> 詳情 https://www.runoob.com/lua/lua-metatables.html

**getmetatable**： 返回對像元表
```lua
getmetatable()
```
> 詳情 https://www.runoob.com/lua/lua-metatables.html

**print**： 打印任何資料在控制台
```lua
print()
```

**log**： 在遊戲路徑/bin/script.log 打印任何資料
```lua
log()
```