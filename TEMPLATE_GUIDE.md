# 3D Web Game Creator Template

這個模板是單檔 Three.js 網頁遊戲架構，核心檔案是 `index.html`。可以直接用瀏覽器載入，也可以用本機 HTTP server 或靜態網站服務部署。

## 主要架構

- `GameApp`: 啟動 Three.js renderer、scene、camera、玩家、主循環、resize、fullscreen、`render_game_to_text` 和 `advanceTime`。
- `WorldRuntime`: 管理世界資料、Three.js mesh 建立、collider、trigger、spawn、destructible、interactive runtime logic。
- `UILayer`: 根據 UI JSON 產生 DOM HUD、loading、results 等畫面，並處理 score、health、message、action 等 binding。
- `CreatorBackend`: `Shift+C` 後台總入口，負責 World Builder / UI Editor 模式切換、專案 save/export/import/reset。
- `WorldBuilder`: 空間編輯器，提供格線吸附、放置、選取、拖曳移動、屬性面板、材質貼圖、物件列表、undo/redo。
- `UIEditor`: 介面編輯器，提供 UI 元件新增、選取框、拖曳、縮放、層級、樣式、圖片、screen preview、undo/redo。

## 編輯器入口

- `Shift + C`: 進入或離開創作後台。
- 後台上方可切換 `World Builder` 與 `UI Editor`。
- `Esc`: 離開後台。
- `Ctrl/Cmd + S`: 儲存。
- `Ctrl/Cmd + Z`: 復原。
- `Ctrl/Cmd + Y` 或 `Ctrl/Cmd + Shift + Z`: 重做。
- `Delete` / `Backspace`: 刪除目前選取物件或 UI 元件。

## World Builder 資料格式

世界資料存在 project JSON 的 `world` 欄位：

```json
{
  "schema": "template.world",
  "version": 1,
  "settings": {
    "gridSize": 0.5,
    "snap": true
  },
  "objects": [
    {
      "id": "floor_main",
      "type": "floor",
      "name": "Main Floor",
      "transform": {
        "position": { "x": 0, "y": -0.1, "z": 0 },
        "rotation": { "x": 0, "y": 0, "z": 0 },
        "scale": { "x": 1, "y": 1, "z": 1 }
      },
      "size": { "x": 16, "y": 0.2, "z": 16 },
      "material": {
        "color": "#4f8f9f",
        "opacity": 1,
        "skin": {
          "mode": "color",
          "src": "",
          "repeat": { "x": 1, "y": 1 },
          "singleFace": "pz",
          "faces": {}
        }
      },
      "flags": {
        "collidable": true,
        "destructible": false,
        "interactive": false,
        "trigger": false,
        "spawn": false
      },
      "logic": {
        "health": 1,
        "message": "",
        "text": ""
      }
    }
  ]
}
```

`type` 目前包含：`block`、`floor`、`wall`、`platform`、`obstacle`、`destructible`、`trigger`、`spawn`、`textSign`、`interactive`。

材質貼圖模式：

- `color`: 純色材質。
- `stretch`: 單張圖片拉伸到物件。
- `repeat`: 單張圖片重複平鋪。
- `single`: 只貼到指定面。
- `perFace`: 每個面可獨立指定圖片。

## UI Editor 資料格式

UI 資料存在 project JSON 的 `ui` 欄位：

```json
{
  "schema": "template.ui",
  "version": 1,
  "settings": {
    "activeScreen": "game"
  },
  "elements": [
    {
      "id": "ui_score",
      "type": "score",
      "screen": "game",
      "name": "Score",
      "x": 3,
      "y": 3,
      "w": 15,
      "h": 5,
      "z": 20,
      "visible": true,
      "locked": false,
      "text": "Score",
      "fontSize": 16,
      "color": "#eef4ff",
      "bgColor": "rgba(7,16,24,.72)",
      "opacity": 1,
      "image": "",
      "borderColor": "rgba(255,255,255,0.24)",
      "radius": 6,
      "bind": "score",
      "frameAspect": "16:9"
    }
  ]
}
```

UI `type` 目前包含：`text`、`button`、`statusBar`、`score`、`image`、`gameFrame`、`loadingPanel`、`resultPanel`、`controlButton`、`decorFrame`。

可用 binding：

- `none`: 純顯示。
- `score`: 顯示遊戲分數。
- `health`: 狀態條填充值。
- `message`: 顯示 runtime message。
- `action`: 呼叫玩家互動。
- `restart`: 重開遊戲狀態。

## 保存與部署

- 自動保存位置：`localStorage["webGameCreatorTemplate.project.v1"]`。
- 後台 `Save`: 手動寫入 localStorage。
- `Export Project`: 匯出完整 project JSON，包含 world 與 UI。
- `Import Project`: 匯入完整 project JSON。
- `Export World` / `Import World`: 只處理世界資料。
- `Export UI` / `Import UI`: 只處理 UI 資料。

部署方式：

1. 保留 `index.html`。
2. 若只使用 CDN three.js，可以直接放到任何靜態網站服務。
3. 本機測試建議用：

```powershell
python -m http.server 5173 --bind 127.0.0.1
```

4. 開啟 `http://127.0.0.1:5173`。

## 建立新遊戲的建議流程

1. 複製這個資料夾作為新遊戲專案。
2. 修改 `WORLD_TYPE_DEFS` 增加你的 prefab 類型。
3. 修改 `createDefaultWorld()` 做新遊戲的預設關卡。
4. 修改 `createDefaultUI()` 做新遊戲的預設 HUD、loading、results。
5. 在 `WorldRuntime.interact()`、`WorldRuntime.updateTriggers()`、`GameApp.update()` 補上你的遊戲規則。
6. 用 `Shift+C` 建立關卡與 UI，確認後 `Export Project` 保存成關卡 preset。
