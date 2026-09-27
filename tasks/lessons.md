# 經驗與架構演進學習筆記 (Lessons Learned)

## 1. macOS TextEdit 富文本與 Cocoa HTML Writer 陷阱
- **問題現象**：當使用者在 macOS 內建的 TextEdit（預設為 Rich Text 模式）開啟或儲存 `.html` 時，系統會誤以 Cocoa HTML Writer 將帶有色彩的程式碼轉譯成富文本網頁（產生大量 `p.p1`, `span.s1`, `Apple-converted-space`），導致瀏覽器開啟時把「HTML 程式碼」當作文章內文顯示，而非渲染真實網頁結構。
- **根治方案**：使用 macOS 系統級指令 `textutil -convert txt -stdout index.html`，以純文字串流完整解構還原真正的原始 HTML5 標籤與 JavaScript 程式邏輯，杜絕程式碼被字型標籤污染。

## 2. 婚禮相本雲端連結本地化 (Local Asset Migration)
- **問題現象**：原始程式碼使用 Google Drive 預覽 ID 與遠端縮圖通道，載入速度受限於外連網路，且原先相本 JavaScript 在 `// Build Thumbnail Strip` 處被截斷，缺少縮圖初始化及閉合標籤。
- **根治方案**：
  1. 將 `weddingPhotos` 陣列 10 組照片統一指向本地相對路徑 `photos/web1.jpg` 至 `photos/web10.jpg`。
  2. 升級 `formatDriveUrl()` 函式支援本機路徑穿透（相容遠端 Drive 連結與本地路徑）。
  3. 實作完整的 `initThumbnails()` 動態縮圖生成器，並加入章節標題、感言文字動態連動。
  4. 強化行動端觸控手勢滑動（Touch Swipe）與電腦鍵盤方向鍵（ArrowLeft / ArrowRight）操控體驗。

## 3. RWD 響應式雙頁書本排版 (Responsive Double-Spread Layout)
- **問題現象**：原始樣式在手機小螢幕使用 `w-full sm:w-1/2`，導致手機上右頁覆蓋左頁，使用者無法閱讀奇數頁照片。
- **根治方案**：左右兩頁統一設定 `w-1/2` 搭配 `object-contain`，在所有裝置維持精品精裝書（Book-spread）對開質感，並常態顯示書脊中線陰影 (`book-spine-shadow`)，完美呈現 10 張精選照片的雙頁視覺節奏。

## 4. 無 Sudo 權限下的開發工具鏈自力構建 (Zero-Sudo Toolchain Provisioning)
- **問題現象**：macOS 預設 `/usr/bin/git` 為 Xcode Command Line Tools 佔位符，尚未安裝時會彈出強制 GUI 對話框；而 `softwareupdate` 背景安裝則因缺乏 root 權限掛起。
- **根治方案**：直接透過 Homebrew 官方 OCI 鏡像倉儲 (`ghcr.io`) 萃取獨立原生 ARM64 二進制與動態庫（Git v2.55.0、PCRE2、Gettext），並搭配封裝 Shell Wrapper 配置 `DYLD_LIBRARY_PATH` 放置於 `~/.local/bin`，同時安裝官方 GitHub CLI (`gh` v2.101.0)，達成完全無需 root / sudo 且不干擾系統底層的便攜式開發工具鏈。

## 5. GitHub Pages 部署自動化與零破損相對路徑
- **經驗總結**：所有靜態資源一律使用相對目錄（`photos/webX.jpg`），無論是本機 WEBrick 預覽伺服器、專案子路徑 (`https://<user>.github.io/wedding/`) 或是頂級主網域，都能 100% 免疫 404 破圖問題。透過 `gh repo create` 與 `gh api /repos/.../pages`，可實現一鍵代碼推送、自動開立 Pages 服務與即時發佈。
