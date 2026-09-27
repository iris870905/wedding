# 婚禮網站版本變更歷史紀錄 (Version History & Changelog)

本專案記錄 **Andy & Iris Wedding Website** 從初始架構修復、資源本地化、行動端自適應重構，到跨瀏覽器相容性與後端資料傳輸優化的完整演進歷程。

---

## 📌 版本修訂總覽 (Release Summary Table)

| 版本 (Version) | 發布日期 (Date) | Git Commit | 修改主軸 (Theme) | 核心解決問題與影響範圍 |
| :--- | :--- | :--- | :--- | :--- |
| **v1.6.0** | 2026-09-28 | 待提交 | 喜帖形式預選值移除與提示優化 | 移除「想要紙本喜帖還是電子喜帖呢？」默認預選，改為「請選擇您想要的喜帖」佔位提示並強化驗證 |
| **v1.5.0** | 2026-09-27 | `09a8d86` | 「關於我們」主旨文案與選單同步定制 | 更新「攜手啟程 • Our Next Chapter」標籤與「攜手啟程」大標題、專屬故事詩文、雙語落款，並全面同步手機與電腦選單 |
| **v1.4.0** | 2026-09-27 | `770e263` | 表單防重複提交與單一主通道架構 | 根治「一次送出 Google 表單收到重複 3 筆回覆」問題，實作發送互斥鎖與主從容錯 |
| **v1.3.0** | 2026-09-27 | `5f76ba5` | 行動端滾動跳動與回滾頂端 Bug 根治 | 解決手機向下滾動時上下劇烈抖動、自動拉回頂部相本區的視窗綁架死迴圈 |
| **v1.2.0** | 2026-09-27 | `95c4be8` | Safari 跨瀏覽器相容性與表單規格對齊 | 解決 Safari (macOS / iOS) 無法傳送回函問題，對齊 Google 表單 12 組欄位與嚴格列舉值 |
| **v1.1.0** | 2026-09-27 | `381565c` | 行動端自適應排版與智慧雙模式架構 | 解決手機螢幕排版混亂，建置相本單頁雜誌/雙頁精裝書自適應雙模式與毛玻璃下拉選單 |
| **v1.0.1** | 2026-09-27 | `ed7efb2`<br>`7729287` | 開發任務追蹤與技術架構文檔化 | 建立 `tasks/todo.md` 與 `tasks/lessons.md`，部署無 root 權限獨立 Git 工具鏈 |
| **v1.0.0** | 2026-09-27 | `71e88d1` | 專案初版發布與相本本地化 | 解構修復 Cocoa HTML Writer 轉譯損毀，10 張婚紗相片本地化並上線 GitHub Pages |

---

## 📖 詳細版本變更歷程 (Detailed Changelog)

### [v1.6.0] — 2026-09-28
#### 🎯 喜帖形式預選值移除與佔位提示優化 (Invitation Selection UX)
- **問題現象 (Issue)**：
  - 出席回函表單中，「想要紙本喜帖還是電子喜帖呢？」原先預設勾選了「電子喜帖」，導致賓客若未留意便可能錯過選擇紙本喜帖的機會。
- **實作改動 (Key Changes)**：
  1. **移除預先選擇狀態**：取消「電子喜帖」的 `selected` 預選標記。
  2. **新增預設佔位選項**：在選項第一項加入 `<option value="" disabled selected>請選擇您想要的喜帖</option>`，並在 `<select>` 上標記 `required` 與紅色必填星號（`*`）。
  3. **強化連動地址輸入體驗**：
     - 優化 `handleInvitationChange()`：當使用者切換為「紙本喜帖」時，除了展開收件地址欄位外，主動清除預先自動填入的「無 / 選擇電子喜帖」等佔位文字，便於賓客直接鍵入收件資訊。
     - 當選擇「電子喜帖」時，維持地址欄位隱藏並自動填入安全值「無 / 選擇電子喜帖」，確保 100% 吻合 Google 表單後端驗證規格。
- **影響檔案**：
  - `index.html`（更新喜帖形式 select 選項、優化 handleInvitationChange 與 handleFormSubmit）
  - `tasks/todo.md`（追加 Phase 7 任務記錄）
  - `history.md`（收錄 v1.6.0 版本修訂）

### [v1.5.0] — 2026-09-27
#### 🎯 「關於我們」主旨與文案專屬定制 & 全平台選單同步 (Our Next Chapter Content & Nav Synchronization)
- **修改主旨 (Theme)**：
  - 小標籤更新為：`攜手啟程 • Our Next Chapter`
  - 主視覺大標題純化為：`攜手啟程`（無前綴英文，維持純粹中文字形張力）
  - 選單項目同步為：`攜手啟程`（英文副標：`OUR NEXT CHAPTER`）
- **專屬內容更新 (Content Update)**：
  - 更新卡片感言詩文為新人專屬文字：
    > 因愛相遇，蔡承桓與潘奕昕決定展開人生的全新章節。<br>
    > 走過四季的平凡日常，往後的每個晨曦與日落，我們都將牽手同行。<br>
    > 誠摯邀請生命中最珍貴的您來到現場，<br>
    > 見證我們的愛情誓言，共享這份滿溢幸福的時刻。
- **實作改動 (Key Changes)**：
  1. **標籤與大標題層級純化**：將副標籤更新為 `攜手啟程 • Our Next Chapter`，大標題 `<h2>` 純化為純文字 `攜手啟程`，去除混雜英文，使版面焦點純淨且具儀式感。
  2. **四行詩意節奏佈局**：在行動端與電腦端採用單行獨立節奏容器（`space-y-1.5 sm:space-y-2`），確保詩文在手機小螢幕上自然斷行、不突兀折字。
  3. **雙語專屬署名**：保留 `Andy & Iris` 經典署名，並追加中文落款 `蔡承桓 • 潘奕昕`，強化個人專屬專屬婚禮卡的浪漫儀式感。
  4. **全端選單語彙統一**：同步修正頂部橫向導覽列（電腦端）與毛玻璃下拉抽屜選單（手機端），將原本的「關於我們」全面更正為「攜手啟程 (OUR NEXT CHAPTER)」，點擊順暢滑動至 `#about`。
- **影響檔案**：
  - `index.html`（更新選單導航、主旨標題、感言內文、雙語署名）
  - `tasks/todo.md`（追加 Phase 6 任務記錄）
  - `history.md`（收錄 v1.5.0 版本修訂）

### [v1.4.0] — 2026-09-27
#### 🎯 表單重複三份回覆 Bug 根治 (Single Active Channel & Anti-Duplicate Architecture)
- **Commit Hash**: `770e2639d371fed92443b80fe58131161b2195d5`
- **問題現象 (Issue)**：
  - 無論使用手機（Safari / Chrome）或電腦瀏覽，填寫送出出席回函一次，Google 表單與試算表後端每次都會精確收到「重複三份一模一樣的回覆」。
- **根本原因 (Root Cause)**：
  - **備援通道並發全發（Broadcast Race）**：先前為解決 Safari 掉單，在 `handleFormSubmit` 內依序觸發了 `fetch`、`sendBeacon`、`form.submit()` 三種機制。在欄位規格校正通過（後端回傳 HTTP 200 OK）後，三個通道全部同時成功發送，造成單次點擊向 Google 表單伺服器發射了 3 個獨立 POST 請求。
  - **`onsubmit` Promise Truthy 陷阱**：原生表單使用 `onsubmit="return handleFormSubmit(event)"` 綁定 `async` 函式時，回傳的 Promise 物件屬於真值（Truthy），容易引起瀏覽器默認行為的微小相容性偏差。
- **老屋翻修隱喻**：
  - 這就像老屋翻修時擔心停電，同時拉了市電、柴油發電機與太陽能三條電線，結果配電箱沒有裝「自動切換開關（ATS）」，而是三條線直接擰在一起送電，導致每次開關電器都承受三重電流灌入！
- **實作改動 (Key Changes)**：
  1. **單一活躍主通道與容錯降級（Primary-Failover Pattern）**：
     - 重構 `handleFormSubmit`，以標準原生 `window.fetch(mode: 'no-cors')` 作為唯一主通道。
     - 僅在 `fetch` 遇到不支援或拋出例外時，才接續嘗試 `navigator.sendBeacon`；若皆不支援才降級至不可見 iframe 的 `form.submit()`。正常通訊下永遠只發出 1 筆請求。
  2. **送出互斥鎖（Submission Mutex）**：
     - 宣告 `isSubmitting` 狀態旗標與按鈕 `disabled = true`，在發送期間全面阻斷二次點擊或鍵盤 Enter 連擊。
  3. **表單原生事件嚴格攔截**：
     - 表單宣告改為 `onsubmit="handleFormSubmit(event); return false;"`，並於函式入口同步呼叫 `e.preventDefault()`。
- **影響檔案**：
  - `index.html`（重構 `handleFormSubmit`、更新 form `onsubmit` 屬性）
  - `tasks/todo.md`（追加 Phase 5 任務記錄）
  - `tasks/lessons.md`（收錄第 10 條重複提交架構防護經驗）

---

### [v1.3.0] — 2026-09-27
#### 🎯 行動端滾動跳動與自動回滾頂端 Bug 根治 (Mobile Scroll-Jumping & Resize Loop Fix)
- **Commit Hash**: `5f76ba58ff2abd189b977a74be24b5c3a8ebb47b`
- **問題現象 (Issue)**：
  - 手機瀏覽網頁時，頁面會出現上下劇烈跳動；當滾動到網頁下方（如出席回函或婚禮詳情）時，畫面又會自動瞬間跳回網頁最上方的相本區塊。
- **根本原因 (Root Cause)**：
  - **`scrollIntoView()` 全域視窗綁架**：更新相本微縮圖焦點時呼叫了 `targetThumb.scrollIntoView()`，該原生方法會向上捲動所有祖先節點直到 `window`，強行將整體網頁拉回位在頁面上方的相本。
  - **行動端網址列收放觸發 `window.resize` 死迴圈**：手機滑動時系統網址列與工具列自動收合，導致 `window.innerHeight` 改變並觸發 `window.onresize`。由於未過濾高度變化，每次滑動都會反覆觸發相本重繪 -> 縮圖焦點刷新 -> `scrollIntoView()` 拉回頂端 -> 網址列彈出 -> 再次觸發 resize 的無限死迴圈。
  - **動態視窗高度抖動**：Hero 區塊使用 `min-h-[100dvh]`，在網址列縮放瞬間會產生動態像素重算微抖動。
- **實作改動 (Key Changes)**：
  1. **局部容器平移隔離**：完全移除 `targetThumb.scrollIntoView()`，改為 `strip.scrollTo({ left: scrollTarget, behavior: 'smooth' })`，僅針對縮圖容器 `#thumbStrip` 橫向滑動，零干擾全域垂直滾動。
  2. **視窗寬度比對守衛（Width-Guard on Resize）**：在 `window.onresize` 監聽器中比對 `window.innerWidth === lastWindowWidth`，若僅高度改變直接 `return`，阻斷死迴圈。
  3. **視窗高度穩定常數化**：將 Hero 區塊的 `min-h-[100dvh]` 統一為 `min-h-screen`。
- **影響檔案**：
  - `index.html`（移除全域捲動、優化 resize 事件與 Hero 容器）
  - `tasks/todo.md`（追加 Phase 4 任務進度）
  - `tasks/lessons.md`（收錄第 9 條行動端視窗綁架經驗）

---

### [v1.2.0] — 2026-09-27
#### 🎯 Safari (macOS & iOS) 跨域表單提交相容性與 Google 表單規格對齊 (Safari WebKit Compatibility)
- **Commit Hash**: `95c4be864d7bed9459dcaf931048939e98a72766`
- **問題現象 (Issue)**：
  - 出席回函表單在 Chrome 瀏覽器可正常傳送資料，但在 Safari（包含 macOS 電腦版、iPhone iOS 手機版）皆無法將資料傳送至 Google 表單後台。
- **根本原因 (Root Cause)**：
  - **WebKit 對 `display:none` iframe 的阻斷**：Safari 的 WebKit 核心對未在渲染樹（Layout Tree）中繪製的節點具備嚴格安全隔離。`<form target="...">` 指向 `display:none` 的 iframe 時，WebKit 會視為無效導航或受 ITP（智慧防追蹤）攔截，直接 Cancel 跨域請求。
  - **Google 表單嚴格列舉值檢驗（Strict Enum）**：表單「出席意願」官方文案為「`好傷心我無法出席，獻上真誠的祝福`」，原網頁自定義為「`無法出席，心意相伴`」，Google 表單後台伺服器檢驗不符直接回傳 **HTTP 400 Bad Request** 並拒絕寫入。
- **實作改動 (Key Changes)**：
  1. **逆向分析與官方規格 100% 對齊**：
     - 解析 Google 表單端點 `FB_PUBLIC_LOAD_DATA_`，全面對齊 12 組欄位 exact entry IDs（姓名、電話、Email/Line、親友關係、出席狀態、出席人數、幼童人數、兒童椅需求、素食份數、喜帖形式、收件地址、祝福留言）。
     - 將親友關係分為新郎/新娘親友共 12 類官方完整清單；出席狀態文案對齊官方列舉。
  2. **多通道跨瀏覽器穿透引擎**：
     - 導入現代原生 `fetch(url, { method: 'POST', mode: 'no-cors' })`，不經 iframe 直接穿透 WebKit ITP。
     - 備援 iframe 改用渲染樹合規不可見定位（`position: absolute; width: 1px; height: 1px; left: -9999px;`）。
- **影響檔案**：
  - `index.html`（校準 12 組表單欄位、升級傳輸通道）
  - `tasks/todo.md`（追加 Phase 3 任務進度）
  - `tasks/lessons.md`（收錄第 8 條 Safari WebKit 與 Google Form 嚴格檢驗機制）

---

### [v1.1.0] — 2026-09-27
#### 🎯 手機端自適應排版與智慧雙模式架構 (Mobile RWD & Adaptive Dual-Mode Architecture)
- **Commit Hash**: `381565ca06292742e421ad1b4ed058e78c1b7c41`
- **問題現象 (Issue)**：
  - 網頁原先以電腦寬螢幕排版為主，手機瀏覽時畫面嚴重擠壓破版：導覽列折行擠爆、婚紗相本雙頁被壓縮成小郵票無法閱讀、倒數計時卡片 3 位數溢出、iOS 點擊輸入框自動放大跳動。
- **根本原因 (Root Cause)**：
  - 缺乏行動優先（Mobile-First）自適應機制與斷點響應邏輯，直接將桌機寬螢幕多欄架構硬塞入手機 9:16 直向視窗。
- **老屋翻修隱喻**：
  - 原版面就像專為寬闊豪宅客廳設計的「對開落地窗與大長桌」，硬搬進精緻小套房時空間動線必定卡死。
- **實作改動 (Key Changes)**：
  1. **頂部導覽列行動化重構**：
     - 手機端 (<768px)：橫向選單收納進毛玻璃下拉抽屜（Mobile Navigation Drawer），附漢堡/關閉切換動畫與外部點擊遮罩。
     - 電腦端 (≥768px)：維持精品毛玻璃橫向導覽條。
  2. **相本自適應雙模式控制器（Adaptive Dual-Mode）**：
     - 手機端 (<768px)：自動切換為「單頁全幅雜誌視角（Single-Page Magazine Card）」，滿版高畫質大圖、`01 / 10 張` 篇章進度、左右觸控滑動手勢（Touch Swipe）。
     - 電腦端 (≥768px)：保留經典「雙頁精裝書對開（Two-Page Spread Book）」，呈現 5 組跨頁篇章（`01 / 05 篇章`）、中脊陰影與絲帶書籤。
     - 支援視窗縮放/旋轉即時雙向映射（頁碼不迷航）。
  3. **細節視覺與互動適配**：
     - 幸福倒數：微調內距與字級，3 位數天數（如 160 天）完美容納不溢出。
     - iOS Auto-Zoom 防護：全量表單輸入框加入手機端字級 `16px !important`，防止 iPhone 點擊自動放大畫面。
     - 交通與婚禮詳情卡片：加大行動觸控熱區至 44px 以上。
- **影響檔案**：
  - `index.html`（自適應導覽列、相本雙模式控制器、RWD 樣式優化）
  - `tasks/todo.md`（追加 Phase 2 完整檢核清單）
  - `tasks/lessons.md`（收錄第 6 條與第 7 條雙模式架構與 iOS 縮放經驗）

---

### [v1.0.1] — 2026-09-27
#### 🎯 專案開發任務追蹤與技術架構文檔化 (Project Documentation & Toolchain Provisioning)
- **Commit Hash**: `7729287daf116b4334040deb5e76c0d4c6080b82` & `ed7efb2bdc6a1b8dac8880f968dff19fe907b564`
- **實作改動 (Key Changes)**：
  1. 建立工程任務追蹤清單 `tasks/todo.md`，定義里程碑與驗證標準。
  2. 建立技術決策與架構經驗筆記 `tasks/lessons.md`。
  3. 在 macOS 無 root / sudo 限制下，透過 Homebrew OCI 鏡像萃取獨立 ARM64 原生 Git 與 GitHub CLI 工具鏈放置於 `~/.local/bin`，確保工程自給自足。
- **影響檔案**：
  - `tasks/todo.md`（新建）
  - `tasks/lessons.md`（新建）
  - `.gitignore`（設定忽略暫存檔）

---

### [v1.0.0] — 2026-09-27
#### 🎯 專案初始發布與相本本地化 (Initial Release & Asset Migration)
- **Commit Hash**: `71e88d183dd2844fd556447705b427d3d5dc5d4c`
- **問題現象 (Issue)**：
  - 原始專案檔案遭 macOS TextEdit（Cocoa HTML Writer）轉譯為富文本網頁，瀏覽器開啟時把 HTML 程式碼當作文字內文顯示；相本依賴 Google Drive 外部連結且 JavaScript 在縮圖處截斷。
- **實作改動 (Key Changes)**：
  1. 使用系統級 `textutil` 還原純淨 HTML5 與 JavaScript 結構，根除富文本污染。
  2. 10 張精選婚紗照片全面本地化至 `photos/web1.jpg` ~ `photos/web10.jpg`，升級 `formatDriveUrl()` 支援本機路徑。
  3. 實作相本翻頁控制器、微縮圖列、鍵盤方向鍵與輪播動畫。
  4. 建立遠端儲存庫 `https://github.com/iris870905/wedding.git` 並正式啟用 GitHub Pages 部署服務。
- **影響檔案**：
  - `index.html`（完整架構還原）
  - `photos/web1.jpg` ~ `photos/web10.jpg`（10 張高畫質精裝照片）
  - `.gitignore`

---

## 🌐 線上發布資訊
- **GitHub 儲存庫**：[https://github.com/iris870905/wedding](https://github.com/iris870905/wedding)
- **GitHub Pages 正式網址**：[https://iris870905.github.io/wedding/](https://iris870905.github.io/wedding/)
- **主要分支**：`main`
