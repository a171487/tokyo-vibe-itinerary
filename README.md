<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>東京 VIBE 旅遊儀表板</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=Noto+Sans+TC:wght@300;400;500;700&display=swap');
        
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            background-color: #0f172a; /* Slate 900 */
            background-image: radial-gradient(at 0% 0%, hsla(253,16%,7%,1) 0, transparent 50%), 
                              radial-gradient(at 50% 0%, hsla(225,39%,30%,1) 0, transparent 50%), 
                              radial-gradient(at 100% 0%, hsla(339,49%,30%,1) 0, transparent 50%);
            color: #e2e8f0;
            min-height: 100vh;
        }

        /* 自定義捲軸 */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #1e293b; }
        ::-webkit-scrollbar-thumb { background: #475569; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #64748b; }

        /* 隱藏數字輸入箭頭 */
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        
        /* 玻璃擬態效果 */
        .glass-panel {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        /* Sticky 效果 (桌面版) */
        @media (min-width: 1024px) {
            .sticky-col {
                position: sticky;
                top: 2rem;
                height: calc(100vh - 4rem);
                overflow-y: auto;
            }
        }
    </style>
</head>
<body class="p-4 lg:p-6 text-sm lg:text-base">

    <!-- 頂部標題 -->
    <header class="max-w-7xl mx-auto mb-6 glass-panel rounded-2xl p-6 relative overflow-hidden shadow-2xl">
        <div class="absolute top-0 right-0 p-6 opacity-20 transform translate-x-4 -translate-y-4">
            <i data-lucide="plane" class="w-32 h-32 text-indigo-400"></i>
        </div>
        <div class="relative z-10">
            <h1 class="text-3xl lg:text-4xl font-black tracking-tight text-transparent bg-clip-text bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 mb-2">
                TOKYO VIBE 2025
            </h1>
            <div class="flex flex-wrap gap-4 text-slate-400 text-sm font-medium">
                <span class="flex items-center"><i data-lucide="calendar" class="w-4 h-4 mr-1"></i> 12/26 出發</span>
                <span class="flex items-center"><i data-lucide="map-pin" class="w-4 h-4 mr-1"></i> 東京・富士山</span>
                <span class="flex items-center text-indigo-300"><i data-lucide="shield-check" class="w-4 h-4 mr-1"></i> 避開人潮攻略</span>
            </div>
        </div>
    </header>

    <!-- 主佈局：三欄式 (Nav | Content | Tools) -->
    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">

        <!-- [左欄] 日期導覽 (佔 2 等份) -->
        <nav class="lg:col-span-2 sticky-col flex flex-row lg:flex-col gap-3 overflow-x-auto lg:overflow-visible pb-2 lg:pb-0 z-20">
            <div id="nav-container" class="flex flex-row lg:flex-col gap-3 w-full">
                <!-- JS 動態生成按鈕 -->
            </div>
        </nav>

        <!-- [中欄] 主要行程與天氣 (佔 7 等份) -->
        <main class="lg:col-span-7 space-y-6">
            
            <!-- 天氣卡片 -->
            <div id="weather-card" class="glass-panel rounded-2xl p-6 transition-all duration-300 relative overflow-hidden group">
                <div class="absolute inset-0 bg-gradient-to-r from-blue-500/10 to-transparent opacity-0 group-hover:opacity-100 transition-opacity"></div>
                <div class="flex justify-between items-start relative z-10">
                    <div class="flex items-center gap-4">
                        <div class="p-3 bg-white/5 rounded-xl text-4xl" id="weather-icon"></div>
                        <div>
                            <p class="text-xs font-bold text-slate-400 uppercase tracking-widest mb-1">WEATHER</p>
                            <h2 id="weather-condition" class="text-xl font-bold text-white"></h2>
                            <p id="weather-note" class="text-sm text-slate-400 mt-1"></p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p id="weather-location" class="text-xs font-bold text-indigo-400 uppercase tracking-widest mb-1"></p>
                        <div class="text-3xl font-black text-white tracking-tight">
                            <span id="weather-high" class="text-red-400"></span>
                            <span class="text-slate-600 text-xl mx-1">/</span>
                            <span id="weather-low" class="text-blue-400 text-2xl"></span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 當日行程標題 -->
            <div class="flex items-center gap-3 pb-2 border-b border-slate-700">
                <h2 id="day-title" class="text-2xl font-bold text-white"></h2>
            </div>

            <!-- 行程列表 -->
            <div id="itinerary-list" class="space-y-4">
                <!-- JS 動態生成行程卡片 -->
            </div>

            <!-- 底部提醒 -->
            <div class="mt-8 pt-6 border-t border-slate-800/50 text-xs text-slate-500 text-center">
                <p>行程資料僅供參考，請依實際路況調整。</p>
            </div>
        </main>

        <!-- [右欄] 工具箱 (佔 3 等份) -->
        <aside class="lg:col-span-3 space-y-6 sticky-col">
            
            <!-- 工具 1: 匯率換算 -->
            <div class="glass-panel rounded-2xl p-5 border-t-4 border-indigo-500">
                <h3 class="text-sm font-bold text-indigo-400 uppercase tracking-widest mb-4 flex items-center">
                    <i data-lucide="coins" class="w-4 h-4 mr-2"></i> 匯率試算
                </h3>
                
                <div class="mb-4 flex items-center justify-between bg-slate-800/50 p-2 rounded-lg border border-slate-700">
                    <span class="text-xs text-slate-400">匯率 (1 TWD =)</span>
                    <div class="flex items-center">
                        <input type="number" id="rateInput" value="4.80" step="0.01" class="w-16 bg-transparent text-right font-mono font-bold text-indigo-300 focus:outline-none">
                        <span class="text-xs text-slate-500 ml-1">JPY</span>
                    </div>
                </div>

                <div class="space-y-3">
                    <div class="relative group">
                        <label class="absolute left-3 top-2 text-[10px] font-bold text-slate-500 group-focus-within:text-indigo-400">TWD (台幣)</label>
                        <input type="number" id="twdInput" placeholder="0" oninput="convert('TWD')"
                            class="w-full bg-slate-900/80 border border-slate-700 rounded-xl p-3 pt-6 text-lg font-mono text-white focus:border-indigo-500 focus:ring-1 focus:ring-indigo-500 transition-all outline-none">
                    </div>
                    <div class="relative flex justify-center">
                        <i data-lucide="arrow-down-up" class="w-4 h-4 text-slate-600"></i>
                    </div>
                    <div class="relative group">
                        <label class="absolute left-3 top-2 text-[10px] font-bold text-slate-500 group-focus-within:text-yellow-400">JPY (日幣)</label>
                        <input type="number" id="jpyInput" placeholder="0" oninput="convert('JPY')"
                            class="w-full bg-slate-900/80 border border-slate-700 rounded-xl p-3 pt-6 text-lg font-mono text-yellow-400 focus:border-yellow-500 focus:ring-1 focus:ring-yellow-500 transition-all outline-none">
                    </div>
                </div>
            </div>

            <!-- 工具 2: 飯店地址卡 -->
            <div class="glass-panel rounded-2xl p-5 border-t-4 border-yellow-500">
                <h3 class="text-sm font-bold text-yellow-500 uppercase tracking-widest mb-4 flex items-center">
                    <i data-lucide="map-pin" class="w-4 h-4 mr-2"></i> 飯店/緊急卡
                </h3>
                
                <div id="hotel-view">
                    <div class="mb-3">
                        <p class="text-[10px] text-slate-500 uppercase mb-1">HOTEL NAME</p>
                        <p id="h-name" class="font-bold text-white text-lg leading-tight">未設定飯店</p>
                    </div>
                    <div class="p-3 bg-black/40 rounded-lg border border-yellow-500/20 mb-4">
                        <p class="text-[10px] text-yellow-600 uppercase mb-1">ADDRESS (JP)</p>
                        <p id="h-addr" class="font-mono text-sm text-yellow-100/90 break-all">請點擊下方設定</p>
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="copyAddr()" class="bg-yellow-600 hover:bg-yellow-500 text-slate-900 font-bold py-2 px-3 rounded-lg text-xs flex items-center justify-center transition-colors">
                            <i data-lucide="copy" class="w-3 h-3 mr-1"></i> 複製地址
                        </button>
                        <button onclick="toggleEdit()" class="bg-slate-700 hover:bg-slate-600 text-white font-medium py-2 px-3 rounded-lg text-xs flex items-center justify-center transition-colors">
                            <i data-lucide="settings-2" class="w-3 h-3 mr-1"></i> 設定
                        </button>
                    </div>
                </div>

                <!-- 編輯模式 -->
                <div id="hotel-edit" class="hidden space-y-2">
                    <input type="text" id="in-name" placeholder="飯店名稱" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2 text-xs text-white focus:border-yellow-500 outline-none">
                    <textarea id="in-addr" rows="3" placeholder="日文地址 (給司機看)" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2 text-xs text-white focus:border-yellow-500 outline-none"></textarea>
                    <input type="text" id="in-phone" placeholder="電話" class="w-full bg-slate-900 border border-slate-700 rounded-lg p-2 text-xs text-white focus:border-yellow-500 outline-none">
                    <div class="grid grid-cols-2 gap-2 mt-2">
                        <button onclick="saveHotel()" class="bg-indigo-600 text-white py-2 rounded-lg text-xs font-bold">儲存</button>
                        <button onclick="toggleEdit()" class="bg-slate-700 text-white py-2 rounded-lg text-xs">取消</button>
                    </div>
                </div>
            </div>

            <!-- 小工具: 快速連結 -->
            <div class="glass-panel rounded-2xl p-4">
                 <h3 class="text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-3">USEFUL LINKS</h3>
                 <div class="flex flex-col gap-2">
                     <a href="https://www.google.com/maps" target="_blank" class="flex items-center text-xs text-indigo-300 hover:text-white transition-colors">
                         <i data-lucide="map" class="w-3 h-3 mr-2"></i> Google Maps
                     </a>
                     <a href="https://translate.google.com/" target="_blank" class="flex items-center text-xs text-indigo-300 hover:text-white transition-colors">
                         <i data-lucide="languages" class="w-3 h-3 mr-2"></i> Google Translate
                     </a>
                 </div>
            </div>

        </aside>

    </div>

    <!-- JS 邏輯 -->
    <script>
        // --- 1. 資料區 ---
        const itineraryData = [
            {
                day: 1, date: "12/26 (四)", title: "抵達與高輪新區",
                weather: { icon: "✈️", cond: "晴朗乾冷", temp: "3° / 10°", loc: "東京", alert: false },
                slots: [
                    { time: "Morning", title: "抵達與入住", desc: "前往 Takanawa Gateway 站辦理入住，感受隈研吾設計的新車站。", icon: "briefcase" },
                    { time: "Lunch", title: "NEWoMan 高輪", desc: "站內輕食或麵包店 (避開人潮)，快速解決午餐。", icon: "coffee" },
                    { time: "Afternoon", title: "KITTE 採購", desc: "移動至東京站，參觀 KITTE 商場與屋頂花園。", icon: "shopping-bag" },
                    { time: "Dinner", title: "品川/高輪居酒屋", desc: "選擇西口巷弄內的小型居酒屋，體驗在地氛圍。", icon: "beer" }
                ]
            },
            {
                day: 2, date: "12/27 (五)", title: "丸之內與八重洲",
                weather: { icon: "🌤️", cond: "多雲轉晴", temp: "4° / 11°", loc: "東京", alert: false },
                slots: [
                    { time: "Morning", title: "皇居外苑", desc: "清晨散步，拍攝二重橋與丸之內大樓群。", icon: "camera" },
                    { time: "Lunch", title: "KITTE 美食", desc: "避開尖峰，選擇 B1 或 5F 的蕎麥麵/豬排店。", icon: "utensils" },
                    { time: "Afternoon", title: "銀座散策", desc: "參觀東京車站畫廊或漫步銀座設計小店。", icon: "map" },
                    { time: "Dinner", title: "Sumiyaki Unafuji", desc: "★ 重點行程：八重洲店鰻魚飯 (務必預約)。", icon: "star", highlight: true }
                ]
            },
            {
                day: 3, date: "12/28 (六)", title: "下町與文青日",
                weather: { icon: "☀️", cond: "晴朗", temp: "2° / 9°", loc: "東京", alert: false },
                slots: [
                    { time: "Morning", title: "築地場外市場", desc: "★ 秘訣：7:00-8:30 前往，避開 9 點後觀光人潮。", icon: "sun" },
                    { time: "Lunch", title: "月島文字燒", desc: "尋找西仲通商店街較低調的店家用餐。", icon: "flame" },
                    { time: "Afternoon", title: "清澄白河", desc: "咖啡街區漫步，參觀現代美術館。", icon: "coffee" },
                    { time: "Dinner", title: "門前仲町", desc: "深川飯或平價燒肉，感受下町熱鬧氣氛。", icon: "utensils" }
                ]
            },
            {
                day: 4, date: "12/29 (日)", title: "富士山一日遊",
                weather: { icon: "🗻", cond: "極凍晴天", temp: "-5° / 1°", loc: "富士山", alert: true, note: "體感極低，請備防風衣物" },
                slots: [
                    { time: "Morning", title: "富士山五合目", desc: "近距離欣賞聖山與雲海 (注意保暖！)。", icon: "mountain" },
                    { time: "Lunch", title: "河口湖鄉土料理", desc: "餺飥麵 (Houtou)，暖身首選。", icon: "soup" },
                    { time: "Afternoon", title: "忍野八海", desc: "觀賞湧泉與雪景，留意地面結冰。", icon: "snowflake" },
                    { time: "Dinner", title: "新宿西口", desc: "避開東口人潮，在西口/南口大樓內用餐。", icon: "utensils" }
                ]
            },
            {
                day: 5, date: "12/30 (一)", title: "代官山時尚",
                weather: { icon: "☀️", cond: "晴朗", temp: "5° / 12°", loc: "東京", alert: false },
                slots: [
                    { time: "Morning", title: "代官山", desc: "蔦屋書店與周邊設計店，享受悠閒早晨。", icon: "book" },
                    { time: "Lunch", title: "惠比壽巷弄", desc: "尋找不排隊的義式或法式午間套餐。", icon: "utensils" },
                    { time: "Afternoon", title: "裏原宿/貓街", desc: "避開竹下通，探索巷弄潮流店。", icon: "shopping-bag" },
                    { time: "Dinner", title: "惠比壽橫丁", desc: "體驗熱鬧居酒屋文化 (可稍早前往佔位)。", icon: "beer" }
                ]
            },
            {
                day: 6, date: "12/31 (二)", title: "谷中銀座與返程",
                weather: { icon: "🌥️", cond: "晴時多雲", temp: "1° / 8°", loc: "東京", alert: false },
                slots: [
                    { time: "Morning", title: "谷中銀座", desc: "下町風情，邊走邊吃炸肉餅與仙貝。", icon: "shopping-bag" },
                    { time: "Lunch", title: "谷中蕎麥麵", desc: "簡單美味的傳統午餐。", icon: "utensils" },
                    { time: "Afternoon", title: "機場採購", desc: "前往機場，購買伴手禮並準備搭機。", icon: "plane" },
                    { time: "Dinner", title: "機上/機場", desc: "旅程結束，平安返家。", icon: "home" }
                ]
            }
        ];

        let currentDay = 1;

        // --- 2. 渲染邏輯 ---
        function renderNav() {
            const container = document.getElementById('nav-container');
            container.innerHTML = '';
            itineraryData.forEach(d => {
                const btn = document.createElement('button');
                const active = d.day === currentDay;
                btn.className = `group flex items-center justify-between p-3 rounded-xl transition-all duration-200 border text-left
                    ${active 
                        ? 'bg-indigo-600 border-indigo-500 text-white shadow-lg shadow-indigo-900/50' 
                        : 'bg-slate-800/50 border-slate-700 text-slate-400 hover:bg-slate-800 hover:text-white'}`;
                btn.innerHTML = `
                    <div>
                        <span class="block text-[10px] font-bold uppercase opacity-60">DAY ${d.day}</span>
                        <span class="font-bold text-sm">${d.date.split(' ')[0]}</span>
                    </div>
                    ${active ? '<i data-lucide="chevron-right" class="w-4 h-4 hidden lg:block"></i>' : ''}
                `;
                btn.onclick = () => { currentDay = d.day; renderMain(); };
                container.appendChild(btn);
            });
        }

        function renderMain() {
            const data = itineraryData.find(d => d.day === currentDay);
            
            // 天氣
            document.getElementById('weather-icon').textContent = data.weather.icon;
            document.getElementById('weather-condition').textContent = data.weather.cond;
            document.getElementById('weather-note').textContent = data.weather.note || '';
            document.getElementById('weather-location').textContent = data.weather.loc;
            
            const [low, high] = data.weather.temp.split(' / ');
            document.getElementById('weather-low').textContent = low;
            document.getElementById('weather-high').textContent = high;

            // 特殊天氣樣式 (富士山)
            const wCard = document.getElementById('weather-card');
            if(data.weather.alert) {
                wCard.className = "glass-panel rounded-2xl p-6 transition-all duration-300 relative overflow-hidden group border border-red-500/50";
                wCard.querySelector('.absolute').className = "absolute inset-0 bg-gradient-to-r from-red-900/40 to-transparent opacity-100";
            } else {
                wCard.className = "glass-panel rounded-2xl p-6 transition-all duration-300 relative overflow-hidden group border border-white/10";
                wCard.querySelector('.absolute').className = "absolute inset-0 bg-gradient-to-r from-blue-500/10 to-transparent opacity-0 group-hover:opacity-100 transition-opacity";
            }

            // 標題
            document.getElementById('day-title').innerHTML = `<span class="text-indigo-400 mr-2">DAY ${data.day}</span> ${data.title}`;

            // 行程列表
            const list = document.getElementById('itinerary-list');
            list.innerHTML = '';
            data.slots.forEach(slot => {
                const el = document.createElement('div');
                const isHigh = slot.highlight;
                el.className = `flex gap-4 p-4 rounded-xl border transition-all hover:scale-[1.01] duration-200
                    ${isHigh 
                        ? 'bg-indigo-900/20 border-indigo-500/50 shadow-lg shadow-indigo-900/20' 
                        : 'bg-slate-800/40 border-slate-700/50 hover:bg-slate-800/80'}`;
                
                el.innerHTML = `
                    <div class="flex-shrink-0 mt-1">
                        <div class="w-10 h-10 rounded-full flex items-center justify-center ${isHigh ? 'bg-indigo-500 text-white' : 'bg-slate-700 text-slate-300'}">
                            <i data-lucide="${slot.icon}" class="w-5 h-5"></i>
                        </div>
                    </div>
                    <div>
                        <p class="text-[10px] font-bold uppercase tracking-wider ${isHigh ? 'text-indigo-300' : 'text-slate-500'}">${slot.time}</p>
                        <h4 class="text-base font-bold text-white mb-1">${slot.title}</h4>
                        <p class="text-sm text-slate-400 leading-relaxed">${slot.desc}</p>
                    </div>
                `;
                list.appendChild(el);
            });

            lucide.createIcons();
            renderNav(); // Update active state
        }

        // --- 3. 工具邏輯 ---
        
        // 匯率
        function convert(source) {
            const rate = parseFloat(document.getElementById('rateInput').value) || 4.8;
            const twdEl = document.getElementById('twdInput');
            const jpyEl = document.getElementById('jpyInput');

            if (source === 'TWD') {
                const val = parseFloat(twdEl.value);
                jpyEl.value = isNaN(val) ? '' : (val * rate).toFixed(0);
            } else {
                const val = parseFloat(jpyEl.value);
                twdEl.value = isNaN(val) ? '' : (val / rate).toFixed(2);
            }
        }

        // 飯店卡 (LocalStorage)
        function loadHotel() {
            const name = localStorage.getItem('trip_h_name') || '未設定飯店';
            const addr = localStorage.getItem('trip_h_addr') || '請點擊下方設定';
            const phone = localStorage.getItem('trip_h_phone') || '';
            
            document.getElementById('h-name').textContent = name;
            document.getElementById('h-addr').textContent = addr;
            
            // 填入編輯框
            document.getElementById('in-name').value = name === '未設定飯店' ? '' : name;
            document.getElementById('in-addr').value = addr === '請點擊下方設定' ? '' : addr;
            document.getElementById('in-phone').value = phone;
        }

        function saveHotel() {
            localStorage.setItem('trip_h_name', document.getElementById('in-name').value);
            localStorage.setItem('trip_h_addr', document.getElementById('in-addr').value);
            localStorage.setItem('trip_h_phone', document.getElementById('in-phone').value);
            toggleEdit();
            loadHotel();
        }

        function toggleEdit() {
            const view = document.getElementById('hotel-view');
            const edit = document.getElementById('hotel-edit');
            if (view.classList.contains('hidden')) {
                view.classList.remove('hidden');
                edit.classList.add('hidden');
            } else {
                view.classList.add('hidden');
                edit.classList.remove('hidden');
            }
        }

        function copyAddr() {
            const text = document.getElementById('h-addr').textContent;
            if(!text || text === '請點擊下方設定') return alert('請先設定地址');
            navigator.clipboard.writeText(text).then(() => {
                alert('地址已複製！');
            });
        }

        // 初始化
        window.onload = () => {
            renderMain();
            loadHotel();
            // 匯率預設值
            document.getElementById('twdInput').value = 1000;
            convert('TWD');
        };

    </script>
</body>
</html>
