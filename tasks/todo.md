# 婚禮網站開發與優化任務清單 (Wedding Website Tasks & Roadmap)

## Phase 1: 基礎架構與圖片路徑修復 (Completed)
- [x] 1. 診斷與架構分析：修復 Cocoa HTML Writer 轉譯損毀問題 <!-- id: 101 -->
- [x] 2. 婚紗相本本地化：10 張精選照片路徑指向 `photos/web1.jpg` ~ `web10.jpg` <!-- id: 102 -->
- [x] 3. 實作相本翻頁控制器與縮圖列初始化邏輯 <!-- id: 103 -->
- [x] 4. GitHub 便攜工具鏈部署與 GitHub Pages 正式上線 <!-- id: 104 -->

---

## Phase 2: 行動端自適應排版與手機瀏覽自動適配 (Completed)
- [x] 1. **診斷與斷點分析 (Diagnosis & Responsive Strategy)** <!-- id: 201 -->
  - [x] 分析手機窄螢幕 (360px ~ 430px) 與電腦寬螢幕 (≥768px) 排版衝突點
  - [x] 確定 Mobile-First 與視窗監聽 (CSS Media Queries + JS `window.matchMedia` / `isMobileViewport`) 架構
- [x] 2. **頂部導覽列行動化重構 (Mobile Navigation Overhaul)** <!-- id: 202 -->
  - [x] 手機端防止文字擠壓折行，建構精緻優雅的毛玻璃行動導覽選單（含漢堡/關閉動畫與下拉遮罩）
  - [x] 電腦端維持原精品毛玻璃橫向導覽條
- [x] 3. **婚紗相本自適應雙模式重構 (Adaptive Gallery: Single vs Spread)** <!-- id: 203 -->
  - [x] 手機端 (< 768px)：自動呈現「單頁全幅雜誌視角」（10 頁大圖，01/10 顯示，左右全螢幕沉浸滑動）
  - [x] 電腦端 (≥ 768px)：保留經典「雙頁精裝書對開」（5 組對頁，01/05 篇章，書脊立體陰影與書籤）
  - [x] JS 控制器支援視窗縮放/旋轉自動切換與頁碼映射，手勢滑動 100% 順暢支援（防誤觸縱向滾動）
- [x] 4. **核心模組行動端視覺與互動體驗修復 (Module Refinements)** <!-- id: 204 -->
  - [x] 幸福倒數：動態調整卡片內距與字級，解決 3 位數（如 160 天）在小手機溢出問題
  - [x] 婚禮詳情 V-Card：最佳化手機邊距與流程圖示對齊，提供滿版導航點擊按鈕
  - [x] 交通資訊摺疊盒：加大行動端觸控熱區 (Min 44px)
  - [x] 出席回函表單：輸入框字級統一採用 16px (text-base) 根除 iOS Safari 自動放大跳動 bug
- [x] 5. **全視窗驗證與多端測試 (Multi-Device Verification)** <!-- id: 205 -->
  - [x] 使用本機 WEBrick 伺服器驗證所有 10 張本地照片皆能順暢載入且無破圖
  - [x] 驗證相本翻頁、自動輪播、手勢滑動與表單填寫各環節
- [x] 6. **成果交付、Git 儲存與經驗沉澱 (Delivery & Lessons Learned)** <!-- id: 206 -->
  - [x] 提交代碼至 Git 並推送至 GitHub Pages 發佈
  - [x] 於 `tasks/lessons.md` 記錄老屋翻修比喻與 RWD 裝置判別架構心得

---

## Phase 3: Safari (Mac & iOS) 跨瀏覽器表單提交相容性根治 (Completed)
- [x] 1. **逆向分析 Google 表單資料結構 (`FB_PUBLIC_LOAD_DATA_`)** <!-- id: 301 -->
  - [x] 萃取官方 12 組欄位 exact entry IDs 與驗證 Enum 清單
  - [x] 修正「出席意願」官方文案 (`好傷心我無法出席，獻上真誠的祝福`)
  - [x] 修正「新人關係」官方全量選項，分組排列
  - [x] 修正兒童人數、素食份數、喜帖形式錯位欄位
- [x] 2. **WebKit / Safari 跨域 POST 傳輸引擎重構** <!-- id: 302 -->
  - [x] 導入原生 `fetch(url, { method: 'POST', mode: 'no-cors' })` 核心傳輸通道
  - [x] 導入 `navigator.sendBeacon` 背景備援通道
  - [x] 將 `display:none` 改為 Safari 合規不可見 iframe 定位（避免 WebKit 取消未渲染節點導航）
- [x] 3. **多情境實機與終端驗證** <!-- id: 303 -->
  - [x] 實測「我要參加」路徑，回傳 HTTP 200 OK
  - [x] 實測「無法出席」路徑，回傳 HTTP 200 OK
  - [x] 推送最新代碼至 GitHub Pages 上線

---

## Phase 4: 行動端滾動跳動與自動回滾頂端 Bug 根治 (Completed)
- [x] 1. **深層病灶定位：`scrollIntoView()` 全域視窗綁架與網址列收放死迴圈** <!-- id: 401 -->
- [x] 2. **移除全域 `scrollIntoView()`，改為局部容器 `strip.scrollTo({ left: ... })`** <!-- id: 402 -->
- [x] 3. **`window.resize` 監聽器加入寬度比對守衛 (`window.innerWidth !== lastWindowWidth`)** <!-- id: 403 -->
- [x] 4. **Hero 封面高度改為穩定 `min-h-screen`，杜絕動態高度微抖動** <!-- id: 404 -->
- [x] 5. **成果提交 Git 並同步更新 GitHub Pages** <!-- id: 405 -->

---

## Phase 5: 表單重複三份回覆 Bug 根治 (Single Active Channel & Anti-Duplicate) (Completed)
- [x] 1. **深層病灶定位：多備援通道同時執行 (Broadcast Race) 與 `onsubmit` Promise Truthy 陷阱** <!-- id: 501 -->
- [x] 2. **重構 `handleFormSubmit`：實作單一活躍通道 (Primary-Failover Pattern)** <!-- id: 502 -->
  - [x] 優先以原生 `fetch(mode: 'no-cors')` 單一請求發送
  - [x] 僅在 fetch 異常或不支持時才依次降級至 `sendBeacon` 或 iframe
- [x] 3. **表單提交互斥鎖與防抖 (`isSubmitting` Flag & `btn.disabled = true`)** <!-- id: 503 -->
- [x] 4. **HTML onsubmit 嚴格布林攔截 (`onsubmit="handleFormSubmit(event); return false;"`)** <!-- id: 504 -->
- [x] 5. **代碼驗證、Git Commit 與 GitHub Pages 自動發佈** <!-- id: 505 -->

---

## Phase 6: 「關於我們」主旨與文案專屬定制 (Our Next Chapter Content Update) (Completed)
- [x] 1. **更新主旨標題**：`Our Next Chapter｜攜手啟程` <!-- id: 601 -->
- [x] 2. **更新感言詩意文案**：加入蔡承桓與潘奕昕專屬故事與婚禮邀請詩文 <!-- id: 602 -->
- [x] 3. **落款排版雙語化**：保留 `Andy & Iris` 英文花體並加入 `蔡承桓 • 潘奕昕` 典雅襯線字 <!-- id: 603 -->
- [x] 4. **同步更新版本歷史文件 `history.md` 並推送 GitHub Pages** <!-- id: 604 -->
- [x] 5. **電腦與手機端選單全面同步**：將桌面導覽條與行動抽屜之「關於我們」更正為「攜手啟程 (OUR NEXT CHAPTER)」 <!-- id: 605 -->
- [x] 6. **標籤與大標題結構精修**：標籤更正為「攜手啟程 • Our Next Chapter」，大標題純化為「攜手啟程」 <!-- id: 606 -->

---

## Phase 7: 喜帖形式選項預設值移除與佔位提示優化 (Invitation Selection UX) (Completed)
- [x] 1. **移除預選狀態**：取消「想要紙本喜帖還是電子喜帖呢？」預設自動選中「電子喜帖」 <!-- id: 701 -->
- [x] 2. **新增預設佔位選項**：加入 `<option value="" disabled selected>請選擇您想要的喜帖</option>` 與必填驗證 <!-- id: 702 -->
- [x] 3. **連動地址邏輯強化**：`handleInvitationChange` 在選中紙本喜帖時自動清空歷史佔位文字，便於賓客直接輸入收件地址 <!-- id: 703 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` 並推送至 GitHub Pages <!-- id: 704 -->

---

## Phase 8: 首頁封面圖片更新為本地精選照 (Hero Cover Image Update) (Completed)
- [x] 1. **檔案收納與大小寫相容**：將 `photos/` 目錄之封面圖片標準化為 `photos/we.png`，確保 Linux / GitHub Pages 跨平台 100% 讀取無誤 <!-- id: 801 -->
- [x] 2. **替換 Hero 封面圖源**：將原遠端圖片連結改為本地相對路徑 `photos/we.png`，保留漸變遮罩與立體文字陰影 <!-- id: 802 -->
- [x] 3. **多裝置視覺與載入驗證**：透過本機 HTTP 伺服器驗證圖片尺寸（2658 x 984）與色彩漸層渲染 <!-- id: 803 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` 並推送至 GitHub Pages <!-- id: 804 -->

---

## Phase 9: 首頁 Hero 文字色彩調整為深咖啡色 (Hero Typography Color Update) (Completed)
- [x] 1. **全域調整深咖啡色**：將 `Andy & Iris` 主標、英文引言、邀請詩句與日期膠囊統一設定為高雅「深咖啡色」（`#4a3427`） <!-- id: 901 -->
- [x] 2. **字體與大小零改動**：嚴格保留原有字體（Cormorant Garamond, Noto Serif TC）與 RWD 字級層次不變 <!-- id: 902 -->
- [x] 3. **對比度與質感調校**：日期膠囊調整為半透明深咖啡邊框與磨砂底色，消除舊有白色文字在淺色背景的辨識度問題 <!-- id: 903 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` 並推送至 GitHub Pages <!-- id: 904 -->

---

## Phase 10: 首頁封面再次更換為 `封面.png` (Hero Cover Image Re-upload) (Completed)
- [x] 1. **檔案收納與多重備援**：納管新人最新上傳之 `photos/封面.png`，並同步建立 `photos/cover.png` 備援以防特定瀏覽器非 ASCII URL 編碼差異 <!-- id: 1001 -->
- [x] 2. **更新 Hero 封面圖源**：將 `index.html` 圖源設為 `photos/封面.png`，並配置 `photos/cover.png` 容錯路徑 <!-- id: 1002 -->
- [x] 3. **服務端驗證**：本機 HTTP 伺服器驗證中文路徑與 URL 編碼路徑皆正常回傳 HTTP 200 OK <!-- id: 1003 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` 並推送至 GitHub Pages <!-- id: 1004 -->

---

## Phase 11: 首頁日期膠囊文字 1.5 倍放大與加粗 (Hero Date Typography Scaling) (Completed)
- [x] 1. **文字放大 1.5 倍**：將 `2027 . 03 . 06 • TAIPEI` 字級由 12/14px 等比放大至 18/20px（`text-lg sm:text-xl`） <!-- id: 1101 -->
- [x] 2. **字重加粗 1.5 倍**：將字重由 Regular (400) 調升為 Semibold (600)，字體嚴格保留 Cormorant Garamond 不變 <!-- id: 1102 -->
- [x] 3. **膠囊容器比例微調**：微幅調整外距與內距（`px-6 sm:px-8 py-2 sm:py-2.5`），使大字號與毛玻璃外框達到黃金視覺平衡 <!-- id: 1103 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` 並推送至 GitHub Pages <!-- id: 1104 -->

---

## Phase 12: 全站內文說明小字 1.5 倍等比放大 (Site-Wide Body Typography Scaling) (Completed)
- [x] 1. **全站文字範圍釐清**：經確認包含首頁 Hero 邀請說明、幸福倒數單位、攜手啟程感言與落款、婚紗相本說明與篇章描述、婚禮詳情卡片與地址穿著、交通指引手風琴內容、出席回函欄位標籤與提示文字、頁尾資訊與成功彈窗 <!-- id: 1201 -->
- [x] 2. **等比級數縮放 (1.5x Scaling Matrix)**：
  - 首頁邀請說明：`text-xs sm:text-base` (12/16px) -> `text-lg sm:text-2xl` (18/24px)
  - 倒數單位與說明：說明升為 `text-base sm:text-lg`，單位升為 `text-xs sm:text-sm`
  - 攜手啟程：故事內文由 `text-sm sm:text-base` 升為 `text-lg sm:text-2xl`，落款升為 `text-sm sm:text-base`
  - 婚紗相本：篇章導言由 `text-xs sm:text-sm` 升為 `text-base sm:text-lg`，翻頁與單位標籤等比放大
  - 婚禮詳情與交通：卡片內文、時間表說明、宴會地址、穿著建議與交通路線均放大 1.5 倍
  - 出席回函與彈窗：表單欄位 Label 由 `text-xs` 放大為 `text-sm sm:text-base`，提示字與彈窗說明同步放大
- [x] 3. **保留字型與設計美感**：嚴格保留原有字體（Cormorant Garamond、Noto Serif TC）與深咖啡色（#4a3427）設定，確保大字在手機窄螢幕上不溢出折行 <!-- id: 1202 -->
- [x] 4. **服務端驗證與版本發佈**：本機伺服器驗證無虞，同步更新 `history.md` (v1.11.0) 並推送至 GitHub Pages <!-- id: 1203 -->

---

## Phase 13: 首頁 Hero 邀請說明字級還原 (Hero Subtitle Typography Reversion) (Completed)
- [x] 1. **單一元素精準還原**：依使用者指示，僅將首頁 Hero「誠摯邀請您，與我們一同見證溫暖而永恆的幸福起點」字級還原為上一個版本的大小（`text-xs sm:text-base`），並恢復容器最大寬度（`max-w-md sm:max-w-xl`）避免手機換行突兀 <!-- id: 1301 -->
- [x] 2. **其餘區塊嚴格凍結**：其餘 7 大模組（幸福倒數、攜手啟程、婚紗相本、婚禮詳情、交通指引、出席回函、頁尾彈窗）的 1.5 倍放大樣式均 100% 保持不動 <!-- id: 1302 -->
- [x] 3. **同步版本紀錄**：更新 `history.md` (v1.12.0) 並推送至 GitHub Pages <!-- id: 1303 -->

---

## Phase 14: 「攜手啟程」故事詩文字級還原 (About Us Story Typography Reversion) (Completed)
- [x] 1. **單一元素精準還原**：依使用者截圖指示，僅將「攜手啟程」區塊中的四行故事文案詩詞字級還原為上個版本的大小（`text-sm sm:text-base`），段落間隔調回 `space-y-1.5 sm:space-y-2`，落款調回 `text-xs`，字體嚴格保留 Noto Serif TC 不變，解決放大後導致「行。」字單獨折行的問題 <!-- id: 1401 -->
- [x] 2. **其餘區塊嚴格凍結**：其餘 6 大模組（幸福倒數、婚紗相本、婚禮詳情、交通指引、出席回函、頁尾彈窗）的 1.5 倍放大樣式均 100% 保持不動 <!-- id: 1402 -->
- [x] 3. **同步版本紀錄**：更新 `history.md` (v1.13.0) 並推送至 GitHub Pages <!-- id: 1403 -->

---

## Phase 15: 交通指引捷運項目預設折疊對齊 (Transportation MRT Accordion Default Collapsed) (Completed)
- [x] 1. **手風琴初始狀態統整**：依使用者指示，將交通指引中原預設展開的「捷運指引」改為與「自行開車與停車」、「公車與計程車」兩項相同，預設均為折疊收合狀態 <!-- id: 1501 -->
- [x] 2. **狀態類別修正**：移除內容容器的 `open` 類別，移除指引箭頭的 `rotate-180` 旋轉樣式，使三項指引在頁面載入時排版整齊統一，點擊後即可流暢展開 <!-- id: 1502 -->
- [x] 3. **同步版本紀錄**：更新 `history.md` (v1.14.0) 並推送至 GitHub Pages <!-- id: 1503 -->

---

## Phase 16: 婚紗相本章節與描述文字移除 (Photo Album Chapter Text Removal) (Completed)
- [x] 1. **單一區塊乾淨移除**：依使用者截圖指示，將婚紗相本圖片下方的「Chapter 01 . 序曲」篇章標題與「執子之手，在光影交織中遇見命中註定的妳」浪漫描述說明文字區塊完整移除，呈現極簡純淨之相本舞台 <!-- id: 1601 -->
- [x] 2. **其餘資訊與功能 100% 保持不動**：婚紗相本之上方大標題、切換按鈕、張數/篇章指示器、自動輪播按鈕、底部縮圖列，以及全站其他所有區塊資訊皆嚴格凍結不動 <!-- id: 1602 -->
- [x] 3. **程式碼健壯性檢驗**：JS 控制器既有之空值守衛 (`if (chapterTitle)`, `if (chapterDesc)`) 保證無任何運行期錯誤，相本翻頁功能運作如常 <!-- id: 1603 -->
- [x] 4. **同步版本紀錄**：更新 `history.md` (v1.15.0) 並推送至 GitHub Pages <!-- id: 1604 -->

