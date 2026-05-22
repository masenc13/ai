---
name: translate-formal
description: |
  腳本寫完後，自動讀取腳本中的旁白（VO）與字卡文字，翻譯成廣播級英文。
  當使用者說「翻譯腳本」「把 VO 翻譯成英文」「腳本英文版」「字卡翻譯」
  「幫我翻譯旁白」「translate the script」時立即使用此 skill。
  注意：只有出現「翻譯」兩字才觸發，單獨說「翻」不觸發。
  支援 .md / .txt / .pptx / .docx 格式的腳本檔案。
---

# translate-formal Skill

腳本翻譯流程：讀取 → 擷取旁白與字卡 → 翻成廣播英文 → 輸出對照表。

---

## Step 1：讀取腳本

接受以下格式：
- **Markdown / TXT**：直接讀取文字
- **PPTX**：用 python-pptx 讀取每頁文字
- **DOCX**：用 python-docx 讀取段落與表格

如果使用者沒指定路徑，詢問腳本在哪裡。

---

## Step 2：擷取旁白與字卡

從腳本中找出所有需要翻譯的文字，辨識規則如下：

### 旁白（VO / Voiceover）
帶有以下前綴的文字（無論在表格、段落或清單中）：
- `VO：` / `VO:` / `旁白：` / `配音：`

擷取前綴後面的引號內容（含引號）。

### 字卡（Title Card / Super）
帶有以下前綴的文字：
- `字卡：` / `字卡:` / `SUPER：` / `CTA：`

擷取前綴後面的引號內容（含引號）。

### 廣告結尾字卡（End Card）
出現在最後一個鏡頭（如 `27–30s`）的字卡標記為 `[END CARD]`。

---

## Step 3：翻譯規則

### 3.1 整體語氣原則

| 內容類型 | 語氣原則 |
|---------|---------|
| **旁白（VO）** | 有溫度的敘事語氣，像是真人開口說話。句子可以長，但每個段落要有韻律感。用 em-dash（—）製造停頓與對比。 |
| **字卡（Title Card）** | 短、有力、像標語。每一行獨立成句，以句號結尾。不超過一個從句。 |
| **結尾字卡（End Card）** | 行動導向，動詞開頭，具體清楚。 |

### 3.2 達盛機械參考風格（G-Man 修訂版學習）

以下風格規則從達盛機械企業影片與產品介紹影片的英文定稿中學習：

**VO 句式範例：**
- 長句 + em-dash 停頓：`"At DAR-SHENG, we don't just build equipment — we aim to be your provider of integrated engineering solutions."`
- 三段並列：`"from the cutting-edge semiconductor industry to the demanding fields of aerospace and nuclear energy engineering, DAR-SHENG is always at the forefront."`
- 結尾收力：`"We go beyond expectations, continuously creating greater value for your business."`

**字卡句式範例：**
- 兩行短句各自收尾：`"Meticulous design. Precise material selection."`
- 並列動詞：`"Professional installation. On-time delivery."`
- 動名詞開頭：`"Relentless pursuit of quality."`
- 對比句式：`"Just imagine — when your vision becomes reality."`

### 3.3 術語詞彙表

翻譯時優先使用以下對應詞，不要自創翻法：

| 中文 | 英文（優先用語） |
|------|----------------|
| 旁白 | Voice-over / VO |
| 字卡 | Title card / Super |
| 衛生福利部 | Ministry of Health and Welfare |
| 健保卡 | National Health Insurance (NHI) card |
| 免費篩檢 | free screening |
| 兩年一次 | once every two years |
| 航太 | Aerospace |
| 核能 | Nuclear Energy |
| 半導體 | Semiconductor |
| 石化 | Petrochemicals |
| 紡織 | Textiles |
| 卓越 | excellence（名詞）/ exceptional（形容詞） |
| 承諾 | commitment / dedication |
| 客製化 | customized / custom-engineered |
| 整合工程解決方案 | integrated engineering solutions |
| 品管 | quality control / quality management |
| 達盛機械 | DAR-SHENG Machinery |
| 整合 | integrate / coordinate |
| 超乎預期 | above and beyond expectations |
| 準時交付 | on-time delivery |
| 售後服務 | after-sales service |
| 信賴夥伴 | trusted partner |

### 3.4 標點與格式規範
- 中文「」改成英文 `""`
- 停頓、對比、轉折用 em-dash：`—`（非 hyphen `-`）
- 字卡每行結尾加句號，即使只有三個字
- 旁白不拆太碎，保持段落流動性

---

## Step 4：輸出格式

輸出一份中英對照表：

```
# 腳本翻譯 — [腳本名稱]

## 旁白（VO）

| # | 類型 | 繁體中文原文 | English Translation |
|---|------|------------|---------------------|
| 1 | VO   | 「你等過什麼比口腔檢查還久？」 | "What have you ever waited longer for than an oral cancer screening?" |

## 字卡（Title Cards / Supers）

| # | 類型 | 繁體中文原文 | English Translation |
|---|------|------------|---------------------|
| 1 | END CARD | 「兩年一次，免費篩檢，今天就去。」 | "Free screening, once every two years. Go today." |
```

如果腳本有多個方向（如方向一、方向二），**分組輸出**：

```
## 方向一：「兩分鐘的事」
...（旁白 + 字卡）

## 方向二：「去了沒，老爸？」
...（旁白 + 字卡）
```

---

## 完成回報

```
✅ 翻譯完成：[腳本名稱]
📝 旁白（VO）：N 條
🪧 字卡（Title Cards）：N 條
```
