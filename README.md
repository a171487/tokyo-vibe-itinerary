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
    <!-- 引入 Firebase 相關 SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, onSnapshot, updateDoc, arrayUnion, arrayRemove } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // 設定 Firebase Debug Log
        // setLogLevel('Debug');

        // 全域變數初始化
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

        let app, db, auth;
        let userId = null;
        let isAuthReady = false;

        // 資料模型
        window.hotel = { name: '請點擊下方按鈕設定飯店', address: '請點擊下方按鈕設定地址', dates: '20XX/XX/XX - 20XX/XX/XX' };
        window.flights = [
            { id: 'f1', date: '12/03 (二)', from: 'TPE (桃園)', to: 'NRT (成田)', airline: '星宇航空', number: 'JX800', price: 'NT$ 10,000', note: '托運行李 25KG' },
            { id: 'f2', date: '12/08 (日)', from: 'NRT (成田)', to: 'TPE (桃園)', airline: '星宇航空', number: 'JX801', price: 'NT$ 10,000', note: '手提行李 7KG' },
        ];
        window.itinerary = [
            { day: 1, date: '12/03 (二)', title: '抵達東京', plans: [{ j: 'NRT 機場', c: '領取行李/交通卡/網卡' }, { j: '京成 Skyliner', c: '前往上野/日暮里' }, { j: '飯店 Check-in', c: '東京上野三井花園酒店' }] },
            { day: 2, date: '12/04 (三)', title: '淺草文化巡禮', plans: [{ j: '淺草寺', c: '雷門、仲見世商店街' }, { j: '隅田川遊船', c: '前往台場' }, { j: '台場', c: '鋼彈、自由女神' }] },
            { day: 3, date: '12/05 (四)', title: '澀谷與新宿', plans: [{ j: '澀谷 Scramble Crossing', c: '知名十字路口' }, { j: '澀谷 Sky', c: '高空觀景台' }, { j: '新宿御苑', c: '日式庭園放鬆' }] },
        ];
        window.shopping = []; // 購物清單
        window.notes = "一些重要備註：\n- 記得帶護照！\n- 旅遊保險已保。"; // 備註
        window.rate = { twd: 1, jpy: 4.8, lastUpdate: new Date().toLocaleDateString() }; // 匯率

        // 預設狀態
        window.activeTab = 'ITINERARY';
        window.itineraryDay = 1;

        // Firebase 初始化與登入
        try {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);

            onAuthStateChanged(auth, async (user) => {
                isAuthReady = true;
                if (user) {
                    userId = user.uid;
                    // console.log("Logged in as user:", userId);
                    await loadDataFromFirebase();
                } else {
                    // console.log("No user, signing in anonymously.");
                    const token = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                    if (token) {
                        await signInWithCustomToken(auth, token);
                    } else {
                        await signInAnonymously(auth);
                    }
                }
                updateMainView(window.activeTab); // 確保在認證後更新視圖
            });

        } catch (e) {
            console.error("Firebase 初始化失敗:", e);
            // 在初始化失敗時也設定 authReady，以便應用程式在沒有 Firebase 的情況下仍可運行
            isAuthReady = true;
            updateMainView(window.activeTab); 
        }

        // --- Firebase 資料操作 ---
        const loadDataFromFirebase = async () => {
            if (!userId) return;

            // 1. 載入飯店資料 (config/hotel)
            try {
                const hotelRef = doc(db, `artifacts/${appId}/users/${userId}/config/hotel`);
                const docSnap = await getDoc(hotelRef);
                if (docSnap.exists()) {
                    window.hotel = docSnap.data();
                    // console.log("Hotel loaded:", window.hotel);
                } else {
                    // console.log("No hotel config found. Saving default.");
                    await window.saveHotel(false); // 儲存預設值，但不強制寫入
                }
                updateHotelUI();
            } catch (e) { console.error("Error loading hotel:", e); }

            // 2. 載入航班資料 (config/flights)
            try {
                const flightRef = doc(db, `artifacts/${appId}/users/${userId}/config/flights`);
                onSnapshot(flightRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().flights) {
                        window.flights = docSnap.data().flights;
                        // console.log("Flights updated:", window.flights);
                        if (window.activeTab === 'FLIGHT') renderFlight(window.flights);
                    }
                });
            } catch (e) { console.error("Error setting up flights listener:", e); }

            // 3. 載入行程資料 (config/itinerary)
            try {
                const itineraryRef = doc(db, `artifacts/${appId}/users/${userId}/config/itinerary`);
                onSnapshot(itineraryRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().itinerary) {
                        window.itinerary = docSnap.data().itinerary;
                        // console.log("Itinerary updated:", window.itinerary);
                        if (window.activeTab === 'ITINERARY') renderItinerary();
                    }
                });
            } catch (e) { console.error("Error setting up itinerary listener:", e); }

            // 4. 載入購物清單 (config/shopping)
            try {
                const shoppingRef = doc(db, `artifacts/${appId}/users/${userId}/config/shopping`);
                onSnapshot(shoppingRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().list) {
                        window.shopping = docSnap.data().list;
                        // console.log("Shopping updated:", window.shopping);
                        if (window.activeTab === 'SHOPPING') renderShoppingList();
                    }
                });
            } catch (e) { console.error("Error setting up shopping listener:", e); }

            // 5. 載入備註 (config/notes)
            try {
                const notesRef = doc(db, `artifacts/${appId}/users/${userId}/config/notes`);
                onSnapshot(notesRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().content) {
                        window.notes = docSnap.data().content;
                        // console.log("Notes updated:", window.notes);
                        if (window.activeTab === 'NOTES') renderNotes();
                    }
                });
            } catch (e) { console.error("Error setting up notes listener:", e); }

            // 6. 載入匯率 (config/rate)
             try {
                const rateRef = doc(db, `artifacts/${appId}/users/${userId}/config/rate`);
                onSnapshot(rateRef, (docSnap) => {
                    if (docSnap.exists()) {
                        window.rate = docSnap.data();
                        // console.log("Rate updated:", window.rate);
                        updateRateUI();
                    }
                });
            } catch (e) { console.error("Error setting up rate listener:", e); }
        };

        // --- UI 更新函數 ---
        window.updateHotelUI = () => {
            document.getElementById('h-name').textContent = window.hotel.name;
            document.getElementById('h-addr').textContent = window.hotel.address;
            document.getElementById('h-date').textContent = window.hotel.dates;
        };

        window.updateRateUI = () => {
            document.getElementById('rate-date').textContent = window.rate.lastUpdate;
            document.getElementById('twdInput').value = window.rate.twd;
            document.getElementById('jpyInput').value = window.rate.jpy;
            // 重新計算以確保更新
            window.calculateCurrency(); 
        };

        window.calculateCurrency = (source = 'twd') => {
            const twdInput = document.getElementById('twdInput');
            const jpyInput = document.getElementById('jpyInput');

            const twd = parseFloat(twdInput.value);
            const jpy = parseFloat(jpyInput.value);

            // 更新 rate 物件以備儲存
            window.rate.twd = twd;
            window.rate.jpy = jpy;

            if (source === 'twd' && !isNaN(twd) && twd > 0) {
                // TWD to JPY: JPY = TWD * (rate.jpy / rate.twd)
                const rateRatio = window.rate.jpy / window.rate.twd;
                const result = (twd * rateRatio).toFixed(2);
                document.getElementById('conversionResult').textContent = `NT$ ${twd.toFixed(2)} 約等於 ¥ ${result}`;
            } else if (source === 'jpy' && !isNaN(jpy) && jpy > 0) {
                // JPY to TWD: TWD = JPY * (rate.twd / rate.jpy)
                const rateRatio = window.rate.twd / window.rate.jpy;
                const result = (jpy * rateRatio).toFixed(2);
                document.getElementById('conversionResult').textContent = `¥ ${jpy.toFixed(2)} 約等於 NT$ ${result}`;
            } else {
                document.getElementById('conversionResult').textContent = '請輸入有效金額';
            }
        };


        // --- 主要內容渲染函數 ---
        window.updateMainView = (tab = 'ITINERARY') => {
            window.activeTab = tab;
            // 更新 Tab 樣式
            document.querySelectorAll('.tab-btn').forEach(btn => {
                if (btn.id === `tab-${tab}`) {
                    btn.classList.add('bg-teal-500', 'text-white', 'shadow-md');
                    btn.classList.remove('bg-gray-100', 'text-gray-700');
                } else {
                    btn.classList.add('bg-gray-100', 'text-gray-700');
                    btn.classList.remove('bg-teal-500', 'text-white', 'shadow-md');
                }
            });

            // 渲染內容
            switch (tab) {
                case 'ITINERARY': window.renderItinerary(); break;
                case 'FLIGHT': window.renderFlight(window.flights); break;
                case 'SHOPPING': window.renderShoppingList(); break;
                case 'NOTES': window.renderNotes(); break;
            }
        };

        // 渲染行程 (ITINERARY) 視圖
        window.renderItinerary = () => {
            const dayData = window.itinerary.find(d => d.day === window.itineraryDay);
            const totalDays = window.itinerary.length;
            
            if (!dayData) {
                document.getElementById('main-content').innerHTML = `<p class="text-center p-8 flat-panel max-w-lg mx-auto">行程資料載入中或不存在。</p>`;
                return;
            }

            document.getElementById('main-content').innerHTML = `
                <div class="space-y-4 max-w-lg mx-auto p-4 md:p-6 flat-panel">
                    <!-- 日期選擇區 -->
                    <div class="flex justify-between items-center mb-4">
                        <button onclick="window.setItineraryDay(Math.max(1, window.itineraryDay - 1))" ${window.itineraryDay === 1 ? 'disabled class="opacity-50 cursor-not-allowed"' : ''} class="p-2 rounded-full bg-teal-100 text-teal-600 hover:bg-teal-200 transition duration-150">
                            <i data-lucide="arrow-left" class="w-5 h-5"></i>
                        </button>
                        <h2 class="text-2xl font-bold text-center">
                            <span class="text-teal-600">DAY ${dayData.day}</span> | ${dayData.title}
                        </h2>
                        <button onclick="window.setItineraryDay(Math.min(${totalDays}, window.itineraryDay + 1))" ${window.itineraryDay === totalDays ? 'disabled class="opacity-50 cursor-not-allowed"' : ''} class="p-2 rounded-full bg-teal-100 text-teal-600 hover:bg-teal-200 transition duration-150">
                            <i data-lucide="arrow-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                    <p class="text-center text-gray-500 -mt-2 mb-6">${dayData.date}</p>

                    <!-- 行程列表 -->
                    <div class="space-y-6">
                        ${dayData.plans.map((p, index) => `
                            <div class="flex">
                                <div class="flex flex-col items-center mr-4">
                                    <!-- 數字圓圈 -->
                                    <div class="w-8 h-8 flex items-center justify-center rounded-full bg-teal-500 text-white font-bold text-sm shadow-md">${index + 1}</div>
                                    <!-- 連接線 -->
                                    ${index < dayData.plans.length - 1 ? '<div class="w-0.5 h-full bg-teal-200 my-1"></div>' : ''}
                                </div>
                                <div class="flex-grow p-3 bg-gray-50 border border-gray-200 rounded-lg shadow-sm">
                                    <div class="text-lg font-semibold text-teal-700">${p.j}</div>
                                    <div class="text-sm text-gray-600 mt-0.5">${p.c}</div>
                                </div>
                            </div>
                        `).join('')}
                    </div>

                    <button onclick="window.showItineraryModal()" class="w-full mt-6 p-3 bg-teal-500 text-white font-semibold rounded-lg shadow-lg hover:bg-teal-600 transition duration-150 flex items-center justify-center">
                        <i data-lucide="pencil" class="w-4 h-4 mr-2"></i> 編輯行程 (DAY ${dayData.day})
                    </button>
                </div>
            `;
            lucide.createIcons();
        };

        // 渲染航班 (FLIGHT) 視圖
        window.renderFlight = (flightData) => {
            document.getElementById('main-content').innerHTML = `
                <!-- 已將 max-w-lg 調整為 max-w-4xl 以拉寬顯示區塊 -->
                <div class="space-y-4 max-w-4xl mx-auto p-4 md:p-6 flat-panel">
                    <h2 class="text-2xl font-bold border-b pb-2 mb-4 border-gray-200 flex items-center"><i data-lucide="plane" class="w-6 h-6 mr-2"></i> 航班資訊</h2>
                    ${flightData.length === 0 ? '<p class="text-center text-gray-500 p-4">目前沒有航班資訊，請點擊下方按鈕新增。</p>' : flightData.map(f => `
                        <div class="p-4 border border-gray-200 rounded-lg shadow-md space-y-3 bg-white">
                            <div class="flex flex-wrap items-center text-gray-500 text-sm">
                                <div class="flex items-center mr-4">
                                    <i data-lucide="calendar" class="w-4 h-4 mr-1 text-red-500"></i> ${f.date}
                                </div>
                                <div class="flex items-center">
                                    <i data-lucide="credit-card" class="w-4 h-4 mr-1 text-red-500"></i> ${f.price}
                                </div>
                            </div>
                            <div class="font-bold text-2xl text-dark-navy flex flex-wrap items-center">
                                ${f.from} 
                                <i data-lucide="arrow-right" class="w-6 h-6 inline-block text-teal-600 mx-3 my-1"></i> 
                                ${f.to}
                            </div>
                            <div class="text-sm text-gray-700 pt-2 border-t mt-3 flex flex-wrap gap-x-4">
                                <div>航空公司: <span class="font-semibold text-teal-600">${f.airline}</span></div>
                                <div>航班編號: <span class="font-semibold">${f.number}</span></div>
                            </div>
                            <div class="text-xs text-gray-500 pt-2 mt-3 border-t">${f.note}</div>
                        </div>
                    `).join('')}
                    <button onclick="window.showFlightModal()" class="w-full mt-4 p-3 bg-teal-500 text-white font-semibold rounded-lg shadow-lg hover:bg-teal-600 transition duration-150 flex items-center justify-center">
                        <i data-lucide="pencil" class="w-4 h-4 mr-2"></i> 編輯航班資訊
                    </button>
                </div>
            `;
            lucide.createIcons();
        };


        // 渲染購物清單 (SHOPPING) 視圖
        window.renderShoppingList = () => {
            const shoppingHtml = window.shopping.length > 0
                ? window.shopping.map(item => `
                    <div id="shopping-item-${item.id}" class="flex items-center justify-between p-3 bg-white rounded-lg shadow-sm border border-gray-200 group transition duration-100 hover:shadow-md">
                        <div class="flex items-center">
                            <input type="checkbox" onchange="window.toggleShoppingItem('${item.id}', this.checked)" ${item.purchased ? 'checked' : ''} class="form-checkbox h-5 w-5 text-teal-600 rounded focus:ring-teal-500 border-gray-300">
                            <span class="ml-3 text-gray-800 ${item.purchased ? 'line-through text-gray-500' : 'font-medium'}">${item.name}</span>
                            ${item.note ? `<span class="ml-3 px-2 py-0.5 text-xs font-medium bg-red-100 text-red-700 rounded-full">${item.note}</span>` : ''}
                        </div>
                        <button onclick="window.deleteShoppingItem('${item.id}')" class="p-1 rounded-full text-red-400 hover:text-red-600 opacity-0 group-hover:opacity-100 transition duration-150">
                            <i data-lucide="trash-2" class="w-5 h-5"></i>
                        </button>
                    </div>
                `).join('')
                : '<p class="text-center text-gray-500 p-4">購物清單是空的，點擊下方按鈕新增。</p>';

            document.getElementById('main-content').innerHTML = `
                <div class="space-y-4 max-w-xl mx-auto p-4 md:p-6 flat-panel">
                    <h2 class="text-2xl font-bold border-b pb-2 mb-4 border-gray-200 flex items-center"><i data-lucide="shopping-cart" class="w-6 h-6 mr-2"></i> 購物清單</h2>
                    <div id="shopping-list" class="space-y-3">
                        ${shoppingHtml}
                    </div>
                    <button onclick="window.showShoppingModal()" class="w-full mt-6 p-3 bg-teal-500 text-white font-semibold rounded-lg shadow-lg hover:bg-teal-600 transition duration-150 flex items-center justify-center">
                        <i data-lucide="plus" class="w-4 h-4 mr-2"></i> 新增購物項目
                    </button>
                </div>
            `;
            lucide.createIcons();
        };

        // 渲染備註 (NOTES) 視圖
        window.renderNotes = () => {
             document.getElementById('main-content').innerHTML = `
                <div class="space-y-4 max-w-2xl mx-auto p-4 md:p-6 flat-panel">
                    <h2 class="text-2xl font-bold border-b pb-2 mb-4 border-gray-200 flex items-center"><i data-lucide="book-open-text" class="w-6 h-6 mr-2"></i> 重要備註</h2>
                    <textarea id="notes-content" class="w-full h-80 p-4 border border-gray-300 rounded-lg focus:ring-teal-500 focus:border-teal-500 bg-gray-50 text-gray-800 font-hand" placeholder="請在此輸入您的重要備註或待辦事項..." style="font-family: 'Patrick Hand', cursive;">${window.notes}</textarea>
                    <button onclick="window.saveNotes()" class="w-full p-3 bg-teal-500 text-white font-semibold rounded-lg shadow-lg hover:bg-teal-600 transition duration-150 flex items-center justify-center">
                        <i data-lucide="save" class="w-4 h-4 mr-2"></i> 儲存備註
                    </button>
                </div>
            `;
            lucide.createIcons();
        };


        // --- Modals / 互動函數 ---

        // 飯店設定
        window.showHotelModal = () => { 
            const n = prompt('輸入飯店名稱', window.hotel.name === '請點擊下方按鈕設定飯店' ? '' : window.hotel.name); 
            const a = prompt('輸入飯店地址', window.hotel.address === '請點擊下方按鈕設定地址' ? '' : window.hotel.address); 
            const d = prompt('輸入入住/退房日期 (例如：2025/12/03 - 2025/12/08)', window.hotel.dates);
            if(n) { 
                window.hotel.name = n; 
                window.hotel.address = a || '請點擊下方按鈕設定地址'; 
                window.hotel.dates = d || '尚未設定日期'; 
                window.saveHotel(); 
                window.updateHotelUI(); 
            }
        };

        // 航班編輯 (使用簡單 Prompt 模擬)
        window.showFlightModal = () => {
            const currentData = JSON.stringify(window.flights, null, 2);
            const newData = prompt('編輯航班資料 (JSON 格式):', currentData);
            if (newData) {
                try {
                    const newFlights = JSON.parse(newData);
                    if (Array.isArray(newFlights)) {
                        window.flights = newFlights.map((f, i) => ({ ...f, id: f.id || `f${i + 1}` }));
                        window.saveFlights();
                    } else {
                        throw new Error("格式錯誤: 必須是 JSON 陣列。");
                    }
                } catch (e) {
                    alert('輸入的 JSON 格式不正確: ' + e.message);
                }
            }
        };

        // 行程編輯 (使用簡單 Prompt 模擬)
        window.showItineraryModal = () => {
            const dayData = window.itinerary.find(d => d.day === window.itineraryDay);
            if (!dayData) return alert('找不到當日行程資料');

            const currentPlans = JSON.stringify(dayData.plans, null, 2);
            const newTitle = prompt(`編輯 DAY ${dayData.day} 標題 (目前: ${dayData.title})`, dayData.title);
            
            if (newTitle !== null) {
                const newPlans = prompt(`編輯 DAY ${dayData.day} 行程 (JSON 格式: [{j: "日文地點", c: "中文備註"}, ...])`, currentPlans);
                
                if (newPlans) {
                     try {
                        const parsedPlans = JSON.parse(newPlans);
                        if (Array.isArray(parsedPlans)) {
                            // 更新現有 dayData
                            dayData.title = newTitle;
                            dayData.plans = parsedPlans.filter(p => p.j); // 移除空項目
                            
                            // 更新整體 itinerary
                            window.itinerary = window.itinerary.map(d => d.day === window.itineraryDay ? dayData : d);
                            
                            window.saveItinerary();
                        } else {
                            throw new Error("行程格式錯誤: 必須是 JSON 陣列。");
                        }
                    } catch (e) {
                        alert('輸入的 JSON 格式不正確: ' + e.message);
                    }
                }
            }
        };
        
        // 購物項目新增
        window.showShoppingModal = () => {
            const itemName = prompt('請輸入購物項目名稱');
            if (itemName) {
                const itemNote = prompt('請輸入備註 (選填)');
                const newItem = {
                    id: crypto.randomUUID(),
                    name: itemName,
                    note: itemNote,
                    purchased: false
                };
                window.shopping.push(newItem);
                window.saveShoppingList();
            }
        };

        // 購物項目切換狀態
        window.toggleShoppingItem = (id, checked) => {
            const item = window.shopping.find(i => i.id === id);
            if (item) {
                item.purchased = checked;
                window.saveShoppingList();
            }
        };

        // 購物項目刪除
        window.deleteShoppingItem = (id) => {
            if (confirm('確定要刪除此購物項目嗎？')) {
                window.shopping = window.shopping.filter(i => i.id !== id);
                window.saveShoppingList();
            }
        };


        // --- 儲存函數 (Save Handlers) ---
        window.saveHotel = async (fb=true) => { 
            if(isAuthReady && userId && fb) {
                try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/hotel`), window.hotel, {merge:true});
                    // console.log("Hotel saved.");
                } catch (e) { console.error("Error saving hotel:", e); }
            }
        };
        
        window.saveFlights = async (fb=true) => {
            if(isAuthReady && userId && fb) {
                 try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/flights`), { flights: window.flights }, {merge:true});
                    // console.log("Flights saved.");
                    if (window.activeTab === 'FLIGHT') window.renderFlight(window.flights);
                } catch (e) { console.error("Error saving flights:", e); }
            }
        };

        window.saveItinerary = async (fb=true) => {
            if(isAuthReady && userId && fb) {
                try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/itinerary`), { itinerary: window.itinerary }, {merge:true});
                    // console.log("Itinerary saved.");
                    if (window.activeTab === 'ITINERARY') window.renderItinerary();
                } catch (e) { console.error("Error saving itinerary:", e); }
            }
        };

        window.saveShoppingList = async (fb=true) => {
            if(isAuthReady && userId && fb) {
                try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/shopping`), { list: window.shopping }, {merge:true});
                    // console.log("Shopping list saved.");
                    if (window.activeTab === 'SHOPPING') window.renderShoppingList();
                } catch (e) { console.error("Error saving shopping list:", e); }
            }
        };

        window.saveNotes = async (fb=true) => {
            const content = document.getElementById('notes-content').value;
            window.notes = content;
            if(isAuthReady && userId && fb) {
                try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/notes`), { content: content }, {merge:true});
                    // console.log("Notes saved.");
                    document.getElementById('save-notes-feedback').textContent = "✅ 備註已儲存！";
                    setTimeout(() => document.getElementById('save-notes-feedback').textContent = "", 2000);
                } catch (e) { 
                    console.error("Error saving notes:", e);
                    document.getElementById('save-notes-feedback').textContent = "❌ 儲存失敗！";
                    setTimeout(() => document.getElementById('save-notes-feedback').textContent = "", 2000);
                }
            }
        };

        window.saveRate = async (fb=true) => {
             // 確保從 UI 取得最新值
            const twd = parseFloat(document.getElementById('twdInput').value) || 1;
            const jpy = parseFloat(document.getElementById('jpyInput').value) || 4.8;

            window.rate.twd = twd;
            window.rate.jpy = jpy;
            window.rate.lastUpdate = new Date().toLocaleDateString();

            if(isAuthReady && userId && fb) {
                try {
                    await setDoc(doc(db, `artifacts/${appId}/users/${userId}/config/rate`), window.rate, {merge:true});
                    // console.log("Rate saved.");
                } catch (e) { console.error("Error saving rate:", e); }
            }
             window.updateRateUI(); // 重新計算並更新日期
        };


        // --- 輔助函數 ---
        window.setItineraryDay = (d) => { window.itineraryDay = d; window.renderItinerary(); };

        window.copyAddress = (id) => { 
            const text = document.getElementById(id).textContent;
            if(!text || text === '請點擊下方按鈕設定地址' || text === '尚未設定地址') {
                const originalText = document.getElementById(id).textContent;
                document.getElementById(id).textContent = "⚠️ 請先設定地址！";
                document.getElementById(id).classList.add('text-red-600', 'font-bold'); // 使用 Tailwind 類別
                setTimeout(() => {
                    document.getElementById(id).textContent = originalText;
                    document.getElementById(id).classList.remove('text-red-600', 'font-bold');
                }, 2000);
                return;
            }
            
            // 複製到剪貼簿
            navigator.clipboard.writeText(text).then(() => {
                // 提供視覺回饋
                const originalText = document.getElementById(id).textContent;
                document.getElementById(id).textContent = "✅ 已複製到剪貼簿！ (1.5秒後恢復)";
                document.getElementById(id).classList.add('text-teal-600', 'font-bold'); 
                setTimeout(() => {
                    document.getElementById(id).textContent = originalText;
                    document.getElementById(id).classList.remove('text-teal-600', 'font-bold');
                }, 1500);
            }).catch(err => {
                console.error('無法複製到剪貼簿', err);
                // 也可以提供一個備用方法或提示
                alert('複製失敗，請手動複製: ' + text);
            });
        };
        
        // 程式初始化
        window.onload = () => {
            // 在 onAuthStateChanged 中會觸發 loadDataFromFirebase 和 updateMainView
        };

    </script>
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
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.06);
        }

        /* 飯店資訊區塊的特殊樣式 */
        .hotel-info-panel {
            background-color: #3B4B5A; /* 比背景稍淺的深色 */
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: var(--color-cream);
            border-radius: 12px;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.2);
        }

        /* 手寫風格字體 */
        .font-hand {
            font-family: 'Patrick Hand', cursive;
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-6xl mx-auto">
        <!-- 頂部標題與狀態 -->
        <header class="mb-8">
            <h1 class="text-4xl font-extrabold flex items-center mb-1">
                <i data-lucide="map-pin" class="w-8 h-8 mr-3 text-teal-500"></i> TOKYO VIBE 
            </h1>
            <p class="text-gray-400 text-lg">您的東京自由行儀表板 (User ID: <span id="user-id-display">載入中...</span>)</p>
        </header>

        <!-- 飯店與匯率資訊 -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
            
            <!-- 飯店/住宿資訊 -->
            <div class="hotel-info-panel p-5 lg:col-span-2">
                <div class="flex items-start justify-between mb-3">
                    <div class="flex items-center">
                         <i data-lucide="building" class="w-6 h-6 mr-3 text-red-400"></i>
                        <h2 class="text-2xl font-bold">住宿資訊</h2>
                    </div>
                    <button onclick="window.showHotelModal()" class="p-2 text-teal-300 hover:text-teal-100 transition duration-150">
                        <i data-lucide="settings" class="w-5 h-5"></i>
                    </button>
                </div>
                
                <div class="space-y-2">
                    <p class="text-xl font-semibold text-teal-300" id="h-name">請點擊右上方按鈕設定飯店</p>
                    <div class="flex items-center text-sm text-gray-300">
                        <i data-lucide="clock" class="w-4 h-4 mr-1"></i> 
                        <span id="h-date">20XX/XX/XX - 20XX/XX/XX</span>
                    </div>
                    <div class="flex items-center justify-between mt-2 pt-2 border-t border-gray-700">
                         <p class="text-sm font-light italic" id="h-addr">請點擊下方按鈕設定地址</p>
                         <button onclick="window.copyAddress('h-addr')" class="ml-4 p-1 rounded-md bg-teal-500 text-white text-xs font-medium hover:bg-teal-600 transition duration-150 flex items-center">
                            <i data-lucide="copy" class="w-3 h-3 mr-1"></i> 複製地址
                         </button>
                    </div>
                </div>
            </div>

            <!-- 匯率資訊 -->
            <div class="hotel-info-panel p-5 lg:col-span-1">
                <div class="flex items-center mb-3">
                    <i data-lucide="trending-up" class="w-6 h-6 mr-3 text-red-400"></i>
                    <h2 class="text-2xl font-bold">即時匯率</h2>
                </div>
                <div class="space-y-3">
                    <p class="text-xs text-gray-400">最後更新: <span id="rate-date">載入中...</span></p>

                    <div class="flex items-center justify-between space-x-2">
                        <div class="flex-1">
                            <label for="twdInput" class="text-xs text-gray-400 block mb-1">新台幣 (TWD)</label>
                            <input type="number" id="twdInput" value="1" step="0.01" oninput="window.calculateCurrency('twd');" class="w-full p-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:ring-teal-500 focus:border-teal-500">
                        </div>
                        <i data-lucide="chevrons-right-left" class="w-5 h-5 text-red-400 self-end mb-2"></i>
                        <div class="flex-1">
                            <label for="jpyInput" class="text-xs text-gray-400 block mb-1">日圓 (JPY)</label>
                            <input type="number" id="jpyInput" value="4.8" step="0.01" oninput="window.calculateCurrency('jpy');" class="w-full p-2 rounded-lg bg-gray-700 border border-gray-600 text-white focus:ring-teal-500 focus:border-teal-500">
                        </div>
                    </div>
                    
                    <p class="text-lg font-semibold text-teal-300 pt-2 border-t border-gray-700" id="conversionResult">點擊輸入框計算</p>
                    <button onclick="window.saveRate()" class="w-full mt-2 p-2 bg-red-500 text-white font-semibold rounded-lg text-sm hover:bg-red-600 transition duration-150">
                        儲存匯率設定
                    </button>
                </div>
            </div>

        </div>

        <!-- 功能 Tab 導覽列 -->
        <nav class="flex space-x-3 mb-6 p-2 rounded-xl bg-gray-700/50 shadow-inner overflow-x-auto">
            <button id="tab-ITINERARY" onclick="window.updateMainView('ITINERARY')" class="tab-btn flex items-center p-3 rounded-xl whitespace-nowrap bg-teal-500 text-white shadow-md transition duration-200">
                <i data-lucide="calendar-check" class="w-5 h-5 mr-2"></i> 行程規劃
            </button>
            <button id="tab-FLIGHT" onclick="window.updateMainView('FLIGHT')" class="tab-btn flex items-center p-3 rounded-xl whitespace-nowrap bg-gray-100 text-gray-700 transition duration-200">
                <i data-lucide="plane" class="w-5 h-5 mr-2"></i> 航班機票
            </button>
            <button id="tab-SHOPPING" onclick="window.updateMainView('SHOPPING')" class="tab-btn flex items-center p-3 rounded-xl whitespace-nowrap bg-gray-100 text-gray-700 transition duration-200">
                <i data-lucide="shopping-cart" class="w-5 h-5 mr-2"></i> 購物清單
            </button>
            <button id="tab-NOTES" onclick="window.updateMainView('NOTES')" class="tab-btn flex items-center p-3 rounded-xl whitespace-nowrap bg-gray-100 text-gray-700 transition duration-200">
                <i data-lucide="book-open-text" class="w-5 h-5 mr-2"></i> 備註
            </button>
        </nav>

        <!-- 主要內容顯示區 -->
        <main id="main-content" class="pb-12"></main>

        <p class="text-center text-gray-500 text-sm mt-8">
            <span id="save-notes-feedback" class="text-teal-400 font-semibold mr-3"></span>
            當前使用者ID: <span class="font-mono text-xs text-gray-400" id="current-user-id">載入中...</span>
        </p>
    </div>

    <script>
        // 腳本區塊：確保在 body 結束標籤前執行
        // 確保 Lucide Icons 在頁面載入後被建立
        window.updateMainView();

        // 顯示使用者 ID (在 onAuthStateChanged 中更新)
        const userIdDisplay = document.getElementById('current-user-id');
        const userHeaderDisplay = document.getElementById('user-id-display');
        
        // 覆寫 onAuthStateChanged 之後的邏輯以確保 UI 更新
        const originalOnAuthStateChanged = window.onAuthStateChanged;
        window.onAuthStateChanged = (auth, callback) => {
            originalOnAuthStateChanged(auth, (user) => {
                if (user) {
                    const id = user.uid;
                    if (userIdDisplay) userIdDisplay.textContent = id;
                    if (userHeaderDisplay) userHeaderDisplay.textContent = id;
                } else {
                    if (userIdDisplay) userIdDisplay.textContent = '匿名使用者';
                    if (userHeaderDisplay) userHeaderDisplay.textContent = '匿名使用者';
                }
                callback(user);
            });
        };

        // 確保一開始就計算匯率（如果已載入預設值）
        if (window.rate.twd && window.rate.jpy) {
             window.calculateCurrency();
        }
    </script>
</body>
</html>
