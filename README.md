<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOKYO VIBE | 鮮明配色儀表板</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* 品牌配色定義 */
        :root {
            --color-dark-navy: #1D2A35;
            --color-cream: #F7FCF5;
            --color-teal: #2CBBAD;
            --color-red: #D83D4F;
        }

        /* 全局樣式：深色基底，淺色字體 */
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            background-color: var(--color-dark-navy); 
            color: var(--color-cream);
            min-height: 10vh;
        }

        /* 主要卡片面板 - 使用淺色搭配深色邊框和陰影，實現高對比 */
        .flat-panel {
            background-color: var(--color-cream); /* 淺色卡片 */
            color: var(--color-dark-navy); /* 卡片內使用深色文字 */
            border: 1px solid rgba(44, 187, 173, 0.2); /* 淡淡的邊框 */
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.08); /* 柔和陰影 */
        }
        
        /* 品牌色按鈕 - 強調色 (Teal) */
        .btn-primary {
            background-color: var(--color-teal);
            color: var(--color-cream);
            transition: background-color 0.2s, transform 0.1s;
        }
        .btn-primary:hover {
            background-color: #24A397; /* 稍微深一點的 Teal */
            transform: translateY(-1px);
        }
        .btn-primary:active {
            transform: translateY(0);
        }

        /* 警告/行動按鈕 - 強調色 (Red) */
        .btn-danger {
            background-color: var(--color-red);
            color: var(--color-cream);
            transition: background-color 0.2s, transform 0.1s;
        }
        .btn-danger:hover {
            background-color: #C03544;
            transform: translateY(-1px);
        }

        /* 導航按鈕 */
        .nav-button {
            transition: color 0.2s, background-color 0.2s;
            cursor: pointer;
        }
        .nav-button:hover {
            color: var(--color-teal);
        }
        .nav-button.active {
            color: var(--color-teal);
            border-bottom: 2px solid var(--color-teal);
        }

        /* 焦點樣式 */
        .teal-accent {
            background-color: rgba(44, 187, 173, 0.2); /* 帶有透明度的 Teal 背景 */
            color: var(--color-teal);
            font-weight: 600;
        }
        .red-accent-text {
            color: var(--color-red);
        }
        
        /* 隱藏原生捲軸 */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }
        
        /* Modal 動畫效果 */
        #modal-container.opacity-100 .flat-panel {
            transform: scale(1.0);
        }

        /* 航班卡片專屬風格 (Starlux Vibe) */
        .flight-card {
            /* 覆蓋 flat-panel，使用更深的背景色 */
            background-color: #1a232b; 
            color: var(--color-cream);
            border: 1px solid #3d4a57;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.2), 0 4px 6px -2px rgba(0, 0, 0, 0.1);
        }
        
        /* 購物清單項目樣式 */
        .list-item-purchased {
            text-decoration: line-through;
            color: #718096 !important; /* 灰色文字 */
            opacity: 0.6;
            font-style: italic;
        }

        /* 時刻表樣式 */
        .timetable-header {
            background-color: rgba(44, 187, 173, 0.1);
            color: var(--color-teal);
            font-weight: 700;
        }
        .timetable-row:nth-child(even) {
            background-color: rgba(255, 255, 255, 0.05);
        }
        .timetable-row:hover {
            background-color: rgba(44, 187, 173, 0.2);
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
                
                <!-- 導航列：依照使用者要求重新排序 -->
                <nav class="flex space-x-4 border-b border-gray-700/50 text-gray-400 overflow-x-auto pb-1 no-scrollbar">
                    
                    <!-- 1. 班機時間 -->
                    <button id="btnFlight" class="nav-button pb-3 px-2 text-base font-semibold active" onclick="setView('FLIGHT')">
                        <i data-lucide="plane" class="w-5 h-5 mr-1 inline-block"></i> 班機時間
                    </button>
                    
                    <!-- 2. SKYLINER 時刻表 (New) -->
                    <button id="btnSkyliner" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('SKYLINER')">
                        <i data-lucide="train-front" class="w-5 h-5 mr-1 inline-block"></i> SKYLINER 時刻表
                    </button>
                    
                    <!-- 3. 行程總覽 -->
                    <button id="btnItinerary" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('ITINERARY')">
                        <i data-lucide="calendar-check" class="w-5 h-5 mr-1 inline-block"></i> 行程總覽
                    </button>
                    
                    <!-- 4. 常用日語 -->
                    <button id="btnJapanese" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('JAPANESE')">
                        <i data-lucide="message-square-text" class="w-5 h-5 mr-1 inline-block"></i> 常用日語
                    </button>
                    
                    <!-- 5. 購物清單 -->
                    <button id="btnShopping" class="nav-button pb-3 px-2 text-base font-semibold" onclick="setView('SHOPPING')">
                        <i data-lucide="shopping-cart" class="w-5 h-5 mr-1 inline-block"></i> 購物清單
                    </button>
                    
                    <!-- 6. 旅遊筆記 -->
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
        import { getFirestore, doc, setDoc, onSnapshot, collection, deleteDoc, addDoc, query, getDocs, where } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // 設定 Firebase 登錄級別為 Debug
        setLogLevel('Debug');

        // 全局變數
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-tokyo-vibe-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

        let app;
        let db;
        let auth;
        let userId = 'anonymous'; // 預設為匿名
        let isAuthReady = false;
        let shoppingList = []; // 購物清單資料陣列

        // 應用程式數據模型
        let hotel = {
            name: '請設定飯店名稱',
            address: '請點擊下方按鈕設定地址',
            dates: 'YYYY/MM/DD - YYYY/MM/DD',
            rate: 4.60 // 1 TWD = 4.60 JPY
        };
        let notes = '';

        let itinerary = [
            {
                day: 1,
                date: '12/26 (五)', 
                activities: [
                    { time: '14:20', description: '抵達東京成田機場 (NRT)' },
                    { time: '16:00', description: '飯店Check-in', location: '飯店地址' }, 
                    { time: '18:00', description: '晚餐：阿美橫丁周邊美食', location: '阿美橫丁' }, 
                    { time: '20:00', description: '購物：無印良品 上野丸井店', location: '無印良品 上野丸井店' },
                    { time: '21:30', description: '購物：3COINS / OS Drug 上野店藥妝店', location: 'OS Drug 上野店藥妝店' },
                    { time: '23:00', description: '返回飯店休息' } 
                ]
            },
            {
                day: 2,
                date: '12/27 (六)',
                activities: [
                    { time: '09:00', description: '築地場外市場', location: '築地場外市場' },
                    { time: '11:30', description: '銀座購物(GU) / UNIQLO旗艦店', location: 'GU 銀座' }, 
                    { time: '15:00', description: '甜點：MARLOWE 焦糖布丁', location: 'MARLOWE 銀座' }, 
                    { time: '18:00', description: '晚餐：新宿燒肉放題', location: '新宿燒肉店' },
                    { time: '20:30', description: '夜景：惠比壽花園廣場燈光秀 (冬季限定)', location: '惠比壽花園廣場' }
                ]
            },
            {
                day: 3,
                date: '12/28 (日)',
                activities: [ 
                    { time: '08:00', description: '丸之內南口集合 (富士山一日遊)', location: '東京車站丸之內南口' },
                    { time: '10:30', description: '新倉山淺間公園 (60分鐘)', location: '新倉山淺間公園' },
                    { time: '11:45', description: '日川時計店 (20分鐘)', location: '日川時計店' },
                    { time: '12:30', description: '忍野八海 (90分鐘, 含午餐: 鰻魚飯/和牛餐食)', location: '忍野八海' },
                    { time: '14:30', description: 'Lawson便利店休息/購物 (20分鐘)', location: 'Lawson 富士河口湖町' },
                    { time: '15:10', description: '河口湖 (車覽)', location: '河口湖' },
                    { time: '15:20', description: '大石公園 (50分鐘)', location: '大石公園' },
                    { time: '18:50', description: '返回東京市區' }
                ]
            },
            {
                day: 4,
                date: '12/29 (一)',
                activities: [ 
                    { time: '09:30', description: '東京都廳 北展望室 (免費觀景)', location: '東京都廳 北展望室' }, 
                    { time: '11:00', description: '新宿周邊逛街' },
                    { time: '11:30', description: '午餐：Sukiyaki Juni Ten (壽喜燒)', location: 'Sukiyaki Juni Ten' },
                    { time: '13:00', description: '前往原宿/表參道', location: '東急Plaza表參道原宿' },
                    { time: '14:30', description: '購物：東急Plaza表參道原宿', location: '東急Plaza表參道原宿' }, 
                    { time: '18:00', description: '晚餐：當地特色料理' },
                    { time: '21:00', description: '返回飯店休息' }
                ]
            },
            {
                day: 5,
                date: '12/30 (二)',
                activities: [
                    { time: '10:00', description: '上野公園/上野動物園', location: '上野動物園' },
                    { time: '14:00', description: '秋葉原動漫/電器街', location: '秋葉原' },
                    { time: '17:00', description: '新宿：NEWoMan TAKANAWA 購物', location: 'NEWoMan TAKANAWA' }, 
                    { time: '19:30', description: '晚餐：特色居酒屋', location: '新宿居酒屋' }
                ]
            },
            {
                day: 6,
                date: '12/31 (三)',
                activities: [
                    { time: '09:00', description: '飯店Check-out, 寄放行李' },
                    { time: '11:00', description: '附近進行最後採買 (土產)' },
                    { time: '13:00', description: '前往成田機場 (NRT)' },
                    { time: '15:40', description: '登機 (JX801 15:40起飛)' }
                ]
            }
        ];

        let appView = 'FLIGHT'; // 預設為第一個頁籤：班機時間
        let itineraryDay = 1; 

        // ===== Firebase 初始化與身份驗證 =====

        const initializeFirebase = async () => {
            try {
                if (!firebaseConfig || Object.keys(firebaseConfig).length === 0) {
                    console.warn("Firebase config is missing or empty. Running as local mode.");
                    return;
                }
                
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                
                // 嘗試使用自定義 token 登入
                const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }

                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        userId = user.uid;
                        console.log("Firebase 認證成功，User ID:", userId);
                    } else {
                        userId = 'anonymous';
                        console.log("Firebase 處於匿名/未登入狀態。");
                    }
                    isAuthReady = true;
                    // 在認證就緒後載入所有資料並啟動監聽
                    loadDataListeners(); 
                });

            } catch (error) {
                console.error("Firebase 初始化失敗:", error);
            }
        };

        // ===== 資料庫操作與即時監聽 (Snapshot Listeners) =====

        // 構建公開資料路徑: /artifacts/{appId}/public/data/{collectionName}
        const getPublicCollectionPath = (collectionName) => {
            return `artifacts/${appId}/public/data/${collectionName}`;
        }

        // 構建私人資料路徑: /artifacts/{appId}/users/{userId}/config/{docId}
        const getPrivateDocPath = (docId) => {
            return `artifacts/${appId}/users/${userId}/config/${docId}`;
        }
        
        // 載入所有資料的即時監聽器
        const loadDataListeners = () => {
            if (!isAuthReady) return;

            // 1. Hotel & Rate (私人資料 - doc)
            const hotelDocRef = doc(db, getPrivateDocPath('hotel'));
            onSnapshot(hotelDocRef, (docSnap) => {
                if (docSnap.exists()) {
                    hotel = docSnap.data();
                } else {
                    console.log("飯店/匯率配置不存在，使用預設值。");
                    saveHotel(false); 
                }
                updateHotelUI();
                convertCurrency(document.getElementById('twdInput')?.value || 0, 'twd', false);
            }, (error) => {
                console.error("監聽飯店/匯率配置失敗:", error);
            });
            
            // 2. Shopping List (公開資料 - collection)
            const shoppingColRef = collection(db, getPublicCollectionPath('shoppingList'));
            onSnapshot(shoppingColRef, (snapshot) => {
                shoppingList = [];
                snapshot.forEach(doc => {
                    const data = doc.data();
                    shoppingList.push({ id: doc.id, ...data });
                });
                
                // 在客戶端進行排序 (未購買在前，然後按名稱排序)
                shoppingList.sort((a, b) => {
                    if (a.purchased !== b.purchased) {
                        return a.purchased ? 1 : -1;
                    }
                    return a.name.localeCompare(b.name, 'zh-TW');
                });

                if (appView === 'SHOPPING') {
                    renderShoppingList();
                }
                updateShoppingSummary();
            }, (error) => {
                console.error("監聽購物清單失敗:", error);
            });

            // 3. Notes (私人資料 - doc)
            const notesDocRef = doc(db, getPrivateDocPath('notes'));
            onSnapshot(notesDocRef, (docSnap) => {
                if (docSnap.exists()) {
                    notes = docSnap.data().content || '';
                } else {
                    notes = '';
                }
                if (appView === 'NOTES') {
                    renderNotes();
                }
            }, (error) => {
                console.error("監聽旅遊筆記失敗:", error);
            });
        };

        // 儲存飯店/匯率資訊
        const saveHotel = async (showFeedback = true) => {
            if (!isAuthReady || userId === 'anonymous') {
                console.warn("認證未就緒或匿名用戶，無法儲存。");
                return;
            }
            try {
                const hotelDocRef = doc(db, getPrivateDocPath('hotel'));
                await setDoc(hotelDocRef, hotel, { merge: true });
                if (showFeedback) {
                    showModal('飯店/匯率資訊已成功儲存！', 'success');
                }
            } catch (e) {
                console.error("儲存飯店/匯率失敗:", e);
                showModal('儲存飯店/匯率失敗，請檢查連線。', 'error');
            }
        };

        // 儲存旅遊筆記
        const saveNotes = async (content) => {
            if (!isAuthReady || userId === 'anonymous') {
                console.warn("認證未就緒或匿名用戶，無法儲存筆記。");
                return;
            }
            try {
                const notesDocRef = doc(db, getPrivateDocPath('notes'));
                await setDoc(notesDocRef, { content: content }, { merge: true });
            } catch (e) {
                console.error("儲存筆記失敗:", e);
            }
        };

        // ===== 購物清單操作函式 =====

        // 新增購物清單項目
        window.addShoppingItem = async (name) => {
            if (!isAuthReady) return console.warn("認證未就緒，無法新增項目。");
            const trimmedName = name.trim();
            if (!trimmedName) return;

            try {
                const shoppingColRef = collection(db, getPublicCollectionPath('shoppingList'));
                await addDoc(shoppingColRef, {
                    name: trimmedName,
                    purchased: false,
                    createdAt: new Date().getTime(),
                    userId: userId 
                });
            } catch (e) {
                console.error("新增購物項目失敗:", e);
                showModal('新增購物項目失敗，請檢查連線。', 'error');
            }
        };

        // 切換購物清單項目狀態
        window.toggleShoppingItem = async (id, currentState) => {
            if (!isAuthReady) return console.warn("認證未就緒，無法切換狀態。");
            try {
                const itemDocRef = doc(db, getPublicCollectionPath('shoppingList'), id);
                await setDoc(itemDocRef, { purchased: !currentState }, { merge: true });
            } catch (e) {
                console.error("切換購物項目狀態失敗:", e);
                showModal('切換狀態失敗，請檢查連線。', 'error');
            }
        };

        // 刪除購物清單項目
        window.deleteShoppingItem = async (id) => {
            if (!isAuthReady) return console.warn("認證未就緒，無法刪除項目。");
            if (!window.confirm("確定要刪除此購物項目嗎？")) return;
            try {
                const itemDocRef = doc(db, getPublicCollectionPath('shoppingList'), id);
                await deleteDoc(itemDocRef);
            } catch (e) {
                console.error("刪除購物項目失敗:", e);
                showModal('刪除項目失敗，請檢查連線。', 'error');
            }
        };
        
        // 清空已完成的項目
        window.clearCompletedShopping = async () => {
            if (!isAuthReady) return console.warn("認證未就緒，無法執行操作。");
            if (!window.confirm("確定要清空所有已購買的項目嗎？")) return;
            try {
                const completedItems = shoppingList.filter(item => item.purchased);
                const batch = [];
                const shoppingColRef = collection(db, getPublicCollectionPath('shoppingList'));

                completedItems.forEach(item => {
                    batch.push(deleteDoc(doc(shoppingColRef, item.id)));
                });
                
                await Promise.all(batch);
                showModal(`已清除 ${completedItems.length} 項已完成的購物項目。`, 'success');

            } catch (e) {
                console.error("清空已完成項目失敗:", e);
                showModal('清空已完成項目失敗，請檢查連線。', 'error');
            }
        };

        // ===== UI 輔助函式 =====

        // 更新飯店資訊區塊
        const updateHotelUI = () => {
            const hName = document.getElementById('h-name');
            const hDates = document.getElementById('h-dates');
            const hAddr = document.getElementById('h-addr');
            const copyBtn = document.getElementById('copy-addr-btn');
            
            if (hName) hName.textContent = hotel.name;
            if (hDates) hDates.textContent = hotel.dates;
            if (hAddr) hAddr.textContent = hotel.address;

            if (copyBtn) {
                if (hotel.address && hotel.address !== '請點擊下方按鈕設定地址') {
                    copyBtn.classList.remove('hidden');
                } else {
                    copyBtn.classList.add('hidden');
                }
            }
            
            // 更新行程視圖中的 Day 1 Check-in 描述
            if (itinerary.length > 0) {
                const checkInActivity = itinerary[0].activities.find(a => a.time === '16:00');
                if (checkInActivity) {
                    checkInActivity.description = `${hotel.name} Check-in`;
                    if (hotel.address && hotel.address !== '請點擊下方按鈕設定地址') {
                         checkInActivity.location = hotel.address;
                    }
                }
                if (appView === 'ITINERARY') {
                    renderItinerary();
                }
            }
        };

        // 匯率轉換器邏輯
        window.convertCurrency = (value, type, updateInput = true) => {
            const twdInput = document.getElementById('twdInput');
            const jpyInput = document.getElementById('jpyInput');
            const rateInfo = document.getElementById('rateInfo');

            const num = parseFloat(value);
            if (isNaN(num)) return;

            if (type === 'twd') {
                const jpy = (num * hotel.rate).toFixed(2);
                if (updateInput && jpyInput) jpyInput.value = jpy;
            } else if (type === 'jpy') {
                const twd = (num / hotel.rate).toFixed(2);
                if (updateInput && twdInput) twdInput.value = twd;
            }

            if (rateInfo) rateInfo.textContent = `當前匯率: 1 TWD = ${hotel.rate.toFixed(2)} JPY`;
        };
        
        // 複製地址到剪貼簿
        window.copyAddress = () => {
            const text = document.getElementById('h-addr').textContent;
            if(!text || text === '請點擊下方按鈕設定地址') {
                const originalText = document.getElementById('h-addr').textContent;
                document.getElementById('h-addr').textContent = "⚠️ 請先設定地址！";
                document.getElementById('h-addr').classList.add('red-accent-text');
                setTimeout(() => {
                    document.getElementById('h-addr').textContent = originalText;
                    document.getElementById('h-addr').classList.remove('red-accent-text');
                }, 2000);
                return;
            }
            
            document.execCommand('copy', false, text);

            const originalText = document.getElementById('h-addr').textContent;
            document.getElementById('h-addr').textContent = "✅ 已複製到剪貼簿！ (1.5秒後恢復)";
            document.getElementById('h-addr').classList.add('teal-accent'); 
            setTimeout(() => {
                document.getElementById('h-addr').textContent = originalText;
                document.getElementById('h-addr').classList.remove('teal-accent');
            }, 1500);
        }

        // 更新左側購物清單摘要
        function updateShoppingSummary() {
            const summaryEl = document.getElementById('shopping-list-summary');
            const pendingCountEl = document.getElementById('pending-count');
            
            const pendingItems = shoppingList.filter(item => !item.purchased);
            
            if (pendingCountEl) pendingCountEl.textContent = pendingItems.length;

            if (!summaryEl) return;

            if (pendingItems.length === 0) {
                summaryEl.innerHTML = '<li class="text-gray-500 text-center py-2">清單為空或已全部完成 🎉</li>';
            } else {
                summaryEl.innerHTML = pendingItems.slice(0, 5).map(item => `
                    <li class="flex items-center space-x-2 truncate">
                        <i data-lucide="chevrons-right" class="w-4 h-4 text-teal-500 flex-shrink-0"></i>
                        <span class="truncate">${item.name}</span>
                    </li>
                `).join('');

                if (pendingItems.length > 5) {
                    summaryEl.innerHTML += `<li class="text-xs text-gray-500 mt-1 text-center">+ ${pendingItems.length - 5} 個更多項目...</li>`;
                }
            }
            lucide.createIcons();
        }


        // ===== 視圖渲染函式 =====

        // 渲染行程總覽視圖 (含導航連結)
        function renderItinerary() {
            const container = document.getElementById('main-content');
            if (!container) return;

            // 調整 Day 導航列，包含 Day 1 到 Day 6
            const dayNav = itinerary.map(dayData => `
                <button 
                    class="px-4 py-2 rounded-full text-sm font-semibold transition duration-150 ${dayData.day === itineraryDay ? 'bg-teal-500 text-white shadow-lg' : 'bg-gray-700/50 text-gray-300 hover:bg-gray-700'} flex-shrink-0"
                    onclick="setItineraryDay(${dayData.day})"
                >
                    Day ${dayData.day}
                </button>
            `).join('');

            const currentDayData = itinerary.find(d => d.day === itineraryDay);
            let dayContent = '';
            
            if (currentDayData) {
                const activities = currentDayData.activities.map(act => {
                    const mapLink = act.location ? `
                        <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(act.location)}" target="_blank" 
                           class="text-teal-400 hover:text-teal-300 ml-3 transition duration-150 flex items-center" title="點擊導航到 ${act.location}">
                            <i data-lucide="map-pin" class="w-4 h-4 inline-block"></i>
                        </a>
                    ` : '';

                    return `
                        <div class="flex border-b border-gray-600/30 py-3 last:border-b-0">
                            <p class="font-mono text-sm text-teal-400 w-1/4 min-w-[70px]">${act.time}</p>
                            <div class="text-gray-200 font-medium w-3/4 flex items-center justify-between">
                                <span>${act.description}</span>
                                ${mapLink}
                            </div>
                        </div>
                    `;
                }).join('');

                dayContent = `
                    <div class="p-4 bg-gray-700/20 rounded-xl mb-4 shadow-inner">
                        <h3 class="text-2xl font-extrabold text-teal-300 mb-1">Day ${currentDayData.day}</h3>
                        <p class="text-sm font-medium text-gray-400">${currentDayData.date}</p>
                    </div>
                    <div class="space-y-1">
                        ${activities}
                    </div>
                `;
            } else {
                 dayContent = '<p class="text-gray-500 text-center py-10">找不到該天的行程資料。</p>';
            }

            container.innerHTML = `
                <div class="flex space-x-3 mb-6 no-scrollbar overflow-x-auto pb-2">
                    ${dayNav}
                </div>
                <div>
                    ${dayContent}
                </div>
            `;
            lucide.createIcons();
        }
        
        // 切換行程 Day
        window.setItineraryDay = (day) => {
            itineraryDay = day;
            renderItinerary();
        };

        // 渲染 SKYLINER 時刻表 (新增功能)
        function renderSkylinerTimetable() {
            const container = document.getElementById('main-content');
            if (!container) return;

            const skylinerData = {
                // 這是模擬的時刻表數據，真實時間請以官方公告為準
                nrtToUeno: [
                    { time: '14:20', train: 'Skyliner 30', type: '特急' },
                    { time: '15:00', train: 'Skyliner 32', type: '特急' },
                    { time: '15:40', train: 'Skyliner 34', type: '特急' },
                    { time: '16:20', train: 'Skyliner 36', type: '特急' },
                    { time: '17:00', train: 'Skyliner 38', type: '特急' },
                    { time: '17:40', train: 'Skyliner 40', type: '特急' },
                    { time: '18:20', train: 'Skyliner 42', type: '特急' },
                ],
                uenoToNRT: [
                    { time: '07:20', train: 'Skyliner 5', type: '特急' },
                    { time: '08:00', train: 'Skyliner 7', type: '特急' },
                    { time: '08:40', train: 'Skyliner 9', type: '特急' },
                    { time: '09:20', train: 'Skyliner 11', type: '特急' },
                    { time: '10:00', train: 'Skyliner 13', type: '特急' },
                    { time: '10:40', train: 'Skyliner 15', type: '特急' },
                    { time: '11:20', train: 'Skyliner 17', type: '特急' },
                ]
            };

            const renderTable = (data, title, direction) => `
                <div class="bg-gray-700/30 p-4 rounded-xl shadow-lg">
                    <h4 class="text-xl font-bold text-teal-300 mb-4 flex items-center">
                        <i data-lucide="${direction === 'outbound' ? 'arrow-right-circle' : 'arrow-left-circle'}" class="w-5 h-5 mr-2"></i>
                        ${title}
                    </h4>
                    <div class="overflow-x-auto">
                        <table class="min-w-full text-left text-sm whitespace-nowrap">
                            <thead>
                                <tr class="timetable-header">
                                    <th class="p-3 rounded-tl-lg">車次名稱</th>
                                    <th class="p-3">出發時間</th>
                                    <th class="p-3">列車類型</th>
                                    <th class="p-3 rounded-tr-lg">抵達時間 (約)</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-700/50">
                                ${data.map(item => `
                                    <tr class="timetable-row text-gray-200">
                                        <td class="p-3 font-semibold">${item.train}</td>
                                        <td class="p-3 text-teal-400">${item.time}</td>
                                        <td class="p-3">${item.type}</td>
                                        <td class="p-3">${item.time.replace(/(\d+):(\d+)/, (match, h, m) => {
                                            const depTime = new Date(0, 0, 0, parseInt(h), parseInt(m));
                                            const travelTime = 45; // 假設上野/日暮里到成田機場約 45 分鐘
                                            depTime.setMinutes(depTime.getMinutes() + travelTime);
                                            return `${depTime.getHours().toString().padStart(2, '0')}:${depTime.getMinutes().toString().padStart(2, '0')}`;
                                        })}</td>
                                    </tr>
                                `).join('')}
                            </tbody>
                        </table>
                    </div>
                    <p class="text-xs text-gray-500 mt-4">注意：此為模擬時刻表，實際時間請依京成電鐵公告為準，乘車時間約 45-55 分鐘。</p>
                </div>
            `;


            container.innerHTML = `
                <h3 class="text-2xl font-bold text-teal-300 mb-6 flex items-center">
                    <i data-lucide="train-front" class="w-6 h-6 mr-2 text-teal-500"></i>
                    京成 Skyliner (京成電鐵) 時刻表
                </h3>
                <div class="space-y-6">
                    ${renderTable(skylinerData.nrtToUeno, '去程：成田機場 (NRT) → 京成上野/日暮里', 'outbound')}
                    ${renderTable(skylinerData.uenoToNRT, '回程：京成上野/日暮里 → 成田機場 (NRT)', 'inbound')}
                </div>
            `;
            lucide.createIcons();
        }

        // 渲染購物清單視圖
        function renderShoppingList() {
            const container = document.getElementById('main-content');
            if (!container) return;

            // 待購買和已購買清單
            const pendingItems = shoppingList.filter(item => !item.purchased);
            const purchasedItems = shoppingList.filter(item => item.purchased);
            
            // 渲染清單項目的輔助函數
            const renderItem = (item) => `
                <div class="flex items-center justify-between p-3 border-b border-gray-700/50 last:border-b-0 group transition duration-150 hover:bg-gray-700/20 rounded-lg">
                    <!-- 左側：核取方塊與名稱 -->
                    <label class="flex items-center flex-grow cursor-pointer ${item.purchased ? 'list-item-purchased' : 'text-gray-200'}" 
                           onclick="toggleShoppingItem('${item.id}', ${item.purchased})">
                        <input type="checkbox" ${item.purchased ? 'checked' : ''} class="form-checkbox h-5 w-5 text-teal-500 border-gray-500 rounded bg-gray-800 transition duration-150">
                        <span class="ml-4 text-base font-medium truncate">${item.name}</span>
                    </label>

                    <!-- 右側：刪除按鈕 -->
                    <button class="text-gray-500 hover:text-red-500 ml-4 p-1 rounded-full transition duration-150 opacity-100 md:opacity-0 group-hover:opacity-100" 
                            onclick="event.stopPropagation(); deleteShoppingItem('${item.id}')">
                        <i data-lucide="trash-2" class="w-5 h-5"></i>
                    </button>
                </div>
            `;
            
            container.innerHTML = `
                <h3 class="text-2xl font-bold text-teal-300 mb-6">東京購物清單 (待辦清單)</h3>
                
                <!-- 新增項目輸入區 -->
                <div class="flex mb-6 space-x-3">
                    <input type="text" id="new-shopping-item" placeholder="新增要購買的物品名稱..." 
                           class="flex-grow p-3 rounded-lg border-2 border-gray-600/50 bg-gray-800 text-white focus:border-teal-500 transition duration-150"
                           onkeypress="if(event.key === 'Enter') addShoppingItemFromInput()">
                    <button class="btn-primary px-5 py-3 rounded-lg flex items-center font-semibold" onclick="addShoppingItemFromInput()">
                        <i data-lucide="plus" class="w-5 h-5 mr-1"></i> 新增
                    </button>
                </div>

                <!-- 清單項目區塊 -->
                <div class="max-h-[60vh] overflow-y-auto pr-2 no-scrollbar">
                    
                    <!-- 待購買清單 -->
                    <div class="mb-6">
                        <h4 class="text-xl font-bold text-gray-300 mb-3 border-b border-gray-700 pb-2 flex justify-between items-center">
                            待購買 (${pendingItems.length} 項)
                            ${pendingItems.length > 0 ? `
                                <button class="text-sm text-red-400 hover:text-red-300 transition duration-150 flex items-center" 
                                        onclick="clearCompletedShopping()">
                                    <i data-lucide="square-x" class="w-4 h-4 mr-1"></i> 一鍵清空所有已完成
                                </button>
                            ` : ''}
                        </h4>
                        <div id="pending-list" class="space-y-1">
                            ${pendingItems.length > 0 ? pendingItems.map(renderItem).join('') : 
                                '<p class="text-gray-500 text-center py-6 border-b border-gray-700/50">沒有待購買的項目！是時候去買點東西了 🛍️</p>'}
                        </div>
                    </div>
                    
                    <!-- 已購買清單 -->
                    <div class="mt-6">
                        <h4 class="text-xl font-bold text-gray-500 mb-3 border-b border-gray-700 pb-2">
                            已購買 (${purchasedItems.length} 項)
                        </h4>
                        <div id="purchased-list" class="space-y-1">
                            ${purchasedItems.length > 0 ? purchasedItems.map(renderItem).join('') : 
                                '<p class="text-gray-600 text-center py-6">還沒有購買任何東西 😔</p>'}
                        </div>
                    </div>
                </div>
            `;
            lucide.createIcons();
        }
        
        // 從輸入框新增項目
        window.addShoppingItemFromInput = () => {
            const input = document.getElementById('new-shopping-item');
            if (input && input.value.trim()) {
                window.addShoppingItem(input.value);
                input.value = '';
                input.focus();
            }
        };

        // 渲染旅遊筆記
        function renderNotes() {
            const container = document.getElementById('main-content');
            if (!container) return;

            container.innerHTML = `
                <h3 class="text-2xl font-bold text-teal-300 mb-4">我的旅遊筆記</h3>
                <p class="text-sm text-gray-400 mb-3">您的筆記會自動儲存。</p>
                <textarea 
                    id="notes-textarea" 
                    class="w-full h-[65vh] p-4 rounded-xl border-2 border-gray-600/50 bg-gray-800 text-white focus:border-teal-500 transition duration-150 resize-none" 
                    placeholder="在這裡寫下您的旅遊心得、注意事項或重要資訊..."
                >${notes}</textarea>
            `;

            const notesTextarea = document.getElementById('notes-textarea');
            if (notesTextarea) {
                notesTextarea.addEventListener('input', (e) => {
                    notes = e.target.value;
                    saveNotes(notes);
                });
            }
        }
        
        // 渲染常用日語速查表
        function renderJapanese() {
            const container = document.getElementById('main-content');
            if (!container) return;

            container.innerHTML = `
                <h3 class="text-2xl font-bold text-teal-300 mb-6">常用日語速查表</h3>
                <p class="text-sm text-gray-400 mb-6">點擊任何短語，即可一鍵複製。</p>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 max-h-[70vh] overflow-y-auto pr-2 no-scrollbar">
                    
                    ${renderJapanesePhrases()}
                    
                </div>
            `;
            
            lucide.createIcons();
        }

        // 生成日語短語卡片
        function renderJapanesePhrases() {
            const phrases = [
                { jp: 'すみません', cn: '不好意思 (打擾、道歉)', type: '招呼' },
                { jp: 'ありがとう ございます', cn: '謝謝 (禮貌)', type: '招呼' },
                { jp: 'いくら ですか', cn: '多少錢？', type: '購物' },
                { jp: 'これ ください', cn: '請給我這個', type: '購物' },
                { jp: 'お会計 お願いします', cn: '結帳，謝謝', type: '購物' },
                { jp: 'どこ ですか', cn: '在哪裡？', type: '問路' },
                { jp: 'トイレ は どこ ですか', cn: '廁所在哪裡？', type: '問路' },
                { jp: '助けて (たすけて)', cn: '救命！', type: '緊急' },
                { jp: '大丈夫 です', cn: '沒關係 / 沒問題', type: '常用' },
                { jp: 'はい / いいえ', cn: '是 / 不是', type: '常用' },
                { jp: 'おいしい', cn: '好吃', type: '常用' },
                { jp: 'もう一度 お願いします', cn: '請再說一次', type: '常用' },
            ];

            // 渲染卡片，點擊觸發複製功能
            return phrases.map(p => `
                <div onclick="copyPhrase('${p.jp}')" class="bg-gray-700/50 p-4 rounded-xl shadow-md hover:bg-gray-600/70 cursor-pointer transition duration-150 relative">
                    <p class="text-2xl font-extrabold text-teal-300 mb-1">${p.jp}</p>
                    <p class="text-sm text-gray-400">【${p.type}】 ${p.cn}</p>
                    <div class="absolute top-2 right-2 text-gray-500 hover:text-teal-400 transition duration-150" title="複製">
                        <i data-lucide="copy" class="w-4 h-4"></i>
                    </div>
                </div>
            `).join('');
        }

        // 複製日語短語
        window.copyPhrase = (text) => {
            document.execCommand('copy', false, text);

            const feedbackEl = document.getElementById('copy-feedback');
            if(feedbackEl) {
                feedbackEl.textContent = `✅ 已複製: ${text}`;
                feedbackEl.classList.remove('hidden', 'opacity-0');
                feedbackEl.classList.add('opacity-100');
                
                setTimeout(() => {
                    feedbackEl.classList.remove('opacity-100');
                    feedbackEl.classList.add('opacity-0');
                    setTimeout(() => feedbackEl.classList.add('hidden'), 300);
                }, 1500);
            }
        };

        // 渲染班機時間視圖 (星宇風格)
        function renderFlightSchedule() {
            const container = document.getElementById('main-content');
            if (!container) return;

            // 航班數據
            const flights = [
                {
                    type: 'Outbound', // 去程
                    date: '12/26 (五)', 
                    flightNum: 'JX800', 
                    departure: 'TPE (桃園)',
                    arrival: 'NRT (成田)',
                    depTime: '10:10',
                    arrTime: '14:20',
                    status: '確認',
                    icon: 'plane-takeoff'
                },
                {
                    type: 'Return', // 回程
                    date: '12/31 (三)', 
                    flightNum: 'JX801', 
                    departure: 'NRT (成田)',
                    arrival: 'TPE (桃園)',
                    depTime: '15:40',
                    arrTime: '18:45',
                    status: '確認',
                    icon: 'plane-landing'
                }
            ];

            const flightCards = flights.map(f => `
                <!-- 使用 flight-card class 和深色背景，模仿星宇的質感 -->
                <div class="flight-card p-6 rounded-xl shadow-lg border-t-4 border-teal-500 hover:shadow-teal-500/30 transition-shadow duration-300">
                    <div class="flex items-center justify-between mb-4 border-b border-gray-700 pb-3">
                        <h3 class="text-xl font-bold text-teal-400 flex items-center">
                            <i data-lucide="${f.icon}" class="w-6 h-6 mr-2 text-teal-500"></i>
                            ${f.type === 'Outbound' ? '去程航班' : '回程航班'}
                        </h3>
                        <span class="text-sm font-semibold px-3 py-1 rounded-full ${f.status === '確認' ? 'bg-teal-500 text-white' : 'bg-red-500 text-white'}">
                            ${f.status}
                        </span>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-y-4">
                        <!-- 航班編號 -->
                        <div>
                            <p class="text-xs text-gray-400 font-semibold">航班號碼</p>
                            <!-- 使用亮眼的 Amber 色系突出關鍵信息 -->
                            <p class="text-2xl font-extrabold text-amber-300 tracking-wide">${f.flightNum}</p>
                        </div>
                        <!-- 日期 -->
                        <div>
                            <p class="text-xs text-gray-400 font-semibold">出發日期</p>
                            <p class="text-xl font-bold text-white">${f.date}</p>
                        </div>
                        
                        <!-- 路線流程 -->
                        <div class="col-span-2 mt-4 space-y-4">
                            <!-- 出發 -->
                            <div class="flex items-start space-x-4">
                                <div class="text-center">
                                    <i data-lucide="clock" class="w-5 h-5 text-gray-400"></i>
                                    <div class="w-px h-8 bg-gray-700 mx-auto my-1"></div>
                                </div>
                                <div>
                                    <p class="text-3xl font-extrabold text-white">${f.depTime}</p>
                                    <p class="text-sm font-medium text-gray-300">${f.departure}</p>
                                </div>
                            </div>
                            
                            <!-- 抵達 -->
                            <div class="flex items-center space-x-4">
                                <div class="text-center">
                                    <i data-lucide="flag" class="w-5 h-5 text-gray-400"></i>
                                </div>
                                <div>
                                    <p class="text-3xl font-extrabold text-white">${f.arrTime}</p>
                                    <p class="text-sm font-medium text-gray-300">${f.arrival}</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <p class="text-xs text-gray-500 mt-6 pt-3 border-t border-gray-700 text-center">
                        請務必提前確認機場報到時間 (通常為起飛前 2.5 小時)
                    </p>
                </div>
            `).join('');

            container.innerHTML = `
                <h3 class="text-2xl font-bold text-teal-300 mb-6 flex items-center">
                    <i data-lucide="plane" class="w-6 h-6 mr-2 text-teal-500 transform rotate-90"></i>
                    星宇航空班機詳情
                </h3>
                <p class="text-sm text-gray-400 mb-6">您的東京之旅航班資訊，請注意起飛與抵達時間均為當地時間。</p>
                
                <div class="space-y-6">
                    ${flightCards}
                </div>
            `;
            lucide.createIcons();
        }

        // 切換主視圖
        function updateMainView() {
            // 重置導航按鈕的 active 狀態
            document.getElementById('btnFlight')?.classList.remove('active'); // 1
            document.getElementById('btnSkyliner')?.classList.remove('active'); // 2 (New)
            document.getElementById('btnItinerary')?.classList.remove('active'); // 3
            document.getElementById('btnJapanese')?.classList.remove('active'); // 4
            document.getElementById('btnShopping')?.classList.remove('active'); // 5
            document.getElementById('btnNotes')?.classList.remove('active'); // 6
            
            const mainContent = document.getElementById('main-content');
            if (mainContent) {
                mainContent.innerHTML = '';
            }

            switch (appView) {
                case 'FLIGHT':
                    document.getElementById('btnFlight')?.classList.add('active');
                    renderFlightSchedule();
                    break;
                case 'SKYLINER':
                    document.getElementById('btnSkyliner')?.classList.add('active');
                    renderSkylinerTimetable();
                    break;
                case 'ITINERARY':
                    document.getElementById('btnItinerary')?.classList.add('active');
                    renderItinerary();
                    break;
                case 'JAPANESE':
                    document.getElementById('btnJapanese')?.classList.add('active');
                    renderJapanese();
                    break;
                case 'SHOPPING':
                    document.getElementById('btnShopping')?.classList.add('active');
                    renderShoppingList();
                    break;
                case 'NOTES':
                    document.getElementById('btnNotes')?.classList.add('active');
                    renderNotes();
                    break;
                default:
                    renderFlightSchedule(); // 預設顯示第一個頁籤
            }
            lucide.createIcons();
        }

        // 設定並切換視圖
        window.setView = (view) => {
            appView = view;
            updateMainView();
        };

        // 程式初始化
        window.onload = () => {
            initializeFirebase();
            // 初始化時設定第一個頁籤為預設視圖
            appView = 'FLIGHT';
            itineraryDay = 1;
            updateMainView();
            
            // 設定匯率輸入框預設值
            document.getElementById('twdInput').value = 1000;
            convertCurrency(1000, 'twd', true);
        };
        
        // --- Modal 相關函式 (為保持簡潔，僅保留必要的操作函數簽名) ---
        
        window.showHotelModal = () => { 
            showModal(`
                <h3 class="text-2xl font-bold mb-4">設定飯店資訊</h3>
                <div class="space-y-4">
                    <div>
                        <label for="modal-h-name" class="block text-sm font-medium text-gray-700">飯店名稱</label>
                        <input type="text" id="modal-h-name" class="w-full p-2 border border-gray-300 rounded-lg" value="${hotel.name === '請設定飯店名稱' ? '' : hotel.name}" placeholder="例如：上野三井花園飯店">
                    </div>
                    <div>
                        <label for="modal-h-addr" class="block text-sm font-medium text-gray-700">飯店地址 (用於導航)</label>
                        <input type="text" id="modal-h-addr" class="w-full p-2 border border-gray-300 rounded-lg" value="${hotel.address === '請點擊下方按鈕設定地址' ? '' : hotel.address}" placeholder="例如：東京都台東區上野公園 4-1">
                    </div>
                    <div>
                        <label for="modal-h-dates" class="block text-sm font-medium text-gray-700">入住/退房日期</label>
                        <input type="text" id="modal-h-dates" class="w-full p-2 border border-gray-300 rounded-lg" value="${hotel.dates}" placeholder="例如：12/26 - 12/31">
                    </div>
                </div>
                <div class="flex justify-end space-x-3 mt-6">
                    <button class="px-4 py-2 bg-gray-300 text-gray-800 rounded-lg font-semibold hover:bg-gray-400 transition" onclick="closeModal()">取消</button>
                    <button class="btn-primary px-4 py-2 rounded-lg font-semibold" onclick="saveHotelSettings()">儲存並更新</button>
                </div>
            `, 'info');
            document.getElementById('modal-h-name').focus();
        };

        window.saveHotelSettings = () => { 
            const name = document.getElementById('modal-h-name').value || '未命名飯店';
            const address = document.getElementById('modal-h-addr').value || '請點擊下方按鈕設定地址';
            const dates = document.getElementById('modal-h-dates').value || 'YYYY/MM/DD - YYYY/MM/DD';
            
            hotel.name = name.trim();
            hotel.address = address.trim();
            hotel.dates = dates.trim();
            
            saveHotel();
            closeModal();
        };

        window.showRateModal = () => { 
             showModal(`
                <h3 class="text-2xl font-bold mb-4">設定最新匯率</h3>
                <p class="text-gray-600 mb-4">請輸入 1 TWD (台幣) 可以兌換多少 JPY (日圓)。</p>
                <div class="flex items-center space-x-2">
                    <span class="text-xl font-bold">1 TWD =</span>
                    <input type="number" id="modal-rate-input" class="flex-grow p-3 border border-gray-300 rounded-lg text-xl font-bold text-center focus:ring-teal-500 focus:border-teal-500" value="${hotel.rate.toFixed(2)}" step="0.01" min="1">
                    <span class="text-xl font-bold">JPY</span>
                </div>
                <p id="rate-modal-warning" class="text-sm text-red-500 mt-2 hidden">請輸入有效的數字。</p>
                <div class="flex justify-end space-x-3 mt-6">
                    <button class="px-4 py-2 bg-gray-300 text-gray-800 rounded-lg font-semibold hover:bg-gray-400 transition" onclick="closeModal()">取消</button>
                    <button class="btn-primary px-4 py-2 rounded-lg font-semibold" onclick="saveRateSettings()">儲存匯率</button>
                </div>
            `, 'info');
            document.getElementById('modal-rate-input').focus();
        };

        window.saveRateSettings = () => { 
            const input = document.getElementById('modal-rate-input');
            const newRate = parseFloat(input.value);

            if (isNaN(newRate) || newRate <= 0) {
                document.getElementById('rate-modal-warning').classList.remove('hidden');
                return;
            }

            hotel.rate = newRate;
            saveHotel();
            closeModal();
            // 更新匯率轉換器 UI
            convertCurrency(document.getElementById('twdInput').value, 'twd', true);
        };
        
        window.closeModal = () => { 
            const modalContainer = document.getElementById('modal-container');
            if (modalContainer) {
                modalContainer.classList.remove('opacity-100');
                modalContainer.classList.add('opacity-0');
                setTimeout(() => modalContainer.classList.add('hidden'), 300);
            }
        };

        function showModal(content, type = 'info') {
            const modalContainer = document.getElementById('modal-container');
            if (!modalContainer) return;
            
            modalContainer.innerHTML = `
                <div class="flat-panel p-6 rounded-xl w-full max-w-md transition-transform duration-300 transform scale-95" onclick="event.stopPropagation()">
                    ${content}
                </div>
            `;
            
            modalContainer.classList.remove('hidden');
            // 延遲添加 opacity-100 類以觸發淡入效果
            setTimeout(() => {
                modalContainer.classList.remove('opacity-0');
                modalContainer.classList.add('opacity-100');
            }, 10);
        }

    </script>
</body>
</html>
