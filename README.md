<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOKYO VIBE | 鮮明配色儀表板（修正版）</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* 品牌配色定義 */
        :root {
            --color-dark-navy: #1D2A35; /* 深藍色基底 */
            --color-cream: #F7FCF5;    /* 奶油色/淺色卡片 */
            --color-teal: #2CBBAD;     /* 水鴨綠，主要強調色 */
            --color-red: #D83D4F;      /* 警示紅，次要強調色 */
            --color-gray-dark: #6C7A89; /* 深灰色，非活躍狀態 */
        }

        /* 全局樣式：深色基底，淺色字體 */
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            background-color: var(--color-dark-navy); 
            color: var(--color-cream);
            min-height: 100vh;
        }

        /* 主要卡片面板 - 使用淺色搭配深色邊框和陰影，實現高對比 */
        .flat-panel {
            background-color: var(--color-cream); /* 淺色卡片 */
            color: var(--color-dark-navy); /* 卡片內使用深色文字 */
            border: 1px solid rgba(44, 187, 173, 0.3); /* 柔和水鴨綠邊框 */
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(29, 42, 53, 0.15); /* 柔和陰影 */
        }

        /* 強調色 1：Teal (水鴨綠) */
        .teal-accent {
            color: var(--color-teal);
        }
        
        /* 強調色 2：Red (警示紅) */
        .red-accent {
            color: var(--color-red);
        }
        
        /* 按鈕基礎樣式 */
        .modern-btn {
             @apply transition-all duration-300 border text-left font-medium flex-shrink-0 min-w-[120px] rounded-lg p-3;
        }

        /* 專家提示區塊 - 使用柔和的淺灰色背景搭配水鴨綠邊條 */
        .expert-tip-box {
            background-color: #E6F3F2; /* 淺綠/淺灰 */
            border-left: 3px solid var(--color-teal);
            color: var(--color-dark-navy);
        }

        /* 固定側邊欄位 */
        @media (min-width: 1024px) {
            .sticky-col {
                position: sticky;
                top: 1.5rem;
                height: calc(100vh - 3rem);
                overflow-y: auto;
                -ms-overflow-style: none;
                scrollbar-width: none;
            }
            .sticky-col::-webkit-scrollbar { display: none; }
        }

        /* 覆寫 Tailwind 滾動條 (手機端導覽列) */
        #nav-container-wrapper::-webkit-scrollbar {
            height: 3px;
        }
        #nav-container-wrapper::-webkit-scrollbar-thumb {
            background-color: var(--color-teal);
            border-radius: 1.5px;
        }
        
        /* 購物清單專用樣式 */
        .item-done {
            text-decoration: line-through;
            opacity: 0.6;
        }
    </style>
</head>
<body class="p-3 lg:p-6 text-sm lg:text-base">

    <!-- 頂部 Header -->
    <header class="max-w-[1400px] mx-auto mb-6 p-6 border-b-2 border-opacity-70 flex items-center justify-between"
            style="background-color: var(--color-dark-navy); border-color: var(--color-teal);">
        <div class="z-10">
            <h1 class="text-2xl lg:text-3xl font-black tracking-widest" style="color: var(--color-teal);">
                TOKYO VIBE 2025
            </h1>
            <p class="text-gray-400 text-xs lg:text-sm font-medium mt-1 flex gap-3">
                <span class="text-gray-300"><i data-lucide="calendar" class="w-3 h-3 inline mr-1"></i>12/26 - 12/31</span>
                <span><i data-lucide="compass" class="w-3 h-3 inline mr-1"></i>鮮明配色儀表板</span>
            </p>
        </div>
        <div class="hidden lg:block">
            <i data-lucide="map" class="w-8 h-8" style="color: var(--color-cream);"></i>
        </div>
    </header>

    <!-- 主內容 Grid -->
    <div class="max-w-[1400px] mx-auto grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">

        <!-- [左欄] Col-2: 導覽 (日期 + 工具切換按鈕) -->
        <nav class="lg:col-span-2 pb-2 lg:pb-0 z-20 sticky-col h-auto lg:h-auto">
            <div id="nav-container-wrapper" class="flex flex-row lg:flex-col gap-2 overflow-x-auto lg:overflow-visible">
                <!-- 日期導覽區 + 工具按鈕 (全部在此容器內生成) -->
                <div id="nav-container" class="flex flex-row lg:flex-col gap-2 w-full lg:flex-shrink">
                    <!-- JS 動態生成所有按鈕 -->
                </div>
            </div>
        </nav>

        <!-- [右欄] Col-10: 動態內容區 (行程 OR 工具) -->
        <main class="lg:col-span-10 space-y-6">

            <!-- 1. 行程內容區 (ITINERARY) - 預設顯示 -->
            <div id="itinerary-view" class="space-y-6">
                <!-- 天氣看板 (淺色卡片，高對比) -->
                <div id="weather-card" class="flat-panel p-5 relative overflow-hidden transition-all duration-500 shadow-lg">
                    <div class="relative z-10 flex justify-between items-center">
                        <div class="flex items-center gap-4">
                            <div class="text-4xl teal-accent" id="weather-icon"></div>
                            <div>
                                <div class="text-xs font-bold text-gray-500 uppercase tracking-widest mb-0.5">WEATHER</div>
                                <div id="weather-condition" class="text-lg font-bold" style="color: var(--color-dark-navy);"></div>
                                <div id="weather-note" class="text-xs text-gray-600 opacity-90"></div>
                            </div>
                        </div>
                        <div class="text-right">
                            <div id="weather-location" class="text-xs red-accent font-bold uppercase tracking-widest mb-0.5">TOKYO</div>
                            <div class="text-2xl font-black" style="color: var(--color-dark-navy);">
                                <span id="weather-high" class="teal-accent"></span>
                                <span class="text-gray-400 text-base mx-1">/</span>
                                <span id="weather-low" class="red-accent"></span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 標題 -->
                <div class="flex items-end gap-3 pb-2 border-b border-opacity-70" style="border-color: var(--color-teal);">
                    <!-- 日數使用次要強調色 (紅) -->
                    <h2 id="day-number" class="text-4xl font-black leading-none select-none" style="color: var(--color-red);">DAY 1</h2>
                    <!-- 標題使用主要強調色 (水鴨綠) -->
                    <h3 id="day-title" class="text-xl lg:text-2xl font-bold teal-accent leading-tight mb-1"></h3>
                </div>

                <!-- 行程列表容器 -->
                <div id="itinerary-list" class="space-y-5">
                    <!-- JS 動態插入行程卡片 -->
                </div>
            </div>


            <!-- 2. 匯率試算區 (CURRENCY) - 預設隱藏 -->
            <div id="currency-view" class="hidden flat-panel rounded-xl p-8 lg:p-12 h-full min-h-[500px]">
                <h3 class="text-3xl font-bold teal-accent mb-8 pb-3 border-b border-gray-300 flex items-center">
                    <i data-lucide="coins" class="w-8 h-8 mr-3"></i> 日幣匯率試算中心
                </h3>
                
                <div class="max-w-md mx-auto space-y-6">
                    <!-- 匯率輸入框 -->
                    <div class="mb-6 flex items-center justify-between bg-gray-100 p-4 rounded-lg border" style="border-color: var(--color-red);">
                        <span class="text-sm text-gray-600 font-medium">目前匯率 (1 TWD =)</span>
                        <div class="flex items-center">
                            <input type="number" id="rateInput" value="4.80" step="0.01" oninput="convert('TWD')" 
                                class="w-20 bg-transparent text-right font-mono font-bold text-xl focus:outline-none teal-accent">
                            <span class="text-sm text-gray-500 ml-2">JPY</span>
                        </div>
                    </div>

                    <!-- TWD 輸入 -->
                    <div class="relative group">
                        <label class="absolute left-4 top-3 text-[10px] font-bold text-gray-500 group-focus-within:teal-accent">TWD (台幣)</label>
                        <input type="number" id="twdInput" placeholder="0" oninput="convert('TWD')"
                            class="w-full bg-gray-100 border border-gray-300 rounded-lg p-4 pt-8 text-2xl font-mono" style="color: var(--color-dark-navy); border-color: var(--color-red);" focus:border-teal-accent transition-all outline-none">
                    </div>
                    
                    <div class="flex justify-center py-2">
                        <div class="rounded-full p-2" style="background-color: var(--color-red);"><i data-lucide="arrow-down-up" class="w-5 h-5 text-white"></i></div>
                    </div>
                    
                    <!-- JPY 輸入 -->
                    <div class="relative group">
                        <label class="absolute left-4 top-3 text-[10px] font-bold text-gray-500 group-focus-within:teal-accent">JPY (日幣)</label>
                        <input type="number" id="jpyInput" placeholder="0" oninput="convert('JPY')"
                            class="w-full bg-gray-100 border border-gray-300 rounded-lg p-4 pt-8 text-2xl font-mono teal-accent" style="border-color: var(--color-red);" focus:border-teal-accent transition-all outline-none">
                    </div>
                </div>
            </div>

            <!-- 3. 飯店/緊急聯絡區 (HOTEL) - 預設隱藏 -->
            <div id="hotel-view-main" class="hidden flat-panel rounded-xl p-8 lg:p-12 h-full min-h-[500px]">
                <h3 class="text-3xl font-bold teal-accent mb-8 pb-3 border-b border-gray-300 flex items-center">
                    <i data-lucide="map-pin" class="w-8 h-8 mr-3"></i> 緊急聯絡/飯店地址卡
                </h3>
                
                <div class="max-w-xl mx-auto space-y-6">
                    <!-- 顯示模式 -->
                    <div id="hotel-display-mode">
                        <div class="mb-4">
                            <div class="text-xs text-gray-500 uppercase tracking-widest mb-1">飯店名稱 (HOTEL NAME)</div>
                            <div id="h-name" class="font-black text-2xl leading-tight" style="color: var(--color-dark-navy);">未設定飯店</div>
                        </div>

                        <!-- 地址卡片，使用強調色邊框和背景 -->
                        <div class="p-6 bg-gray-100 rounded-lg border-2 mb-6 relative group cursor-pointer shadow-xl" 
                             style="border-color: var(--color-teal);" onclick="copyAddr()">
                            <div class="text-xs teal-accent uppercase mb-2 flex justify-between items-center font-medium">
                                <span>地址 (ADDRESS - JP) <span class="text-gray-500">(給司機或路人看)</span></span>
                                <i data-lucide="copy" class="w-4 h-4 teal-accent opacity-80 group-hover:opacity-100 transition-opacity"></i>
                            </div>
                            <div id="h-addr" class="font-mono text-base break-all leading-relaxed" style="color: var(--color-dark-navy);">請點擊下方按鈕設定地址</div>
                            <div class="text-xs text-gray-500 mt-3 border-t border-gray-300 pt-2">
                                電話: <span id="h-phone">無資料</span>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-4">
                            <button onclick="copyAddr()" class="text-white font-bold py-3 rounded-lg text-base transition-colors flex items-center justify-center shadow-lg"
                                    style="background-color: var(--color-teal); hover:opacity-90;">
                                <i data-lucide="clipboard-check" class="w-5 h-5 mr-2"></i> 複製日文地址
                            </button>
                            <button onclick="toggleEdit()" class="text-white font-medium py-3 rounded-lg text-base transition-colors flex items-center justify-center"
                                    style="background-color: var(--color-red); hover:opacity-90;">
                                <i data-lucide="settings-2" class="w-5 h-5 mr-2"></i> 設定/修改資料
                            </button>
                        </div>
                    </div>

                    <!-- 編輯模式 (預設隱藏) -->
                    <div id="hotel-edit-mode" class="hidden space-y-4">
                        <input type="text" id="in-name" placeholder="飯店名稱" class="w-full bg-gray-100 border border-gray-300 rounded-lg p-3 text-sm" style="color: var(--color-dark-navy); border-color: var(--color-red);" focus:border-teal-accent outline-none">
                        <textarea id="in-addr" rows="4" placeholder="日文地址 (最重要)" class="w-full bg-gray-100 border border-gray-300 rounded-lg p-3 text-sm" style="color: var(--color-dark-navy); border-color: var(--color-red);" focus:border-teal-accent outline-none"></textarea>
                        <input type="text" id="in-phone" placeholder="飯店電話" class="w-full bg-gray-100 border border-gray-300 rounded-lg p-3 text-sm" style="color: var(--color-dark-navy); border-color: var(--color-red);" focus:border-teal-accent outline-none">
                        <div class="grid grid-cols-2 gap-4 mt-4">
                            <button onclick="saveHotel()" class="text-white py-3 rounded-lg text-base font-bold" style="background-color: var(--color-teal); hover:opacity-90;">儲存</button>
                            <button onclick="toggleEdit()" class="text-white py-3 rounded-lg text-base" style="background-color: var(--color-red); hover:opacity-90;">取消</button>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- 4. 購物清單/預算追蹤區 (SHOPPING) - 預設隱藏 -->
            <div id="shopping-view" class="hidden flat-panel rounded-xl p-8 lg:p-12 h-full min-h-[500px]">
                <h3 class="text-3xl font-bold teal-accent mb-8 pb-3 border-b border-gray-300 flex items-center">
                    <i data-lucide="list-checks" class="w-8 h-8 mr-3"></i> 購物清單 & 預算追蹤
                </h3>
                
                <!-- 總結看板 -->
                <div class="grid grid-cols-3 gap-4 mb-8 text-center text-white font-bold">
                    <div class="p-4 rounded-lg shadow-md" style="background-color: var(--color-red);">
                        <div class="text-xs opacity-80 mb-1">總項目數</div>
                        <div id="total-items" class="text-2xl">0</div>
                    </div>
                    <div class="p-4 rounded-lg shadow-md" style="background-color: var(--color-dark-navy);">
                        <div class="text-xs opacity-80 mb-1">已購總額 (JPY)</div>
                        <div id="total-cost-jpy" class="text-2xl font-mono">¥ 0</div>
                    </div>
                    <div class="p-4 rounded-lg shadow-md" style="background-color: var(--color-teal);">
                        <div class="text-xs opacity-80 mb-1">台幣參考價 (TWD)</div>
                        <div id="total-cost-twd" class="text-2xl font-mono">NT$ 0</div>
                    </div>
                </div>
                
                <!-- 新增項目表單 -->
                <div class="mb-8 p-4 bg-gray-100 rounded-lg border" style="border-color: var(--color-red);">
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="itemName" placeholder="商品名稱 (e.g. 薯條三兄弟)" class="col-span-1 md:col-span-2 p-2 border rounded-lg focus:outline-none focus:ring-1" style="border-color: var(--color-red);">
                        <input type="number" id="itemPrice" placeholder="單價 (JPY)" class="p-2 border rounded-lg focus:outline-none focus:ring-1" style="border-color: var(--color-red);">
                        <button onclick="addItem()" class="text-white font-bold py-2 rounded-lg transition-colors flex items-center justify-center shadow-lg"
                                style="background-color: var(--color-teal); hover:opacity-90;">
                            <i data-lucide="plus" class="w-5 h-5 mr-1"></i> 加入清單
                        </button>
                    </div>
                </div>

                <!-- 購物清單列表 -->
                <div id="shopping-list-container" class="space-y-2">
                    <!-- JS 動態插入購物清單項目 -->
                    <div class="text-center text-gray-400 p-8" id="empty-list-message">清單尚無項目。開始購物吧！</div>
                </div>
            </div>

            <!-- 5. 常用日語速查表區 (PHRASEBOOK) - NEW -->
            <div id="phrasebook-view" class="hidden flat-panel rounded-xl p-8 lg:p-12 h-full min-h-[500px]">
                <h3 class="text-3xl font-bold teal-accent mb-8 pb-3 border-b border-gray-300 flex items-center">
                    <i data-lucide="message-square" class="w-8 h-8 mr-3"></i> 常用日語速查表 (觀光情境)
                </h3>
                
                <div class="space-y-6">
                    <!-- Category 1: 基礎禮貌 -->
                    <div class="border-b pb-4" style="border-color: var(--color-red);">
                        <h4 class="text-xl font-bold mb-3 flex items-center red-accent"><i data-lucide="smile" class="w-5 h-5 mr-2"></i> 基礎與禮貌</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">謝謝 (禮貌)</p><p class="teal-accent text-lg">ありがとうございます</p><p class="text-xs text-gray-500">Arigato Gozaimasu</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">不好意思 / 請問</p><p class="teal-accent text-lg">すみません</p><p class="text-xs text-gray-500">Sumimasen</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">對不起</p><p class="teal-accent text-lg">ごめんなさい</p><p class="text-xs text-gray-500">Gomennasai</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">請多關照</p><p class="teal-accent text-lg">よろしくお願いします</p><p class="text-xs text-gray-500">Yoroshiku Onegaishimasu</p></div>
                        </div>
                    </div>

                    <!-- Category 2: 購物與服務 -->
                    <div class="border-b pb-4" style="border-color: var(--color-red);">
                        <h4 class="text-xl font-bold mb-3 flex items-center red-accent"><i data-lucide="shopping-bag" class="w-5 h-5 mr-2"></i> 購物與服務</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">這個多少錢?</p><p class="teal-accent text-lg">いくらですか</p><p class="text-xs text-gray-500">Ikura desu ka?</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">請給我看那個</p><p class="teal-accent text-lg">あれを見せてください</p><p class="text-xs text-gray-500">Are o misete kudasai</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">我要這個</p><p class="teal-accent text-lg">これください</p><p class="text-xs text-gray-500">Kore kudasai</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">有免稅嗎?</p><p class="teal-accent text-lg">免税 (めんぜい) ありますか</p><p class="text-xs text-gray-500">Menzei arimasu ka?</p></div>
                        </div>
                    </div>

                    <!-- Category 3: 交通與尋路 -->
                    <div>
                        <h4 class="text-xl font-bold mb-3 flex items-center red-accent"><i data-lucide="navigation" class="w-5 h-5 mr-2"></i> 交通與尋路</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">請問廁所在哪裡?</p><p class="teal-accent text-lg">トイレはどこですか</p><p class="text-xs text-gray-500">Toire wa doko desu ka?</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">到車站怎麼走?</p><p class="teal-accent text-lg">駅 (えき) はどう行けばいいですか</p><p class="text-xs text-gray-500">Eki wa dou ikeba ii desu ka?</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">這個到新宿嗎?</p><p class="teal-accent text-lg">これ、新宿 (しんじゅく) に行きますか</p><p class="text-xs text-gray-500">Kore, Shinjuku ni ikimasu ka?</p></div>
                            <div class="bg-gray-100 p-3 rounded-lg border" style="border-color: rgba(44, 187, 173, 0.2);"><p class="text-sm font-bold">請在這裡停車</p><p class="teal-accent text-lg">ここで停 (と) めてください</p><p class="text-xs text-gray-500">Koko de tomete kudasai</p></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="h-12"></div> <!-- 底部留白 -->
        </main>

    </div>

    <!-- JavaScript 邏輯 -->
    <script>
        // --- 1. 詳細行程資料 (更新：加入 mapQuery) ---
        const itineraryData = [
            {
                day: 1, date: "12/26 (四)", title: "抵達與高輪新區",
                weather: { icon: "plane", cond: "晴朗乾冷", temp: "3° / 10°", loc: "東京", alert: false },
                slots: [
                    { 
                        time: "Morning", title: "抵達與入住 Takanawa Gateway", icon: "plane",
                        desc: "前往飯店辦理入住或寄放行李。Takanawa Gateway 是隈研吾設計的新車站，以「折紙」為靈感的屋頂結構非常值得一看。",
                        hours: "車站全日開放 / 飯店通常 15:00 入住",
                        mapQuery: "Takanawa Gateway Station, Tokyo", 
                        expert: "【建築迷必看】車站的挑高玻璃帷幕在早晨光影下非常美。如果行李不多，建議走去二樓的無人商店體驗一下高科技結帳。"
                    },
                    { 
                        time: "Lunch", title: "NEWoMan 高輪/品川", icon: "coffee",
                        desc: "為了避開人潮，建議在站內的麵包店或 NEWoMan 的輕食區用餐。",
                        hours: "11:00 - 21:00 (餐廳區)",
                        mapQuery: "NEWoMan Takanawa, Tokyo", 
                        expert: "【避雷指南】品川站中午人潮非常恐怖，建議避開「品達麵達」拉麵街，改去 NEWoMan 的 Blue Bottle 或麵包名店，環境舒服很多。"
                    },
                    { 
                        time: "Afternoon", title: "KITTE 丸之內", icon: "shopping-bag",
                        desc: "由舊東京中央郵局改建的商場，保留了昭和時代的現代主義風格。",
                        hours: "11:00 - 20:00",
                        mapQuery: "KITTE Marunouchi, Tokyo", 
                        expert: "【攝影點】務必上 6F 的屋頂花園「KITTE Garden」。這裡是免費拍攝東京車站丸之內紅磚站舍「全景」的最佳位置，不用人擠人。"
                    },
                    { 
                        time: "Dinner", title: "品川周邊在地居酒屋", icon: "beer",
                        desc: "探索品川港南口巷弄內的傳統居酒屋，感受上班族下班後的氛圍。",
                        hours: "17:00 - 23:00",
                        mapQuery: "Shinagawa Konan Exit Izakaya, Tokyo", 
                        expert: "【點餐攻略】找掛有紅燈籠的店準沒錯。推薦點「煮込み (內臟燉煮)」配生啤酒。如果不會日文，用 Google 翻譯圖片菜單通常沒問題。"
                    }
                ]
            },
            {
                day: 2, date: "12/27 (五)", title: "丸之內與八重洲",
                weather: { icon: "cloud-sun", cond: "多雲轉晴", temp: "4° / 11°", loc: "東京", alert: false },
                slots: [
                    { 
                        time: "Morning", title: "皇居外苑散策", icon: "camera",
                        desc: "清晨的皇居外苑空氣清新，是東京市中心的綠洲。",
                        hours: "全日開放",
                        mapQuery: "Imperial Palace East Garden, Tokyo", 
                        expert: "【歷史小知識】必拍的「二重橋」其實平常是關閉的，只有新年或天皇即位等大典才會開放。清晨來可以看到最美的倒影。"
                    },
                    { 
                        time: "Lunch", title: "KITTE 美食街", icon: "utensils",
                        desc: "選擇 B1 的根室花丸迴轉壽司(需排隊) 或 5F 的特色餐廳。",
                        hours: "11:00 - 22:00",
                        mapQuery: "KITTE B1 Gourmet, Tokyo", 
                        expert: "【省時技巧】根室花丸通常要排 2 小時。若不想等，直接去 5F 的蕎麥麵店，視野好且翻桌快。記得選靠窗位可以看到東京站火車進出。"
                    },
                    { 
                        time: "Afternoon", title: "東京車站畫廊 / 銀座", icon: "map",
                        desc: "位於東京車站紅磚建築內的精緻美術館，保留了百年前的紅磚牆。",
                        hours: "10:00 - 18:00 (週一休)",
                        mapQuery: "Tokyo Station Gallery", 
                        expert: "【文青推薦】這裡的展覽通常很有品味且人少。看完展覽沿著八重洲地下街走到銀座，一路上有很多日本各地的「物產館」(Antenna Shop) 可以買伴手禮。"
                    },
                    { 
                        time: "Dinner", title: "Sumiyaki Unafuji (八重洲)", icon: "star", highlight: true,
                        desc: "來自名古屋的特級鰻魚飯，炭火直烤，皮脆肉嫩。",
                        hours: "11:00 - 22:00 (需預約)",
                        mapQuery: "Sumiyaki Unafuji Yaesu", 
                        expert: "【饕客吃法】這裡有名的是「Hitsumabushi (鰻魚三吃)」。第一碗吃原味，第二碗加蔥花芥末，第三碗加高湯變茶泡飯。這是名古屋流的正宗吃法！"
                    }
                ]
            },
            {
                day: 3, date: "12/28 (六)", title: "下町懷舊日",
                weather: { icon: "sun", cond: "晴朗", temp: "2° / 9°", loc: "東京", alert: false },
                slots: [
                    { 
                        time: "Morning", title: "築地場外市場", icon: "sunrise",
                        desc: "雖然市場搬去豐洲，但場外的美食街依然熱鬧非凡。",
                        hours: "05:00 - 14:00 (各店不同)",
                        mapQuery: "Tsukiji Outer Market, Tokyo", 
                        expert: "【行家指南】絕對要在 8:30 前到。必吃「玉子燒」串和「狐狸屋」的牛雜丼(Horumon Don)。牛雜丼味道很重，不喜歡內臟的可以改吃海鮮丼。"
                    },
                    { 
                        time: "Lunch", title: "月島文字燒街", icon: "flame",
                        desc: "整條街都是文字燒店，自己動手煎非常有樂趣。",
                        hours: "11:00 - 22:00",
                        mapQuery: "Tsukishima Monja Street, Tokyo", 
                        expert: "【操作教學】如果不熟練，大膽請店員幫忙煎(Sumimasen, help!)。推薦點「明太子麻糬起司」口味，這是絕對不會失敗的經典組合。"
                    },
                    { 
                        time: "Afternoon", title: "清澄白河", icon: "coffee",
                        desc: "東京的咖啡激戰區，擁有Blue Bottle一號店和許多獨立烘豆所。",
                        hours: "10:00 - 18:00",
                        mapQuery: "Kiyosumi Shirakawa, Tokyo", 
                        expert: "【散步路線】從東京都現代美術館開始，沿路逛 fukadaso (舊公寓改建的雜貨店)。氣氛非常悠閒，適合買咖啡豆當伴手禮。"
                    },
                    { 
                        time: "Dinner", title: "門前仲町", icon: "utensils",
                        desc: "充滿江戶風情的街區，是當地人下班喝酒的聖地。",
                        hours: "17:00 - 深夜",
                        mapQuery: "Monzen-Nakacho Izakaya, Tokyo", 
                        expert: "【美食推薦】這裡的名物是「深川飯」(蛤蜊炊飯)。如果想吃肉，這裡也有很多高CP值的立食燒肉店，站著吃更便宜！"
                    }
                ]
            },
            {
                day: 4, date: "12/29 (日)", title: "富士山一日遊",
                weather: { icon: "mountain", cond: "極凍晴天", temp: "-5° / 1°", loc: "富士山", alert: true, note: "體感極低，務必穿著防風外套" },
                slots: [
                    { 
                        time: "Morning", title: "富士山五合目", icon: "mountain",
                        desc: "巴士直達富士山半山腰，最近距離感受神山的魄力。",
                        hours: "道路視天候開放",
                        mapQuery: "Fuji Subaru Line 5th Station", 
                        expert: "【重要提醒】12月底五合目可能因積雪道路封閉，若無法上山，巴士通常會改去「富士山世界遺產中心」。記得去神社買「富士山御守」，很靈驗。"
                    },
                    { 
                        time: "Lunch", title: "河口湖鄉土料理", icon: "soup",
                        desc: "品嚐山梨縣名物「餺飥麵 (Houtou)」，蔬菜味噌湯底配寬麵。",
                        hours: "11:00 - 15:00",
                        mapQuery: "Houtou Fudo Kawaguchiko", 
                        expert: "【名店推薦】「不動茶屋 (Houtou Fudo)」很有名。如果不喜歡排隊，隨便一家家庭料理店的餺飥麵其實都很好吃，重點是加一點店家特製的辣醬。"
                    },
                    { 
                        time: "Afternoon", title: "忍野八海", icon: "snowflake",
                        desc: "富士山融雪形成的八個池塘，水質清澈見底。",
                        hours: "全日開放",
                        mapQuery: "Oshino Hakkai, Yamanashi", 
                        expert: "【冬季限定】冬天人稍微少一點，空氣乾燥讓富士山看起來更清晰。小心池邊木棧道結冰會滑。這裡的水豆腐和烤草餅是必吃小點心。"
                    },
                    { 
                        time: "Dinner", title: "新宿西口晚餐", icon: "utensils",
                        desc: "一日遊巴士回到新宿後，直接在西口大樓內用餐。",
                        hours: "11:00 - 23:00",
                        mapQuery: "Shinjuku West Exit Dinner", 
                        expert: "【避人潮】不要去歌舞伎町(東口)，人多又雜。西口的 Odakyu 或 Keio 百貨樓上餐廳街品質穩定，而且通常不用排太久。"
                    }
                ]
            },
            {
                day: 5, date: "12/30 (一)", title: "代官山時尚",
                weather: { icon: "sun", cond: "晴朗", temp: "5° / 12°", loc: "東京", alert: false },
                slots: [
                    { 
                        time: "Morning", title: "代官山 蔦屋書店", icon: "book",
                        desc: "被譽為世界最美書店之一，在此享受高品質的閱讀時光。",
                        hours: "09:00 - 22:00",
                        mapQuery: "Daikanyama T-Site Tsutaya Books", 
                        expert: "【氛圍體驗】二樓的 Anjin Lounge 可以邊喝調酒邊看珍稀古書。早上去人最少，是拍照打卡的最佳時機，光線最好。"
                    },
                    { 
                        time: "Lunch", title: "惠比壽巷弄午餐", icon: "utensils",
                        desc: "這一帶有很多隱藏版的義式或法式小餐館。",
                        hours: "11:30 - 14:30",
                        mapQuery: "Ebisu Lunch Spots, Tokyo", 
                        expert: "【高CP值】尋找門口有寫「Lunch Menu 1000 yen」的小黑板。東京的午間套餐通常非常超值，能用便宜價格吃到精緻料理。"
                    },
                    { 
                        time: "Afternoon", title: "裏原宿 / 貓街", icon: "shopping-bag",
                        desc: "連接澀谷與原宿的步行街，充滿個性潮牌與古著店。",
                        hours: "11:00 - 20:00",
                        mapQuery: "Cat Street, Harajuku", 
                        expert: "【逛街攻略】不要走竹下通(人太多)。走貓街(Cat Street)可以逛到很多限量球鞋店和設計師品牌。推薦去「RAGTAG」挖寶二手名牌。"
                    },
                    { 
                        time: "Dinner", title: "惠比壽橫丁", icon: "beer",
                        desc: "室內屋台街，集合了多家不同風格的居酒屋。",
                        hours: "17:00 - 04:00",
                        mapQuery: "Ebisu Yokocho, Tokyo", 
                        expert: "【社交聖地】這裡氣氛非常High，日本人很喜歡來這聯誼。如果想體驗熱鬧氣氛來這準沒錯，但記得要早點到，不然一位難求。"
                    }
                ]
            },
            {
                day: 6, date: "12/31 (二)", title: "谷中銀座與返程",
                weather: { icon: "cloud-sun", cond: "晴時多雲", temp: "1° / 8°", loc: "東京", alert: false },
                slots: [
                    { 
                        time: "Morning", title: "谷中銀座商店街", icon: "shopping-bag",
                        desc: "倖存於二戰空襲的老街，保有昭和時代的懷舊感。",
                        hours: "10:00 - 18:00",
                        mapQuery: "Yanaka Ginza Shotengai, Tokyo", 
                        expert: "【貓奴必去】這裡被稱為「貓町」，雖然真貓不一定多，但有很多貓咪造型的甜點和雜貨。必吃「肉之鈴木」的元氣炸肉餅 (Menchi Katsu)。"
                    },
                    { 
                        time: "Lunch", title: "谷中蕎麥麵", icon: "utensils",
                        desc: "在老宅改建的麵店享用最後的日式午餐。",
                        hours: "11:00 - 15:00",
                        mapQuery: "Yanaka Soba Restaurant, Tokyo", 
                        expert: "【過年習俗】12/31 日本人有吃「年越蕎麥麵」的習慣，象徵斬斷今年的厄運。所以今天的蕎麥麵店可能會大排長龍，建議提早用餐！"
                    },
                    { 
                        time: "Afternoon", title: "前往機場", icon: "plane",
                        desc: "搭乘 Skyliner (從日暮里) 或 NEX 前往機場。",
                        hours: "-",
                        mapQuery: "Narita Airport T1", 
                        expert: "【最後採購】如果還沒買夠，機場免稅店的「Tokyo Banana」和「白色戀人」雖然老套但絕對受歡迎。記得預留至少 3 小時到機場。"
                    },
                    { 
                        time: "Dinner", title: "機上/機場晚餐", icon: "home",
                        desc: "結束美好的旅程。",
                        hours: "-",
                        mapQuery: "Home", 
                        expert: "【回家小心】檢查護照、錢包。期待下次再來日本！"
                    }
                ]
            }
        ];

        let currentDay = 1;
        // 追蹤目前的顯示模式: 'ITINERARY', 'CURRENCY', 'HOTEL', 'SHOPPING', 'PHRASEBOOK'
        let viewMode = 'ITINERARY'; 


        // --- 2. 介面渲染及切換邏輯 (Render and View Switching Logic) ---
        
        // 切換主內容區塊的顯示
        function updateMainView() {
            // 隱藏所有內容區塊
            document.getElementById('itinerary-view').classList.add('hidden');
            document.getElementById('currency-view').classList.add('hidden');
            document.getElementById('hotel-view-main').classList.add('hidden');
            document.getElementById('shopping-view').classList.add('hidden'); 
            document.getElementById('phrasebook-view').classList.add('hidden'); // NEW Phrasebook

            // 顯示當前的區塊
            if (viewMode === 'ITINERARY') {
                document.getElementById('itinerary-view').classList.remove('hidden');
                renderItinerary();
            } else if (viewMode === 'CURRENCY') {
                document.getElementById('currency-view').classList.remove('hidden');
            } else if (viewMode === 'HOTEL') {
                document.getElementById('hotel-view-main').classList.remove('hidden');
            } else if (viewMode === 'SHOPPING') {
                document.getElementById('shopping-view').classList.remove('hidden');
                renderShoppingList(); // 渲染購物清單
            } else if (viewMode === 'PHRASEBOOK') { // NEW
                 document.getElementById('phrasebook-view').classList.remove('hidden');
            }
            
            // 更新導覽按鈕狀態
            renderNav();
        }

        // 設置新的視圖模式
        function setView(mode) {
            viewMode = mode;
            // 如果從工具頁面切回行程，則預設回到 DAY 1
            if (mode === 'ITINERARY' && currentDay === 'TOOL') {
                currentDay = 1;
            }
            updateMainView();
        }

        // 渲染左側導覽列 (包含日期和工具按鈕的選中狀態)
        function renderNav() {
            const container = document.getElementById('nav-container');
            container.innerHTML = '';
            
            // 統一的按鈕樣式基礎
            const baseClass = 'modern-btn group flex items-center justify-between text-white shadow-md';
            // 活躍狀態: 水鴨綠背景，奶油色文字
            const activeBg = 'var(--color-teal)';
            const activeColor = 'var(--color-cream)'; 
            // 非活躍狀態: 深海軍藍背景，奶油色文字，警示紅邊框
            const inactiveBg = 'var(--color-dark-navy)';
            const inactiveColor = 'var(--color-cream)';
            const inactiveBorder = 'var(--color-red)';


            // --- A. 渲染日期按鈕 ---
            itineraryData.forEach(d => {
                const btn = document.createElement('button');
                const active = d.day === currentDay && viewMode === 'ITINERARY';
                
                // 應用動態顏色
                btn.className = `${baseClass} ${active ? 'font-extrabold' : 'font-medium'}`;
                btn.style.backgroundColor = active ? activeBg : inactiveBg;
                btn.style.color = active ? activeColor : inactiveColor;
                btn.style.borderColor = active ? 'transparent' : inactiveBorder;
                
                btn.innerHTML = `
                    <div>
                        <span class="block text-[10px] font-bold uppercase tracking-wider text-gray-400">DAY ${d.day}</span>
                        <span class="font-bold text-sm lg:text-base">${d.date.split(' ')[0]}</span>
                    </div>
                    ${active ? '<i data-lucide="chevron-right" class="w-5 h-5 hidden lg:block text-white"></i>' : ''}
                `;
                btn.onclick = () => { currentDay = d.day; setView('ITINERARY'); }; 
                container.appendChild(btn);
            });
            
            // --- B. 渲染分隔線 (僅電腦上顯示) ---
            const separator = document.createElement('div');
            separator.className = "hidden lg:block h-px my-2";
            separator.style.backgroundColor = 'var(--color-red)'; /* 警示紅分隔線 */
            container.appendChild(separator);


            // --- C. 渲染工具按鈕 ---

            // 1. 日幣匯率試算
            const btnCurrency = document.createElement('button');
            const activeCurrency = viewMode === 'CURRENCY';
            btnCurrency.className = `${baseClass} flex items-center justify-center lg:justify-start ${activeCurrency ? 'font-extrabold' : 'font-medium'}`;
            btnCurrency.style.backgroundColor = activeCurrency ? activeBg : inactiveBg;
            btnCurrency.style.color = activeCurrency ? activeColor : inactiveColor;
            btnCurrency.style.borderColor = activeCurrency ? 'transparent' : inactiveBorder;

            btnCurrency.innerHTML = `<i data-lucide="coins" class="w-5 h-5 mr-0 lg:mr-2"></i> 
                                     <span class="hidden lg:inline">日幣試算</span>`;
            btnCurrency.onclick = () => setView('CURRENCY');
            container.appendChild(btnCurrency);

            // 2. 緊急聯絡/飯店地址卡 (HOTEL)
            const btnHotel = document.createElement('button');
            const activeHotel = viewMode === 'HOTEL';
            btnHotel.className = `${baseClass} flex items-center justify-center lg:justify-start ${activeHotel ? 'font-extrabold' : 'font-medium'}`;
            btnHotel.style.backgroundColor = activeHotel ? activeBg : inactiveBg;
            btnHotel.style.color = activeHotel ? activeColor : inactiveColor;
            btnHotel.style.borderColor = activeHotel ? 'transparent' : inactiveBorder;

            btnHotel.innerHTML = `<i data-lucide="map-pin" class="w-5 h-5 mr-0 lg:mr-2"></i> 
                                  <span class="hidden lg:inline">緊急聯絡卡</span>`;
            btnHotel.onclick = () => setView('HOTEL');
            container.appendChild(btnHotel);
            
            // 3. 常用日語速查表 (PHRASEBOOK) - NEW: 放在緊急聯絡卡右邊/下方
            const btnPhrasebook = document.createElement('button');
            const activePhrasebook = viewMode === 'PHRASEBOOK';
            btnPhrasebook.className = `${baseClass} flex items-center justify-center lg:justify-start ${activePhrasebook ? 'font-extrabold' : 'font-medium'}`;
            btnPhrasebook.style.backgroundColor = activePhrasebook ? activeBg : inactiveBg;
            btnPhrasebook.style.color = activePhrasebook ? activeColor : inactiveColor;
            btnPhrasebook.style.borderColor = activePhrasebook ? 'transparent' : inactiveBorder;
            btnPhrasebook.innerHTML = `<i data-lucide="message-square" class="w-5 h-5 mr-0 lg:mr-2"></i> 
                                     <span class="hidden lg:inline">常用日語</span>`;
            btnPhrasebook.onclick = () => setView('PHRASEBOOK');
            container.appendChild(btnPhrasebook);


            // 4. 購物清單
            const btnShopping = document.createElement('button');
            const activeShopping = viewMode === 'SHOPPING';
            btnShopping.className = `${baseClass} flex items-center justify-center lg:justify-start ${activeShopping ? 'font-extrabold' : 'font-medium'}`;
            btnShopping.style.backgroundColor = activeShopping ? activeBg : inactiveBg;
            btnShopping.style.color = activeShopping ? activeColor : inactiveColor;
            btnShopping.style.borderColor = activeShopping ? 'transparent' : inactiveBorder;
            btnShopping.innerHTML = `<i data-lucide="list-checks" class="w-5 h-5 mr-0 lg:mr-2"></i> 
                                     <span class="hidden lg:inline">購物清單</span>`;
            btnShopping.onclick = () => setView('SHOPPING');
            container.appendChild(btnShopping);
            

            lucide.createIcons(); // 重新渲染新加入的 icon
        }
        
        // 渲染中間的行程內容 (只有在 viewMode = 'ITINERARY' 時被呼叫)
        function renderItinerary() {
            const data = itineraryData.find(d => d.day === currentDay);
            
            // 1. 更新天氣卡片
            const weatherIconHtml = `<i data-lucide="${data.weather.icon}" class="w-8 h-8"></i>`;
            document.getElementById('weather-icon').innerHTML = weatherIconHtml;

            document.getElementById('weather-condition').textContent = data.weather.cond;
            document.getElementById('weather-note').textContent = data.weather.note || '';
            document.getElementById('weather-location').textContent = data.weather.loc;
            
            const [low, high] = data.weather.temp.split(' / ');
            document.getElementById('weather-low').textContent = low;
            document.getElementById('weather-high').textContent = high;

            const wCard = document.getElementById('weather-card');
            // 警報狀態使用紅色邊框
            if(data.weather.alert) {
                wCard.className = "flat-panel rounded-lg p-5 relative overflow-hidden transition-all duration-500 border-2 shadow-lg";
                wCard.style.borderColor = 'var(--color-red)';
            } else {
                wCard.className = "flat-panel rounded-lg p-5 relative overflow-hidden transition-all duration-500";
                wCard.style.borderColor = 'rgba(44, 187, 173, 0.3)';
            }

            // 2. 更新標題 
            document.getElementById('day-number').textContent = `DAY ${data.day}`;
            document.getElementById('day-title').textContent = data.title;

            // 3. 更新行程列表 
            const list = document.getElementById('itinerary-list');
            list.innerHTML = '';
            
            data.slots.forEach(slot => {
                const el = document.createElement('div');
                const isHigh = slot.highlight;
                
                // 圖標背景使用水鴨綠 (強調色 1)
                const slotIconBg = 'var(--color-teal)'; 
                const slotIconColor = 'var(--color-cream)'; // 奶油色圖標
                // 卡片邊框使用警示紅 (強調色 2)
                const slotBorderColor = isHigh ? 'var(--color-red)' : 'rgba(44, 187, 173, 0.3)';
                const slotBorderWidth = isHigh ? 'border-2' : 'border';

                // 計算地圖查詢字串 (使用 mapQuery，如果沒有則使用 title)
                const mapSearchQuery = encodeURIComponent(slot.mapQuery || slot.title + ", Tokyo");
                
                el.innerHTML = `
                    <div class="flat-panel p-5 rounded-xl ${slotBorderWidth} shadow-lg hover:shadow-xl transition-all duration-300" 
                         style="border-color: ${slotBorderColor};">
                        <div class="flex gap-4 mb-4">
                            <div class="flex-shrink-0">
                                <div class="w-12 h-12 rounded-full flex items-center justify-center shadow-md" style="background-color: ${slotIconBg}; color: ${slotIconColor};">
                                    <i data-lucide="${slot.icon}" class="w-6 h-6"></i>
                                </div>
                            </div>
                            <div class="flex-grow">
                                <div class="flex flex-wrap justify-between items-start mb-1">
                                    <span class="text-[10px] font-bold uppercase tracking-wider text-white px-2 py-0.5 rounded-sm" style="background-color: var(--color-red);">
                                        ${slot.time}
                                    </span>
                                    <span class="text-[10px] text-gray-600 flex items-center bg-gray-200 border border-gray-300 px-2 py-0.5 rounded-sm">
                                        <i data-lucide="clock" class="w-3 h-3 mr-1 text-gray-500"></i> ${slot.hours}
                                    </span>
                                </div>
                                <h4 class="text-lg font-bold" style="color: var(--color-dark-navy);">${slot.title}</h4>
                                <p class="text-sm text-gray-700 leading-relaxed">${slot.desc}</p>
                            </div>
                        </div>
                        
                        <!-- NEW: 地圖連結按鈕 (一鍵導航) -->
                        <div class="mt-3">
                            <a href="https://www.google.com/maps/search/?api=1&query=${mapSearchQuery}" 
                               target="_blank" 
                               class="inline-flex items-center text-xs font-medium px-3 py-1 rounded-full text-white transition-colors duration-200 shadow-md hover:opacity-90"
                               style="background-color: var(--color-teal);">
                                <i data-lucide="map" class="w-3 h-3 mr-1"></i>
                                在地圖上開啟
                            </a>
                        </div>

                        <!-- 專家/導遊提示區塊 -->
                        <div class="expert-tip-box rounded-lg p-3 text-xs lg:text-sm flex gap-3 shadow-inner mt-4">
                            <i data-lucide="lightbulb" class="w-4 h-4" style="color: var(--color-teal); flex-shrink-0 mt-0.5;"></i>
                            <div>
                                <span class="font-bold teal-accent block mb-0.5">VIBE TIP:</span>
                                ${slot.expert}
                            </div>
                        </div>
                    </div>
                `;
                list.appendChild(el);
            });

            lucide.createIcons();
        }

        // --- 3. 工具箱邏輯 (Tools Logic) ---
        
        // 匯率換算
        function convert(source) {
            // 從 LocalStorage 獲取最新的匯率，如果沒有就用預設值 4.80
            const rate = parseFloat(localStorage.getItem('trip_rate') || document.getElementById('rateInput').value) || 4.80;
            document.getElementById('rateInput').value = rate.toFixed(2); // 確保顯示最新值

            const twdEl = document.getElementById('twdInput');
            const jpyEl = document.getElementById('jpyInput');

            if (source === 'TWD') {
                const val = parseFloat(twdEl.value);
                jpyEl.value = isNaN(val) ? '' : (val * rate).toFixed(0);
            } else {
                const val = parseFloat(jpyEl.value);
                twdEl.value = isNaN(val) ? '' : (val / rate).toFixed(2);
            }
            // 每次輸入時都儲存匯率，以便購物清單使用
            localStorage.setItem('trip_rate', document.getElementById('rateInput').value);
        }

        // 飯店卡資料讀取 (LocalStorage)
        function loadHotel() {
            const name = localStorage.getItem('trip_h_name') || '未設定飯店';
            const addr = localStorage.getItem('trip_h_addr') || '請點擊下方按鈕設定地址';
            const phone = localStorage.getItem('trip_h_phone') || '無資料';
            
            document.getElementById('h-name').textContent = name;
            document.getElementById('h-addr').textContent = addr;
            document.getElementById('h-phone').textContent = phone;
            
            // 同步填入編輯框
            document.getElementById('in-name').value = name === '未設定飯店' ? '' : name;
            document.getElementById('in-addr').value = addr === '請點擊下方按鈕設定地址' ? '' : addr;
            document.getElementById('in-phone').value = phone === '無資料' ? '' : phone;
        }

        // 飯店卡資料儲存
        function saveHotel() {
            localStorage.setItem('trip_h_name', document.getElementById('in-name').value || '未設定飯店');
            localStorage.setItem('trip_h_addr', document.getElementById('in-addr').value || '請點擊下方按鈕設定地址');
            localStorage.setItem('trip_h_phone', document.getElementById('in-phone').value || '無資料');
            toggleEdit();
            loadHotel();
            // 在儲存後停留在飯店卡頁面
            setView('HOTEL');
        }

        // 切換飯店卡編輯/顯示模式
        function toggleEdit() {
            const view = document.getElementById('hotel-display-mode');
            const edit = document.getElementById('hotel-edit-mode');
            if (view.classList.contains('hidden')) {
                view.classList.remove('hidden');
                edit.classList.add('hidden');
            } else {
                // 從顯示切換到編輯時，確保輸入框載入最新資料
                loadHotel(); 
                view.classList.add('hidden');
                edit.classList.remove('hidden');
            }
        }

        // 複製地址功能 (已強化提示與錯誤處理)
        function copyAddr() {
            const hAddrEl = document.getElementById('h-addr');
            const text = hAddrEl.textContent;
            // 由於 h-addr 的顏色是寫在 inline style 上，必須先抓取原色
            const originalColor = hAddrEl.style.color; 
            const originalText = text;

            if(!text || text === '請點擊下方按鈕設定地址' || text.includes('設定地址')) {
                hAddrEl.textContent = "⚠️ 請先設定地址！";
                hAddrEl.style.color = 'var(--color-red)'; // Error color
                setTimeout(() => {
                    hAddrEl.textContent = originalText;
                    hAddrEl.style.color = originalColor; // Restore original color
                }, 2000);
                return;
            }
            
            let success = false;
            const el = document.createElement('textarea');
            el.value = text;
            // 將 textarea 隱藏並移出可視區域
            el.style.position = 'absolute';
            el.style.left = '-9999px';
            document.body.appendChild(el);

            try {
                // 確保 textarea 被選中，才能執行複製命令
                el.select();
                success = document.execCommand('copy');
            } catch (err) {
                console.error('Failed to copy text: ', err);
            } finally {
                // 無論成功與否，都要移除臨時元素
                document.body.removeChild(el);
            }

            // 提供視覺回饋
            if (success) {
                hAddrEl.textContent = "✅ 已複製到剪貼簿！ (1.5秒後恢復)";
                hAddrEl.style.color = 'var(--color-teal)'; // Success color
                setTimeout(() => {
                    hAddrEl.textContent = originalText;
                    hAddrEl.style.color = originalColor; // Restore original color
                }, 1500);
            } else {
                 hAddrEl.textContent = "❌ 複製失敗，請手動複製。 (2秒後恢復)";
                 hAddrEl.style.color = 'var(--color-red)'; // Failure color
                 setTimeout(() => {
                    hAddrEl.textContent = originalText;
                    hAddrEl.style.color = originalColor; // Restore original color
                }, 2000);
            }
        }

        // --- 4. 購物清單邏輯 (Shopping List Logic) ---
        let shoppingList = JSON.parse(localStorage.getItem('shopping_list') || '[]');
        
        function saveShoppingList() {
            localStorage.setItem('shopping_list', JSON.stringify(shoppingList));
        }

        function renderShoppingList() {
            const container = document.getElementById('shopping-list-container');
            const rate = parseFloat(localStorage.getItem('trip_rate') || 4.80);
            
            container.innerHTML = '';
            let totalCostJPY = 0;
            let totalDoneItems = 0;

            if (shoppingList.length === 0) {
                document.getElementById('empty-list-message').classList.remove('hidden');
                container.innerHTML = `<div class="text-center text-gray-400 p-8">清單尚無項目。開始購物吧！</div>`;
            } else {
                document.getElementById('empty-list-message').classList.add('hidden');
                
                shoppingList.forEach((item, index) => {
                    const itemTWD = (item.price / rate).toFixed(0);
                    
                    if (item.done) {
                        totalCostJPY += item.price;
                        totalDoneItems++;
                    }
                    
                    const el = document.createElement('div');
                    const doneClass = item.done ? 'item-done' : '';
                    const doneIcon = item.done ? 'square-check' : 'square';

                    el.className = `item-list-row p-4 flex items-center justify-between border-b last:border-b-0 ${doneClass}`;
                    el.style.borderColor = 'rgba(44, 187, 173, 0.2)'; // 柔和的邊界 (水鴨綠)

                    el.innerHTML = `
                        <div class="flex items-center space-x-4 flex-grow min-w-0">
                            <button onclick="toggleDone(${index})" class="teal-accent flex-shrink-0 hover:opacity-70 transition-opacity">
                                <i data-lucide="${doneIcon}" class="w-6 h-6"></i>
                            </button>
                            <span class="font-medium text-base truncate" style="color: inherit;">${item.name}</span>
                        </div>
                        <div class="flex items-center space-x-6 flex-shrink-0">
                            <div class="text-right font-mono">
                                <span class="block text-sm font-bold text-gray-700" style="color: inherit;">¥ ${item.price.toLocaleString()}</span>
                                <span class="block text-xs text-gray-500 mt-0.5">~NT$ ${itemTWD}</span>
                            </div>
                            <button onclick="deleteItem(${index})" class="text-red-500 hover:text-red-700 transition-colors">
                                <i data-lucide="trash-2" class="w-5 h-5"></i>
                            </button>
                        </div>
                    `;
                    container.appendChild(el);
                });
            }
            
            // 更新總結看板
            document.getElementById('total-items').textContent = shoppingList.length;
            document.getElementById('total-cost-jpy').textContent = `¥ ${totalCostJPY.toLocaleString()}`;
            document.getElementById('total-cost-twd').textContent = `NT$ ${(totalCostJPY / rate).toFixed(0).toLocaleString()}`;
            
            lucide.createIcons();
        }
        
        function addItem() {
            const name = document.getElementById('itemName').value.trim();
            const price = parseInt(document.getElementById('itemPrice').value);
            
            if (name === "" || isNaN(price) || price <= 0) {
                // 使用自定義的提示方式代替 alert
                // 這裡簡化為 console.error，避免使用 alert
                console.error("請輸入有效的商品名稱和日幣價格！"); 
                return;
            }

            const newItem = {
                name: name,
                price: price,
                done: false
            };
            
            shoppingList.unshift(newItem); // 新增到清單最前面
            saveShoppingList();
            renderShoppingList();
            
            // 清空輸入欄位
            document.getElementById('itemName').value = '';
            document.getElementById('itemPrice').value = '';
        }

        function toggleDone(index) {
            shoppingList[index].done = !shoppingList[index].done;
            // 將已完成的項目移到清單底部
            if(shoppingList[index].done) {
                 const [item] = shoppingList.splice(index, 1);
                 shoppingList.push(item);
            } 

            saveShoppingList();
            renderShoppingList();
        }

        function deleteItem(index) {
            // 由於不能使用 confirm()，這裡省略刪除確認，直接刪除
            shoppingList.splice(index, 1);
            saveShoppingList();
            renderShoppingList();
        }
        
        // 程式初始化
        window.onload = () => {
            // 確保匯率有預設值
            if (!localStorage.getItem('trip_rate')) {
                localStorage.setItem('trip_rate', '4.80');
            }
            loadHotel(); // 初始化載入飯店資料
            updateMainView(); // 顯示預設的 ITINERARY 視圖 (DAY 1)
            
            // 設定匯率輸入框預設值
            document.getElementById('twdInput').value = 1000;
            convert('TWD');
        };

    </script>
</body>
</html>
