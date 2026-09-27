# 婚禮網站圖片路徑修正與架構優化任務 (Wedding Website Photo Link Correction & Architecture Refactor)

- [x] 1. **診斷與架構分析 (Diagnosis & Analysis)** <!-- id: 0 -->
  - [x] 檢視當前 `index.html` 狀態與檔案編碼損毀問題（TextEdit RTF/Cocoa HTML Writer 混入） <!-- id: 1 -->
  - [x] 確認 `photos/` 目錄內 10 張婚紗相片（web1.jpg ~ web10.jpg）規格與尺寸 <!-- id: 2 -->
  - [x] 檢查所有 HTML / JS 中的圖片連結引用點（相本、封面圖、縮圖） <!-- id: 3 -->
- [x] 2. **修復與重構 HTML/JS (Correction & Implementation)** <!-- id: 4 -->
  - [x] 將 Cocoa HTML Writer 包裝的偽 HTML 還原為純淨原生 HTML5 格式 <!-- id: 5 -->
  - [x] 更新 `weddingPhotos` 配置陣列，由遠端 Google Drive 連結替換為本地相對路徑 `photos/web1.jpg` ~ `photos/web10.jpg` <!-- id: 6 -->
  - [x] 實作被截斷的縮圖列建置邏輯 (`initThumbnails`) 與相本翻頁控制器 <!-- id: 7 -->
  - [x] 修正相本在行動裝置與桌面端的雙頁排版及陰影細節 <!-- id: 8 -->
  - [x] 補齊關閉標籤（`</script></body></html>`），確保 DOM 樹結構完整 <!-- id: 9 -->
- [x] 3. **驗證與測試 (Verification & Testing)** <!-- id: 10 -->
  - [x] 測試相本翻頁功能（上一頁、下一頁、自動輪播、縮圖點擊跳轉） <!-- id: 11 -->
  - [x] 驗證所有 10 張本地照片皆能順暢載入且無破圖 <!-- id: 12 -->
  - [x] 驗證表單互動、倒數計時器及 RWD 響應式排版正常運作 <!-- id: 13 -->
- [x] 4. **總結與知識沉澱 (Summary & Lessons Learned)** <!-- id: 14 -->
  - [x] 產出繁體中文技術交接文件與說明 <!-- id: 15 -->
  - [x] 記錄經驗至 `tasks/lessons.md` <!-- id: 16 -->
