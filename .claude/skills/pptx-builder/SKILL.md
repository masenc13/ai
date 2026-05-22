---
name: pptx-builder
description: |
  使用 python-pptx 建立或重新排版 .pptx 簡報，並自動執行版面驗證。
  當使用者說「幫我做簡報」「產出 PPT」「重新排版這份 PPT」「套用新風格」
  「幫我用 Python 生成投影片」時一定要使用此 skill。
  包含三個強制步驟：建立簡報 → 版面驗證（overflow + 文字重疊）→ 自動修正直到乾淨。
---

# PPTX Builder Skill

每次產出 .pptx 都要走完以下三步，缺一不可。

---

## Step 1：建立簡報

用 python-pptx 寫 Python 腳本產生 .pptx 檔案。

**設計原則：**
- 每個 shape 都要設定明確的 `left / top / width / height`，不要依賴預設位置
- 文字框的 height 要留足夠空間，避免文字被截斷
- 投影片尺寸通常是 `13.33 × 7.5 inches`（16:9 寬螢幕）
- Footer（© 版權文字、頁碼）固定放在 `top = Inches(7.08)`，不會超出邊界

---

## Step 2：版面驗證（每次生成後必跑）

生成 .pptx 後，立即執行 `scripts/validate_pptx.py` 進行驗證。

```bash
python3 /Users/masen/Documents/ap/.claude/skills/pptx-builder/scripts/validate_pptx.py <path_to_pptx>
```

驗證項目：
1. **Overflow 檢查**：任何 shape 的 right 或 bottom 超出 slide 邊界（允許 0.02" 誤差）
2. **文字重疊檢查**：偵測所有文字框兩兩 bounding box 重疊

排除以下「可接受的重疊」：
- 純數字浮水印（如 `01` `02` `1` `2` 等章節數字，刻意做成深色背景）
- `©` 開頭的 footer 文字
- 刻意疊放在背景圖片上方的文字（文字框內容與背景 shape 的 overlap）

輸出格式：
- 若有問題：列出每個問題的頁碼、shape 名稱、文字內容、座標
- 若全部通過：`✓ 全部 N 頁：無 overflow，無文字重疊`

---

## Step 3：自動修正

如果 Step 2 發現問題，**不要直接回報給使用者，先自行修正**：

**Overflow 修正策略：**
- 計算超出量，縮小 width 或向左移動 shape
- 如果是多個平排的 card/column，重新均分間距：
  `card_width = (可用寬度 - 邊距*2) / 卡片數`

**文字重疊修正策略：**
- 計算兩個文字框的 y 範圍，增加第二個框的 top 使其 = 第一個框的 bottom + 間距（至少 0.05"）
- 如果多個元素堆疊（header + title + subtitle + divider），確保每個元素的 top >= 前一個元素的 bottom

修正後重跑 Step 2，直到輸出 `✓ 全部 N 頁：無 overflow，無文字重疊` 為止。

最多嘗試 3 次修正迴圈。如果 3 次後仍有問題，列出剩餘問題請使用者確認。

---

## 完成回報格式

```
✅ 簡報已生成：<檔案路徑>
📊 共 N 頁
✓ 版面驗證通過：無 overflow，無文字重疊
```

如果有修正過，補充說明：
```
🔧 已自動修正：
  - 第 N 頁：<修正說明>
```
