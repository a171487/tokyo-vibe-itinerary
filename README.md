<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOKYO VIBE | 鮮明配色儀表板</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- 引入手寫風字體 (Google Fonts) -->
    <link href="https://fonts.googleapis.com/css2?family=Patrick+Hand&family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
    <style>
        /* 品牌配色定義 */
        :root {
            --color-dark-navy: #1D2A35;
            --color-cream: #F7FCF5;
            --color-teal: #2CBBAD;
            --color-red: #D83D4F;
        }

        /* 全局樣式 */
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            background-color: var(--color-dark-navy); 
            color: var(--color-cream);
            min-height: 100vh;
        }

        /* ------------------- 組件樣式 ------------------- */

        /* 卡片面板 */
        .flat-panel {
            background-color: var(--color-cream);
            color: var(--color-dark-navy);
            border: 1px solid rgba(44, 187, 173, 0.2);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        /* 按鈕 */
        .nav-button {
            transition: all 0.2s;
            cursor: pointer;
        }
        .nav-button.active {
            color: var(--color-teal);
            border-bottom: 3px solid var(--color-teal);
        }
        .btn-primary {
            background-color: var(--color-teal);
            color: var(--color-cream);
            transition: 0.2s;
        }
        .btn-primary:hover { background-color: #24A397; transform: translateY(-1px); }
        .btn-danger {
            background-color: var(--color-red);
            color: var(--color-cream);
            transition: 0.2s;
        }
        .btn-danger:hover { background-color: #C03544; transform: translateY(-1px); }


        /* 顏色強調 */
        .teal-accent { background-color: rgba(44, 187, 173, 0.1); color: var(--color-teal); }
        .red-accent-text { color: var(--color-red); }

        /* 捲軸隱藏 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* ------------------- SKYLINER 時刻表樣式 ------------------- */
        .timetable-header {
            background-color: #1D2A35;
            color: #fff;
            font-weight: 700;
        }
        .timetable-row {
            border-bottom: 1px solid #e5e7eb;
        }
        /* 用戶指定的紅框班次 */
        .highlight-train {
            background-color: #FEF2F2; /* 淺紅背景 */
            border: 2px solid #D83D4F; /* 紅框 */
            position: relative;
            z-index: 10;
        }
        .highlight-train td {
            color: #D83D4F;
            font-weight: 800;
        }

        /* ------------------- 混合風格機票樣式 (手繪 x 科技 x 日式) ------------------- */
        .ticket-container { font-family: 'Patrick Hand', 'Noto Sans TC', cursive; max-width: 1100px; margin: 0 auto; }

        .boarding-pass { background-color: #FAF9F6; border-radius: 12px; position: relative; box-shadow: 10px 10px 0px rgba(0,0,0,0.15); overflow: hidden; border: 2px dashed #333; margin-bottom: 2rem; width: 100%; }

        /* 科技感裝飾線條 */
        .tech-line {
            height: 4px;
            background: repeating-linear-gradient(
                45deg,
                #D4AF37,
                #D4AF37 10px,
                #1D2A35 10px,
                #1D2A35 20px
            );
        }

        .pass-header {
            background-color: #1D2A35; /* 深藍/星宇風 */
            color: #D4AF37; /* 土金/玫瑰金 */
            padding: 1rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .pass-body { padding: 1.5rem; display: grid; grid-template-columns: 3fr 2fr; gap: 2rem; }

        /* 機場代碼大字 */
        .airport-code {
            font-size: 3.5rem;
            font-weight: 900;
            line-height: 1;
            color: #1D2A35;
            font-family: 'Inter', sans-serif; /* 科技感字體 */
            letter-spacing: -2px;
        }

        .flight-info-box {
            border: 2px solid #1D2A35;
            border-radius: 8px;
            padding: 0.5rem;
            margin-bottom: 0.5rem;
        }
        
        .label {
            font-size: 0.75rem;
            color: #666;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .value {
            font-size: 1.25rem;
            font-weight: bold;
            color: #D83D4F;
        }

        /* 飛機圖示 */
        .plane-icon-path {
            stroke-dasharray: 10;
            animation: dash 30s linear infinite;
        }
        @keyframes dash {
            to { stroke-dashoffset: -1000; }
        }

        @media (max-width: 640px) {
            .pass-body { padding: 1.5rem; display: grid; grid-template-columns: 3fr 2fr; gap: 2rem; }
        }
        
        /* 購物清單項目樣式 */
        .list-item-purchased {
            text-decoration: line-through;
            color: #718096 !important; /* 灰色文字 */
            opacity: 0.6;
            font-style: italic;
        }
    
/* ============================== */
/* 📱 iPhone 17 Pro Max RWD 調整   */
/* ============================== */
@media (max-width: 480px) {
    .pass-body { padding: 1rem !important; grid-template-columns: 1fr !important; gap: 1rem !important; }
    .airport-code { font-size: 2.6rem !important; letter-spacing:-1px; }
    .pass-body .grid { grid-template-columns: 1fr !important; gap:0.75rem !important; }
    .ticket-container { max-width:100% !important; padding:0 8px; }
    .boarding-pass { margin-bottom:1.5rem !important; border-width:1.5px !important; }
    .pass-header { padding:0.75rem 1rem !important; }
    .pass-header div { font-size:0.9rem; }
}

</style>
</head>
<body class="p-4 md:p-8">
    <div id="app" class="max-w-7xl mx-auto">
        <!-- 頂部導航與標題 -->
        <header class="mb-8">
            <h1 class="text-4xl md:text-5xl font-extrabold text-white tracking-tight mb-2">
                TOKYO VIBE <span class="text-3xl font-medium text-teal-400">| 東京旅程儀表板</span>
            </h1>
            <p id="h-addr" class="text-sm font-mono text-gray-400 cursor-pointer" onclick="copyAddress()">請點擊下方按鈕設定地址</p>
        </header>

        <!-- 主要佈局：兩欄 -->
        <main class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            
            <!-- 左側：控制面板 / 快速資訊 (佔 1/3) -->
            <section class="lg:col-span-1 space-y-6">
                
                <!-- 飯店資訊與地址 -->
                <div class="flat-panel p-6 rounded-xl shadow-lg">
                    <h2 class="text-xl font-bold mb-3 flex items-center">
                        <i data-lucide="building-2" class="w-5 h-5 mr-2"></i>
                        飯店資訊
                    </h2>
                    <p id="h-name" class="text-xl font-extrabold mb-1">... 載入中 ...</p>
                    <p id="h-dates" class="text-sm text-gray-500 mb-3">...</p>
                    <button class="btn-primary w-full py-2 rounded-lg text-sm font-semibold shadow-md mt-2" onclick="showHotelModal()">
                        <i data-lucide="settings" class="w-4 h-4 mr-2 inline-block"></i>
                        設定飯店/地址
                    </button>
                    <button class="btn-primary w-full py-2 rounded-lg text-sm font-semibold shadow-md mt-2 hidden" id="copy-addr-btn" onclick="copyAddress()">
                        <i data-lucide="copy" class="w-4 h-4 mr-2 inline-block"></i>
                        複製地址
                    </button>
                </div>

                <!-- 匯率轉換器 -->
                <div class="flat-panel p-6 rounded-xl shadow-lg">
                    <h2 class="text-xl font-bold mb-3 flex items-center">
                        <i data-lucide="trending-up" class="w-5 h-5 mr-2"></i>
                        匯率轉換器 (TWD 兌 JPY)
                    </h2>
                    <div class="space-y-3">
                        <div>
                            <label for="twdInput" class="block text-xs font-medium text-gray-500">台幣 (TWD)</label>
                            <input type="number" id="twdInput" oninput="convertCurrency(this.value, 'twd')" class="w-full p-2 border border-gray-300 rounded-lg focus:ring-teal-500 focus:border-teal-500 transition duration-150">
                        </div>
                        <div class="text-center font-bold text-gray-600">
                            <i data-lucide="arrow-down-up" class="w-5 h-5 inline-block"></i>
                        </div>
                        <div>
                            <label for="jpyInput" class="block text-xs font-medium text-gray-500">日圓 (JPY)</label>
                            <input type="number" id="jpyInput" oninput="convertCurrency(this.value, 'jpy')" class="w-full p-2 border border-gray-300 rounded-lg focus:ring-teal-500 focus:border-teal-500 transition duration-150">
                        </div>
                    </div>
                    <p id="rateInfo" class="text-sm text-gray-500 mt-3 text-center">當前匯率: 1 TWD = 4.60 JPY</p>
                    <button class="btn-primary w-full py-2 rounded-lg text-sm font-semibold shadow-md mt-3" onclick="showRateModal()">
                        <i data-lucide="calculator" class="w-4 h-4 mr-2 inline-block"></i>
                        設定匯率
                    </button>
                </div>
                
                <!-- 緊急聯絡卡 -->
                <div class="flat-panel p-6 rounded-xl shadow-lg">
                    <h2 class="text-xl font-bold mb-3 flex items-center red-accent-text">
                        <i data-lucide="alert-triangle" class="w-5 h-5 mr-2 text-red-500"></i>
                        緊急聯絡卡
                    </h2>
                    <div class="space-y-2 text-sm">
                        <p class="font-bold text-red-500">日本緊急電話</p>
                        <p class="text-gray-600"><i data-lucide="ambulance" class="w-4 h-4 mr-1 inline-block"></i> 救護車/火警: 119</p>
                        <p class="text-gray-600"><i data-lucide="phone-call" class="w-4 h-4 mr-1 inline-block"></i> 警察: 110</p>
                        <p class="font-bold text-red-500 mt-3">台灣駐日代表處</p>
                        <p class="text-gray-600"><i data-lucide="building" class="w-4 h-4 mr-1 inline-block"></i> 03-3280-7811</p>
                        <p class="text-xs text-gray-500 mt-2">請妥善保存旅遊保險資料。</p>
                    </div>
                </div>

                <!-- 購物清單摘要 (快速入口) -->
                <div class="flat-panel p-6 rounded-xl shadow-lg">
                    <h2 class="text-xl font-bold mb-3 flex items-center">
                        <i data-lucide="shopping-bag" class="w-5 h-5 mr-2"></i>
                        待辦清單 (共 <span id="pending-count" class="font-extrabold text-teal-500 ml-1">0</span> 項)
                    </h2>
                    <ul id="shopping-list-summary" class="space-y-1 text-sm text-gray-700">
                        <li class="text-gray-500 text-center py-2">清單為空</li>
                    </ul>
                    <button class="btn-primary w-full py-2 rounded-lg text-sm font-semibold shadow-md mt-4" onclick="setView('SHOPPING')">
                        <i data-lucide="list-checks" class="w-4 h-4 mr-2 inline-block"></i>
                        管理完整清單
                    </button>
                </div>

            </section>

            <!-- 右側：主要內容區 (佔 2/3) -->
            <section class="lg:col-span-2 space-y-6">
                
                <!-- 導航列 -->
                <nav class="flex space-x-4 border-b border-gray-700/50 text-gray-400 overflow-x-auto pb-1 no-scrollbar">
                    <button id="btnFlight" class="nav-button pb-3 px-2 text-base font-semibold active" onclick="setView('FLIGHT')">
                        <i data-lucide="plane" class="w-5 h-5 mr-1 inline-block"></i> 班機時間
                    </button>
                    <button id="btnSkyliner" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('SKYLINER')">
                        <i data-lucide="train-front" class="w-5 h-5 mr-1 inline-block"></i> SKYLINER
                    </button>
                    <button id="btnItinerary" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('ITINERARY')">
                        <i data-lucide="calendar-check" class="w-5 h-5 mr-1 inline-block"></i> 行程總覽
                    </button>
                    <button id="btnJapanese" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('JAPANESE')">
                        <i data-lucide="message-square-text" class="w-5 h-5 mr-1 inline-block"></i> 常用日語
                    </button>
                    <button id="btnShopping" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('SHOPPING')">
                        <i data-lucide="shopping-cart" class="w-5 h-5 mr-1 inline-block"></i> 購物清單
                    </button>
                    <button id="btnNotes" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('NOTES')">
                        <i data-lucide="sticky-note" class="w-5 h-5 mr-1 inline-block"></i> 旅遊筆記
                    </button>
                </nav>

                <!-- 內容容器 -->
                <div id="main-content" class="min-h-[60vh] bg-gray-800/20 p-6 rounded-xl shadow-xl transition-all duration-300">
                    <!-- 內容將由 JS 渲染 -->
                    <p class="text-gray-500 text-center py-10">載入中...</p>
                </div>

            </section>
        </main>
    </div>

    <!-- Modal 容器 -->
    <div id="modal-container" class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center p-4 z-50 hidden transition-opacity duration-300 opacity-0">
        <!-- Modal 內容將由 JS 填充 -->
    </div>
    
    <!-- 複製反饋訊息 -->
    <div id="copy-feedback" class="fixed bottom-0 right-0 m-4 p-3 bg-teal-500 text-white rounded-lg shadow-xl hidden transition-opacity duration-300 opacity-0">
        ✅ 已複製！
    </div>


    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, deleteDoc, addDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase Config (Consolidated)
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-tokyo-vibe-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

        let app, db, auth, userId = 'anonymous', isAuthReady = false;
        let hotel = { name: '請設定飯店名稱', address: '請點擊下方按鈕設定地址', dates: 'YYYY/MM/DD - YYYY/MM/DD', rate: 4.60 };
        let shoppingList = [], notes = '';
        let appView = 'FLIGHT'; // Default view
        let itineraryDay = 1; 
        let noteList = [
             { id: 1, text: '記得帶轉接頭' },
             { id: 2, text: '下載 Suica App' }
        ];

        // Itinerary Data
        let itinerary = [
            { day: 1, date: '12/26 (五)', activities: [
                { time: '14:20', description: '抵達成田機場 (NRT)' },
                { time: '16:00', description: '飯店Check-in', location: '飯店地址' },
                { time: '18:00', description: '晚餐：阿美橫丁周邊美食', location: '阿美橫丁' },
                { time: '20:00', description: '購物：無印良品 上野丸井店', location: '無印良品 上野丸井店' },
                { time: '21:30', description: '購物：OS Drug 藥妝店', location: 'OS Drug 上野店' },
                { time: '23:00', description: '返回飯店休息' }
            ]},
            { day: 2, date: '12/27 (六)', activities: [
                { time: '09:00', description: '築地場外市場', location: '築地場外市場' },
                { time: '11:30', description: '銀座購物(GU) / UNIQLO旗艦店', location: 'GU 銀座' },
                { time: '15:00', description: '甜點：MARLOWE 焦糖布丁', location: 'MARLOWE 銀座' },
                { time: '18:00', description: '晚餐：新宿燒肉放題', location: '新宿燒肉店' },
                { time: '20:30', description: '夜景：惠比壽花園廣場燈光秀 (冬季限定)', location: '惠比壽花園廣場' }
            ]},
            { day: 3, date: '12/28 (日)', activities: [
                { time: '08:00', description: '丸之內南口集合 (富士山一日遊)', location: '東京車站丸之內南口' },
                { time: '10:30', description: '新倉山淺間公園', location: '新倉山淺間公園' },
                { time: '11:45', description: '日川時計店', location: '日川時計店' },
                { time: '12:30', description: '忍野八海 (含午餐)', location: '忍野八海' },
                { time: '15:20', description: '大石公園', location: '大石公園' },
                { time: '18:50', description: '返回東京市區' }
            ]},
            { day: 4, date: '12/29 (一)', activities: [
                { time: '09:30', description: '東京都廳 北展望室 (免費觀景)', location: '東京都廳 北展望室' },
                { time: '11:30', description: '午餐：Sukiyaki Juni Ten', location: 'Sukiyaki Juni Ten' },
                { time: '14:30', description: '東急Plaza表參道原宿', location: '東急Plaza表參道原宿' },
                { time: '18:00', description: '晚餐：當地特色料理' }
            ]},
            { day: 5, date: '12/30 (二)', activities: [
                { time: '10:00', description: '上野公園/上野動物園', location: '上野動物園' },
                { time: '14:00', description: '秋葉原動漫', location: '秋葉原' },
                { time: '17:00', description: '新宿：NEWoMan TAKANAWA 購物', location: 'NEWoMan TAKANAWA' },
                { time: '19:30', description: '晚餐：特色居酒屋', location: '新宿居酒屋' }
            ]},
            { day: 6, date: '12/31 (三)', activities: [
                { time: '09:00', description: '飯店Check-out, 寄放行李' },
                { time: '13:00', description: '前往成田機場 (NRT)' },
                { time: '15:40', description: '登機 (JX801)' }
            ]}
        ];

        // --- Firebase Init ---
        const init = async () => {
             if (Object.keys(firebaseConfig).length > 0) {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                const token = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                if (token) await signInWithCustomToken(auth, token); else await signInAnonymously(auth);
                onAuthStateChanged(auth, u => {
                    userId = u ? u.uid : 'anonymous';
                    isAuthReady = true;
                    loadData();
                });
            }
        };

        // --- Load Data ---
        const loadData = () => {
            if (!isAuthReady) return;
            onSnapshot(doc(db, `artifacts/${appId}/users/${userId}/config/hotel`), s => {
                if (s.exists()) { hotel = s.data(); updateHotelUI(); convertCurrency(document.getElementById('twdInput')?.value||0, 'twd', false); }
            });
            onSnapshot(collection(db, `artifacts/${appId}/public/data/shoppingList`), s => {
                shoppingList = [];
                s.forEach(d => shoppingList.push({id: d.id, ...d.data()}));
                shoppingList.sort((a,b) => (a.purchased === b.purchased) ? 0 : a.purchased ? 1 : -1);
                if (appView === 'SHOPPING') renderShoppingList();
                updateShoppingSummary();
            });
             // Load Note List (New Structure)
             // Assuming noteList is stored in a collection for granular updates or a single doc with array
             // For simplicity, keeping array in local state synced from single doc for now as per user requirement
             onSnapshot(doc(db, `artifacts/${appId}/users/${userId}/config/noteListDoc`), s => {
                if (s.exists()) { noteList = s.data().list || []; if(appView === 'NOTES') renderNotes(); }
            });
        };

        // --- UI Helper Functions ---
        window.updateHotelUI = () => {
             document.getElementById('h-name').textContent = hotel.name;
             document.getElementById('h-dates').textContent = hotel.dates;
             document.getElementById('h-addr').textContent = hotel.address;
             if(hotel.address !== '請點擊下方按鈕設定地址') document.getElementById('copy-addr-btn').classList.remove('hidden');
             // Update Itinerary Day 1 Check-in
             const checkIn = itinerary[0].activities.find(a => a.time === '16:00');
             if(checkIn) checkIn.description = `${hotel.name} Check-in`;
             if (appView === 'ITINERARY') renderItinerary();
        };

        window.convertCurrency = (val, type, update=true) => {
            const t = document.getElementById('twdInput');
            const j = document.getElementById('jpyInput');
            const r = document.getElementById('rateInfo');
            if(type==='twd') { if(update && j) j.value = (val * hotel.rate).toFixed(0); }
            if(type==='jpy') { if(update && t) t.value = (val / hotel.rate).toFixed(0); }
            if(r) r.textContent = `當前匯率: 1 TWD = ${hotel.rate.toFixed(2)} JPY`;
        };

        window.updateShoppingSummary = () => {
            const el = document.getElementById('shopping-list-summary');
            const cnt = document.getElementById('pending-count');
            const pending = shoppingList.filter(i => !i.purchased);
            if(cnt) cnt.textContent = pending.length;
            if(el) el.innerHTML = pending.length ? pending.slice(0,3).map(i=>`<li>• ${i.name}</li>`).join('') : '<li>清單為空</li>';
        };

        // --- View Renderers ---
        window.renderItinerary = () => {
            const main = document.getElementById('main-content');
            if(!main) return;
            const currentDay = itinerary[itineraryDay - 1];
            const dayNav = itinerary.map((d, i) => `
                <button onclick="window.setItineraryDay(${i+1})" class="px-4 py-2 rounded-lg text-sm font-bold mr-2 mb-2 flex-shrink-0 ${itineraryDay===i+1 ? 'bg-teal-500 text-white shadow-md' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'}">Day ${d.day}</button>
            `).join('');
            
            const activities = currentDay.activities.map(a => `
                <div class="flex py-4 border-b border-gray-200 last:border-0 items-start">
                    <div class="w-16 text-teal-600 font-mono text-sm font-bold pt-1">${a.time}</div>
                    <div class="flex-1 text-gray-900 font-medium text-lg">
                        ${a.description}
                        ${a.location ? `<a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(a.location)}" target="_blank" class="ml-2 text-teal-500 hover:text-teal-700 inline-block"><i data-lucide="map-pin" class="w-4 h-4 inline"></i></a>` : ''}
                    </div>
                </div>
            `).join('');

            main.innerHTML = `
                <div class="mb-6 flex overflow-x-auto no-scrollbar pb-2">${dayNav}</div>
                <div class="flat-panel p-6 rounded-xl bg-white">
                    <h2 class="text-3xl font-extrabold text-gray-900 mb-2">${currentDay.title}</h2>
                    <div class="text-sm font-bold text-teal-600 mb-6">${currentDay.date}</div>
                    <div class="space-y-2">${activities}</div>
                </div>
            `;
            lucide.createIcons();
        };

        window.renderFlightInfo = () => {
             const main = document.getElementById('main-content');
             if(!main) return;
             main.innerHTML = `
                <div class="ticket-container w-full max-w-2xl mx-auto">
                    <div class="space-y-8">
                        <!-- 去程 -->
                        <div class="boarding-pass">
                            <div class="tech-line"></div>
                            <div class="pass-header">
                                <div class="font-bold tracking-widest text-lg">STARLUX AIRLINES</div>
                                <div class="text-sm font-mono">JX800</div>
                            </div>
                            <div class="pass-body relative">
                                <div class="flex justify-between items-center mb-6">
                                    <div class="text-center"><div class="airport-code">TPE</div><div class="text-xs font-bold text-gray-500 tracking-widest">TAIPEI</div></div>
                                    <div class="text-center text-gray-400"><i data-lucide="plane" class="w-6 h-6 inline-block transform rotate-90"></i><div class="text-[10px] tracking-widest mt-1">------------</div></div>
                                    <div class="text-center"><div class="airport-code">NRT</div><div class="text-xs font-bold text-gray-500 tracking-widest">TOKYO</div></div>
                                </div>
                                <div class="grid grid-cols-2 gap-4">
                                    <div class="flight-info-box"><div class="label">DATE</div><div class="value">26 DEC</div></div>
                                    <div class="flight-info-box"><div class="label">BOARDING</div><div class="value">09:40</div></div>
                                    <div class="flight-info-box"><div class="label">DEPARTURE</div><div class="value">10:10</div></div>
                                    <div class="flight-info-box"><div class="label">ARRIVAL</div><div class="value">14:20</div></div>
                                </div>
                            </div>
                        </div>
                        <!-- 回程 -->
                         <div class="boarding-pass">
                            <div class="tech-line"></div>
                            <div class="pass-header">
                                <div class="font-bold tracking-widest text-lg">STARLUX AIRLINES</div>
                                <div class="text-sm font-mono">JX801</div>
                            </div>
                            <div class="pass-body relative">
                                <div class="flex justify-between items-center mb-6">
                                    <div class="text-center"><div class="airport-code">NRT</div><div class="text-xs font-bold text-gray-500 tracking-widest">TOKYO</div></div>
                                    <div class="text-center text-gray-400"><i data-lucide="plane" class="w-6 h-6 inline-block transform rotate-90"></i><div class="text-[10px] tracking-widest mt-1">------------</div></div>
                                    <div class="text-center"><div class="airport-code">TPE</div><div class="text-xs font-bold text-gray-500 tracking-widest">TAIPEI</div></div>
                                </div>
                                <div class="grid grid-cols-2 gap-4">
                                    <div class="flight-info-box"><div class="label">DATE</div><div class="value">31 DEC</div></div>
                                    <div class="flight-info-box"><div class="label">BOARDING</div><div class="value">15:10</div></div>
                                    <div class="flight-info-box"><div class="label">DEPARTURE</div><div class="value">15:40</div></div>
                                    <div class="flight-info-box"><div class="label">ARRIVAL</div><div class="value">18:45</div></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>`;
             lucide.createIcons();
        };

        window.renderSkylinerTimetable = () => {
             const main = document.getElementById('main-content');
             if(!main) return;
             const skylinerData = [
                { t: "09:20", n: "Skyliner 10" }, { t: "09:40", n: "Skyliner 12" }, { t: "10:20", n: "Skyliner 16", h: true }, { t: "10:40", n: "Skyliner 18" },
                { t: "11:20", n: "Skyliner 22" }, { t: "11:40", n: "Skyliner 24", h: true }, { t: "12:20", n: "Skyliner 28" }, { t: "12:40", n: "Skyliner 30", h: true },
                { t: "13:20", n: "Skyliner 34" }, { t: "13:40", n: "Skyliner 36" }, { t: "14:20", n: "Skyliner 40", h: true }, { t: "14:40", n: "Skyliner 42" }
             ];
             main.innerHTML = `
                <div class="flat-panel p-6 rounded-xl">
                    <h2 class="text-2xl font-bold text-gray-900 mb-4 flex items-center"><i data-lucide="train-front" class="w-6 h-6 mr-2 text-teal-600"></i> Skyliner 時刻表 (上野 -> 成田)</h2>
                    <div class="overflow-x-auto rounded-lg border border-gray-200"><table class="min-w-full text-sm text-left">
                        <thead class="timetable-header"><tr><th class="p-3">班次</th><th class="p-3">時間</th><th class="p-3">起點</th><th class="p-3">終點</th></tr></thead>
                        <tbody class="divide-y divide-gray-200 text-gray-800">
                            ${skylinerData.map(d => `<tr class="timetable-row ${d.h?'highlight-train':''}"><td class="p-3 font-medium">${d.n} ${d.h?'<span class="text-xs bg-red-100 text-red-600 px-2 py-0.5 rounded ml-2">推薦</span>':''}</td><td class="p-3 font-bold">${d.t}</td><td class="p-3">上野</td><td class="p-3">成田機場</td></tr>`).join('')}
                        </tbody>
                    </table></div>
                </div>
             `;
             lucide.createIcons();
        };
        
        window.renderShoppingList = () => {
             const main = document.getElementById('main-content');
             if(!main) return;
             main.innerHTML = `
                <div class="flat-panel p-6 rounded-xl">
                    <h2 class="text-2xl font-bold text-teal-600 mb-6 flex items-center"><i data-lucide="shopping-cart" class="w-6 h-6 mr-2"></i> 購物清單</h2>
                    <div class="flex gap-2 mb-6">
                        <input type="text" id="newShopItem" placeholder="新增購物項目..." class="flex-grow p-3 border border-gray-300 rounded-lg text-gray-900" onkeypress="if(event.key==='Enter') window.addShopItem()">
                        <button onclick="window.addShopItem()" class="btn-primary px-6 rounded-lg font-bold">新增</button>
                    </div>
                    <div id="shopListUI" class="space-y-2"></div>
                </div>
             `;
             window.renderShopListItems();
             lucide.createIcons();
        };

        window.renderShopListItems = () => {
             const container = document.getElementById('shopListUI');
             if(!container) return;
             if(shoppingList.length === 0) { container.innerHTML = '<p class="text-center text-gray-400 py-4">清單是空的</p>'; return; }
             container.innerHTML = shoppingList.map((item, i) => `
                <div class="flex items-center justify-between p-3 bg-white border border-gray-100 rounded-lg shadow-sm hover:shadow transition">
                    <div class="flex items-center gap-3 cursor-pointer" onclick="window.toggleShop('${item.id}', ${item.purchased})">
                        <i data-lucide="${item.purchased ? 'check-circle' : 'circle'}" class="w-5 h-5 ${item.purchased ? 'text-teal-500' : 'text-gray-400'}"></i>
                        <span class="text-lg ${item.purchased ? 'text-gray-400 line-through' : 'text-gray-800 font-medium'}">${item.name}</span>
                    </div>
                    <button onclick="window.delShop('${item.id}')" class="text-gray-400 hover:text-red-500 p-2"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                </div>
             `).join('');
             lucide.createIcons();
        };

        window.renderNotes = () => {
             const main = document.getElementById('main-content');
             if(!main) return;
             main.innerHTML = `
                <div class="flat-panel p-6 rounded-xl">
                    <h2 class="text-2xl font-bold text-teal-600 mb-6 flex items-center"><i data-lucide="sticky-note" class="w-6 h-6 mr-2"></i> 旅遊筆記</h2>
                    <div class="flex gap-2 mb-6">
                        <input type="text" id="newNoteItem" placeholder="新增筆記..." class="flex-grow p-3 border border-gray-300 rounded-lg text-gray-900" onkeypress="if(event.key==='Enter') window.addNoteItem()">
                        <button onclick="window.addNoteItem()" class="btn-primary px-6 rounded-lg font-bold">新增</button>
                    </div>
                    <div id="noteListUI" class="space-y-2"></div>
                </div>
             `;
             window.renderNoteListItems();
             lucide.createIcons();
        };

        window.renderNoteListItems = () => {
             const container = document.getElementById('noteListUI');
             if(!container) return;
             container.innerHTML = noteList.map((note, i) => `
                <div class="flex items-center justify-between p-3 bg-yellow-50 border border-yellow-100 rounded-lg shadow-sm">
                    <span class="text-lg text-gray-800 font-medium pl-2 border-l-4 border-teal-400">${note.text}</span>
                    <button onclick="window.delNote('${note.id}')" class="text-gray-400 hover:text-red-500 p-2"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                </div>
             `).join('');
             lucide.createIcons();
        }

        // --- Action Handlers ---
        window.addShopItem = async () => {
            const input = document.getElementById('newShopItem');
            if(input && input.value.trim()) {
                if(isAuthReady) await addDoc(collection(db, `artifacts/${appId}/public/data/shoppingList`), { name: input.value.trim(), purchased: false });
                else alert('請等待資料庫連線');
                input.value = '';
            }
        };
        window.toggleShop = async (id, status) => {
             if(isAuthReady) await setDoc(doc(db, `artifacts/${appId}/public/data/shoppingList`, id), { purchased: !status }, { merge: true });
        };
        window.delShop = async (id) => {
             if(confirm('刪除?')) await deleteDoc(doc(db, `artifacts/${appId}/public/data/shoppingList`, id));
        };
        
        window.addNoteItem = async () => {
             const input = document.getElementById('newNoteItem');
             if(input && input.value.trim()) {
                 // For simplicity, updating local array and syncing whole array to doc
                 const newId = Date.now().toString();
                 noteList.push({id: newId, text: input.value.trim()});
                 if(isAuthReady) await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/noteListDoc`), { list: noteList });
                 else renderNoteListItems();
                 input.value = '';
             }
        };
        window.delNote = async (id) => {
             noteList = noteList.filter(n => n.id !== id);
             if(isAuthReady) await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/noteListDoc`), { list: noteList });
             renderNoteListItems();
        };


        // --- Main View Switcher ---
        window.setView = (view) => {
            appView = view;
            document.querySelectorAll('.nav-button').forEach(b => b.classList.remove('active'));
            const btn = document.getElementById({FLIGHT:'btnFlight', SKYLINER:'btnSkyliner', ITINERARY:'btnItinerary', JAPANESE:'btnJapanese', SHOPPING:'btnShopping', NOTES:'btnNotes'}[view]);
            if(btn) btn.classList.add('active');

            const main = document.getElementById('main-content');
            main.innerHTML = '';
            
            switch(view) {
                case 'FLIGHT': renderFlightInfo(); break;
                case 'SKYLINER': renderSkylinerTimetable(); break;
                case 'ITINERARY': renderItinerary(); break;
                case 'JAPANESE': 
                    main.innerHTML = `<h2 class="text-2xl font-bold text-teal-600 mb-4">常用日語</h2><div class="grid grid-cols-1 md:grid-cols-2 gap-4">${[{j:'すみません',c:'不好意思'},{j:'ありがとう',c:'謝謝'},{j:'いくらですか',c:'多少錢'},{j:'これください',c:'我要這個'},{j:'トイレはどこですか',c:'廁所在哪'},{j:'お会計お願いします',c:'買單'}].map(p=>`<div onclick="navigator.clipboard.writeText('${p.j}').then(()=>alert('已複製'))" class="flat-panel p-4 rounded-lg cursor-pointer hover:bg-gray-100 relative group"><div class="text-xl text-teal-600 font-bold">${p.j}</div><div class="text-sm text-gray-500">${p.c}</div><i data-lucide="copy" class="w-4 h-4 absolute top-4 right-4 text-gray-400 opacity-0 group-hover:opacity-100"></i></div>`).join('')}</div>`;
                    lucide.createIcons();
                    break;
                case 'SHOPPING': renderShoppingList(); break;
                case 'NOTES': renderNotes(); break;
            }
        };
        
        // --- Helpers ---
        window.setItineraryDay = (d) => { itineraryDay = d; renderItinerary(); };
        window.copyAddress = () => { navigator.clipboard.writeText(hotel.address).then(()=>alert('地址已複製')); };
        window.showHotelModal = () => { 
            const n = prompt('飯店名稱', hotel.name); 
            const a = prompt('地址', hotel.address); 
            const d = prompt('日期', hotel.dates);
            if(n) { hotel.name=n; hotel.address=a; hotel.dates=d; saveHotel(); updateHotelUI(); }
        };
        window.saveHotel = async (fb=true) => { if(isAuthReady && fb) await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/hotel`), hotel, {merge:true}); };
        window.showRateModal = () => { const r = prompt('匯率', hotel.rate); if(r) { hotel.rate=parseFloat(r); saveHotel(); convertCurrency(1000, 'twd'); } };


        // Init
        window.onload = () => { init(); setView('FLIGHT'); };

    </script>
</body>
</html>
