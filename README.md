<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>東京六天五夜深度遊 | 避開人潮美食與景點</title>
    <!-- 載入 Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- 載入 Lucide Icons for aesthetic icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* 使用 Inter 字體 */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            /* 使用深色漸變背景，增加質感 */
            background: linear-gradient(135deg, #111827 0%, #1e1b4b 100%);
            min-height: 100vh;
            color: #e2e8f0;
        }
        .scrollable-content {
            /* 計算高度讓行程內容區可以捲動，但不影響其他區塊 */
            max-height: calc(100vh - 200px); 
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        /* 自定義捲軸樣式 (美工優化) */
        .scrollable-content::-webkit-scrollbar {
            width: 8px;
        }
        .scrollable-content::-webkit-scrollbar-thumb {
            background-color: #6366f1; /* 藍色捲軸 */
            border-radius: 4px;
        }
        .scrollable-content::-webkit-scrollbar-track {
            background-color: #1f2937; 
        }
        /* 隱藏數字輸入框的上下箭頭 */
        input[type="number"] {
            -moz-appearance: textfield; 
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none; 
            margin: 0;
        }
        /* 調整移動裝置上的導航列樣式 */
        @media (max-width: 1023px) {
            #day-navigation {
                padding: 1rem 0;
            }
            .day-button {
                flex-shrink: 0;
            }
        }
    </style>
</head>
<body class="min-h-screen p-4 sm:p-8">

    <!-- 主容器：採用更深的背景和更明顯的陰影 -->
    <div class="max-w-5xl mx-auto bg-gray-900 rounded-3xl shadow-2xl shadow-black/50 overflow-hidden border border-gray-700">
        
        <!-- 標題區 -->
        <header class="p-6 bg-slate-950 text-white shadow-lg border-b border-indigo-500/30 relative overflow-hidden">
            <div class="absolute top-0 right-0 p-4 opacity-10">
                <i data-lucide="plane" class="w-24 h-24 text-white"></i>
            </div>
            <h1 class="text-3xl font-extrabold mb-1 text-indigo-400 tracking-wider flex items-center gap-3">
                <i data-lucide="map" class="w-8 h-8"></i> 東京 VIBE 旅遊手冊
            </h1>
            <p class="text-slate-400 text-sm ml-11">出發日期：12月26日 | 避開排隊名店與觀光人潮</p>
        </header>

        <!-- 行程內容區 -->
        <div class="flex flex-col lg:flex-row h-full">
            
            <!-- 左側：日期導覽列 -->
            <nav id="day-navigation" class="lg:w-1/4 p-4 lg:p-6 bg-gray-950/80 border-b lg:border-r border-gray-800 flex lg:flex-col overflow-x-auto lg:overflow-y-auto whitespace-nowrap lg:whitespace-normal">
                <!-- 按鈕將由 JS 動態生成 -->
            </nav>

            <!-- 右側：詳細行程與功能 -->
            <main class="lg:w-3/4 p-4 sm:p-6 lg:p-8 bg-gray-900 relative">
                
                <!-- 功能區塊：兩欄佈局 -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                    
                    <!-- 1. 緊急聯絡與飯店地址卡 -->
                    <div id="emergency-card" class="p-5 bg-gradient-to-br from-yellow-900/40 to-yellow-950/40 text-yellow-100 rounded-2xl shadow-lg border border-yellow-700/30 backdrop-blur-sm">
                        <h3 class="text-lg font-bold text-yellow-400 mb-3 flex items-center">
                            <i data-lucide="building-2" class="w-5 h-5 mr-2"></i> 飯店地址卡 (給司機看)
                        </h3>
                        <div id="hotel-display-card">
                            <div class="mb-2">
                                <p class="text-xs text-yellow-500/80 uppercase font-semibold">飯店名稱</p>
                                <p id="hotel-name-display" class="text-base font-medium text-white">（載入中...）</p>
                            </div>
                            <div class="mb-4 p-3 bg-black/40 rounded-lg border border-yellow-500/20">
                                <p class="text-xs text-yellow-500/80 uppercase mb-1 font-semibold">日文地址</p>
                                <p id="hotel-address-display" class="text-lg font-mono text-yellow-50 break-words">尚未設定</p>
                            </div>
                            <button onclick="copyAddress()" class="w-full py-2 bg-yellow-600 hover:bg-yellow-500 text-gray-900 font-bold rounded-lg transition-all duration-200 shadow-md flex items-center justify-center gap-2">
                                <i data-lucide="copy" class="w-4 h-4"></i>
                                複製地址
                            </button>
                        </div>

                        <!-- 設定表單 (預設隱藏) -->
                        <div id="hotel-setup-form" class="mt-4 border-t border-yellow-800/50 pt-4 hidden">
                             <p class="text-sm font-medium mb-2 text-yellow-300">設定住宿資訊：</p>
                             <input type="text" id="setup-name" placeholder="飯店名稱" class="w-full p-2 mb-2 bg-gray-900 text-gray-200 rounded-lg border border-gray-700 focus:border-yellow-500 outline-none text-sm">
                             <input type="text" id="setup-address" placeholder="日文地址 (最重要!)" class="w-full p-2 mb-2 bg-gray-900 text-gray-200 rounded-lg border border-gray-700 focus:border-yellow-500 outline-none text-sm">
                             <input type="tel" id="setup-phone" placeholder="聯絡電話" class="w-full p-2 mb-2 bg-gray-900 text-gray-200 rounded-lg border border-gray-700 focus:border-yellow-500 outline-none text-sm">
                             <button onclick="saveHotelInfo()" class="w-full py-2 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-lg text-sm shadow-lg">
                                儲存資訊
                            </button>
                        </div>

                        <p class="mt-3 text-center text-xs cursor-pointer text-yellow-500/70 hover:text-yellow-400 underline decoration-dotted" onclick="toggleHotelSetup()">
                            設定/修改
                        </p>
                    </div>

                    <!-- 2. 日幣匯率換算器 -->
                    <div id="currency-converter" class="p-5 bg-gradient-to-br from-slate-800/50 to-slate-900/50 rounded-2xl shadow-lg border border-slate-700/50 backdrop-blur-sm">
                        <h3 class="text-lg font-bold text-indigo-400 mb-3 flex items-center">
                            <i data-lucide="banknote" class="w-5 h-5 mr-2"></i> 匯率試算
                        </h3>
                        <div class="mb-3 flex items-center justify-between bg-black/20 p-2 rounded-lg">
                            <label for="rateInput" class="text-xs text-gray-400 font-medium">匯率 (1 TWD = ? JPY)</label>
                            <div class="flex items-center">
                                <input type="number" id="rateInput" value="4.80" step="0.01" min="0.01" 
                                    class="w-16 p-1 text-right bg-transparent text-yellow-400 font-bold border-b border-gray-600 focus:border-indigo-500 outline-none">
                            </div>
                        </div>
                        
                        <div class="space-y-3">
                            <div class="relative">
                                <label for="twdInput" class="absolute left-3 top-2 text-xs text-gray-500 font-bold">TWD</label>
                                <input type="number" id="twdInput" placeholder="0" 
                                    class="w-full p-2 pt-6 bg-gray-900 text-white text-lg font-mono rounded-lg border border-gray-700 focus:border-indigo-500 outline-none transition-colors" oninput="convertCurrency('TWD')">
                            </div>
                            <div class="relative">
                                <label for="jpyInput" class="absolute left-3 top-2 text-xs text-gray-500 font-bold">JPY</label>
                                <input type="number" id="jpyInput" placeholder="0" 
                                    class="w-full p-2 pt-6 bg-gray-900 text-yellow-300 text-lg font-mono rounded-lg border border-gray-700 focus:border-indigo-500 outline-none transition-colors" oninput="convertCurrency('JPY')">
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 天氣預報卡片 (依據日期變動) -->
                <div id="weather-card" class="rounded-2xl p-4 mb-6 shadow-lg transition-all duration-300 border border-gray-700 bg-gray-800">
                    <div class="flex items-center justify-between mb-2">
                        <div class="flex items-center">
                            <span id="weather-icon" class="text-3xl mr-3"></span>
                            <div>
                                <h2 id="weather-condition" class="text-lg font-bold text-gray-100"></h2>
                                <p id="weather-note" class="text-sm text-gray-400"></p>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="text-xs text-gray-500 uppercase font-bold tracking-wider">氣溫預測</p>
                            <div class="flex items-end justify-end gap-2">
                                <span id="weather-low" class="text-xl font-bold text-blue-400"></span>
                                <span class="text-gray-600">/</span>
                                <span id="weather-high" class="text-xl font-bold text-red-400"></span>
                            </div>
                            <p id="weather-location" class="text-xs text-gray-400 mt-1"></p>
                        </div>
                    </div>
                </div>

                <!-- 每日標題 -->
                <h2 id="current-day-title" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-indigo-400 to-purple-400 mb-6 pb-2 border-b border-gray-800"></h2>
                
                <!-- 行程列表 -->
                <div id="itinerary-content" class="space-y-4 scrollable-content pb-20">
                    <!-- 詳細行程將由 JS 動態載入 -->
                </div>
                
                <!-- 底部備註 -->
                <footer class="mt-8 pt-6 border-t border-gray-800 text-xs text-gray-500">
                    <p class="font-bold text-gray-400 mb-2 flex items-center"><i data-lucide="info" class="w-3 h-3 mr-1"></i> 行程小提醒</p>
                    <ul class="space-y-1 list-disc list-inside">
                        <li><strong class="text-indigo-400">Sumiyaki Unafuji：</strong> 排隊名店，請務必提前預約。</li>
                        <li><strong class="text-indigo-400">富士山 (Day 4)：</strong> 12月底極冷，請準備防風保暖衣物。</li>
                        <li><strong class="text-indigo-400">年末營業：</strong> 12/29-12/31 部分店家可能公休，請事先確認。</li>
                    </ul>
                </footer>
            </main>
        </div>

    </div>

    <script>
        // --- 1. 行程數據 (6天5夜) ---
        const itinerary = [
            {
                day: 1, date: "12月26日 (四)", title: "抵達與高輪新區探索",
                weather: { location: "東京市區", high: "10°C", low: "3°C", condition: "晴朗", icon: "☀️", note: "天氣乾冷，能見度佳。" },
                morning: { title: "抵達東京與入住", detail: "從機場前往酒店，在 Takanawa Gateway 站附近辦理入住。" },
                lunch: { title: "NEWoMan高輪輕食 (避峰)", detail: "在 NEWoMan 或站內選擇咖啡廳/麵包店，享用流動率高的簡餐，避開正式餐廳人潮。" },
                afternoon: { title: "高輪/KITTE採購", detail: "逛 NEWoMan 高輪商場，體驗新地標的設計感，之後搭車前往東京站。" },
                dinner: { title: "品川/高輪居酒屋", detail: "選擇品川站西口或酒店附近巷弄的串燒店，體驗當地人下班後的氣氛，避免百貨排隊店。" }
            },
            {
                day: 2, date: "12月27日 (五)", title: "丸之內與八重洲排隊名店攻略",
                weather: { location: "東京市區", high: "11°C", low: "4°C", condition: "多雲轉晴", icon: "🌤️", note: "日夜溫差大，注意保暖。" },
                morning: { title: "KITTE丸之內與皇居外苑", detail: "早上前往 KITTE 丸之內，直奔屋頂花園拍攝東京車站全景，之後到皇居外苑散步。" },
                lunch: { title: "KITTE丸之內 B1/5F", detail: "選擇蕎麥麵或炸豬排店 (通常翻桌率高)，避開洋食或海鮮的排隊人龍。" },
                afternoon: { title: "銀座/東京車站畫廊", detail: "在銀座逛逛設計商店，或參觀東京車站畫廊，感受藝術氣息。" },
                dinner: { title: "Sumiyaki Unafuji (東京中城八重洲店)", detail: "**務必提前在線預約！** 這是名古屋的鰻魚飯名店。預約是唯一的避排隊方法。" }
            },
            {
                day: 3, date: "12月28日 (六)", title: "下町懷舊與清澄白河文青日",
                weather: { location: "東京市區", high: "9°C", low: "2°C", condition: "晴朗", icon: "☀️", note: "週末市區人潮增加，建議早上前往郊區。" },
                morning: { title: "築地場外市場 (清晨前往)", detail: "在 7:00-8:30 前往築地，避開 9 點後大量湧入的觀光客，享用早餐。" },
                lunch: { title: "月島文字燒 (當地人小店)", detail: "前往月島西仲通商店街，選擇一家外觀較低調的文字燒店，當地店通常比觀光名店排隊短。" },
                afternoon: { title: "清澄白河慢活", detail: "在清澄白河的文青咖啡街區漫步，參觀東京都現代美術館或清澄庭園。" },
                dinner: { title: "門前仲町深川飯/燒肉", detail: "在門前仲町品嚐傳統的深川飯 (蛤蜊炊飯)，或在平價燒肉店結束一天。" }
            },
            {
                day: 4, date: "12月29日 (日)", title: "富士山一日遊：五合目與忍野八海",
                weather: { location: "富士山 (五合目/河口湖)", high: "1°C", low: "-5°C", condition: "晴朗且極度寒冷", icon: "❄️", note: "**極端寒冷！** 五合目體感溫度低於 -10°C，務必穿著專業防寒衣物。" },
                morning: { title: "富士山五合目 (近距離接觸)", detail: "乘坐巴士前往五合目，欣賞近距離的富士山及雲海景觀。注意防風保暖。" },
                lunch: { title: "河口湖周邊鄉土料理", detail: "在河口湖地區的家庭式餐廳，品嚐熱騰騰的當地特色料理，如餺飥麵 (Houtou)。" },
                afternoon: { title: "忍野八海 (冬季水景)", detail: "遊覽忍野八海，清澈的湧泉和白頭富士相映成趣。注意地面可能有結冰。" },
                dinner: { title: "新宿西口/南口晚餐", detail: "回到新宿後，避開東口人潮，在西口或南口商業大樓內用餐，選擇多且相對不擁擠。" }
            },
            {
                day: 5, date: "12月30日 (一)", title: "代官山與裏原宿時尚探索",
                weather: { location: "東京市區", high: "12°C", low: "5°C", condition: "晴朗", icon: "☀️", note: "年末購物潮，人潮較多，建議錯開主要購物區。" },
                morning: { title: "代官山漫步", detail: "逛代官山蔦屋書店、設計小店和安靜的服飾店，享受悠閒的質感時光。" },
                lunch: { title: "惠比壽/代官山巷弄午餐", detail: "選擇巷弄內不排隊但評價好的法式或義式午間套餐，價格實惠且氛圍優雅。" },
                afternoon: { title: "表參道/貓街 (Cat Street)", detail: "逛表參道後，轉入「貓街」探索裏原宿的潮流小店，避開竹下通主街的擁擠人潮。" },
                dinner: { title: "惠比壽橫丁體驗", detail: "前往熱鬧的惠比壽橫丁，體驗日本庶民居酒屋文化。雖然熱鬧，但翻桌率高，可錯峰前往。" }
            },
            {
                day: 6, date: "12月31日 (二)", title: "谷中銀座與歸途",
                weather: { location: "東京市區", high: "8°C", low: "1°C", condition: "晴時多雲", icon: "🌥️", note: "年末許多店家公休，早點出發前往機場。" },
                morning: { title: "谷中銀座商店街", detail: "在懷舊的谷中銀座商店街感受下町風情，可以在這裡邊走邊吃烤仙貝、可樂餅等小吃。" },
                lunch: { title: "谷中蕎麥麵/當地簡餐", detail: "在谷中找一家傳統的蕎麥麵店，或在車站附近享用最後一餐。" },
                afternoon: { title: "採購與前往機場", detail: "在東京站或機場購買伴手禮，前往機場準備返程。" },
                dinner: { title: "機場或機上晚餐", detail: "根據班機時間，在機場或機上簡單用餐。" }
            }
        ];

        let currentDay = 1;

        // --- 2. 貨幣換算邏輯 ---
        function getExchangeRate() {
            const rateInput = document.getElementById('rateInput');
            const rate = parseFloat(rateInput.value);
            return isNaN(rate) || rate <= 0 ? 4.80 : rate; 
        }

        function convertCurrency(source) {
            const rate = getExchangeRate();
            const twdInput = document.getElementById('twdInput');
            const jpyInput = document.getElementById('jpyInput');
            
            if (!twdInput || !jpyInput) return;
            
            if (source === 'TWD') {
                const twd = parseFloat(twdInput.value);
                if (isNaN(twd) || twd < 0) { jpyInput.value = ''; return; }
                jpyInput.value = (twd * rate).toFixed(0); 
            } else if (source === 'JPY') {
                const jpy = parseFloat(jpyInput.value);
                if (isNaN(jpy) || jpy < 0) { twdInput.value = ''; return; }
                twdInput.value = (jpy / rate).toFixed(2);
            }
        }

        // --- 3. 飯店資訊卡邏輯 (使用 localStorage) ---
        function loadHotelInfo() {
            const name = localStorage.getItem('hotelName') || '您的飯店名稱';
            const address = localStorage.getItem('hotelAddress') || '尚未設定日文地址';
            const phone = localStorage.getItem('hotelPhone') || '';

            // 顯示文字
            const nameEl = document.getElementById('hotel-name-display');
            const addrEl = document.getElementById('hotel-address-display');
            if(nameEl) nameEl.textContent = name;
            if(addrEl) addrEl.textContent = address;

            // 預填表單
            const sName = document.getElementById('setup-name');
            const sAddr = document.getElementById('setup-address');
            const sPhone = document.getElementById('setup-phone');
            if(sName) sName.value = name === '您的飯店名稱' ? '' : name;
            if(sAddr) sAddr.value = address === '尚未設定日文地址' ? '' : address;
            if(sPhone) sPhone.value = phone;
        }

        function saveHotelInfo() {
            const name = document.getElementById('setup-name').value.trim() || '未命名飯店';
            const address = document.getElementById('setup-address').value.trim() || '尚未設定日文地址';
            const phone = document.getElementById('setup-phone').value.trim() || '';

            if (!address || address === '尚未設定日文地址') {
                const display = document.getElementById('hotel-address-display');
                if(display) display.textContent = '⚠️ 請輸入有效的日文地址！';
                return;
            }

            localStorage.setItem('hotelName', name);
            localStorage.setItem('hotelAddress', address);
            localStorage.setItem('hotelPhone', phone);
            loadHotelInfo();
            toggleHotelSetup();
        }

        function toggleHotelSetup() {
            const form = document.getElementById('hotel-setup-form');
            if(form) form.classList.toggle('hidden');
        }

        function copyAddress() {
            const text = document.getElementById('hotel-address-display').textContent;
            if (!text || text.includes('⚠️') || text === '尚未設定日文地址') {
                alert('請先設定有效的日文地址！');
                return;
            }
            
            const textArea = document.createElement("textarea");
            textArea.value = text;
            document.body.appendChild(textArea);
            textArea.select();
            try {
                document.execCommand('copy');
                const btn = document.querySelector('#emergency-card button');
                const originalHTML = btn.innerHTML;
                btn.innerHTML = '<i data-lucide="check" class="w-4 h-4"></i> 已複製';
                btn.classList.add('bg-green-600', 'text-white');
                btn.classList.remove('bg-yellow-600');
                
                setTimeout(() => {
                    btn.innerHTML = originalHTML;
                    btn.classList.remove('bg-green-600', 'text-white');
                    btn.classList.add('bg-yellow-600');
                    if(typeof lucide !== 'undefined') lucide.createIcons();
                }, 2000);
            } catch (err) {
                console.error('複製失敗', err);
            }
            document.body.removeChild(textArea);
        }

        // --- 4. 介面渲染邏輯 ---
        function renderNavigation() {
            const nav = document.getElementById('day-navigation');
            if(!nav) return;
            nav.innerHTML = '';
            itinerary.forEach(item => {
                const btn = document.createElement('button');
                const isActive = item.day === currentDay;
                btn.className = `day-button block p-4 rounded-xl text-left transition-all duration-200 lg:mb-3 mr-3 lg:mr-0 min-w-[120px] lg:w-full border
                    ${isActive 
                        ? 'bg-indigo-600 border-indigo-500 text-white shadow-lg shadow-indigo-900/50 scale-105' 
                        : 'bg-gray-800 border-gray-700 text-gray-400 hover:bg-gray-700 hover:text-white'}`;
                
                btn.innerHTML = `
                    <span class="block text-xs font-bold uppercase tracking-wider opacity-70">DAY ${item.day}</span>
                    <span class="block text-sm font-semibold">${item.date.split(' ')[0]}</span>
                `;
                btn.onclick = () => { currentDay = item.day; updateUI(); };
                nav.appendChild(btn);
            });
        }

        function renderContent(data) {
            // 標題
            const titleEl = document.getElementById('current-day-title');
            if(titleEl) titleEl.textContent = data.title;

            // 天氣
            const wIcon = document.getElementById('weather-icon');
            const wCond = document.getElementById('weather-condition');
            const wHigh = document.getElementById('weather-high');
            const wLow = document.getElementById('weather-low');
            const wLoc = document.getElementById('weather-location');
            const wNote = document.getElementById('weather-note');
            const wCard = document.getElementById('weather-card');

            if(wIcon) wIcon.textContent = data.weather.icon;
            if(wCond) wCond.textContent = data.weather.condition;
            if(wHigh) wHigh.textContent = data.weather.high;
            if(wLow) wLow.textContent = data.weather.low;
            if(wLoc) wLoc.textContent = data.weather.location;
            if(wNote) wNote.textContent = data.weather.note;

            // 富士山日特殊樣式
            if(wCard) {
                if (data.day === 4) {
                    wCard.className = 'rounded-2xl p-4 mb-6 shadow-lg transition-all duration-300 border border-red-500/50 bg-gradient-to-r from-red-900/30 to-gray-800';
                } else {
                    wCard.className = 'rounded-2xl p-4 mb-6 shadow-lg transition-all duration-300 border border-gray-700 bg-gray-800';
                }
            }

            // 行程區塊
            const contentDiv = document.getElementById('itinerary-content');
            if(!contentDiv) return;
            contentDiv.innerHTML = '';
            
            const slots = [
                { key: 'morning', label: '上午', icon: 'sun' },
                { key: 'lunch', label: '午餐', icon: 'utensils' },
                { key: 'afternoon', label: '下午', icon: 'shopping-bag' },
                { key: 'dinner', label: '晚餐', icon: 'moon' }
            ];

            slots.forEach(slot => {
                const info = data[slot.key];
                if(info) {
                    const div = document.createElement('div');
                    div.className = 'group p-5 bg-gray-800/50 hover:bg-gray-800 rounded-xl border border-gray-700 hover:border-indigo-500/50 transition-all duration-300 shadow-md';
                    div.innerHTML = `
                        <div class="flex items-start gap-4">
                            <div class="p-2 rounded-lg bg-gray-900 text-indigo-400 group-hover:text-indigo-300 group-hover:scale-110 transition-transform">
                                <i data-lucide="${slot.icon}" class="w-5 h-5"></i>
                            </div>
                            <div>
                                <h4 class="text-sm font-bold text-gray-400 uppercase tracking-wide mb-1">${slot.label}</h4>
                                <h3 class="text-lg font-bold text-white mb-1">${info.title}</h3>
                                <p class="text-sm text-gray-400 leading-relaxed">${info.detail}</p>
                            </div>
                        </div>
                    `;
                    contentDiv.appendChild(div);
                }
            });
        }

        function updateUI() {
            renderNavigation();
            const data = itinerary.find(i => i.day === currentDay);
            if(data) renderContent(data);
            if(typeof lucide !== 'undefined') lucide.createIcons();
        }

        // --- 5. 初始化 ---
        window.onload = function() {
            loadHotelInfo(); // 1. 載入飯店資訊
            
            // 2. 初始化匯率計算 (預設 TWD 1000)
            const twd = document.getElementById('twdInput');
            if(twd) {
                twd.value = 1000;
                convertCurrency('TWD');
            }

            // 3. 渲染畫面
            updateUI();
            
            // 監聽匯率輸入變化
            const rateInput = document.getElementById('rateInput');
            if(rateInput) {
                rateInput.addEventListener('input', () => {
                    if(document.getElementById('twdInput').value) convertCurrency('TWD');
                });
            }
        };
    </script>
</body>
</html>
