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
      font-size: 18px;          /* 整體字體放大 */
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
          行程・匯率・天氣・富士山・記帳・檢查清單・購物清單，一次搞定
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
        <span>單一 HTML 檔，適合放到 GitHub Pages</span>
      </div>
    </footer>
  </div>

  <script>
    /* ----------- Tabs & Swipe ----------- */
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

    /* ----------- Data: 行程 ----------- */
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

    /* ----------- 匯率 & 天氣 & 富士山 ----------- */
    let fxJPYtoTWD = null; // 1 JPY -> TWD

    async function fetchFxRate() {
      const rateEl = document.getElementById('fxRate');
      const srcEl  = document.getElementById('fxSource');
      if (!rateEl) return;
      try {
        // 以 JPY 為基準，抓 TWD 匯率（1 日圓等於幾元台幣）
        const res = await fetch('https://api.exchangerate.host/latest?base=JPY&symbols=TWD');
        const data = await res.json();
        if (data && data.rates && data.rates.TWD) {
          fxJPYtoTWD = data.rates.TWD;
          const jpyToTwd = fxJPYtoTWD.toFixed(4);
          const twdToJpy = (1 / fxJPYtoTWD).toFixed(2);
          rateEl.textContent = `1 JPY ≈ ${jpyToTwd} TWD（約 1 TWD ≈ ${twdToJpy} JPY）`;
          if (srcEl) srcEl.textContent = '來源：exchangerate.host 公開匯率 API（可能受網路或 API 限制影響）';
          updateFxCalc();
        } else {
          rateEl.textContent = '無法取得匯率（可能是 API 回應異常）';
        }
      } catch (err) {
        console.error(err);
        rateEl.textContent = '匯率讀取錯誤（可能是網路或 API 限制）';
      }
    }

    function updateFxCalc() {
      const input = document.getElementById('fxInputJPY');
      const out   = document.getElementById('fxOutputTWD');
      if (!input || !out) return;
      const val = parseFloat(input.value);
      if (isNaN(val) || !fxJPYtoTWD) {
        out.textContent = '--';
        return;
      }
      const twd = val * fxJPYtoTWD;
      out.textContent = '約 NT$ ' + twd.toFixed(0).toLocaleString();
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
          el.textContent = `${desc}，約 ${t}°C（JMA 模式預報）`;
        } else {
          el.textContent = '無法取得氣象資料';
        }
      } catch (err) {
        console.error(err);
        el.textContent = '氣象讀取錯誤（可能是網路或 API 限制）';
      }
    }

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

    /* ----------- 首頁渲染 ----------- */
    function renderHome() {
      const main = document.getElementById('main');
      main.innerHTML = `
        <section class="space-y-4">
          <div class="grid md:grid-cols-2 gap-4">
            <!-- 匯率 + 試算器 -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">即時匯率（TWD ↔ JPY）</h2>
              <p id="fxRate" class="text-lg font-semibold text-sky-600 mb-1">讀取中…</p>
              <p id="fxSource" class="text-xs text-slate-600 mb-3"></p>
              <div class="border-t border-slate-200 pt-2 mt-1">
                <p class="text-sm font-semibold mb-1">匯率試算（輸入日幣，換算為台幣）：</p>
                <div class="flex items-center gap-2 mb-1">
                  <input
                    id="fxInputJPY"
                    type="number"
                    inputmode="numeric"
                    oninput="updateFxCalc()"
                    placeholder="例如：5000（日圓）"
                    class="flex-1 px-3 py-2 border border-slate-300 rounded-lg text-base"
                  />
                </div>
                <p class="text-sm">
                  估算結果：<span id="fxOutputTWD" class="font-bold text-emerald-600">--</span>
                </p>
                <p class="text-xs text-slate-500 mt-1">
                  ※ 僅為即時匯率估算，實際刷卡匯率與手續費依銀行/發卡機構為準。
                </p>
              </div>
              <p class="text-xs text-slate-500 mt-2">
                日本一般免稅：同一店家合計「未稅」滿 5,000 日圓以上可免稅（以店家實際公告為準）。
              </p>
            </div>

            <!-- 東京天氣 -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">東京即時天氣</h2>
              <p id="tokyoWeather" class="text-base text-slate-800 mb-1">讀取中…</p>
              <p class="text-xs text-slate-500 mt-1">
                資料來源：Open-Meteo JMA API（使用日本氣象廳預報模型）。
              </p>
            </div>

            <!-- 富士山直播 + 手動能見度 -->
            <div class="bg-white rounded-xl shadow p-4">
              <h2 class="text-xl font-bold mb-2">富士山直播縮圖 + 今日能見度</h2>
              <a href="https://live.fujigoko.tv/?e=1&n=3" target="_blank" class="block mb-3">
                <img
                  src="https://cam.fujigoko.tv/livecam3/cam1_8726.jpg"
                  alt="富士山直播縮圖（若載入失敗，可點擊開啟直播網站）"
                  class="w-full h-40 object-cover rounded-lg border border-slate-200"
                />
              </a>
              <label class="text-sm text-slate-700">請看直播畫面後，自己評估今日能見度：</label>
              <input id="fujiLevel" type="range" min="1" max="5" value="3" class="w-full mt-2" />
              <p id="fujiText" class="text-sm text-slate-700 mt-1"></p>
              <p class="text-xs text-slate-500 mt-1">
                ※ 若縮圖連結失效，可自行替換 <code>img</code> 的 <code>src</code> 為你喜歡的富士山即時圖片網址。
              </p>
            </div>

            <!-- 緊急電話 & OHDr LINE -->
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
                  href="https://lin.ee/aJ65BGe"
                  target="_blank"
                  class="inline-flex items-center px-3 py-2 rounded-lg bg-[#06C755] text-white text-sm font-bold"
                >
                  加入 OHDr. LINE 官方帳號
                </a>
                <p class="text-xs text-slate-500 mt-1">
                  手機點擊後會直接開啟 LINE，加為好友即可線上諮詢、預約看診。
                </p>
              </div>
            </div>
          </div>

          <!-- 富士山能見度預報表格（fuji-san.info） -->
          <div class="bg-white rounded-xl shadow p-4">
            <h2 class="text-xl font-bold mb-2">富士山能見度預報表（fuji-san.info）</h2>
            <p class="text-sm text-slate-700 mb-2">
              下方為 <a href="https://fuji-san.info/zh-tw/index.html" target="_blank" class="text-sky-600 underline">fuji-san.info</a>
              官方網站的能見度預報表。可以左右滑動、上下捲動查看幾天內不同時間的預測。
            </p>
            <div class="overflow-x-auto">
              <iframe
                src="https://fuji-san.info/zh-tw/index.html"
                class="w-[900px] max-w-full h-[520px] border border-slate-200 rounded-lg"
              ></iframe>
            </div>
            <p class="text-xs text-slate-500 mt-2">
              ※ 若日後網站改版導致表格顯示異常，可直接開新分頁查看原始網頁。
            </p>
          </div>

          <!-- 常用日語 -->
          <div class="bg-white rounded-xl shadow p-4">
            <h2 class="text-xl font-bold mb-3">常用日語（點一下即可複製）</h2>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-3 text-base">
              ${[
                ['すみません','不好意思 / 麻煩你'],
                ['ありがとうございます','非常感謝'],
                ['いくらですか','多少錢？'],
                ['これください','我要這個'],
                ['トイレはどこですか','請問廁所在哪裡？'],
                ['お会計お願いします','我要結帳']
              ].map(([jp, zh]) => `
                <button
                  class="border border-slate-200 rounded-lg px-3 py-2 text-left hover:bg-slate-50"
                  onclick="copyPhrase('${jp}')"
                >
                  <div class="font-semibold text-sky-600 mb-1 text-lg">${jp}</div>
                  <div class="text-sm text-slate-600">${zh}</div>
                </button>
              `).join('')}
            </div>
          </div>
        </section>
      `;
      fetchFxRate();
      fetchTokyoWeather();
      initFujiVisibilitySlider();
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

    /* ----------- 行程頁 ----------- */
    function renderItinerary() {
      const main = document.getElementById('main');
      main.innerHTML = itineraryData.map(day => `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-2">${day.date}</h2>
          <div class="space-y-2">
            ${day.items.map(item => `
              <div class="flex items-start justify-between gap-2 border-b border-slate-100 py-2 last:border-0">
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
            `).join('')}
          </div>
        </section>
      `).join('');
    }

    /* ----------- 記帳 + CSV ----------- */
    let expenses = [];

    function renderAccount() {
      const main = document.getElementById('main');
      const total = expenses.reduce((s,e) => s + (e.amount || 0), 0);
      main.innerHTML = `
        <section class="bg-white rounded-xl shadow p-4 mb-4">
          <h2 class="text-xl font-bold mb-3">記帳 & CSV 匯出</h2>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-2 mb-3 text-sm">
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
              placeholder="金額（TWD 或 JPY）"
              class="px-3 py-2 border border-slate-300 rounded-lg"
            />
            <button
              onclick="addExpense()"
              class="px-3 py-2 rounded-lg bg-primary text-white font-semibold"
            >
              新增
            </button>
          </div>
          <div class="flex items-center justify-between text-sm mb-2">
            <span>目前總額：<span class="font-bold text-rose-600">${total.toLocaleString()}</span></span>
            <button
              onclick="exportCSV()"
              class="px-3 py-2 rounded-lg border border-sky-500 text-sky-600 hover:bg-sky-50"
            >
              匯出 CSV
            </button>
          </div>
          <div id="accList" class="max-h-80 overflow-y-auto text-sm border-t border-slate-100 pt-1">
            ${expenses.length === 0 ? `
              <p class="text-slate-400 text-center py-4">尚未有記帳資料，先新增一筆吧。</p>
            ` :
              expenses.map((e,i) => `
                <div class="flex items-center justify-between py-2 border-b border-slate-100 last:border-0">
                  <div>
                    <div class="font-semibold">${e.date || '未填日期'}｜${e.item}</div>
                    <div class="text-xs text-slate-600">金額：${e.amount.toLocaleString()}</div>
                  </div>
                  <button
                    onclick="deleteExpense(${i})"
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

    /* ----------- 旅遊檢查清單 ----------- */
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
      checklistItems.splice(idx,1);
      renderChecklist();
    }

    /* ----------- 飯店資訊頁 ----------- */
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

    /* ----------- 購物清單 ----------- */
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
      shoppingItems.splice(idx,1);
      renderShopping();
    }

    /* ----------- 初始化 ----------- */
    document.addEventListener('DOMContentLoaded', () => {
      attachSwipe();
      setTabByIndex(0);
    });
  </script>
</body>
</html>
