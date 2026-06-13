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

## Skill：script-writer

**何時使用：** 使用者提供客戶需求問卷（.docx 路徑或直接貼上 Q1–Q5 內容）時，必須使用 `script-writer` skill 產出提案腳本。

**觸發關鍵字：**
- 「幫我寫腳本」「根據問卷產出腳本」「用問卷生成腳本」
- 「提案腳本」「幫我做腳本」「這份問卷幫我寫」
- 使用者給出 `客戶需求問卷_XXX.docx` 的路徑

**功能：**
- 讀取問卷 Q1–Q5，自動產出格式完整的 7 段分鏡腳本
- 輸出包含：核心訊息、故事大綱、風格情緒、目標觀眾、影片規格、分鏡表
- 格式對標藍麒科技_貿協提案用.docx（Markdown 輸出，可直接複製進 Word）
- **整合 4 個 Agent 知識庫：** 市場分析師（問卷解讀）→ 創意總監（核心訊息 + VO）→ 導演（影像說明）→ 視覺設計師（字卡策略）

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

## Skill：pinterest-image-fetch

**何時使用：** 需要從 Pinterest 搜尋並下載參考圖片，嵌入腳本 docx 的「參考圖片」欄時使用。

**觸發關鍵字：**
- 「幫我找參考圖片」「從 Pinterest 下載圖片」「嵌入參考圖」「加圖片進腳本」

**標準流程：**
1. 請 Masen 用 **Cookie-Editor** 擴充套件在 pinterest.com 匯出 JSON cookies
2. 存至 `/tmp/pinterest_cookies.json`
3. 執行 `/tmp/pinterest_cookie_fetch.js`（Puppeteer stealth + cookie 注入）
4. 圖片用 PIL `.convert("RGB")` + BytesIO 轉換後嵌入 python-docx

**重要限制：**
- Pinterest API 封鎖一般開發者（`consumer type not supported`）
- Masen 的帳號是 Google OAuth，無法 email/password 登入
- Cookie 有效期約 30 天，過期重新匯出即可

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

## Skill：business-proposal

**何時使用：** 使用者提供一個**客戶資料夾路徑**（內含「專案內容資料夾」素材與一份「影片企劃範例」格式範本），想一次產出企業形象影片提案企劃報告時，必須使用 `business-proposal` skill。

**觸發關鍵字：**
- `/business-proposal`、「企業形象提案」
- 「幫我做提案企劃」「根據這個客戶資料夾提案」
- 直接給出客戶資料夾路徑要你提案

**自動執行四個 Phase（中間不等使用者回覆）：**
1. 盤點與閱讀：掃描客戶資料夾 → 讀完「專案內容資料夾」全部素材 → 解析「影片企劃範例」並**判斷它是簡報還是文件**
2. 三位專家協作（平行 Sub-Agent）：市場分析師、創意總監、導演各讀人設+知識庫進入角色，產出各自視角觀點
3. 整合成三案：把三種視角交叉組合成三個都由市場+創意+影像共同支撐的完整切入點
4. 產出簡報：用內建產生器 `scripts/proposal_deck.py` 產出 `.pptx`（定案版型，對標鼎康範例），跑 validate_pptx 驗證

**輸出（一份簡報，存回客戶資料夾）：**
- `<客戶資料夾>/【客戶名稱】企業形象提案企劃.pptx`（封面→大綱→專案理解·市場洞察→每案3頁（區塊頁/核心訊息主視覺/影片架構+提案重點）→結尾，給 context 時 13 頁）
- 定案視覺：深藍/主藍/淺藍/品牌綠，照片區用原生色塊（禁 JPG/PNG）
- 範本若是純文字 Word 文件才改輸出 .docx（備案）
- 完成後統一報告一次，不中途打擾

---

## Skill：newscript

**何時使用：** 客戶**確認採用哪一個提案策略**後，要把那一案發展成含完整分鏡的提案腳本時，必須使用 `newscript` skill。是 `business-proposal`（提三案）的下一棒。

**觸發關鍵字：**
- `/Newscript`、「做提案腳本」「依選定策略寫腳本」
- 「客戶選好策略了幫我寫腳本」「把確認的策略發展成腳本」
- 給出已含「企劃整理」結論的客戶資料夾要你寫腳本

**輸入：** 同一個客戶資料夾路徑，其中 `企劃整理/` 放會議結果文字 + 確認採用哪一案。

**自動執行三個 Phase：**
1. 盤點與閱讀：讀 `企劃整理/`（鎖定選定策略 + 會議要求）→ 提案簡報 .pptx（取該案內容）→ 影片需求單（規格 + 必出現場景）
2. 兩位專家共同創作（平行 Sub-Agent）：導演 + 創意總監**都看全部、各出完整草稿**，主線整合
3. 整合產出：用內建產生器 `scripts/build_script_doc.py` 產出對標「提案腳本架構範本」的 `.docx`（5 欄分鏡表）

**輸出：** `<客戶資料夾>/【客戶名稱】提案腳本.docx`；規格一律以需求單為準、必出現場景全納入；需中英對照可接 `translate-formal`。

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
