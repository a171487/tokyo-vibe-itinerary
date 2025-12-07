<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>東京旅遊助理 Dashboard</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#0ea5e9',
            accent: '#22c55e'
          }
        }
      }
    };
  </script>
  <style>
    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Noto Sans TC", sans-serif;
      font-size: 18px;
      line-height: 1.6;
    }
    .tab-active {
      background-color: #0ea5e9;
      color: white;
    }
  </style>
</head>
<body class="bg-slate-100 text-slate-900">
  <div class="min-h-screen flex flex-col items-stretch">
    <header class="w-full max-w-4xl mx-auto px-4 pt-4 pb-2 flex items-center justify-between">
      <div>
        <h1 class="text-3xl font-bold">東京旅遊助理</h1>
        <p class="text-sm text-slate-600">
          行程・匯率試算・天氣預報・富士山・記帳・檢查清單・購物清單，一次搞定
        </p>
      </div>
    </header>

    <!-- Tabs -->
    <nav class="w-full max-w-4xl mx-auto px-3 mt-2">
      <div class="flex bg-white rounded-full shadow text-sm font-semibold overflow-hidden">
        <button class="flex-1 py-2 tab-btn tab-active" data-tab="home">首頁</button>
        <button class="flex-1 py-2 tab-btn" data-tab="itinerary">行程</button>
        <button class="flex-1 py-2 tab-btn" data-tab="account">記帳</button>
        <button class="flex-1 py-2 tab-btn" data-tab="checklist">旅遊檢查清單</button>
        <button class="flex-1 py-2 tab-btn" data-tab="hotel">飯店</button>
        <button class="flex-1 py-2 tab-btn" data-tab="shopping">購物清單</button>
      </div>
    </nav>

    <!-- Main swipe area -->
    <main id="main" class="flex-1 w-full max-w-4xl mx-auto px-3 pt-4 pb-20 overflow-x-hidden"></main>

    <footer class="w-full fixed bottom-0 left-0 right-0 bg-white border-t border-slate-200">
      <div class="max-w-4xl mx-auto px-4 py-2 flex justify-between text-xs text-slate-600">
        <span>⬅️ 右滑 / 左滑 切換頁面</span>
        <span>單一 HTML 檔，可直接放 GitHub Pages</span>
      </div>
    </footer>
  </div>

  <script>
    /* -------- Tabs + Swipe -------- */
    const tabs = ['home','itinerary','account','checklist','hotel','shopping'];
    let currentTabIndex = 0;
    let touchStartX = null;

    function setTabByIndex(idx) {
      if (idx < 0 || idx >= tabs.length) return;
      currentTabIndex = idx;
      const tab = tabs[idx];
      document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('tab-active'));
      const activeBtn = document.querySelector('.tab-btn[data-tab="' + tab + '"]');
      if (activeBtn) activeBtn.classList.add('tab-active');
      renderTab(tab);
    }

    function renderTab(name) {
      if (name === 'home') renderHome();
      else if (name === 'itinerary') renderItinerary();
      else if (name === 'account') renderAccount();
      else if (name === 'checklist') renderChecklist();
      else if (name === 'hotel') renderHotel();
      else if (name === 'shopping') renderShopping();
    }

    document.addEventListener('click', (e) => {
      const btn = e.target.closest('.tab-btn');
      if (!btn) return;
      const tab = btn.dataset.tab;
      const idx = tabs.indexOf(tab);
      if (idx !== -1) setTabByIndex(idx);
    });

    function attachSwipe() {
      const main = document.getElementById('main');
      main.addEventListener('touchstart', (e) => {
        touchStartX = e.touches[0].clientX;
      }, { passive: true });
      main.addEventListener('touchend', (e) => {
        if (touchStartX === null) return;
        const diffX = e.changedTouches[0].clientX - touchStartX;
        if (Math.abs(diffX) > 60) {
          if (diffX < 0) setTabByIndex(currentTabIndex + 1);
          else setTabByIndex(currentTabIndex - 1);
        }
        touchStartX = null;
      }, { passive: true });
    }

    /* -------- 行程資料（加預算） -------- */
    const itineraryData = [
      {
        date: '12/26（五）上野',
        items: [
          { time: '14:20', text: '抵達成田機場 (NRT) T2', map: 'Narita Airport Terminal 2' },
          { time: '16:00', text: '飯店 Check-in：上野站前1號遊客酒店', map: '上野站前1號遊客酒店 東京都 台東區 東上野 2-18-18' },
          { time: '18:00', text: '晚餐：阿美橫丁', map: '阿美橫丁' },
          { time: '20:00', text: '購物：無印良品上野丸井店、OS Drug 藥妝店', map: '無印良品 上野丸井店 OS Drug 上野' }
        ]
      },
      {
        date: '12/27（六）東京、銀座',
        items: [
          { time: '09:00', text: 'Tricolore coffee（早餐：蘋果派、閃電泡芙）', map: 'トリコロール 本店 銀座' },
          { time: '12:00', text: '牛たんの檸檬 有楽町店', map: '牛たんの檸檬 有楽町店' },
          { time: '15:00', text: 'MARLOWE 焦糖布丁', map: 'MARLOWE プリン 東京' },
          { time: '20:30', text: '東京・お台場海浜公園花火 / 惠比壽花園廣場燈光秀', map: 'お台場海浜公園' }
        ]
      },
      {
        date: '12/28（日）富士山一日遊',
        items: [
          { time: '08:00', text: '丸之內南口集合', map: '東京駅 丸の内南口' },
          { time: '10:30', text: '新倉山淺間公園', map: '新倉山浅間公園' },
          { time: '11:45', text: '日川時計店', map: '日川時計店 忍野' },
          { time: '12:30', text: '忍野八海（含午餐）', map: '忍野八海' },
          { time: '15:20', text: '大石公園', map: '大石公園 河口湖' },
          { time: '18:50', text: '返回東京市區', map: '東京駅' }
        ]
      },
      {
        date: '12/29（一）東京、澀谷',
        items: [
          { time: '11:30', text: '壽喜燒：Sukiyaki Juni Ten', map: 'Sukiyaki Juni Ten 東京' },
          { time: '14:30', text: '東急 Plaza 表參道原宿', map: '東急プラザ 表参道原宿' },
          { time: '19:30', text: '中目黑', map: '中目黒駅' }
        ]
      },
      {
        date: '12/30（二）新宿、秋葉原',
        items: [
          { time: '11:00', text: '新宿：NEWoMan 百貨', map: 'NEWoMan 新宿' },
          { time: '18:30', text: '二木菓子（買伴手禮）', map: '二木の菓子 秋葉原' }
        ]
      },
      {
        date: '12/31（三）成田市',
        items: [
          { time: '09:30', text: '成田山新勝寺', map: '成田山新勝寺' },
          { time: '10:30', text: '成田山表參道', map: '成田山 表参道' },
          { time: '11:30', text: '成田夢牧場 門前店', map: '成田ゆめ牧場 門前店' },
          { time: '12:30', text: '成田機場 (NRT)', map: 'Narita Airport' }
        ]
      }
    ];

    // 行程預算（TWD），key: "dayIndex-itemIndex"
    const itineraryBudgets = {};

    function updateItineraryBudget(el) {
      const key = el.dataset.itkey;
      const v = parseFloat(el.value);
      if (!key) return;
      if (isNaN(v)) {
        delete itineraryBudgets[key];
      } else {
        itineraryBudgets[key] = v;
      }
    }

    function getItineraryTotalBudget() {
      return Object.values(itineraryBudgets).reduce((s, v) => s + v, 0);
    }

    /* -------- 匯率試算（手動） -------- */
    let manualFxRate = 0.22; // 1 JPY = 0.22 TWD

    function setManualRate() {
      const input = document.getElementById('fxRateManual');
      if (!input) return;
      const v = parseFloat(input.value);
      if (!isNaN(v) && v > 0) {
        manualFxRate = v;
        updateFxCalc();
        updateFxCalcReverse();
      }
    }

    function updateFxCalc() {
      const jpyInput = document.getElementById('fxInputJPY');
      const out = document.getElementById('fxOutputTWD');
      if (!jpyInput || !out) return;
      const val = parseFloat(jpyInput.value);
      if (isNaN(val) || !manualFxRate) {
        out.textContent = '--';
        return;
      }
      const twd = val * manualFxRate;
      out.textContent = '約 NT$ ' + Math.round(twd).toLocaleString();
    }

    function updateFxCalcReverse() {
      const twdInput = document.getElementById('fxInputTWD');
      const out = document.getElementById('fxOutputJPY');
      if (!twdInput || !out) return;
      const val = parseFloat(twdInput.value);
      if (isNaN(val) || !manualFxRate) {
        out.textContent = '--';
        return;
      }
      const jpy = val / manualFxRate;
      out.textContent = '約 ¥ ' + Math.round(jpy).toLocaleString();
    }

    /* -------- 東京天氣：即時 + 一週 -------- */
    async function fetchTokyoWeather() {
      const currentEl = document.getElementById('tokyoWeatherCurrent');
      const weekEl = document.getElementById('tokyoWeatherWeek');
      if (!currentEl || !weekEl) return;
      try {
        const url = 'https://api.open-meteo.com/v1/forecast?latitude=35.6895&longitude=139.6917&current_weather=true&daily=temperature_2m_max,temperature_2m_min,weathercode&timezone=Asia%2FTokyo';
        const res = await fetch(url);
        const data = await res.json();
        if (!data || !data.current_weather || !data.daily) {
          currentEl.textContent = '無法取得氣象資料';
          weekEl.innerHTML = '<p class="text-sm text-slate-500">一週預報無法顯示。</p>';
          return;
        }
        const c = data.current_weather;
        const desc = weatherCodeToText(c.weathercode);
        currentEl.textContent = `${desc}，約 ${c.temperature}°C`;

        const days = data.daily.time;
        const tmax = data.daily.temperature_2m_max;
        const tmin = data.daily.temperature_2m_min;
        const codes = data.daily.weathercode;

        let html = '<div class="overflow-x-auto"><table class="min-w-full text-sm text-left"><thead><tr class="border-b"><th class="py-1 pr-4">日期</th><th class="py-1 pr-4">天氣</th><th class="py-1 pr-4">最高</th><th class="py-1 pr-4">最低</th></tr></thead><tbody>';
        for (let i = 0; i < days.length; i++) {
          const d = days[i];
          const label = d.substring(5); // MM-DD
          html += `<tr class="border-b last:border-0">
            <td class="py-1 pr-4">${label}</td>
            <td class="py-1 pr-4">${weatherCodeToText(codes[i])}</td>
            <td class="py-1 pr-4">${tmax[i]}°C</td>
            <td class="py-1 pr-4">${tmin[i]}°C</td>
          </tr>`;
        }
        html += '</tbody></table></div>';
        weekEl.innerHTML = html;
      } catch (err) {
        console.error(err);
        currentEl.textContent = '氣象讀取錯誤（可能是網路或 API 限制）';
        weekEl.innerHTML = '<p class="text-sm text-slate-500">一週預報無法顯示。</p>';
      }
    }

    function weatherCodeToText(code) {
      if (code === 0) return '晴朗';
      if (code === 1 || code === 2) return '大致晴朗';
      if (code === 3) return '多雲';
      if (code === 45 || code === 48) return '霧 / 霾';
      if (code === 51 || code === 53 || code === 55) return '毛毛雨';
      if (code >= 61 && code <= 67) return '雨';
      if (code >= 71 && code <= 77) return '雪';
      if (code >= 80 && code <= 82) return '陣雨';
      if (code >= 95) return '雷雨';
      return '陰 / 不穩定';
    }

    /* -------- 富士山能見度 Slider -------- */
    function initFujiVisibilitySlider() {
      const slider = document.getElementById('fujiLevel');
      const label  = document.getElementById('fujiText');
      if (!slider || !label) return;
      const map = {
        1: '1 / 5：幾乎看不到，建議改排室內行程',
        2: '2 / 5：能見度差，只看得到模糊輪廓',
        3: '3 / 5：普通，肉眼可見，拍照 OK',
        4: '4 / 5：清晰，很適合拍照、散步',
        5: '5 / 5：超清晰，一定要多拍幾張！'
      };
      function update() {
        const v = Number(slider.value) || 3;
        label.textContent = map[v];
      }
      slider.addEventListener('input', update);
      update();
    }

    /* -------- 富士山能見度預報（本地表格示意） -------- */
    const fujiForecastData = [
      { day: '第1天', time: '早上', level: '★★★★☆', note: '大致晴朗，可清楚看見富士山' },
      { day: '第1天', time: '下午', level: '★★★☆☆', note: '稍有雲霧，仍可拍照' },
      { day: '第1天', time: '傍晚', level: '★★☆☆☆', note: '雲層偏多，拍夕陽需碰碰運氣' },
      { day: '第2天', time: '早上', level: '★★★★★', note: '能見度極佳，非常適合拍照' },
      { day: '第2天', time: '下午', level: '★★★★☆', note: '天氣穩定，視野良好' },
      { day: '第2天', time: '傍晚', level: '★★★☆☆', note: '稍有雲霧，但仍看得到山形' },
      { day: '第3天', time: '早上', level: '★★☆☆☆', note: '雲霧偏多，山形較不清楚' },
      { day: '第3天', time: '下午', level: '★☆☆☆☆', note: '多雲或有雨，幾乎看不到' },
      { day: '第3天', time: '傍晚', level: '★★☆☆☆', note: '天氣略有好轉，視野略佳' }
    ];

    function renderFujiForecastTable() {
      const el = document.getElementById('fujiTable');
      if (!el) return;
      let html = '<div class="overflow-x-auto"><table class="min-w-full text-sm text-left">';
      html += '<thead><tr class="border-b"><th class="py-1 pr-4">日別</th><th class="py-1 pr-4">時段</th><th class="py-1 pr-4">能見度</th><th class="py-1 pr-4">說明</th></tr></thead><tbody>';
      fujiForecastData.forEach(row => {
        html += `<tr class="border-b last:border-0">
          <td class="py-1 pr-4">${row.day}</td>
          <td class="py-1 pr-4">${row.time}</td>
          <td class="py-1 pr-4">${row.level}</td>
          <td class="py-1 pr-4">${row.note}</td>
        </tr>`;
      });
      html += '</tbody></table></div>';
      el.innerHTML = html;
    }

    /* -------- 常用日語資料 -------- */
    const phrases = {
      restaurant: [
        ['すみません、予約しています。','不好意思，我有訂位。'],
        ['おすすめは何ですか。','請問有推薦的料理嗎？'],
        ['これと同じものをもう一つください。','這個再來一份。'],
        ['別々に会計できますか。','可以分開結帳嗎？'],
        ['お水をお願いします。','麻煩給我水。']
      ],
      transport: [
        ['○○駅へはどう行きますか。','請問要怎麼去○○站？'],
        ['この電車は○○駅に止まりますか。','這班電車有停靠○○站嗎？'],
        ['一日乗車券はありますか。','有一日券嗎？'],
        ['ICカードはどこで買えますか。','哪裡可以買 IC 卡（Suica 等）？']
      ],
      emergency: [
        ['助けてください。','請幫幫我。'],
        ['具合が悪いです。','我身體不太舒服。'],
        ['警察を呼んでください。','請幫我叫警察。'],
        ['救急車を呼んでください。','請幫我叫救護車。']
      ],
      shopping: [
        ['これはいくらですか。','這個多少錢？'],
        ['免税できますか。','可以免稅嗎？'],
        ['サイズ違いはありますか。','有不同尺寸嗎？'],
        ['試着してもいいですか。','可以試穿嗎？']
      ],
      basic: [
        ['こんにちは。','你好（白天的問候）。'],
        ['おはようございます。','早安。'],
        ['こんばんは。','晚安（打招呼）。'],
        ['ありがとうございます。','非常感謝。'],
        ['すみません。','不好意思 / 抱歉。']
      ]
    };

    function phraseButtons(list) {
      return list.map(([jp, zh]) => `
        <button
          class="border border-slate-200 rounded-lg px-3 py-2 text-left hover:bg-slate-50"
          onclick="copyPhrase('${jp}')"
        >
          <div class="font-semibold text-sky-600 mb-1 text-lg">${jp}</div>
          <div class="text-sm text-slate-600">${zh}</div>
        </button>
      `).join('');
    }

    function copyPhrase(text) {
      if (!navigator.clipboard) {
        alert(text);
        return;
      }
      navigator.clipboard.writeText(text).then(() => {
        alert('已複製：' + text);
      });
    }

    /* -------- 首頁渲染 -------- */
    function renderHome() {
      const main = document.getElementById('main');
      const totalBudget = getItineraryTotalBudget();
      main.innerHTML = `
        <section class="space-y-4">
          <div class="grid md:grid-cols-2 gap-4">
            <!-- 匯率試算 -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">匯率試算（手動輸入匯率）</h2>
              <div class="mb-2">
                <label class="text-sm font-semibold">今天匯率：1 日圓 = 幾元台幣？</label>
                <div class="flex items-center gap-2 mt-1">
                  <span class="text-sm">1 JPY =</span>
                  <input
                    id="fxRateManual"
                    type="number"
                    step="0.0001"
                    value="${manualFxRate}"
                    oninput="setManualRate()"
                    class="w-28 px-2 py-1 border border-slate-300 rounded-lg text-sm"
                  />
                  <span class="text-sm">TWD</span>
                </div>
                <p class="text-xs text-slate-500 mt-1">
                  ※ 請依照你當天實際刷卡匯率或銀行牌告自行輸入。
                </p>
              </div>
              <div class="border-t border-slate-200 pt-2 mt-2">
                <p class="text-sm font-semibold mb-1">日幣 → 台幣：</p>
                <div class="flex items-center gap-2 mb-1">
                  <input
                    id="fxInputJPY"
                    type="number"
                    inputmode="numeric"
                    oninput="updateFxCalc()"
                    placeholder="例如：5000（JPY）"
                    class="flex-1 px-3 py-2 border border-slate-300 rounded-lg text-base"
                  />
                </div>
                <p class="text-sm">
                  估算結果：<span id="fxOutputTWD" class="font-bold text-emerald-600">--</span>
                </p>
              </div>
              <div class="border-t border-slate-200 pt-2 mt-2">
                <p class="text-sm font-semibold mb-1">台幣 → 日幣：</p>
                <div class="flex items-center gap-2 mb-1">
                  <input
                    id="fxInputTWD"
                    type="number"
                    inputmode="numeric"
                    oninput="updateFxCalcReverse()"
                    placeholder="例如：3000（TWD）"
                    class="flex-1 px-3 py-2 border border-slate-300 rounded-lg text-base"
                  />
                </div>
                <p class="text-sm">
                  估算結果：<span id="fxOutputJPY" class="font-bold text-sky-600">--</span>
                </p>
              </div>
            </div>

            <!-- 東京即時天氣 + 一週預報 -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">東京天氣（即時 + 一週預報）</h2>
              <p id="tokyoWeatherCurrent" class="text-base text-slate-800 mb-2">讀取中…</p>
              <div id="tokyoWeatherWeek" class="text-sm text-slate-800 mb-2"></div>
              <p class="text-xs text-slate-500">
                資料來源：Open-Meteo 氣象 API（日本當地時間）。
              </p>
            </div>

            <!-- 富士山直播縮圖 + Slider -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">富士山直播縮圖 + 今日能見度</h2>
              <a href="https://fuji-san.info/zh-tw/livecamera.html" target="_blank" class="block mb-3">
                <img
                  src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Mount_Fuji_from_Hotel_Mt_Fuji_1995-3-14.jpg/640px-Mount_Fuji_from_Hotel_Mt_Fuji_1995-3-14.jpg"
                  alt="富士山示意圖（點擊開啟直播頁面）"
                  class="w-full h-40 object-cover rounded-lg border border-slate-200"
                />
              </a>
              <label class="text-sm text-slate-700">看完直播畫面後，請自己評估今日能見度：</label>
              <input id="fujiLevel" type="range" min="1" max="5" value="3" class="w-full mt-2" />
              <p id="fujiText" class="text-sm text-slate-700 mt-1"></p>
              <p class="text-xs text-slate-500 mt-1">
                ※ 圖片為示意圖，實際畫面請點擊上方連結開啟官方直播。
              </p>
            </div>

            <!-- 緊急電話 & OHDr 中文 LINE -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">緊急電話 & 線上醫療</h2>
              <ul class="text-sm space-y-1 mb-2">
                <li><span class="font-semibold">110</span>：警察（報案、走失等）</li>
                <li><span class="font-semibold">119</span>：救護車 / 火警</li>
                <li><span class="font-semibold">台灣駐日代表處：</span>+81-3-3280-7811</li>
                <li><span class="font-semibold">旅遊保險緊急專線：</span>建議自行填入保險公司電話</li>
              </ul>
              <div class="mt-2 border-t border-slate-200 pt-2">
                <p class="text-sm font-semibold mb-1">OHDr. 中文線上門診（LINE 官方帳號）</p>
                <a
                  href="https://line.me/R/ti/p/@406vicce"
                  target="_blank"
                  class="inline-flex items-center px-3 py-2 rounded-lg bg-[#06C755] text-white text-sm font-bold"
                >
                  加入 OHDr. LINE 官方帳號（繁中）
                </a>
                <p class="text-xs text-slate-500 mt-1">
                  手機點擊後會直接開啟 LINE，加為好友即可線上諮詢、預約看診。
                </p>
              </div>
            </div>
          </div>

          <!-- 富士山能見度預報（本地表格） -->
          <div class="bg-white rounded-xl shadow p-4">
            <h2 class="text-xl font-bold mb-2">富士山能見度預報（簡化表格）</h2>
            <p class="text-sm text-slate-700 mb-2">
              下表為依照 <a href="https://fuji-san.info/zh-tw/index.html" target="_blank" class="text-sky-600 underline">fuji-san.info</a>
              的結構整理之示意表，可左右滑動。最新官方預報請點上方連結查看原網站。
            </p>
            <div id="fujiTable"></div>
          </div>

          <!-- 行程預算總額 -->
          <div class="bg-white rounded-xl shadow p-4">
            <h2 class="text-xl font-bold mb-2">行程預算總額</h2>
            <p class="text-base text-slate-800">
              目前在「行程」頁面輸入的預算合計：約
              <span class="font-bold text-rose-600">NT$ ${Math.round(totalBudget).toLocaleString()}</span>
            </p>
            <p class="text-xs text-slate-500 mt-1">
              ※ 本金額為你在各行程項目「預算（TWD）」欄位輸入的加總，可自行調整。
            </p>
          </div>

          <!-- 常用日語（分類） -->
          <div class="bg-white rounded-xl shadow p-4 space-y-4">
            <h2 class="text-xl font-bold mb-2">常用日語（點一下即可複製）</h2>

            <div>
              <h3 class="text-lg font-semibold mb-2">🍽 餐廳用語</h3>
              <div class="grid md:grid-cols-3 gap-3 text-base">
                ${phraseButtons(phrases.restaurant)}
              </div>
            </div>

            <div>
              <h3 class="text-lg font-semibold mb-2">🚉 交通用語</h3>
              <div class="grid md:grid-cols-3 gap-3 text-base">
                ${phraseButtons(phrases.transport)}
              </div>
            </div>

            <div>
              <h3 class="text-lg font-semibold mb-2">⛑ 緊急求助</h3>
              <div class="grid md:grid-cols-3 gap-3 text-base">
                ${phraseButtons(phrases.emergency)}
              </div>
            </div>

            <div>
              <h3 class="text-lg font-semibold mb-2">🛍 購物用語</h3>
              <div class="grid md:grid-cols-3 gap-3 text-base">
                ${phraseButtons(phrases.shopping)}
              </div>
            </div>

            <div>
              <h3 class="text-lg font-semibold mb-2">🙌 基本問候</h3>
              <div class="grid md:grid-cols-3 gap-3 text-base">
                ${phraseButtons(phrases.basic)}
              </div>
            </div>
          </div>
        </section>
      `;
      fetchTokyoWeather();
      initFujiVisibilitySlider();
      renderFujiForecastTable();
      // 匯率區重新套用目前值
      const rateInput = document.getElementById('fxRateManual');
      if (rateInput) rateInput.value = manualFxRate;
      updateFxCalc();
      updateFxCalcReverse();
    }

    /* -------- 行程頁（含備註 + 預算） -------- */
    function renderItinerary() {
      const main = document.getElementById('main');
      main.innerHTML = itineraryData.map((day, dayIndex) => `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-2">${day.date}</h2>
          <div class="space-y-3">
            ${day.items.map((item, itemIndex) => {
              const key = dayIndex + '-' + itemIndex;
              const budget = itineraryBudgets[key] || '';
              return `
                <div class="border-b border-slate-100 pb-3 last:border-0">
                  <div class="flex items-start justify-between gap-2 mb-1">
                    <div class="w-20 text-xs font-mono text-sky-600 pt-1">${item.time}</div>
                    <div class="flex-1 text-base">${item.text}</div>
                    <a
                      href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(item.map)}"
                      target="_blank"
                      class="text-xs text-sky-600 underline flex-shrink-0 mt-1"
                    >
                      導航
                    </a>
                  </div>
                  <div class="mt-1 pl-20 space-y-1">
                    <div>
                      <label class="text-xs text-slate-600">備註：</label>
                      <textarea
                        rows="1"
                        class="w-full px-2 py-1 border border-slate-300 rounded-lg text-sm"
                        placeholder="可填寫交通方式、訂位編號、同行人數等（純文字，不會匯出）"
                      ></textarea>
                    </div>
                    <div class="flex items-center gap-2">
                      <label class="text-xs text-slate-600">預算（TWD）：</label>
                      <input
                        type="number"
                        inputmode="numeric"
                        value="${budget}"
                        data-itkey="${key}"
                        oninput="updateItineraryBudget(this)"
                        class="w-32 px-2 py-1 border border-slate-300 rounded-lg text-sm"
                        placeholder="如：500"
                      />
                    </div>
                  </div>
                </div>
              `;
            }).join('')}
          </div>
        </section>
      `).join('');
    }

    /* -------- 記帳（幣別 + 照片 3 張） -------- */
    let expenses = [];

    function renderAccount() {
      const main = document.getElementById('main');
      const totalTWD = expenses.reduce((s, e) => {
        if (e.currency === 'TWD') return s + (e.amount || 0);
        if (e.currency === 'JPY' && manualFxRate) return s + (e.amount * manualFxRate);
        return s;
      }, 0);
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-3">記帳 & CSV 匯出</h2>
          <div class="grid md:grid-cols-5 gap-2 mb-3 text-sm">
            <input
              id="accDate"
              placeholder="日期 如 12/26"
              class="px-3 py-2 border border-slate-300 rounded-lg"
            />
            <input
              id="accItem"
              placeholder="項目 如 早餐 / 伴手禮"
              class="px-3 py-2 border border-slate-300 rounded-lg"
            />
            <input
              id="accAmount"
              type="number"
              inputmode="numeric"
              placeholder="金額"
              class="px-3 py-2 border border-slate-300 rounded-lg"
            />
            <select
              id="accCurrency"
              class="px-2 py-2 border border-slate-300 rounded-lg"
            >
              <option value="JPY">JPY（日幣）</option>
              <option value="TWD">TWD（台幣）</option>
            </select>
            <input
              id="accPhotos"
              type="file"
              accept="image/*"
              multiple
              class="text-xs"
            />
          </div>
          <div class="mb-3">
            <textarea
              id="accNote"
              rows="2"
              class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm"
              placeholder="備註（例如：在哪一間店買、誰一起吃、發票號碼…）"
            ></textarea>
          </div>
          <div class="flex items-center justify-between text-sm mb-2">
            <span>目前預估總額（換算為 TWD）：<span class="font-bold text-rose-600">NT$ ${Math.round(totalTWD).toLocaleString()}</span></span>
            <div class="flex gap-2">
              <button
                onclick="addExpense()"
                class="px-3 py-2 rounded-lg bg-primary text-white font-semibold"
              >
                新增記帳
              </button>
              <button
                onclick="exportCSV()"
                class="px-3 py-2 rounded-lg border border-sky-500 text-sky-600 hover:bg-sky-50"
              >
                匯出 CSV
              </button>
            </div>
          </div>
          <div id="accList" class="max-h-80 overflow-y-auto text-sm border-t border-slate-100 pt-1">
            ${expenses.length === 0 ? `
              <p class="text-slate-400 text-center py-4">尚未有記帳資料，先新增一筆吧。</p>
            ` :
              expenses.map((e, i) => {
                const amountStr = e.amount.toLocaleString() + ' ' + e.currency;
                let approx = '';
                if (e.currency === 'JPY' && manualFxRate) {
                  const twd = e.amount * manualFxRate;
                  approx = `（約 NT$ ${Math.round(twd).toLocaleString()}）`;
                }
                return `
                  <div class="py-2 border-b border-slate-100 last:border-0">
                    <div class="flex items-center justify-between">
                      <div>
                        <div class="font-semibold">${e.date || '未填日期'}｜${e.item}</div>
                        <div class="text-xs text-slate-600">
                          金額：${amountStr} ${approx}
                        </div>
                        ${e.note ? `<div class="text-xs text-slate-600 mt-1">備註：${e.note}</div>` : ''}
                      </div>
                      <button
                        onclick="deleteExpense(${i})"
                        class="text-rose-600 text-xs px-2"
                      >
                        刪除
                      </button>
                    </div>
                    ${e.photos && e.photos.length ? `
                      <div class="flex gap-2 mt-2 flex-wrap">
                        ${e.photos.map(src => `
                          <img src="${src}" class="w-16 h-16 object-cover rounded border border-slate-200" />
                        `).join('')}
                      </div>
                    ` : ''}
                  </div>
                `;
              }).join('')}
          </div>
          <p class="text-xs text-slate-500 mt-2">
            ※ 照片僅暫存在此頁面瀏覽器記憶體，重新整理或關閉頁面後不會保留，也不會出現在 CSV 檔案中。
          </p>
        </section>
      `;
    }

    function readFileAsDataURL(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => resolve(reader.result);
        reader.onerror = reject;
        reader.readAsDataURL(file);
      });
    }

    async function addExpense() {
      const d = document.getElementById('accDate').value.trim();
      const i = document.getElementById('accItem').value.trim();
      const a = parseFloat(document.getElementById('accAmount').value);
      const c = document.getElementById('accCurrency').value;
      const note = document.getElementById('accNote').value.trim();
      const files = document.getElementById('accPhotos').files;

      if (!i || isNaN(a)) {
        alert('請至少填「項目」與「金額」。');
        return;
      }

      const photos = [];
      const max = Math.min(files.length, 3);
      for (let idx = 0; idx < max; idx++) {
        const file = files[idx];
        try {
          const dataUrl = await readFileAsDataURL(file);
          photos.push(dataUrl);
        } catch (err) {
          console.error(err);
        }
      }

      expenses.push({ date: d, item: i, amount: a, currency: c, note, photos });
      // 清空輸入欄位
      document.getElementById('accDate').value = '';
      document.getElementById('accItem').value = '';
      document.getElementById('accAmount').value = '';
      document.getElementById('accNote').value = '';
      document.getElementById('accPhotos').value = '';
      renderAccount();
    }

    function deleteExpense(idx) {
      expenses.splice(idx, 1);
      renderAccount();
    }

    function exportCSV() {
      if (!expenses.length) {
        alert('尚無記帳資料可匯出');
        return;
      }
      const header = ['date','item','currency','amount','note'];
      const rows = expenses.map(e => [e.date || '', e.item || '', e.currency || '', e.amount || 0, e.note || '']);
      const csv = [header].concat(rows).map(r =>
        r.map(field => {
          const s = String(field);
          if (s.includes(',') || s.includes('"') || s.includes('\n')) {
            return '"' + s.replace(/"/g,'""') + '"';
          }
          return s;
        }).join(',')
      ).join('\n');
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'tokyo-expenses.csv';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }

    /* -------- 旅遊檢查清單 -------- */
    let checklistItems = [
      { text: '護照（有效期 6 個月以上）', done: false },
      { text: '日本入境卡 / 海關申報（可線上預填）', done: false },
      { text: '日幣現金、金融卡、信用卡', done: false },
      { text: '旅遊保險（保單 + 緊急聯絡電話）', done: false },
      { text: '國際漫遊 / eSIM / Wi-Fi 分享器', done: false },
      { text: '常備藥品（感冒藥、止痛藥、腸胃藥等）', done: false },
      { text: '充電器、轉接頭（日本 A 型 110V）', done: false },
      { text: '保暖衣物 / 雨具', done: false }
    ];

    function renderChecklist() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-3">旅遊檢查清單</h2>
          <div class="flex gap-2 mb-3 text-sm">
            <input
              id="checkInput"
              class="flex-1 px-3 py-2 border border-slate-300 rounded-lg"
              placeholder="新增項目，如：確認某餐廳預約、預先買票…"
            />
            <button
              onclick="addChecklistItem()"
              class="px-3 py-2 rounded-lg bg-primary text-white font-semibold"
            >
              新增
            </button>
          </div>
          <div id="checkList" class="max-h-80 overflow-y-auto text-sm">
            ${checklistItems.length === 0 ? `
              <p class="text-slate-400 text-center py-4">目前沒有清單項目。</p>
            ` :
              checklistItems.map((c,i) => `
                <div class="flex items-center justify-between py-2 border-b border-slate-100 last:border-0">
                  <label class="flex items-center gap-2 flex-1 cursor-pointer">
                    <input
                      type="checkbox"
                      ${c.done ? 'checked' : ''}
                      onchange="toggleChecklist(${i})"
                    />
                    <span class="${c.done ? 'line-through text-slate-400' : ''}">${c.text}</span>
                  </label>
                  <button
                    onclick="deleteChecklist(${i})"
                    class="text-rose-600 text-xs px-2"
                  >
                    刪除
                  </button>
                </div>
              `).join('')}
          </div>
        </section>
      `;
    }

    function addChecklistItem() {
      const input = document.getElementById('checkInput');
      const v = input.value.trim();
      if (!v) {
        alert('請輸入要新增的檢查項目');
        return;
      }
      checklistItems.push({ text: v, done: false });
      input.value = '';
      renderChecklist();
    }

    function toggleChecklist(idx) {
      checklistItems[idx].done = !checklistItems[idx].done;
      renderChecklist();
    }

    function deleteChecklist(idx) {
      checklistItems.splice(idx, 1);
      renderChecklist();
    }

    /* -------- 飯店資訊 -------- */
    function renderHotel() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4 text-base">
          <h2 class="text-xl font-bold mb-3">飯店資訊</h2>
          <div class="space-y-3">
            <div>
              <div class="font-semibold">飯店名稱</div>
              <div>上野站前1號遊客酒店</div>
            </div>
            <div>
              <div class="font-semibold">飯店地址</div>
              <div>東京都, 東京, Taito-ku Higashiueno 2-18-18, 日本</div>
            </div>
            <div>
              <div class="font-semibold">入住 / 退房時間（範例，可依實際訂房確認）</div>
              <div>Check-in：15:00 之後</div>
              <div>Check-out：11:00 之前</div>
            </div>
            <div>
              <div class="font-semibold">交通方式備註（可自行補充）</div>
              <ul class="list-disc ml-5 text-sm">
                <li>成田機場 → 乘坐 Skyliner 至上野站，步行前往飯店</li>
                <li>飯店附近車站：上野站、稻荷町站等（請依實際確認）</li>
              </ul>
            </div>
            <div>
              <a
                href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent('上野站前1號遊客酒店 東京都 台東區 東上野 2-18-18')}"
                target="_blank"
                class="inline-block mt-2 px-4 py-2 rounded-lg bg-primary text-white text-sm"
              >
                在 Google Maps 開啟飯店位置
              </a>
            </div>
            <div class="pt-3 border-t border-slate-200 mt-2">
              <div class="font-semibold">入住注意事項（可自行修改）</div>
              <ul class="list-disc ml-5 text-sm">
                <li>確認是否可提前寄放行李</li>
                <li>確認是否需加收住宿稅、清潔費</li>
                <li>確認是否有夜間門禁或門卡進出限制</li>
              </ul>
            </div>
          </div>
        </section>
      `;
    }

    /* -------- 購物清單 -------- */
    let shoppingItems = [];

    function renderShopping() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-3">購物清單</h2>
          <div class="flex gap-2 mb-3 text-sm">
            <input
              id="shopInput"
              class="flex-1 px-3 py-2 border border-slate-300 rounded-lg"
              placeholder="新增項目，如：無印良品收納盒、藥妝、伴手禮…"
            />
            <button
              onclick="addShoppingItem()"
              class="px-3 py-2 rounded-lg bg-primary text-white font-semibold"
            >
              新增
            </button>
          </div>
          <div id="shopList" class="max-h-80 overflow-y-auto text-sm">
            ${shoppingItems.length === 0 ? `
              <p class="text-slate-400 text-center py-4">購物清單目前是空的。</p>
            ` :
              shoppingItems.map((s,i) => `
                <div class="flex items-center justify-between py-2 border-b border-slate-100 last:border-0">
                  <span
                    class="${s.done ? 'line-through text-slate-400' : ''}"
                    onclick="toggleShopping(${i})"
                  >
                    ${s.text}
                  </span>
                  <button
                    onclick="deleteShopping(${i})"
                    class="text-rose-600 text-xs px-2"
                  >
                    刪除
                  </button>
                </div>
              `).join('')}
          </div>
        </section>
      `;
    }

    function addShoppingItem() {
      const input = document.getElementById('shopInput');
      const v = input.value.trim();
      if (!v) {
        alert('請輸入購物項目');
        return;
      }
      shoppingItems.push({ text: v, done: false });
      input.value = '';
      renderShopping();
    }

    function toggleShopping(idx) {
      shoppingItems[idx].done = !shoppingItems[idx].done;
      renderShopping();
    }

    function deleteShopping(idx) {
      shoppingItems.splice(idx, 1);
      renderShopping();
    }

    /* -------- 初始化 -------- */
    document.addEventListener('DOMContentLoaded', () => {
      attachSwipe();
      setTabByIndex(0);
    });
  </script>
</body>
</html>
