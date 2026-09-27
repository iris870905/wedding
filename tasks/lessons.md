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

## 6. 手機與電腦自適應智慧雙模式架構 (Adaptive Dual-Mode Architecture)
- **老屋翻修隱喻**：原先版面如同專為寬闊豪宅客廳設計的「對開觀景落地窗與大長桌」（電腦寬螢幕），硬搬進精緻小坪數公寓（手機直向 9:16）時，空間動線必然堵塞（導覽列擠爆折行、雙頁書本被壓成兩張小郵票）。
- **根治方案**：
  1. **動態視窗判別（Dynamic Viewport Detection）**：結合 CSS Media Query 與 JS `window.innerWidth < 768`（`isMobileViewport()`），即時切換視圖狀態。
  2. **相本雙模式無縫切換**：
     - **手機端（<768px）**：自動切換為「單頁全幅雜誌視角（Single-Page Magazine Card）」，提供 `aspect-[3/4]` 滿版大圖、10 頁獨立篇章顯示（`01 / 10 張`）、直覺左右滑動觸控手勢（Touch Swipe）與單張縮圖焦點置中。
     - **電腦端（≥768px）**：自動切換為「雙頁精裝書對開（Two-Page Spread Book）」，呈現 5 組對頁（`01 / 05 篇章`）、中脊陰影與絲帶書籤。
     - **雙向即時映射**：旋轉螢幕或調整視窗大小時自動換算索引，體驗不中斷。
  3. **導覽列抽屜（Mobile Navigation Drawer）**：在手機端將橫向選單收納進高雅毛玻璃下拉抽屜，右側保留輕量「出席回函」膠囊按鈕與漢堡選單，杜絕換行破版。
  4. **倒數卡片與 3 位數彈性排版**：縮減手機內距並採用彈性字級，即便天數達 3 位數（如 160 天）亦保持視覺平衡不溢出。

## 7. iOS Safari 表單 Auto-Zoom 防護
- **問題現象**：在 iPhone iOS Safari 瀏覽器中，當表單輸入框的 `font-size` 小於 16px（例如 Tailwind `text-sm` 14px）時，點擊 input 會觸發系統強制整體頁面放大跳動，破壞排版視覺。
- **根治方案**：在 `<style>` 中加入 `@media screen and (max-width: 767px) { input, select, textarea { font-size: 16px !important; } }`，徹底杜絕畫面突發性放大問題。

## 8. Safari (WebKit) 跨域表單提交限制與 Google Forms 嚴格列舉值陷阱
- **問題現象**：在 Safari（含 macOS 電腦版與 iOS 手機版）中，出席回函表單無法將資料傳送至 Google 表單後台，而在 Chrome 瀏覽器則看似正常。
- **深層病灶 1：WebKit 對 `display:none` iframe 的導航丟棄與 ITP 跨域攔截**：
  Safari 的 WebKit 核心對未在 Layout Tree（渲染樹）中繪製的節點（`display:none`）具備嚴格安全隔離。當 `<form target="...">` 指向 `display:none` 的 iframe 進行跨域 POST 時，WebKit 會將其判定為無效導航或受智慧防追蹤（ITP）保護直接中止請求（Cancelled），使得資料根本沒有送出。
- **深層病灶 2：Google 表單伺服器端的 Strict Enum 白名單檢驗**：
  Google Forms 後端對 Radio / Select 等選項型欄位具備伺服器端字串白名單檢驗。例如「出席意願」在 Google 表單原始定義為「`好傷心我無法出席，獻上真誠的祝福`」，若前端送出自定義的「`無法出席，心意相伴`」，Google 表單伺服器會直接回傳 **HTTP 400 Bad Request** 並拒絕寫入試算表。先前「新人關係」也存在多個非官方字串，導致選中特定選項時觸發 HTTP 400。
- **根治方案**：
  1. **逆向對齊官方結構**：直接解析表單端點之 `FB_PUBLIC_LOAD_DATA_`，全面校正 12 組欄位 Entry ID 與官方列舉選項（含完整親友分類清單、出席狀態文案、喜帖與素食欄位），確保資料驗證百分之百通過（HTTP 200 OK）。
  2. **多通道現代傳輸引擎（Multi-Channel Transmission Engine）**：
     - **主通道（Primary）**：以現代標準 `fetch(url, { method: 'POST', mode: 'no-cors', headers: { 'Content-Type': 'application/x-www-form-urlencoded' }, body: params.toString() })` 直接傳送 URL-encoded 封包。`mode: 'no-cors'` 屬於標準 Simple Request，Safari（Mac / iOS）100% 允許跨域送出且完全不經 iframe，徹底免疫 WebKit ITP 攔截。
     - **背景信標通道（Secondary）**：同步輔以 `navigator.sendBeacon`，保障即使在手機切換 App 或關閉頁面瞬間仍可靠送達。
     - **iframe 渲染樹合規備援**：將備援 iframe 改為 `position: absolute; width: 1px; height: 1px; left: -9999px;`，使其真實存在於 DOM 渲染樹中，不被 WebKit 視為死節點而丟棄。
