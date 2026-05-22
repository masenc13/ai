# Project Overview

This project lives at `/Users/masen/Documents/ap`.

## Agent Directory

All agent definitions are stored under `Agent/`.

| Path | Description |
|------|-------------|
| `Agent/` | Root directory for all agents |
| `Agent/視覺設計師/` | Visual Designer agent |
| `Agent/導演/` | Director agent |
| `Agent/市場分析師/` | Market Analyst agent |
| `Agent/創意總監/` | Creative Director agent |

---

## 何時使用各 Agent

### 視覺設計師 `Agent/視覺設計師/`
**使用時機：**
- 需要設計視覺素材：社群貼文、Banner、品牌識別、UI 介面
- 討論色彩配色、字體選擇、版面構成
- 建立或審查設計系統（Design System）
- 評估設計工具選用（Figma、Adobe、Canva）
- 需要交付規格建議（尺寸、格式、解析度）

**不適用：**市場數據分析、影片腳本、廣告策略

---

### 導演 `Agent/導演/`
**使用時機：**
- 發展影片腳本（Script）或分鏡（Storyboard）
- 規劃拍攝計畫：鏡頭語言、攝影機運動、場景調度
- 選擇視覺風格：色調、畫面比例、燈光方向
- 製作串流內容、品牌影片、廣告 TVC
- 跨部門溝通：攝影師、演員、剪輯師、製片協調

**不適用：**靜態設計、市場數據、品牌策略文件

---

### 市場分析師 `Agent/市場分析師/`
**使用時機：**
- 進行市場規模評估（TAM / SAM / SOM）
- 執行競品分析、產業五力分析、SWOT
- 研究目標消費者輪廓（Persona）與行為
- 蒐集並解讀產業報告數據（Gartner、IDC、Nielsen 等）
- 提供市場進入策略或商業決策的數據依據

**不適用：**視覺設計執行、影片製作、創意概念發展

---

### 創意總監 `Agent/創意總監/`
**使用時機：**
- 制定品牌創意方向與視覺語言
- 撰寫或審查創意簡報（Creative Brief）
- 發展廣告 Campaign 概念，統籌跨媒體執行
- 把關所有對外創意輸出的品質與一致性
- 提案簡報規劃與說服邏輯設計
- 整合視覺設計師、導演、文案的工作成果

**不適用：**原始市場數據蒐集、鏡頭拍攝執行、UI/UX 細節設計

---

## Agent 協作建議

| 任務類型 | 建議 Agent 組合 |
|----------|----------------|
| 新品牌上市 | 市場分析師 → 創意總監 → 視覺設計師 |
| 廣告影片製作 | 創意總監 → 導演 → 視覺設計師 |
| 競品研究報告 | 市場分析師 |
| 品牌識別建立 | 創意總監 → 視覺設計師 |
| 社群內容規劃 | 創意總監 → 視覺設計師 / 導演 |

---

## Skill：translate-formal

**何時使用：** 腳本寫完後，需要把旁白（VO）和字卡翻譯成英文時，必須使用 `translate-formal` skill。

**觸發關鍵字：**
- 「翻譯腳本」「把 VO 翻成英文」「腳本英文版」
- 「字卡翻譯」「幫我翻旁白」「translate the script」

**功能：**
- 自動從 .md / .txt / .pptx 腳本中擷取 `VO：` 旁白與 `字卡：` 字卡
- 翻譯成廣播級正式英文
- 輸出中英對照表

---

## Skill：pptx-builder

**何時使用：** 每次需要產出或重新排版 `.pptx` 簡報時，必須使用 `pptx-builder` skill。

**觸發關鍵字：**
- 「幫我做簡報」「產出 PPT」「生成投影片」
- 「重新排版這份 PPT」「套用新風格到簡報」
- 「幫我用 Python 生成簡報」

**強制流程（不可跳過）：**
1. 用 python-pptx 建立 .pptx
2. 執行 `scripts/validate_pptx.py` 檢查 overflow 與文字重疊
3. 自動修正直到驗證通過，最多 3 次迴圈

**驗證腳本路徑：**
`/Users/masen/Documents/ap/.claude/skills/pptx-builder/scripts/validate_pptx.py`

---

## Compact 規則

當對話接近 **60% context limit** 時，主動提醒 Masen 並執行以下流程：

**提醒格式：**
```
⚠️ Context 已使用約 60%，建議提前 compact。

本次 compact 將壓縮以下內容：
- [列出將被壓縮的對話段落 / 任務紀錄]

以下內容會保留在壓縮摘要中：
- [列出將保留的關鍵決策 / 進行中任務 / 重要脈絡]

確認 compact 嗎？（或告訴我哪些需要保留）
```

**規則：**
- 不等 Masen 發現才說，主動在 60% 時提出
- compact 前必須列清單讓 Masen 確認，不可靜默執行
- 若有進行中任務（如等客戶回饋的案件），明確標示會保留在摘要中
- Masen 確認後再執行 `/compact`

---

## Session 管理規則

在以下情況，主動提醒 Masen 做「下班報告」並更新知識庫：

**觸發條件：**
- 對話已經很長（感覺接近壓縮邊緣）
- 一個主要任務完成
- Masen 說出：「先這樣」、「今天到這裡」、「結束了」、「我要關了」

**下班報告格式：**

```
📋 本次回顧 → 這次做了什麼
💡 重要發現 → 有什麼值得記下來的洞察或決策
📁 建議更新 → 哪些 knowledge / memory 應該補充或修改
🚀 下次起點 → 下次對話可以從哪裡繼續
```
