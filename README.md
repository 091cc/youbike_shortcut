# YouBike Status（iOS 捷徑）

> [🇹🇼正體中文](#%E6%AD%A3%E9%AB%94%E4%B8%AD%E6%96%87) ・ [🇺🇸/🇬🇧English](#english)

## 🇹🇼 正體中文

查詢附近 YouBike 站點即時車況的 iOS 捷徑。定位後自動判斷所在縣市，找出周邊 500 公尺內最近的站點；若最近站沒有可借車輛，會自動往下檢查第二近、第三近的站點，直到找到有車的站，並整合成一則通知。

## 安裝方式
1. 由網址[https://www.icloud.com/shortcuts/1a0973e0a1e644658988cad06faf3013](https://www.icloud.com/shortcuts/1a0973e0a1e644658988cad06faf3013) 或 [YouBike_Status.shortcut](YouBike_Status.shortcut)下載至裝置
2. 依提示加入捷徑（Shortcuts app）
3. 點選分享，加入桌面
4. 到桌面點選腳踏車圖案運行

> [!NOTE]
> 可新增自動化功能
>
> iOS 16
> 
> 點選下方自動化 > 點擊新增自動化操作 > 點選特定時間 > 輸入想自動運行的時間 > 重複建議選每週 > 點選下一步 > 找到我的捷徑 > 選擇 `YouBike Status (public version)`
>
> iOS 17+
> 
> 長按YouBike Status捷徑 > 點擊編輯 > 下面點搜尋 > 自動化 > 特定時間 > 重複頻率建議選每週 > 選執行天數（星期幾）

## 功能特色

- 📍 自動定位並判斷所在縣市，比對 YouBike 服務範圍
- 🔍 抓取周邊 500m 內的站點，依距離排序找出前三近
- 🚲 依序檢查前三近站點的即時可借車輛數（一般車＋電輔車），沒車自動往下一站查
- 🔔 用一則系統通知呈現最終結果，含跳過站點的提示與找到站點的完整資訊
- 🌐 依裝置語言自動切換中文／英文訊息

## 版本要求

**iPhone**

需要 iOS 16.0 或以上版本

**iPad**

需要 iPadOS 16.0 或以上版本

**Mac**

需要 macOS Ventura (13.0) 或以上版本

**watchOS**

需要 watchOS 7.0 或以上版本

> [!WARNING]
> 此捷徑僅在 iOS 16、iOS 17 及 watchOS 10 測試過

## 架構總覽

```
┌─────────────┐         ┌──────────────────────┐         ┌──────────────────────┐
│     iOS     │  HTTPS  │   Cloudflare Worker  │  HTTPS  │         TDX          │
│             │ ─────▶ │                      │ ─────▶ │                      │
│  Shortcuts  │ ◀───── │ (youbike-tdx-worker) │ ◀───── │ tdx.transportdata.tw │
└─────────────┘         └──────────────────────┘         └──────────────────────┘
```

## Shortcuts 捷徑邏輯

1. **語言判斷**：取得裝置判定為中文（`ZhTw`），否則為英文（`En`）
2. **定位與縣市比對**：取得目前位置，轉換成 `MyState`
3. **呼叫 `/station`**：帶入 `MyState` 與目前座標，取得周邊 500m 站點清單
4. **前三近排序**：站點清單並計算與目前位置的距離，用三層距離比較邏輯，動態維護「最近、第二近、第三近」三組站點資料
5. **依序檢查車況**：把前三近站點包成一份清單，用單一迴圈＋`Found` 旗標依序呼叫 `/availability`；若某站無車，記錄提示文字後檢查下一站；找到有車的站即組出完整訊息並停止
6. **推播通知**：把所有跳過站點的提示，加上最終找到的站點資訊（或「附近皆無車可借」），整合成一則 `Show notification`

---

## 🇺🇸/🇬🇧 English

An iOS Shortcut to check real-time YouBike availability nearby. After locating your current position, it automatically determines your county/city and searches for stations within 500 meters. If the closest station has no available bikes, it automatically checks the second and third closest stations until a station with available bikes is found, consolidating the result into a single notification.

## Installation
1. Download to your device via [https://www.icloud.com/shortcuts/1a0973e0a1e644658988cad06faf3013](https://www.icloud.com/shortcuts/1a0973e0a1e644658988cad06faf3013) or [YouBike_Status.shortcut](YouBike_Status.shortcut).
2. Follow the prompt to add it to the Shortcuts app.
3. Tap Share and select **Add to Home Screen**.
4. Tap the bicycle icon on your Home Screen to run the shortcut.

> [!NOTE]
> **Setting Up Automation**
>
> **iOS 16**
> 
> Tap **Automation** at the bottom > Tap **New Automation** > Select **Time of Day** > Set your preferred time > Set repeat frequency (Weekly recommended) > Tap **Next** > Find **My Shortcuts** > Select `YouBike Status (public version)`.
>
> **iOS 17+**
> 
> Long-press the `YouBike Status` shortcut > Tap **Edit** > Search for **Automation** at the bottom > Select **Time of Day** > Set repeat frequency (Weekly recommended) > Select days of the week.

## Features

- 📍 **Auto-location**: Detects your location and matches your county/city with YouBike service areas.
- 🔍 **Nearby Stations**: Fetches stations within 500 meters and sorts the top 3 closest stations by distance.
- 🚲 **Smart Availability Checks**: Sequentially checks bike availability (regular + e-bikes) for top 3 stations; skips empty stations automatically.
- 🔔 **Unified Notification**: Consolidates results into a single system notification, listing skipped stations and full details for the available station.
- 🌐 **Bilingual Support**: Automatically switches between Chinese and English based on your device language settings.

## System Requirements

**iPhone**  
Requires iOS 16.0 or later.

**iPad**  
Requires iPadOS 16.0 or later.

**Mac**  
Requires macOS Ventura (13.0) or later.

**watchOS**  
Requires watchOS 7.0 or later.

> [!WARNING]
> This shortcut has only been tested on iOS 16, iOS 17, and watchOS 10.

## Architecture Overview

```
┌─────────────┐         ┌──────────────────────┐         ┌──────────────────────┐
│     iOS     │  HTTPS  │   Cloudflare Worker  │  HTTPS  │         TDX          │
│             │ ──────> │                      │ ──────> │                      │
│  Shortcuts  │ <────── │ (youbike-tdx-worker) │ <────── │ tdx.transportdata.tw │
└─────────────┘         └──────────────────────┘         └──────────────────────┘
```

## Shortcuts Logic Flow

1. **Language Detection**: Detects device language as Chinese (`ZhTw`); otherwise defaults to English (`En`).
2. **Location & County Matching**: Retrieves current location coordinates and converts them to `MyState`.
3. **Call `/station` Endpoint**: Passes `MyState` and current coordinates to fetch station list within 500m.
4. **Sort Top 3 Closest**: Calculates distance to each station using 3-tier comparison logic to dynamically track the 1st, 2nd, and 3rd closest stations.
5. **Check Bike Availability**: Bundles the top 3 stations, loops with a `Found` flag, and calls `/availability`. If a station has no bikes, logs a skip message and moves to the next; stops when an available station is found.
6. **Push Notification**: Combines all skipped station notices and final station info (or "No bikes available nearby") into a single `Show notification`.

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
