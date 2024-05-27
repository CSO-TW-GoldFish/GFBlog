---
menu_id: blog
title: CSO LUA 自訂義UI物件
date: 2024-05-04 06:45:12
categories: CSO LUA
tags: [CSO LUA, CSO LUA小技巧]
---

當你想要在CSO LUA中建立更多元的UI物件時，官方僅提供了UI.Box和UI.Text兩個選項。但是，如果你希望有更多自定義的UI物件，你可以透過手刻Class物件來實現。這樣一來，你就能夠根據自己的需求來客製化UI物件了！

## Lua 中的物件導向

Lua 是一種輕量級的語言，沒有內建的 class 概念，但可以通過其他方式模擬類似的行為，例如使用表（table）和元表（metatable）來實現對象導向的編程。如果對此不太熟悉，可以參考一些線上資源，比如[這個網站](https://opensourcedoc.com/lua-programming/object/)。

## 文字陰影 UI 物件

假設我們要創建一個自定義的文字陰影 UI 物件。首先，讓我們看一下如果不使用 class 的話，程式碼會是怎麼樣的：

```lua=
TextShadow = UI.Text.Create()
TextShadow:Set({
    text = "哈囉", font = "small", align = "left",
    x = 2, --<< 偏移2點
    y = 2, --<< 偏移2點
    width = 100,
    height = 30,
    r = 255, g = 255, b = 255
})

Text   = UI.Text.Create()
Text:Set({
    text = "哈囉", font = "small", align = "left",
    x = 0,
    y = 0,
    width = 100,
    height = 30,
    r = 255, g = 255, b = 255
})
```

這樣的程式碼看起來相當冗長且不易維護。現在，讓我們看看如何使用 class 來實現同樣的功能。

### 定義 TextShadow Class

1. `TextShadow = {}`：這一行創建了一個名為 `TextShadow` 的空表，用於存儲自定義物件的方法和屬性。
2. `function TextShadow:Create()`：這一行定義了一個叫做 `Create` 的方法，該方法用於創建新的 `TextShadow` 物件。使用 : 語法定義的方法隱式地將物件自身作為第一個參數（類似於其他語言中的 this 或 self）。
3. 在 `Create` 方法中：
- 首先，創建了一個名為 `newObject` 的新表，用於存儲 `TextShadow` 物件的屬性和方法。
- 在 `newObject` 表中，設置了一個叫做 `SetArg` 的表，其中包含了一系列的屬性，用於設置陰影效果的參數。這些參數包括了文本內容 (text)、字體 (font)、對齊方式 (align)，，位置(x 和 y)，大小 (width 和 height)，顏色 (r、g、b 和 a) 以及偏移量 (offset)。
- 接著，創建了一個 `UI` 表，其中包含了兩個名為 `back` 和 `front` 的屬性，這些屬性分別表示陰影的背景和前景，並且使用了 `UI.Text.Create()` 方法來創建相應的文本 UI 物件。
- 最後，設置了一個名為 `visible` 的屬性，表示該陰影是否可見，並將其設置為預設值 `true`。

總的來說，這段程式碼創建了一個具有自定義屬性和方法的 `TextShadow` 物件，用於表示文本的陰影效果。通過這種方式，可以方便地創建。

> 來自 ChatGPT 說明

```lua=
-- 定義物件
TextShadow = {}

function TextShadow:Create()
    -- 自訂義物件內容並給預設值，內容怎樣都行。
    local newObject = {
        SetArg = {
            text = "",
            font = "small",
            align = "left",
            x = 0,
            y = 0,
            width = 0,
            height = 0,
            r = 255, g = 255, b = 255, a = 255,
            offset = 2
        },
        UI = {
            back = UI.Text.Create(),
            front = UI.Text.Create()
        },
        visible = true
    }
    
    return setmetatable(newObject, {__index = TextShadow})
end
```

上述程式碼定義了一個 TextShadow Class，其中包含了一個 Create 方法用於創建新的 TextShadow 物件。該物件包含了一個 SetArg 表，用於存儲設定參數，一個 UI 表用於存儲 UI 元件，以及一個 visible 屬性用於表示陰影是否可見。

### 更新 UI

接下來我們在定義一個Update函式，來更新我們的UI要怎麼設定。

1. `function TextShadow:Update()`：這一行定義了一個叫做 `Update` 的方法，該方法用於更新 `TextShadow` 物件的 UI。
2. `local arg = self.SetArg`：這一行將 `self.SetArg` 表示的陰影設置參數儲存在本地變數 `arg` 中，以便於後續使用。
3. `local setting = {...}`：這一行創建了一個名為 `setting` 的表，其中包含了陰影效果的設定。對於每個 UI 元素（back 和 front），設置了相應的位置（x 和 y）、顏色（r、g、b 和 a）等參數。
4. `for k,v in pairs(self.UI) do`：這一行使用 `pairs` 函數遍歷 `self.UI` 表中的所有元素，其中 k 是鍵（back 或 front），而 `v` 則是相應的值（UI.Text 物件）。
5. 在迴圈中，創建了一個名為 `temp` 的表，用於存儲更新後的 UI 參數。這些參數包括了位置 (x 和 y)，大小 (width 和 height)，顏色 (r、g、b 和 a)，以及文本相關的屬性（text、font、align）。
6. `v:Set(temp)`：這一行調用了 UI.Text 物件的 `Set` 方法，並將更新後的參數 `temp` 傳遞給該方法，從而更新了 UI 的屬性。

總的來說，這段程式碼是用來更新 TextShadow 物件的 UI，並根據設置的參數來更新相應的屬性，例如位置、大小、顏色等。這樣一來，可以在程式執行過程中動態地改變陰影效果的外觀。

> 來自 ChatGPT 說明

```lua=
-- 更新UI
function TextShadow:Update()
    local arg = self.SetArg
    local setting = {
        back  = {x = arg.offset, y = arg.offset, r = 0, g = 0, b = 0, a = arg.a},
        front = {x = 0, y = 0, r = arg.r, g = arg.g, b = arg.b, a = arg.a}
    }

    for k,v in pairs(self.UI) do
        local temp = {
            text = arg.text,
            font = arg.font,
            align = arg.align,

            x = arg.x + setting[k].x,
            y = arg.y + setting[k].y,
            width = arg.width,
            height = arg.height,

            r = setting[k].r,
            g = setting[k].g,
            b = setting[k].b,
            a = setting[k].a
        }
        v:Set(temp)
    end
end
```

上述程式碼定義了一個 Update 方法，用於更新 TextShadow 物件的 UI。該方法根據設定參數更新 UI 的各個屬性，包括位置、大小、顏色等。

### 其他必需的函式

1. `function TextShadow:Set(args)`：這一行定義了一個名為 `Set` 的方法，該方法用於設置陰影效果的屬性。首先，它檢查傳入的參數是否為一個表，如果是的話，則調用 `setArgs` 函數將該表中的值設置到 `self.SetArg` 中。最後，它調用了 `self:Update()` 方法來更新 UI。
2. `function TextShadow:Get()`：這一行定義了一個名為 `Get` 的方法，該方法用於取得陰影效果目前的設置值。它調用了 `clone` 函數來複製 `self.SetArg` 表，以防止直接返回引用。
3. `function TextShadow:Show()`：這一行定義了一個名為 `Show` 的方法，該方法用於顯示陰影效果的 UI。它調用了 `deepCall` 函數來遞迴地調用 UI 表中每個元素的 `Show` 方法，以顯示相應的 UI。最後，將 `self.visible` 設置為 true，表示陰影效果已經顯示。
4. `function TextShadow:Hide()`：這一行定義了一個名為 `Hide` 的方法，該方法用於隱藏陰影效果的 UI。它同樣調用了 `deepCall` 函數來遞迴地調用 `UI` 表中每個元素的 `Hide` 方法，以隱藏相應的 UI。最後，將 `self.visible` 設置為 false，表示陰影效果已經隱藏。
5. `function TextShadow:IsVisible()`：這一行定義了一個名為 `IsVisible` 的方法，該方法用於返回陰影效果是否可見的狀態。它簡單地返回 `self.visible` 的值，以表示陰影效果目前是否處於顯示狀態。

總的來說，這些方法為 TextShadow 物件提供了設置、取得、顯示和隱藏陰影效果的功能，並提供了一個方法來查詢陰影效果目前是否可見。

> 來自 ChatGPT 說明

```lua=
-- 設置
function TextShadow:Set(args)
    if type(args) == "table" then
        setArgs(self.SetArg, args)
    end
    self:Update()
end

-- 取得設定值
function TextShadow:Get()
    return clone(self.SetArg)
end

-- 顯示
function TextShadow:Show()
    deepCall(self.UI, "Show")
    self.visible = true
end

-- 隱藏
function TextShadow:Hide()
    deepCall(self.UI, "Hide")
    self.visible = false
end

-- 返回顯示狀態
function TextShadow:IsVisible()
    return self.visible
end
```

上述程式碼定義了一些必需的函式，包括設置、取得設定值、顯示、隱藏和返回顯示狀態等功能。這些函式使得 TextShadow 物件更加易用和靈活。

### 輔助函式

1. `local function setArgs(data, args)`：這是一個局部函數，用於更新表中的值。它接受兩個參數，`data` 表示待更新的表，`args` 表示包含新值的表。該函數通過遍歷 `data` 表中的每個鍵，檢查是否存在相應的 args 表中的值，如果是的話，則將 `data` 表中的值更新為 `args` 表中的對應值。如果鍵對應的值是一個表，則遞迴調用 `setArgs` 函數以處理嵌套的表。
2. `local function deepCall(table, funcName, args)`：這是一個局部函數，用於在表中深層遞迴地調用指定的函式。它接受三個參數，`table` 表示待遍歷的表，`funcName` 表示要調用的函式名稱，`args` 表示要傳遞給該函式的參數。該函數遍歷 `table` 表中的每個元素，如果元素是一個表，則遞迴調用 `deepCall` 函數以處理嵌套的表；如果元素是一個 userdata，則調用該 userdata 對象的指定函式，並傳遞參數 `args`。
3. `local function clone(table)`：這是一個局部函數，用於深層複製表。它接受一個表作為參數，並返回該表的深層複製。該函數遍歷原始表中的每個鍵值對，如果值是一個表，則遞迴調用自身以處理嵌套的表；否則，將鍵值對直接複製到新的表中。

這些輔助函數提供了對表的操作，使得程式碼更加模組化和易於理解。通過這些函數，可以更方便地處理複雜的數據結構和執行深層遞迴操作。

> 來自 ChatGPT 說明

```lua=
-- 更新設定值
local function setArgs(data, args)
    for k in pairs(data) do
        if type(data[k]) == type(args[k]) then
            if type(data[k]) == "table" then
                setArgs(data[k], args[k])
            else
                data[k] = args[k]
            end
        end
    end
end

-- 深層調用，調用指定函式
local function deepCall(table, funcName, args)
    for _,v in pairs(table) do
        if type(v) == "table" then
            deepCall(v, funcName, args)
        elseif type(v) == "userdata" then
            v[funcName](v, args)
        end
    end
end

-- 複製表(table)
local function clone(table)
    local temp = {}
    for k,v in pairs(table) do
        if type(v) == "table" then
            temp[k] = clone(v)
        else
            temp[k] = v
        end
    end
    return temp
end
```

上述程式碼定義了三個輔助函式，分別是更新設定值的 setArgs、深層調用的 deepCall 和複製表的 clone。這些函式用於處理表的操作，使得程式碼更加模組化和易於理解。

### 完整代碼

```lua=
-- 更新設定值
local function setArgs(data, args)
    for k in pairs(data) do
        if type(data[k]) == type(args[k]) then
            if type(data[k]) == "table" then
                setArgs(data[k], args[k])
            else
                data[k] = args[k]
            end
        end
    end
end

-- 深層調用，調用指定函式
local function deepCall(table, funcName, args)
    for _,v in pairs(table) do
        if type(v) == "table" then
            deepCall(v, funcName, args)
        elseif type(v) == "userdata" then
            v[funcName](v, args)
        end
    end
end

-- 複製表(table)
local function clone(table)
    local temp = {}
    for k,v in pairs(table) do
        if type(v) == "table" then
            temp[k] = clone(v)
        else
            temp[k] = v
        end
    end
    return temp
end

-- 定義物件
TextShadow = {}

function TextShadow:Create()
    -- 自訂義物件內容並給預設值，內容怎樣都行。
    local newObject = {
        SetArg = {
            text = "",
            font = "small",
            align = "left",
            x = 0,
            y = 0,
            width = 0,
            height = 0,
            r = 255, g = 255, b = 255, a = 255,
            offset = 2
        },
        UI = {
            back = UI.Text.Create(),
            front = UI.Text.Create()
        },
        visible = true
    }

    return setmetatable(newObject, {__index = TextShadow})
end


-- 更新UI
function TextShadow:Update()
    local arg = self.SetArg
    local setting = {
        back  = {x = arg.offset, y = arg.offset, r = 0, g = 0, b = 0, a = arg.a},
        front = {x = 0, y = 0, r = arg.r, g = arg.g, b = arg.b, a = arg.a}
    }

    for k,v in pairs(self.UI) do
        local temp = {
            text = arg.text,
            font = arg.font,
            align = arg.align,

            x = arg.x + setting[k].x,
            y = arg.y + setting[k].y,
            width = arg.width,
            height = arg.height,

            r = setting[k].r,
            g = setting[k].g,
            b = setting[k].b,
            a = setting[k].a
        }
        v:Set(temp)
    end
end

-- 設置
function TextShadow:Set(args)
    if type(args) == "table" then
        setArgs(self.SetArg, args)
    end
    self:Update()
end

-- 取得設定值
function TextShadow:Get()
    return clone(self.SetArg)
end

-- 顯示
function TextShadow:Show()
    deepCall(self.UI, "Show")
    self.visible = true
end

-- 隱藏
function TextShadow:Hide()
    deepCall(self.UI, "Hide")
    self.visible = false
end

-- 返回顯示狀態
function TextShadow:IsVisible()
    return self.visible
end


-- 測試
local screen = UI.ScreenSize()
local center = {x = screen.width / 2, y = screen.height / 2}

text = TextShadow:Create()
text:Set({
    text = "哈囉 你好~",
    font = "medium",
    align = "center",
    x = 0,
    y = center.y,
    width = screen.width,
    height = 50,
    r = 0, g = 128, b = 255,
    offset = 5
})
```

上述是完整的程式碼範例，展示了如何使用 Class 物件來建立自定義的文字陰影 UI 物件。這樣的設計使得程式碼更加模組化和易於理解，同時也提供了更好的可擴展性和重用性。