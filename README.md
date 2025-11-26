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
            background: linear-gradient(135deg, #1f2937 0%, #0f172a 100%);
            min-height: 100vh;
        }
        .scrollable-content {
            max-height: calc(100vh - 200px); 
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        /* 自定義捲軸樣式 */
        .scrollable-content::-webkit-scrollbar {
            width: 8px;
        }
        .scrollable-content::-webkit-scrollbar-thumb {
            background-color: #6366f1; /* 藍色捲軸，更有活力 */
            border-radius: 4px;
        }
        .scrollable-content::-webkit-scrollbar-track {
            background-color: #374151; 
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
    </style>
</head>
<body class="min-h-screen p-4 sm:p-8">

    <!-- 主容器：採用更深的背景和更明顯的陰影 -->
    <div class="max-w-4xl mx-auto bg-gray-900 rounded-3xl shadow-2xl shadow-gray-950/70 overflow-hidden border border-gray-700">
        
        <!-- 標題區 -->
        <header class="p-6 bg-slate-950 text-white shadow-2xl border-b border-indigo-700/50">
            <h1 class="text-3xl font-extrabold mb-1 text-indigo-400 tracking-wider">東京 VIBE 旅遊 App</h1>
            <p class="text-slate-400 text-sm">出發日期：12月26日 | 避開排隊名店與觀光人潮</p>
        </header>

        <!-- 行程內容區 -->
        <div class="flex flex-col lg:flex-row">
            
            <!-- 左側：日期導覽列 (使用深色背景，按鈕對比更強) -->
            <nav id="day-navigation" class="lg:w-1/4 p-4 lg:p-6 bg-gray-950 border-b lg:border-r border-gray-800 flex lg:flex-col overflow-x-auto lg:overflow-y-auto whitespace-nowrap lg:whitespace-normal">
                <!-- 按鈕將由 JS 動態生成 -->
            </nav>

            <!-- 右側：詳細行程與功能 -->
            <main class="lg:w-3/4 p-4 sm:p-6 lg:p-8 bg-gray-900">
                
                <!-- 新增功能：緊急聯絡與飯店地址卡 -->
                <div id="emergency-card" class="p-5 bg-yellow-900/30 text-yellow-100 rounded-xl shadow-inner shadow-yellow-900 mb-6 border border-yellow-700/50">
                    <h3 class="text-xl font-bold text-yellow-300 mb-3 flex items-center">
                        <i data-lucide="map-pin" class="w-5 h-5 mr-2"></i> 緊急聯絡與飯店地址
                    </h3>
                    <div id="hotel-display-card">
                        <!-- 靜態顯示區塊 -->
                        <div class="mb-2">
                            <p class="text-xs text-yellow-400 uppercase">飯店名稱:</p>
                            <p id="hotel-name-display" class="text-lg font-semibold">（點擊下方設定）</p>
                        </div>
                        <div class="mb-4 p-3 bg-yellow-900 rounded-lg">
                            <p class="text-xs text-yellow-400 uppercase mb-1">日文地址 (給計程車司機看):</p>
                            <p id="hotel-address-display" class="text-xl font-mono">尚未設定</p>
                        </div>
                        <button onclick="copyAddress()" class="w-full py-2 bg-yellow-600 hover:bg-yellow-700 text-gray-900 font-bold rounded-lg transition-all duration-200 shadow-md">
                            <i data-lucide="copy" class="w-4 h-4 mr-2 inline-block"></i>
                            複製日文地址
                        </button>
                    </div>

                    <div id="hotel-setup-form" class="mt-4 border-t border-yellow-800 pt-4 hidden">
                         <p class="text-sm font-medium mb-2">設定您的住宿資訊：</p>
                         <input type="text" id="setup-name" placeholder="飯店/民宿名稱" class="w-full p-2 mb-2 bg-gray-800 text-gray-200 rounded-lg border border-yellow-800">
                         <input type="text" id="setup-address" placeholder="日文地址 (最重要！)" class="w-full p-2 mb-2 bg-gray-800 text-gray-200 rounded-lg border border-yellow-800">
                         <input type="tel" id="setup-phone" placeholder="聯絡電話" class="w-full p-2 mb-2 bg-gray-800 text-gray-200 rounded-lg border border-yellow-800">
                         <button onclick="saveHotelInfo()" class="w-full py-2 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-lg transition-all duration-200 shadow-lg">
                            儲存資訊
                        </button>
                    </div>

                    <p class="mt-4 text-center text-sm cursor-pointer text-yellow-500 hover:text-yellow-300" onclick="toggleHotelSetup()">
                        點擊此處設定/修改住宿資訊
                    </p>
                </div>
                <!-- 緊急聯絡卡結束 -->


                <!-- 日幣匯率換算器 (TWD ↔ JPY) -->
                <div id="currency-converter" class="p-6 bg-gray-800 rounded-xl shadow-lg shadow-gray-950/50 mb-6 border border-gray-700">
                    <h3 class="text-xl font-bold text-indigo-400 mb-4 flex items-center">
                        <i data-lucide="coins" class="w-5 h-5 mr-2"></i>
                        日幣匯率換算 (TWD ↔ JPY)
                    </h3>
                    <div class="mb-4 p-3 bg-gray-900 rounded-lg border border-gray-700">
                        <label for="rateInput" class="block text-sm font-medium text-gray-400">手動設定當前匯率 (1 TWD 可換取 JPY)：</label>
                        <div class="flex items-center mt-1">
                            <input type="number" id="rateInput" value="4.80" step="0.01" min="0.01" 
                                class="w-24 p-2 text-center bg-gray-700 text-yellow-400 rounded-lg focus:ring-indigo-500 focus:border-indigo-500 border-none">
                            <span class="ml-2 text-lg font-bold text-gray-300">JPY</span>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label for="twdInput" class="block text-sm font-medium text-gray-300">台幣 (TWD)</label>
                            <input type="number" id="twdInput" placeholder="輸入台幣金額" 
                                class="w-full p-3 mt-1 bg-gray-700 text-white rounded-lg focus:ring-indigo-500 focus:border-indigo-500 border-none shadow-inner" oninput="convertCurrency('TWD')">
                        </div>
                        <div>
                            <label for="jpyInput" class="block text-sm font-medium text-gray-300">日圓 (JPY)</label>
                            <input type="number" id="jpyInput" placeholder="輸入日圓金額" 
                                class="w-full p-3 mt-1 bg-gray-700 text-white rounded-lg focus:ring-indigo-500 focus:border-indigo-500 border-none shadow-inner" oninput="convertCurrency('JPY')">
                        </div>
                    </div>
                    <p class="mt-4 text-xs text-gray-500 italic">**小提醒：** 本 App 無法連線至即時匯率 API，請手動調整上方匯率後使用。</p>
                </div>

                <!-- 天氣預報卡片 -->
                <div id="weather-card" class="rounded-xl p-4 mb-6 shadow-lg transition-all duration-300 border border-gray-700">
                    <div class="flex items-center justify-between mb-2">
                        <h2 class="text-xl font-semibold flex items-center">
                            <i data-lucide="cloud-sun" class="w-6 h-6 mr-2 text-indigo-400"></i>
                            <span id="weather-icon" class="text-2xl mr-2"></span>
                            <span id="weather-condition" class="text-gray-200"></span>
                        </h2>
                        <p id="weather-note" class="text-sm italic text-gray-400"></p>
                    </div>
                    <div class="flex justify-between text-gray-300">
                        <p><span class="font-medium">最高溫:</span> <span id="weather-high" class="text-red-400 font-bold"></span></p>
                        <p><span class="font-medium">最低溫:</span> <span id="weather-low" class="text-blue-400 font-bold"></span></p>
                        <p><span class="font-medium">地點:</span> <span id="weather-location" class="font-medium"></span></p>
                    </div>
                </div>

                <h2 id="current-day-title" class="text-2xl font-bold text-gray-100 mb-6 border-b border-indigo-500/50 pb-2"></h2>
                
                <div id="itinerary-content" class="space-y-6 scrollable-content">
                    <!-- 詳細行程將由 JS 動態載入 -->
                </div>
                
                <!-- 底部備註 -->
                <footer class="mt-8 pt-4 border-t border-gray-700 text-sm text-gray-500">
                    <p class="font-semibold text-gray-300 mb-2">💡 行程小提醒：</p>
                    <ul class="list-disc list-inside space-y-1">
                        <li>**鰻魚飯預約：** Sumiyaki Unafuji 是排隊名店，務必提前透過官網或電話預約。</li>
                        <li>**富士山：** 12月底五合目極冷，請準備**防水防風**的極地保暖衣物和暖暖包。</li>
                        <li>**年末休業：** 12/29-12/31 許多餐廳和私人小店可能開始公休，出發前請再次確認營業時間。</li>
                    </ul>
                </footer>
            </main>
        </div>

    </div>

    <script>
        // --- 行程數據 (保持不變) ---
        const itinerary = [
            // Day 1: 12/26 (四) - 週五前人潮較少
            {
                day: 1, date: "12月26日 (四)", title: "抵達與高輪新區探索",
                weather: { location: "東京市區", high: "10°C", low: "3°C", condition: "晴朗", icon: "☀️", note: "天氣乾冷，能見度佳。" },
                morning: { title: "抵達東京與入住", detail: "從機場前往酒店，在 Takanawa Gateway 站附近辦理入住。" },
                lunch: { title: "NEWoMan高輪輕食 (避峰)", detail: "在 NEWoMan 或站內選擇咖啡廳/麵包店，享用流動率高的簡餐，避開正式餐廳人潮。" },
                afternoon: { title: "高輪/KITTE採購", detail: "逛 NEWoMan 高輪商場，體驗新地標的設計感，之後搭車前往東京站。" },
                dinner: { title: "品川/高輪居酒屋", detail: "選擇品川站**西口**或酒店附近巷弄的串燒店，體驗當地人下班後的氣氛，避免百貨排隊店。" }
            },
            // Day 2: 12/27 (五) - 經典地標與排隊名店攻略
            {
                day: 2, date: "12月27日 (五)", title: "丸之內與八重洲排隊名店攻略",
                weather: { location: "東京市區", high: "11°C", low: "4°C", condition: "多雲轉晴", icon: "🌤️", note: "日夜溫差大，注意保暖。" },
                morning: { title: "KITTE丸之內與皇居外苑", detail: "早上前往 KITTE 丸之內，直奔屋頂花園拍攝東京車站全景，之後到皇居外苑散步。" },
                lunch: { title: "KITTE丸之內 B1/5F", detail: "選擇蕎麥麵或炸豬排店 (通常翻桌率高)，避開洋食或海鮮的排隊人龍。" },
                afternoon: { title: "銀座/東京車站畫廊", detail: "在銀座逛逛設計商店，或參觀東京車站畫廊，感受藝術氣息。" },
                dinner: { title: "Sumiyaki Unafuji (東京中城八重洲店)", detail: "**務必提前在線預約！** 這是名古屋的鰻魚飯名店。預約是唯一的避排隊方法。" }
            },
            // Day 3: 12/28 (六) - 下町與文青區
            {
                day: 3, date: "12月28日 (六)", title: "下町懷舊與清澄白河文青日",
                weather: { location: "東京市區", high: "9°C", low: "2°C", condition: "晴朗", icon: "☀️", note: "週末市區人潮增加，建議早上前往郊區。" },
                morning: { title: "築地場外市場 (清晨前往)", detail: "在 7:00-8:30 前往築地，避開 9 點後大量湧入的觀光客，享用早餐。" },
                lunch: { title: "月島文字燒 (當地人小店)", detail: "前往月島西仲通商店街，選擇一家外觀較低調的文字燒店，當地店通常比觀光名店排隊短。" },
                afternoon: { title: "清澄白河慢活", detail: "在清澄白河的文青咖啡街區漫步，參觀東京都現代美術館或清澄庭園。" },
                dinner: { title: "門前仲町深川飯/燒肉", detail: "在門前仲町品嚐傳統的深川飯 (蛤蜊炊飯)，或在平價燒肉店結束一天。" }
            },
            // Day 4: 12/29 (日) - 富士山一日遊 (最冷)
            {
                day: 4, date: "12月29日 (日)", title: "富士山一日遊：五合目與忍野八海",
                weather: { location: "富士山 (五合目/河口湖)", high: "1°C", low: "-5°C", condition: "晴朗且極度寒冷", icon: "❄️", note: "**極端寒冷！** 五合目體感溫度低於 -10°C，務必穿著專業防寒衣物。" },
                morning: { title: "富士山五合目 (近距離接觸)", detail: "乘坐巴士前往五合目，欣賞近距離的富士山及雲海景觀。注意防風保暖。" },
                lunch: { title: "河口湖周邊鄉土料理", detail: "在河口湖地區的家庭式餐廳，品嚐熱騰騰的當地特色料理，如餺飥麵 (Houtou)。" },
                afternoon: { title: "忍野八海 (冬季水景)", detail: "遊覽忍野八海，清澈的湧泉和白頭富士相映成趣。注意地面可能有結冰。" },
                dinner: { title: "新宿西口/南口晚餐", detail: "回到新宿後，避開東口人潮，在西口或南口商業大樓內用餐，選擇多且相對不擁擠。" }
            },
            // Day 5: 12/30 (一) - 時尚與設計
            {
                day: 5, date: "12月30日 (一)", title: "代官山與裏原宿時尚探索",
                weather: { location: "東京市區", high: "12°C", low: "5°C", condition: "晴朗", icon: "☀️", note: "年末購物潮，人潮較多，建議錯開主要購物區。" },
                morning: { title: "代官山漫步", detail: "逛代官山蔦屋書店、設計小店和安靜的服飾店，享受悠閒的質感時光。" },
                lunch: { title: "惠比壽/代官山巷弄午餐", detail: "選擇巷弄內不排隊但評價好的法式或義式午間套餐，價格實惠且氛圍優雅。" },
                afternoon: { title: "表參道/貓街 (Cat Street)", detail: "逛表參道後，轉入「貓街」探索裏原宿的潮流小店，避開竹下通主街的擁擠人潮。" },
                dinner: { title: "惠比壽橫丁體驗", detail: "前往熱鬧的惠比壽橫丁，體驗日本庶民居酒屋文化。雖然熱鬧，但翻桌率高，可錯峰前往。" }
            },
            // Day 6: 12/31 (二) - 懷舊與返程
            {
                day: 6, date: "12月31日 (二)", title: "谷中銀座與歸途",
                weather: { location: "東京市區", high: "8°C", low: "1°C", condition: "晴時多雲", icon: "🌥️", note: "年末許多店家公休，早點出發前往機場。" },
                morning: { title: "谷中銀座商店街", detail: "在懷舊的谷中銀座商店街感受下町風情，可以在這裡邊走邊吃烤仙貝、可樂餅等小吃。" },
                lunch: { title: "谷中蕎麥麵/當地簡餐", detail: "在谷中找一家傳統的蕎麥麵店，或在車站附近享用最後一餐。" },
                afternoon: { title: "採購與前往機場", detail: "在東京站或機場購買伴手禮，前往機場準備返程。" },
                dinner: { title: "機場或機上晚餐", detail: "根據班機時間，在機場或機上簡單用餐。" }
            }
        ];

        // --- 核心功能變數 ---

        let currentDay = 1;
        // 確保元素存在，如果 HTML 載入後，腳本在運行時它們應該已經存在
        const rateInput = document.getElementById('rateInput');
        const twdInput = document.getElementById('twdInput');
        const jpyInput = document.getElementById('jpyInput');


        // --- 貨幣換算邏輯 ---
        
        function getExchangeRate() {
            const rate = parseFloat(rateInput.value);
            return isNaN(rate) || rate <= 0 ? 4.80 : rate; 
        }

        function convertCurrency(source) {
            const rate = getExchangeRate();
            
            if (source === 'TWD') {
                const twd = parseFloat(twdInput.value);
                if (isNaN(twd) || twd < 0) {
                    jpyInput.value = '';
                    return;
                }
                const jpy = (twd * rate).toFixed(0); 
                jpyInput.value = jpy;
            } else if (source === 'JPY') {
                const jpy = parseFloat(jpyInput.value);
                if (isNaN(jpy) || jpy < 0) {
                    twdInput.value = '';
                    return;
                }
                const twd = (jpy / rate).toFixed(2);
                twdInput.value = twd;
            }
        }

        // 監聽匯率變動
        if(rateInput) {
            rateInput.addEventListener('input', () => {
                if (rateInput.value === '') {
                    rateInput.value = 4.80; 
                }
                // 任何一方有值時，重新換算
                if (twdInput.value) {
                    convertCurrency('TWD');
                } else if (jpyInput.value) {
                    convertCurrency('JPY');
                }
            });
        }


        // --- 緊急卡片邏輯 (使用 localStorage 模擬儲存) ---
        
        function loadHotelInfo() {
            const name = localStorage.getItem('hotelName') || '您的飯店名稱';
            const address = localStorage.getItem('hotelAddress') || '尚未設定日文地址';
            const phone = localStorage.getItem('hotelPhone') || '';

            document.getElementById('hotel-name-display').textContent = name;
            document.getElementById('hotel-address-display').textContent = address;

            // 預填表單
            if (document.getElementById('setup-name')) {
                document.getElementById('setup-name').value = name === '您的飯店名稱' ? '' : name;
                document.getElementById('setup-address').value = address === '尚未設定日文地址' ? '' : address;
                document.getElementById('setup-phone').value = phone;
            }
        }

        function saveHotelInfo() {
            const name = document.getElementById('setup-name').value.trim() || '未命名飯店';
            const address = document.getElementById('setup-address').value.trim() || '尚未設定日文地址';
            const phone = document.getElementById('setup-phone').value.trim() || '';

            if (address === '尚未設定日文地址' || address === '') {
                // 這裡我們不使用 alert(), 而是讓使用者點擊複製時顯示錯誤
                document.getElementById('hotel-address-display').textContent = '⚠️ 請輸入有效的日文地址！';
            } else {
                localStorage.setItem('hotelName', name);
                localStorage.setItem('hotelAddress', address);
                localStorage.setItem('hotelPhone', phone);
                loadHotelInfo(); // 重新載入顯示
                toggleHotelSetup(); // 隱藏設定表單
            }
        }

        function toggleHotelSetup() {
            const form = document.getElementById('hotel-setup-form');
            form.classList.toggle('hidden');
        }
        
        function copyAddress() {
            const addressText = document.getElementById('hotel-address-display').textContent;
            
            if (addressText === '尚未設定日文地址' || addressText.includes('⚠️')) {
                // 避免 alert，直接在介面顯示訊息
                const display = document.getElementById('hotel-address-display');
                display.textContent = '地址無效，請先設定！';
                setTimeout(() => {
                    // 幾秒後恢復原狀
                    loadHotelInfo();
                }, 2000);
                return;
            }

            // 使用 document.execCommand('copy') 來確保在 iFrame 環境下能正常運作
            const tempInput = document.createElement('textarea');
            tempInput.value = addressText;
            document.body.appendChild(tempInput);
            tempInput.select();
            
            try {
                document.execCommand('copy');
                const copyButton = document.querySelector('#emergency-card button');
                const originalText = copyButton.innerHTML;
                copyButton.innerHTML = '<i data-lucide="check" class="w-4 h-4 mr-2 inline-block"></i> 複製成功！';
                copyButton.classList.remove('bg-yellow-600');
                copyButton.classList.add('bg-green-600');
                
                setTimeout(() => {
                    copyButton.innerHTML = originalText;
                    copyButton.classList.remove('bg-green-600');
                    copyButton.classList.add('bg-yellow-600');
                    lucide.createIcons(); // 重新渲染圖標
                }, 1500);

            } catch (err) {
                console.error('無法複製文字:', err);
            }
            document.body.removeChild(tempInput);
        }

        // --- UI 渲染邏輯 ---
        
        function renderNavigation() {
            const nav = document.getElementById('day-navigation');
            nav.innerHTML = '';
            itinerary.forEach(item => {
                const button = document.createElement('button');
                button.setAttribute('data-day', item.day);
                // 更強烈的互動效果
                button.className = `day-button block p-3 px-4 rounded-xl lg:w-full text-left font-medium transition-all duration-150 ease-in-out lg:mb-2 mr-2 lg:mr-0 
                    ${item.day === currentDay 
                        ? 'bg-indigo-600 text-white shadow-xl shadow-indigo-500/40 border border-indigo-400' 
                        : 'bg-gray-800 text-gray-200 hover:bg-gray-700 hover:text-indigo-300'}`;
                button.innerHTML = `<span class="block text-sm">DAY ${item.day}</span><span class="block text-xs opacity-80">${item.date.split(' ')[0]}</span>`;
                button.addEventListener('click', () => {
                    currentDay = item.day;
                    updateUI();
                });
                nav.appendChild(button);
            });
            const activeBtn = nav.querySelector(`.day-button[data-day="${currentDay}"]`);
            if (activeBtn) {
                activeBtn.scrollIntoView({ behavior: 'smooth', inline: 'center' });
            }
        }

        function renderItinerary(dayData) {
            document.getElementById('current-day-title').textContent = `${dayData.title}`;
            
            // 渲染天氣
            document.getElementById('weather-icon').textContent = dayData.weather.icon;
            document.getElementById('weather-condition').textContent = dayData.weather.condition;
            document.getElementById('weather-high').textContent = dayData.weather.high;
            document.getElementById('weather-low').textContent = dayData.weather.low;
            document.getElementById('weather-location').textContent = dayData.weather.location;
            document.getElementById('weather-note').textContent = dayData.weather.note;

            // 調整天氣卡片的樣式 (更高級的材質感)
            const weatherCard = document.getElementById('weather-card');
            if (dayData.day === 4) {
                // 富士山極冷警告 (使用深紅搭配淺紅邊框)
                weatherCard.className = 'bg-red-900/40 border-l-4 border-red-500 rounded-xl p-4 mb-6 shadow-lg shadow-red-900/50 transition-all duration-300';
            } else {
                // 一般天氣 (使用深藍灰搭配靛藍邊框)
                weatherCard.className = 'bg-gray-800 border-l-4 border-indigo-500 rounded-xl p-4 mb-6 shadow-lg shadow-gray-950/50 transition-all duration-300';
            }
            
            // 渲染行程內容
            const contentDiv = document.getElementById('itinerary-content');
            contentDiv.innerHTML = '';
            
            const timeSlots = ['morning', 'lunch', 'afternoon', 'dinner'];
            
            timeSlots.forEach((slot
