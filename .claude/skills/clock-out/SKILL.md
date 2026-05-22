---
name: clock-out
description: |
  Session 結束前的下班報告流程。
  當使用者說「下班」、「先這樣」、「今天到這裡」、「結束了」、
  「我要關了」、或用 /clock-out 觸發時自動執行。
---

# 下班報告流程

請依以下四步回顧本次 Session:

1. **本次回顧** — 我們解決了什麼問題?用了什麼方法?
2. **重要發現** — 有沒有 bug 原因、架構決定、踩過的雷需要留下來?
3. **建議更新** — 哪些資訊應該寫進 CLAUDE.md?
   哪些該進 knowledge 檔案?有沒有新偏好或糾正要加進 memory.md?
4. **下次起點** — 下次 Session 開始時,我需要先知道什麼才能繼續?

## 輸出格式
直接給可貼進對應檔案的 Markdown 內容,不要寫成段落。

## 強度判斷
- 如果這次 Session 對話不長(20 輪以內)→ 用「快速版」
- 一般長度 → 用「完整版」(預設)
- 如果 Session 涵蓋失敗 / 學新東西 / 大型決定 → 用「反思版」

(三種強度的差異見第 7.8 節)

## 寫入流程
1. 先輸出完整下班報告給 Masen review
2. 報告結尾列出「本次將寫入以下檔案：」清單，讓 Masen 確認
3. Masen 確認後（說「好」、「寫入」、「沒問題」等），直接用 Write / Edit 工具把內容寫進對應檔案：
   - 新的 memory 條目 → 寫入 `/Users/masen/.claude/projects/-Users-masen-Documents-ap/memory/` 對應 .md 並更新 MEMORY.md 索引
   - knowledge 更新 → 寫入 `Agent/[角色]/knowledge/` 對應 .md
   - CLAUDE.md 更新 → 直接 Edit `/Users/masen/Documents/ap/CLAUDE.md`
4. 全部寫入完成後提醒：「已寫入完畢，可以 /clear 或 /compact 了。」
