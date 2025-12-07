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
    body { font-family: system-ui, -apple-system, BlinkMacSystemFont, "Noto Sans TC", sans-serif; }
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
        <h1 class="text-2xl font-bold">東京旅遊助理</h1>
        <p class="text-xs text-slate-500">行程 / 匯率 / 天氣 / 富士山 / 記帳 / 清單 一次搞定</p>
      </div>
    </header>

    <!-- Tabs -->
    <nav class="w-full max-w-4xl mx-auto px-3 mt-1">
      <div class="flex bg-white rounded-full shadow text-xs font-semibold overflow-hidden">
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
      <div class="max-w-4xl mx-auto px-4 py-2 flex justify-between text-[11px] text-slate-500">
        <span>⬅️ 右滑 / 左滑切換頁面</span>
        <span>這是單一 HTML 檔，適合放在 GitHub</span>
      </div>
    </footer>
  </div>

  <script>
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
      });
    }

    const itineraryData = [
      {
        date: '12/26（五）上野',
        items: [
          { time: '14:20', text: '抵達成田機場 (NRT) T2', map: 'Narita Airport Terminal 2' },
          { time: '16:00', text: '飯店 Check-in', map: '上野 飯店' },
          { time: '18:00', text: '晚餐：阿美橫丁', map: '阿美橫丁' },
          { time: '20:00', text: '購物：無印良品上野丸井店、OS Drug 藥妝店', map: '無印良品 上野丸井' }
        ]
      },
      {
        date: '12/27（六）東京、銀座',
        items: [
          { time: '09:00', text: 'Tricolore coffee（早餐：蘋果派、閃電泡芙）', map: 'トリコロール 本店 銀座' },
          { time: '12:00', text: '牛たんの檸檬 有楽町店', map: '牛たんの檸檬 有楽町店' },
          { time: '15:00', text: 'MARLOWE 焦糖布丁', map: 'Marlowe pudding' },
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
          { time: '11:30', text: '壽喜燒：Sukiyaki Juni Ten', map: 'Sukiyaki Juni Ten' },
          { time: '14:30', text: '東急 Plaza 表參道原宿', map: '東急プラザ 表参道原宿' },
          { time: '19:30', text: '中目黑', map: '中目黒駅' }
        ]
      },
      {
        date: '12/30（二）新宿、秋葉原',
        items: [
          { time: '11:00', text: '新宿：NEWoMan TAKANAWA百貨', map: 'NEWoMan 新宿' },
          { time: '18:30', text: '二木菓子（買伴手禮）', map: '二木の菓子 秋葉原' }
        ]
      },
      {
        date: '12/31（三）成田市',
        items: [
          { time: '09:30', text: '成田山新勝寺', map: '成田山新勝寺' },
          { time: '10:30', text: '成田山表參道', map: '成田 山 表参道' },
          { time: '11:30', text: '成田夢牧場 門前店', map: '成田ゆめ牧場 門前店' },
          { time: '12:30', text: '成田機場 (NRT)', map: 'Narita Airport' }
        ]
      }
    ];

    async function fetchFxRate() {
      const el = document.getElementById('fxRate');
      const srcEl = document.getElementById('fxSource');
      if (!el) return;
      try {
        const res = await fetch('https://api.exchangerate.host/latest?base=TWD&symbols=JPY');
        const data = await res.json();
        if (data && data.rates && data.rates.JPY) {
          el.textContent = '1 TWD ≈ ' + data.rates.JPY.toFixed(2) + ' JPY';
          if (srcEl) srcEl.textContent = '來源：exchangerate.host 公開匯率 API';
        } else {
          el.textContent = '無法取得匯率';
        }
      } catch (err) {
        el.textContent = '匯率讀取錯誤';
      }
    }

    async function fetchTokyoWeather() {
      const el = document.getElementById('tokyoWeather');
      if (!el) return;
      try {
        const url = 'https://api.open-meteo.com/v1/jma?latitude=35.6895&longitude=139.6917&current=temperature_2m,weather_code';
        const res = await fetch(url);
        const data = await res.json();
        if (data && data.current) {
          const t = data.current.temperature_2m;
          const code = data.current.weather_code;
          let desc = '多雲';
          if (code === 0) desc = '晴朗';
          else if (code < 3) desc = '大致晴朗';
          else if (code < 60) desc = '陰 / 雲多';
          else desc = '可能有降雨';
          el.textContent = desc + '，約 ' + t + '°C（JMA 模式預報）';
        } else {
          el.textContent = '無法取得氣象資料';
        }
      } catch (err) {
        el.textContent = '氣象讀取錯誤';
      }
    }

    function initFujiVisibility() {
      const slider = document.getElementById('fujiLevel');
      const label = document.getElementById('fujiText');
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

    function renderHome() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="space-y-4">
          <div class="grid md:grid-cols-2 gap-4">
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-lg font-bold mb-1">即時匯率（TWD → JPY）</h2>
              <p id="fxRate" class="text-xl font-semibold text-sky-500 mb-1">讀取中…</p>
              <p id="fxSource" class="text-[11px] text-slate-500 mb-1"></p>
              <p class="text-[11px] text-slate-500">
                日本免稅：同一店家合計未稅滿 5,000 日圓以上可免稅（以店家實際公告為準）。
              </p>
            </div>

            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-lg font-bold mb-1">東京即時天氣</h2>
              <p id="tokyoWeather" class="text-sm text-slate-700">讀取中…</p>
              <p class="text-[11px] text-slate-500 mt-1">
                資料來源：Open-Meteo JMA API（使用日本氣象廳預報模型）。
              </p>
            </div>

            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-lg font-bold mb-1">富士山直播縮圖 + 能見度</h2>
              <a href="https://live.fujigoko.tv/?e=1&n=3" target="_blank" class="block mb-2">
                <img src="https://cam.fujigoko.tv/livecam3/cam1_8726.jpg" alt="富士山直播縮圖（示意）" class="w-full h-40 object-cover rounded-lg border border-slate-200" />
              </a>
              <label class="text-xs text-slate-500">請根據直播畫面自行評估當日能見度：</label>
              <input id="fujiLevel" type="range" min="1" max="5" value="3" class="w-full mt-1" />
              <p id="fujiText" class="text-[11px] text-slate-600 mt-1"></p>
              <p class="text-[11px] text-slate-500 mt-1">
                ※ 若縮圖連結失效，可自行替換 img 的 src 為你喜歡的富士山即時圖片網址。
              </p>
            </div>

            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-lg font-bold mb-1">緊急電話 & 線上醫療</h2>
              <ul class="text-xs space-y-1">
                <li><span class="font-semibold">110</span>：警察（報案、走失等）</li>
                <li><span class="font-semibold">119</span>：救護車 / 火警</li>
                <li><span class="font-semibold">台灣駐日代表處：</span>+81-3-3280-7811</li>
                <li><span class="font-semibold">旅遊保險緊急專線：</span>建議自行填入保險公司電話</li>
                <li><span class="font-semibold">OHDr. 中文線上門診：</span>
                  <a href="https://ohdr.co" target="_blank" class="text-sky-500 underline">開啟網站</a>
                </li>
              </ul>
            </div>
          </div>

          <div class="bg-white rounded-xl shadow p-4">
            <h2 class="text-lg font-bold mb-2">常用日語（點一下即可複製）</h2>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-3 text-sm">
              ${[
                ['すみません','不好意思 / 麻煩你'],
                ['ありがとうございます','非常感謝'],
                ['いくらですか','多少錢？'],
                ['これください','我要這個'],
                ['トイレはどこですか','請問廁所在哪裡？'],
                ['お会計お願いします','我要結帳']
              ].map(([jp, zh]) => `
                <button class="border border-slate-200 rounded-lg px-3 py-2 text-left hover:bg-slate-50"
                        onclick="copyPhrase('${jp}')">
                  <div class="font-semibold text-sky-500 mb-1">${jp}</div>
                  <div class="text-xs text-slate-500">${zh}</div>
                </button>
              `).join('')}
            </div>
          </div>
        </section>
      `;
      fetchFxRate();
      fetchTokyoWeather();
      initFujiVisibility();
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

    function renderItinerary() {
      const main = document.getElementById('main');
      main.innerHTML = itineraryData.map(day => `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-lg font-bold mb-2">${day.date}</h2>
          <div class="space-y-2">
            ${day.items.map(item => `
              <div class="flex items-start justify-between gap-2 border-b border-slate-100 py-2 last:border-0">
                <div class="w-14 text-[11px] font-mono text-sky-500 pt-1">${item.time}</div>
                <div class="flex-1 text-sm">${item.text}</div>
                <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(item.map)}"
                   target="_blank"
                   class="text-[11px] text-sky-500 underline flex-shrink-0 mt-1">
                  導航
                </a>
              </div>
            `).join('')}
          </div>
        </section>
      `).join('');
    }

    let expenses = [];

    function renderAccount() {
      const main = document.getElementById('main');
      const total = expenses.reduce((s,e) => s + (e.amount || 0), 0);
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-lg font-bold mb-3">記帳 & CSV 匯出</h2>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-2 mb-3 text-xs">
            <input id="accDate" placeholder="日期 如 12/26"
                   class="px-2 py-1 border border-slate-300 rounded" />
            <input id="accItem" placeholder="項目 如 早餐 / 伴手禮"
                   class="px-2 py-1 border border-slate-300 rounded" />
            <input id="accAmount" type="number" placeholder="金額"
                   class="px-2 py-1 border border-slate-300 rounded" />
            <button onclick="addExpense()" class="px-3 py-1 rounded bg-primary text-white font-semibold">新增</button>
          </div>
          <div class="flex items-center justify-between text-xs mb-2">
            <span>目前總額：<span class="font-bold text-red-500">${total.toLocaleString()}</span></span>
            <button onclick="exportCSV()" class="px-3 py-1 rounded border border-sky-500 text-sky-500 hover:bg-sky-50">
              匯出 CSV
            </button>
          </div>
          <div id="accList" class="max-h-72 overflow-y-auto text-xs border-t border-slate-100 pt-1">
            ${expenses.length === 0 ? `<p class="text-slate-400 text-center py-4">尚未有記帳資料，先新增一筆吧。</p>` :
              expenses.map((e,i) => `
                <div class="flex items-center justify-between py-1 border-b border-slate-100 last:border-0">
                  <div>
                    <div class="font-semibold">${e.date || '未填日期'}｜${e.item}</div>
                    <div class="text-[11px] text-slate-500">${e.amount.toLocaleString()}</div>
                  </div>
                  <button onclick="deleteExpense(${i})" class="text-red-500 text-[11px] px-2">刪除</button>
                </div>
              `).join('')}
          </div>
        </section>
      `;
    }

    function addExpense() {
      const d = document.getElementById('accDate').value.trim();
      const i = document.getElementById('accItem').value.trim();
      const a = parseFloat(document.getElementById('accAmount').value);
      if (!i || isNaN(a)) {
        alert('請至少填「項目」與「金額」');
        return;
      }
      expenses.push({ date: d, item: i, amount: a });
      renderAccount();
    }

    function deleteExpense(idx) {
      expenses.splice(idx,1);
      renderAccount();
    }

    function exportCSV() {
      if (!expenses.length) {
        alert('尚無記帳資料可匯出');
        return;
      }
      const header = ['date','item','amount'];
      const rows = expenses.map(e => [e.date || '', e.item || '', e.amount || 0]);
      const csv = [header].concat(rows).map(r => r.map(field => {
        const s = String(field);
        if (s.includes(',') || s.includes('"') || s.includes('\n')) {
          return '"' + s.replace(/"/g,'""') + '"';
        }
        return s;
      }).join(',')).join('\n');
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
          <h2 class="text-lg font-bold mb-3">旅遊檢查清單</h2>
          <div class="flex gap-2 mb-3 text-xs">
            <input id="checkInput" class="flex-1 px-2 py-1 border border-slate-300 rounded"
                   placeholder="新增項目，如：行前確認某間餐廳預約…" />
            <button onclick="addChecklistItem()" class="px-3 py-1 rounded bg-primary text-white font-semibold">新增</button>
          </div>
          <div id="checkList" class="max-h-80 overflow-y-auto text-xs">
            ${checklistItems.length === 0 ? `<p class="text-slate-400 text-center py-4">目前沒有清單項目。</p>` :
              checklistItems.map((c,i) => `
                <div class="flex items-center justify-between py-1 border-b border-slate-100 last:border-0">
                  <label class="flex items-center gap-2 flex-1 cursor-pointer">
                    <input type="checkbox" ${c.done ? 'checked' : ''} onchange="toggleChecklist(${i})" />
                    <span class="${c.done ? 'line-through text-slate-400' : ''}">${c.text}</span>
                  </label>
                  <button onclick="deleteChecklist(${i})" class="text-red-500 text-[11px] px-2">刪除</button>
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
      checklistItems.splice(idx,1);
      renderChecklist();
    }

    function renderHotel() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4 text-sm">
          <h2 class="text-lg font-bold mb-3">飯店資訊</h2>
          <div class="space-y-2">
            <div>
              <div class="font-semibold">飯店名稱</div>
              <div>（請自行填入：例如 XXX Hotel Ueno）</div>
            </div>
            <div>
              <div class="font-semibold">入住 / 退房時間</div>
              <div>Check-in：15:00 之後（依飯店規定）</div>
              <div>Check-out：11:00 之前（依飯店規定）</div>
            </div>
            <div>
              <div class="font-semibold">地址</div>
              <div>（請貼上飯店完整地址）</div>
            </div>
            <div>
              <div class="font-semibold">交通方式備註</div>
              <ul class="list-disc ml-5 text-xs">
                <li>從成田機場 → 搭乘 Skyliner / N&apos;EX 至上野 / 東京，再轉地鐵</li>
                <li>最近車站：請填入最近的 JR / 地鐵站</li>
              </ul>
            </div>
            <div>
              <a href="https://www.google.com/maps" target="_blank" class="inline-block mt-2 px-3 py-1 rounded bg-primary text-white text-xs">
                在 Google Maps 搜尋飯店
              </a>
            </div>
            <div class="pt-2 border-t border-slate-100 mt-2">
              <div class="font-semibold">入住注意事項（可自行修改）</div>
              <ul class="list-disc ml-5 text-xs">
                <li>確認是否可提前寄放行李</li>
                <li>確認是否需加收住宿稅、清潔費</li>
                <li>確認是否有 24 小時櫃台 / 夜間門禁</li>
              </ul>
            </div>
          </div>
        </section>
      `;
    }

    let shoppingItems = [];

    function renderShopping() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-lg font-bold mb-3">購物清單</h2>
          <div class="flex gap-2 mb-3 text-xs">
            <input id="shopInput" class="flex-1 px-2 py-1 border border-slate-300 rounded"
                   placeholder="新增項目，如：無印良品收納盒 / 藥妝 / 伴手禮…" />
            <button onclick="addShoppingItem()" class="px-3 py-1 rounded bg-primary text-white font-semibold">新增</button>
          </div>
          <div id="shopList" class="max-h-80 overflow-y-auto text-xs">
            ${shoppingItems.length === 0 ? `<p class="text-slate-400 text-center py-4">購物清單目前是空的。</p>` :
              shoppingItems.map((s,i) => `
                <div class="flex items-center justify-between py-1 border-b border-slate-100 last:border-0">
                  <span class="${s.done ? 'line-through text-slate-400' : ''}" onclick="toggleShopping(${i})">${s.text}</span>
                  <button onclick="deleteShopping(${i})" class="text-red-500 text-[11px] px-2">刪除</button>
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
      shoppingItems.splice(idx,1);
      renderShopping();
    }

    document.addEventListener('DOMContentLoaded', () => {
      attachSwipe();
      setTabByIndex(0);
    });
  </script>
</body>
</html>
