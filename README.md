# YouBike Status（iOS 捷徑）

查詢附近 YouBike 站點即時車況的 iOS 捷徑。定位後自動判斷所在縣市，找出周邊 500 公尺內最近的站點；若最近站沒有可借車輛，會自動往下檢查第二近、第三近的站點，直到找到有車的站，並整合成一則通知。

## 安裝方式
1. 由網址[https://www.icloud.com/shortcuts/9caa274334214b85b88221d795c5ee81](https://www.icloud.com/shortcuts/9caa274334214b85b88221d795c5ee81) 或 [YouBike_Status.shortcut](YouBike_Status.shortcut)下載至裝置
2. 一提示加入捷徑（Shortcuts app）
3. 點選分享，加入桌面
4. 到桌面點選腳踏車圖案運行

> [!NOTE]
> 可新增自動化功能
>
> iOS 26
> 
> 點選下方自動化 > 點擊新增自動化操作 > 點選特定時間 > 輸入想自動運行的時間 > 重複建議選每週 > 點選下一步 > 找到我的捷徑 > 選擇 `YouBike Status (public version)`
>
> iOS 27
> 長按YouBike Status捷徑 > 點擊編輯 > 下面點搜尋 > 自動化 > 特定時間 > 重複頻率建議選每週 > 選執行天數（星期幾

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

需要 MacOS Tahoe(16.0) 或以上版本

> [!NOTE]
> 此捷徑僅在iOS26及27測試

## 架構總覽

```
┌─────────────┐        ┌──────────────────────┐        ┌──────────────────────┐
│     iOS     │  HTTPS │  Cloudflare Worker   │  HTTPS │          TDX         │
│             │ ─────▶ │                      │ ─────▶ │                      │
│  Shortcuts  │ ◀───── │ (youbike-tdx-worker) │ ◀───── │ tdx.transportdata.tw │
└─────────────┘        └──────────────────────┘        └──────────────────────┘
```

## Shortcuts 捷徑邏輯

1. **語言判斷**：取得裝置判定為中文（`ZhTw`），否則為英文（`En`）
2. **定位與縣市比對**：取得目前位置，轉換成 `MyState`
3. **呼叫 `/station`**：帶入 `MyState` 與目前座標，取得周邊 500m 站點清單
4. **前三近排序**：站點清單並計算與目前位置的距離，用三層距離比較邏輯，動態維護「最近、第二近、第三近」三組站點資料
5. **依序檢查車況**：把前三近站點包成一份清單，用單一迴圈＋`Found` 旗標依序呼叫 `/availability`；若某站無車，記錄提示文字後檢查下一站；找到有車的站即組出完整訊息並停止
6. **推播通知**：把所有跳過站點的提示，加上最終找到的站點資訊（或「附近皆無車可借」），整合成一則 `Show notification`

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
