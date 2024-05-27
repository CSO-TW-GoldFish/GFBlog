---
menu_id: blog
title: CSO LUA 檔案建置與配置
date: 2024-05-04 06:49:22
categories: CSO LUA
tags: [CSO LUA, CSO LUA小技巧]
---

## 🛠️建置

**建議使用視窗化建立。**

初次使用 LUA，應從遊戲內進行創建，因為檔案位置可能會變動，導致無法找到檔案。

{% box color:blue %}
如果在讀取檔案時找不到檔案，可能是因為官方已更改儲存位置。此時，只需按下「新建立」按鈕即可。
{% endbox %}

**建立方法有兩種：**
- 在遊戲內按下「新建立」按鈕。
- 如果已知道檔案儲存路徑，可以直接在相應資料夾內建立。

## 📑配置

在讀取檔案時，只能讀取 project.json 檔案。

### 讀取的方式

當遊戲房間初次連線時，會執行一次 game.lua 檔案。之後每位玩家連線後，都會執行 ui.lua 檔案。房間內有幾個玩家就會讀取幾次 ui.lua 檔案。

預設情況下，配置如下所示
```json
{
    "game":[
        "game.lua"
    ],
    "ui":[
        "ui.lua"
    ]
}
```

### 讀取檔案的順序

檔案會按照配置中的順序進行讀取。在 game 部分，會先讀取 common.lua 檔案，再讀取 game.lua；而在 ui 部分，則會先讀取 common.lua 檔案，再讀取 ui.lua。

後面讀取的檔案中，前面的檔案所定義的變數是可以被後面的檔案所使用的。

> 若使用 local 變數，則只能在當前檔案中使用，無法跨檔案使用。

```json
{
    "game":[
        "common.lua",
        "game.lua"
    ],
    "ui":[
        "common.lua",
        "ui.lua"
    ]
}
```

### 使用Common功能

若需要使用 Common 功能，需另外新增一個檔案，命名為 common.lua，然後在 project.json 檔案中進行配置。

只需建立一個 common.lua 檔案即可使用 Common 功能，但前提是要在 game 和 ui 中同時讀取。

> common.lua 的名稱可以自訂，不一定要命名為 common.lua。

```json
{
    "game":[
        "common.lua",
        "game.lua"
    ],
    "ui":[
        "common.lua",
        "ui.lua"
    ]
}
```

**不理想的寫法**

下方想要使用Common的功能，但讀取的檔案卻不一樣，這樣的話common.lua和data.lua，都需要寫出相同的Common功能，才能使用。

取同名稱的檔案可以節省檔案並方便管理。

```json
{
    "game":[
        "data.lua",
        "game.lua"
    ],
    "ui":[
        "common.lua",
        "ui.lua"
    ]
}
```