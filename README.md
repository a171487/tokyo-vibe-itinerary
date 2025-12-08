<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>東京旅遊助理 v5.1</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    /* === 基本樣式 === */
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", system-ui, sans-serif;
      background: #0f172a;
      color: #e5e7eb;
      font-size: 18px;
    }
    header {
      background: linear-gradient(135deg, #14532d, #15803d);
      color: #ecfdf5;
      padding: 14px 16px;
      font-size: 22px;
      font-weight: 700;
      text-align: center;
      box-shadow: 0 4px 12px rgba(0,0,0,0.4);
    }

    /* === 頁籤列 === */
    nav {
      display: flex;
      background: #020617;
      box-shadow: 0 2px 4px rgba(0,0,0,0.5);
      position: sticky;
      top: 0;
      z-index: 10;
    }
    nav button {
      flex: 1;
      padding: 10px 6px;
      border: none;
      background: #020617;
      color: #9ca3af;
      font-size: 16px;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 4px;
      cursor: pointer;
    }
    nav button.active {
      background: #14532d;
      color: #f9fafb;
      font-weight: 600;
    }

    main {
      padding: 12px;
      max-width: 700px;
      margin: 0 auto;
    }
    section {
      display: none;
    }
    section.active {
      display: block;
    }

    /* === 卡片 === */
    .card {
      background: #020617;
      border-radius: 16px;
      padding: 14px 14px 16px;
      margin-bottom: 14px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.7);
      border: 1px solid #1f2937;
    }
    .card h2 {
      margin: 0 0 6px;
      font-size: 20px;
      font-weight: 700;
    }
    .card h3 {
      margin: 0 0 6px;
      font-size: 18px;
      font-weight: 700;
      color: #bfdbfe;
    }
    .label {
      font-size: 14px;
      color: #cbd5f5;
      margin-top: 6px;
    }
    .small {
      font-size: 13px;
      color: #9ca3af;
    }
    .highlight-number {
      font-size: 22px;
      font-weight: 700;
      color: #facc15;
    }
    .pill {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 12px;
      padding: 2px 8px;
      border-radius: 999px;
      border: 1px solid #4b5563;
      background: #020617;
      color: #e5e7eb;
    }
    .pill-accent {
      border-color: #22c55e;
      background: rgba(34,197,94,0.15);
      color: #bbf7d0;
    }

    input, select, textarea {
      width: 100%;
      padding: 8px 10px;
      margin-top: 4px;
      border-radius: 10px;
      border: 1px solid #374151;
      background: #020617;
      color: #e5e7eb;
      font-size: 16px;
    }
    textarea {
      min-height: 70px;
      resize: vertical;
    }
    input::placeholder, textarea::placeholder {
      color: #6b7280;
    }

    .btn-row {
      display: flex;
      gap: 8px;
      margin-top: 10px;
    }
    button.primary {
      flex: 1;
      border: none;
      border-radius: 999px;
      padding: 8px 10px;
      background: linear-gradient(135deg, #22c55e, #16a34a);
      color: #022c22;
      font-weight: 600;
      font-size: 16px;
      cursor: pointer;
    }
    button.secondary {
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid #4b5563;
      background: #020617;
      color: #e5e7eb;
      font-size: 14px;
      cursor: pointer;
    }
    button.danger {
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid #f87171;
      background: rgba(127,29,29,0.7);
      color: #fee2e2;
      font-size: 13px;
      cursor: pointer;
    }
    button:disabled {
      opacity: 0.6;
      cursor: default;
    }

    /* === 匯率 === */
    .rate-grid {
      display: grid;
      grid-template-columns: 1.2fr 1fr;
      gap: 10px;
      margin-top: 10px;
    }

    /* === 天氣表格 === */
    .weather-now-main {
      font-size: 18px;
      font-weight: 600;
    }
    .weather-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0,1fr));
      gap: 6px;
      margin-top: 6px;
    }
    .weather-grid-item {
      font-size: 13px;
      color: #e5e7eb;
      background: #020617;
      border-radius: 10px;
      padding: 6px 8px;
      border: 1px solid #1f2937;
    }
    .weather-grid-item span {
      color: #facc15;
      font-weight: 600;
    }
    .table-scroll {
      overflow-x: auto;
      margin-top: 8px;
    }
    table {
      border-collapse: collapse;
      width: 100%;
      font-size: 13px;
      min-width: 520px;
    }
    th, td {
      border-bottom: 1px solid #1f2937;
      padding: 4px 6px;
      text-align: center;
      white-space: nowrap;
    }
    th {
      color: #cbd5f5;
    }

    /* === YouTube / Maps === */
    iframe {
      border: 0;
    }
    .video {
      width: 100%;
      height: 220px;
      border-radius: 12px;
      overflow: hidden;
      margin-top: 8px;
    }
    .map-embed {
      width: 100%;
      height: 220px;
      border-radius: 12px;
      overflow: hidden;
      margin-top: 10px;
      border: 1px solid #1f2937;
    }

    /* === 行程 === */
    .day-card-title {
      font-size: 19px;
      font-weight: 700;
      color: #bfdbfe;
      margin-bottom: 6px;
    }
    .schedule-item {
      border-radius: 12px;
      border: 1px solid #1f2937;
      padding: 8px 10px;
      margin-top: 6px;
      background: #020617;
    }
    .schedule-header {
      display: flex;
      gap: 8px;
      align-items: baseline;
      margin-bottom: 4px;
    }
    .schedule-time {
      font-size: 16px;
      font-weight: 700;
      color: #f97316;
      min-width: 58px;
    }
    .schedule-title {
      font-size: 16px;
      font-weight: 600;
      color: #e5e7eb;
    }
    .schedule-desc {
      font-size: 14px;
      color: #d1d5db;
      margin-top: 2px;
    }
    .schedule-hours {
      font-size: 13px;
      color: #9ca3af;
      margin-top: 2px;
    }
    .schedule-nav {
      margin-top: 6px;
    }
    .nav-link {
      display: inline-block;
      padding: 4px 10px;
      border-radius: 999px;
      background: #14532d;
      color: #bbf7d0;
      text-decoration: none;
      font-size: 14px;
    }
    .nav-link:hover {
      background: #166534;
    }

    /* === 記帳列表 === */
    .expense-item {
      border-radius: 12px;
      border: 1px solid #1f2937;
      padding: 8px 10px;
      margin-top: 6px;
      background: #020617;
      font-size: 14px;
    }
    .expense-header {
      display: flex;
      justify-content: space-between;
      gap: 8px;
      margin-bottom: 4px;
    }
    .expense-title {
      font-weight: 600;
      font-size: 15px;
    }
    .expense-amount {
      font-weight: 700;
      color: #facc15;
    }

    /* === 行前準備清單 / 購物清單 === */
    .prep-item, .shop-item {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 6px 8px;
      border-radius: 10px;
      border: 1px solid #1f2937;
      background: #020617;
      margin-top: 6px;
      font-size: 14px;
    }
    .prep-text, .shop-text {
      flex: 1;
    }
    .prep-text.done, .shop-text.done {
      text-decoration: line-through;
      opacity: 0.6;
    }
    .tiny-btn {
      border-radius: 999px;
      border: 1px solid #4b5563;
      background: #020617;
      color: #e5e7eb;
      font-size: 12px;
      padding: 2px 6px;
      cursor: pointer;
    }
    .tiny-btn.danger {
      border-color: #f87171;
      color: #fecaca;
    }

    .tag {
      font-size: 12px;
      padding: 1px 6px;
      border-radius: 999px;
      background: #14532d;
      color: #bbf7d0;
      margin-left: 4px;
    }
  </style>
</head>
<body>
  <header>東京旅遊助理 v5.1</header>

  <nav>
    <button class="active" data-tab="home">🏠 首頁</button>
    <button data-tab="plan">🗺️ 行程</button>
    <button data-tab="expense">💰 記帳</button>
    <button data-tab="list">📝 清單</button>
  </nav>

  <main>
    <!-- 首頁 -->
    <section id="home" class="active">
      <!-- 匯率試算 -->
      <div class="card">
        <h2>匯率試算 💱</h2>
        <div class="small">手動輸入今日匯率：1 日圓 = 幾元台幣？</div>
        <label class="label">1 日圓 = 幾元台幣</label>
        <input id="rateTwdPerJpy" type="number" step="0.0001" placeholder="例如：0.22" />

        <div class="rate-grid">
          <div>
            <label class="label">日圓金額（JPY）→ 台幣</label>
            <input id="rateJpyInput" type="number" placeholder="例如：5000" />
          </div>
          <div>
            <div class="label">換算結果（TWD）</div>
            <div class="highlight-number" id="rateResult">—</div>
            <div class="small">依上方匯率即時計算</div>
          </div>
        </div>

        <div class="rate-grid" style="margin-top:10px;">
          <div>
            <label class="label">台幣金額（TWD）→ 日圓</label>
            <input id="rateTwdInput" type="number" placeholder="例如：1000" />
          </div>
          <div>
            <div class="label">換算結果（JPY）</div>
            <div class="highlight-number" id="rateResultJpy">—</div>
            <div class="small">依上方匯率即時計算</div>
          </div>
        </div>
      </div>

      <!-- 東京天氣 -->
      <div class="card">
        <h2>東京天氣 🌤️</h2>
        <div id="weatherNow" class="weather-now-main">正在取得東京即時天氣…</div>
        <div id="weatherExtra" class="weather-grid"></div>
        <div class="small" id="airNow" style="margin-top:6px;">空氣品質資料讀取中…</div>

        <div class="label" style="margin-top:8px;">未來一週（高低溫 / 日落 / UV / 下雪機率）</div>
        <div class="table-scroll">
          <table>
            <thead>
              <tr>
                <th>日期</th>
                <th>天氣</th>
                <th>最高 / 最低</th>
                <th>日落</th>
                <th>UV 最大值</th>
                <th>下雪機率</th>
                <th>AQI</th>
              </tr>
            </thead>
            <tbody id="weatherWeekBody">
              <tr><td colspan="7">讀取中…</td></tr>
            </tbody>
          </table>
        </div>
        <div class="small" style="margin-top:6px;">
          ※ 下雪機率以降水機率與預測降雪量估算，僅供參考。
        </div>
      </div>

      <!-- 富士山直播 -->
      <div class="card">
        <h2>富士山直播 🗻</h2>
        <div class="small">
          連線兩個 YouTube 富士山直播，建議在 Wi-Fi 環境下觀看。
        </div>
        <div class="video">
          <iframe src="https://www.youtube.com/embed/bdUbACCWmoY"
                  title="Mt. Fuji Live 1"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                  allowfullscreen></iframe>
        </div>
        <div class="video">
          <iframe src="https://www.youtube.com/embed/Gn2CJjzY068"
                  title="Mt. Fuji Live 2"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                  allowfullscreen></iframe>
        </div>
        <div class="label" style="margin-top:8px;">富士山能見度預報</div>
        <a class="pill pill-accent" href="https://fuji-san.info/zh-tw/index.html" target="_blank" rel="noopener">
          🔗 開啟 fuji-san.info 能見度預報
        </a>
      </div>

      <!-- 緊急電話 & 線上醫療 -->
      <div class="card">
        <h2>緊急電話 & 線上醫療 🚑</h2>
        <div class="label">日本緊急電話</div>
        <div class="small">
          ・110：警察<br />
          ・119：救護車 / 火警
        </div>

        <div class="label" style="margin-top:8px;">台灣駐日單位（參考）</div>
        <div class="small">
          ・台北駐日經濟文化代表處：+81-3-3280-7811<br />
          ・急難救助專線：+81-80-1009-5909
        </div>

        <div class="label" style="margin-top:8px;">OHDr. 中文線上門診</div>
        <div class="small">
          旅途中若身體不適，可透過 LINE 連線中文醫師線上看診。
        </div>
        <a class="pill pill-accent" href="https://line.me/R/ti/p/@406vicce" target="_blank" rel="noopener">
          🔗 加入 OHDr. LINE 中文官方帳號
        </a>
      </div>

      <!-- 飯店資訊 -->
      <div class="card">
        <h2>飯店資訊 🏨</h2>
        <div class="label">飯店名稱</div>
        <div style="font-size:16px;font-weight:600;">上野站前1號遊客酒店（Hotel New Ueno）</div>

        <div class="label">地址</div>
        <div class="small">
          東京都台東區東上野 2-18-18，日本
        </div>

        <div class="label">入住 / 退房</div>
        <div class="small">
          Check-in：<b>15:00</b> 後<br />
          Check-out：<b>10:00 之前</b>
        </div>

        <div class="map-embed">
          <iframe
            loading="lazy"
            src="https://www.google.com/maps?q=Hotel+New+Ueno,2-18-18+Higashiueno,Taito+City,Tokyo&output=embed">
          </iframe>
        </div>
      </div>
    </section>

    <!-- 行程 -->
    <section id="plan">
      <!-- 12/26 -->
      <div class="card">
        <div class="day-card-title">12/26（五） 上野</div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">14:20</div>
            <div class="schedule-title">抵達成田機場 (NRT) T2</div>
          </div>
          <div class="schedule-desc">
            入境、領行李、辦理網卡／交通票券，開始東京旅程。
          </div>
          <div class="schedule-hours">營業時間：機場 24 小時營運</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Narita+Airport+Terminal+2">
              📍 導航到成田機場 T2
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">16:00</div>
            <div class="schedule-title">飯店 Check-in：上野站前1號遊客酒店</div>
          </div>
          <div class="schedule-desc">
            抵達上野站附近飯店放行李，熟悉周邊便利商店與車站出入口。
          </div>
          <div class="schedule-hours">Check-in：15:00 起，依飯店公告為準。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Hotel+New+Ueno+Tokyo">
              📍 導航到飯店
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">18:00</div>
            <div class="schedule-title">晚餐：阿美橫丁</div>
          </div>
          <div class="schedule-desc">
            阿美橫丁商店街聚集居酒屋、拉麵、海鮮丼與小吃，是感受庶民風情的好地方。
          </div>
          <div class="schedule-hours">營業時間：多數店家約 11:00–22:00（依各店為準）。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Ameya-Yokocho+Market+Tokyo">
              📍 導航到阿美橫丁
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">20:00</div>
            <div class="schedule-title">購物：無印良品 上野丸井店、OS Drug 藥妝店</div>
          </div>
          <div class="schedule-desc">
            採買生活用品、保養品、藥妝與零食，順便觀察物價與貨品種類。
          </div>
          <div class="schedule-hours">營業時間：百貨與藥妝多為 10:00–21:00 左右。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Muji+Ueno+Marui">
              📍 導航到 無印良品 上野丸井店
            </a>
          </div>
        </div>
      </div>

      <!-- 12/27 -->
      <div class="card">
        <div class="day-card-title">12/27（六） 東京・銀座</div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">09:00</div>
            <div class="schedule-title">Tricolore coffee（早餐）</div>
          </div>
          <div class="schedule-desc">
            老牌咖啡店，搭配蘋果派、閃電泡芙享用早餐，感受銀座復古氛圍。
          </div>
          <div class="schedule-hours">營業時間：多為 8:00–22:00，依店鋪公告為準。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Tricolore+Coffee+Ginza">
              📍 導航到 Tricolore Coffee
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">12:00</div>
            <div class="schedule-title">牛たんの檸檬 有楽町店</div>
          </div>
          <div class="schedule-desc">
            品嚐炭烤厚切牛舌或套餐，鹽味與檸檬風味是人氣選擇。
          </div>
          <div class="schedule-hours">營業時間：午餐 11:00–15:00、晚餐 17:00–22:00 左右。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=%E7%89%9B%E3%81%9F%E3%82%93%E3%81%AE%E6%AA%AC+%E6%9C%89%E6%A5%BD%E7%94%BA">
              📍 導航到 牛たんの檸檬 有楽町店
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">15:00</div>
            <div class="schedule-title">MARLOWE 焦糖布丁</div>
          </div>
          <div class="schedule-desc">
            以玻璃杯裝盛的焦糖布丁著名，也適合作為伴手禮帶回飯店冰起來吃。
          </div>
          <div class="schedule-hours">營業時間：多為 10:00–20:00 左右。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=MARLOWE+Pudding+Ginza">
              📍 導航到 MARLOWE 銀座店附近
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">20:30</div>
            <div class="schedule-title">台場花火 / 惠比壽花園廣場燈飾</div>
          </div>
          <div class="schedule-desc">
            可依當天狀況選擇台場海濱公園欣賞花火，或前往惠比壽花園廣場看燈光秀。
          </div>
          <div class="schedule-hours">活動時間依官方公告為準。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Odaiba+Seaside+Park">
              📍 導航到 台場海濱公園
            </a>
          </div>
        </div>
      </div>

      <!-- 12/28 -->
      <div class="card">
        <div class="day-card-title">12/28（日） 富士山一日遊</div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">08:00</div>
            <div class="schedule-title">東京站 丸之內南口集合</div>
          </div>
          <div class="schedule-desc">
            搭乘一日遊巴士前往富士山地區，建議提早抵達避免錯過集合。
          </div>
          <div class="schedule-hours">依旅行社行程表為準。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Tokyo+Station+Marunouchi+South+Exit">
              📍 導航到 東京站丸之內南口
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">10:30</div>
            <div class="schedule-title">新倉山淺間公園</div>
          </div>
          <div class="schedule-desc">
            經典「五重塔 + 富士山」構圖拍照點，氣候良好時景色非常壯觀。
          </div>
          <div class="schedule-hours">公園全天開放，夜間請注意安全。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Arakurayama+Sengen+Park">
              📍 導航到 新倉山淺間公園
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">11:45</div>
            <div class="schedule-title">日川時計店</div>
          </div>
          <div class="schedule-desc">
            在地老字號鐘錶店，因富士山背景拍照而走紅，是頗具味道的小鎮街景。
          </div>
          <div class="schedule-hours">營業時間：約 9:00–18:00（依店家為準）。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=%E6%97%A5%E5%B7%9D%E6%99%82%E8%A8%88%E5%BA%97">
              📍 導航到 日川時計店
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">12:30</div>
            <div class="schedule-title">忍野八海（含午餐）</div>
          </div>
          <div class="schedule-desc">
            以清澈湧泉池聞名，可邊散步邊品嚐蕎麥麵、烤仙貝等在地美食。
          </div>
          <div class="schedule-hours">店家營業多為 9:00–17:00 左右。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Oshino+Hakkai">
              📍 導航到 忍野八海
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">15:20</div>
            <div class="schedule-title">大石公園</div>
          </div>
          <div class="schedule-desc">
            河口湖畔賞花與拍攝富士山的熱門地點，天氣晴朗時視野極佳。
          </div>
          <div class="schedule-hours">公園全天開放，咖啡廳多營業至傍晚。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Oishi+Park+Kawaguchiko">
              📍 導航到 大石公園
            </a>
          </div>
        </div>

        <div class="schedule-item">
          <div class="schedule-header">
            <div class="schedule-time">18:50</div>
            <div class="schedule-title">返回東京站</div>
          </div>
          <div class="schedule-desc">
            傍晚返程回東京站，結束一整天的富士山行程，回飯店休息或自由活動。
          </div>
          <div class="schedule-hours">抵達時間依交通狀況可能略有變動。</div>
          <div class="schedule-nav">
            <a class="nav-link" target="_blank"
               href="https://www.google.com/maps/search/?api=1&query=Tokyo+Station">
              📍 導航到 東京站
            </a>
          </div>
        </div>
      </div>

      <!-- 你也可以在這裡再補 12/29、12/30、12/31 其他行程 -->
    </section>

    <!-- 記帳 -->
    <section id="expense">
      <div class="card">
        <h2>旅費記帳 💰</h2>
        <div class="small">照片上傳總量上限 10MB（示範版僅存在本機，不會上傳到雲端）。</div>

        <label class="label">日期</label>
        <input type="date" id="expDate" />

        <label class="label">時間</label>
        <input type="time" id="expTime" />

        <label class="label">項目名稱</label>
        <input type="text" id="expName" placeholder="例如：晚餐、伴手禮、交通費" />

        <label class="label">金額</label>
        <input type="number" id="expAmount" placeholder="例如：1200" />

        <label class="label">幣別</label>
        <select id="expCurrency">
          <option value="JPY">JPY（日圓）</option>
          <option value="TWD">TWD（台幣）</option>
        </select>

        <label class="label">備註</label>
        <textarea id="expNote" placeholder="可記錄店名、誰先付錢、分攤方式等"></textarea>

        <label class="label">照片（最多 10MB，示範版不實際上傳）</label>
        <input id="expImg" type="file" accept="image/*" multiple />

        <div class="btn-row">
          <button class="primary" id="expSubmitBtn">新增記帳</button>
        </div>
      </div>

      <div class="card">
        <h3>記帳列表</h3>
        <div id="expenseList" class="small">目前尚無記帳紀錄。</div>
      </div>
    </section>

    <!-- 清單 -->
    <section id="list">
      <!-- 行前準備清單 -->
      <div class="card">
        <h2>行前準備清單 ✅</h2>
        <div class="small">勾選代表已完成，打勾項目會顯示刪除線，可隨時編輯與刪除。</div>

        <label class="label">新增項目</label>
        <div class="btn-row">
          <input id="prepInput" type="text" placeholder="例如：護照、外幣、行動電源…" />
          <button class="secondary" id="prepAddBtn">新增</button>
        </div>

        <div id="prepList" style="margin-top:8px;"></div>
      </div>

      <!-- 購物清單 -->
      <div class="card">
        <h2>購物清單 🛍️</h2>

        <label class="label">品項名稱</label>
        <input id="shopName" type="text" placeholder="例如：藥妝、防曬、零食禮盒…" />

        <label class="label">金額</label>
        <input id="shopAmount" type="number" placeholder="例如：3000" />

        <label class="label">幣別</label>
        <select id="shopCurrency">
          <option value="JPY">JPY（日圓）</option>
          <option value="TWD">TWD（台幣）</option>
        </select>

        <label class="label">備註</label>
        <textarea id="shopNote" placeholder="可記錄要買給誰、品牌、款式顏色等"></textarea>

        <label class="label">照片（最多 10MB，示範版不實際上傳）</label>
        <input id="shopImg" type="file" accept="image/*" multiple />

        <div class="btn-row">
          <button class="primary" id="shopAddBtn">新增購物項目</button>
        </div>

        <div id="shopList" style="margin-top:8px;"></div>
      </div>
    </section>
  </main>

  <script>
    // === 頁籤切換（取消左右滑動誤觸，只保留點擊） ===
    const tabButtons = document.querySelectorAll("nav button");
    const sections = document.querySelectorAll("main section");
    tabButtons.forEach(btn => {
      btn.addEventListener("click", () => {
        const id = btn.dataset.tab;
        tabButtons.forEach(b => b.classList.toggle("active", b === btn));
        sections.forEach(s => s.classList.toggle("active", s.id === id));
      });
    });

    // === 匯率試算 ===
    const rateTwdPerJpy = document.getElementById("rateTwdPerJpy");
    const rateJpyInput = document.getElementById("rateJpyInput");
    const rateResult = document.getElementById("rateResult");
    const rateTwdInput = document.getElementById("rateTwdInput");
    const rateResultJpy = document.getElementById("rateResultJpy");

    function updateFromJpy() {
      const r = parseFloat(rateTwdPerJpy.value);
      const j = parseFloat(rateJpyInput.value);
      if (!isNaN(r) && !isNaN(j)) {
        const twd = j * r;
        rateResult.textContent = twd.toFixed(0) + " 元";
      } else {
        rateResult.textContent = "—";
      }
    }
    function updateFromTwd() {
      const r = parseFloat(rateTwdPerJpy.value);
      const t = parseFloat(rateTwdInput.value);
      if (!isNaN(r) && !isNaN(t) && r > 0) {
        const jpy = t / r;
        rateResultJpy.textContent = jpy.toFixed(0) + " 日圓";
      } else {
        rateResultJpy.textContent = "—";
      }
    }
    rateTwdPerJpy.addEventListener("input", () => { updateFromJpy(); updateFromTwd(); });
    rateJpyInput.addEventListener("input", updateFromJpy);
    rateTwdInput.addEventListener("input", updateFromTwd);

    // === 東京天氣＋空氣品質 ===
    const weatherNowEl = document.getElementById("weatherNow");
    const weatherExtraEl = document.getElementById("weatherExtra");
    const airNowEl = document.getElementById("airNow");
    const weatherWeekBody = document.getElementById("weatherWeekBody");

    const weatherCodeMap = {
      0: "晴朗",
      1: "幾乎晴朗",
      2: "多雲",
      3: "陰天",
      45: "有霧",
      48: "霧凇",
      51: "毛毛雨",
      61: "小雨",
      63: "中雨",
      65: "大雨",
      71: "小雪",
      73: "中雪",
      75: "大雪",
      95: "雷雨"
    };

    async function loadWeather() {
      const lat = 35.6895;
      const lon = 139.6917;

      const forecastUrl =
        `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}` +
        `&current_weather=true` +
        `&daily=weathercode,temperature_2m_max,temperature_2m_min,sunrise,sunset,uv_index_max,precipitation_probability_max,snowfall_sum` +
        `&timezone=Asia%2FTokyo`;

      const airUrl =
        `https://air-quality-api.open-meteo.com/v1/air-quality?latitude=${lat}&longitude=${lon}` +
        `&hourly=european_aqi,pm2_5&timezone=Asia%2FTokyo&forecast_days=1`;

      let aqiValue = null;

      try {
        // 先抓空氣品質
        const airRes = await fetch(airUrl);
        if (airRes.ok) {
          const airData = await airRes.json();
          const h = airData.hourly || {};
          const times = h.time || [];
          const aqArr = h.european_aqi || [];
          const pmArr = h.pm2_5 || [];
          if (times.length && aqArr.length) {
            const idx = aqArr.length - 1;
            aqiValue = aqArr[idx];
            const pm = pmArr[idx];
            let levelLabel = "";
            if (aqiValue <= 50) levelLabel = "（良好）";
            else if (aqiValue <= 100) levelLabel = "（普通）";
            else if (aqiValue <= 150) levelLabel = "（對敏感族群不健康）";
            else levelLabel = "（空氣品質較差，注意防護）";

            airNowEl.innerHTML =
              `空氣品質 AQI：<b>${aqiValue}</b> ${levelLabel}<br>` +
              `PM2.5：約 <b>${pm != null ? pm.toFixed(1) : "—"}</b> μg/m³`;
          } else {
            airNowEl.textContent = "空氣品質：暫時無法取得資料";
          }
        } else {
          airNowEl.textContent = "空氣品質：暫時無法取得資料";
        }
      } catch (e) {
        console.error(e);
        airNowEl.textContent = "空氣品質：暫時無法取得資料";
      }

      // 再抓天氣
      try {
        const res = await fetch(forecastUrl);
        if (!res.ok) throw new Error("weather fetch failed");
        const data = await res.json();
        const cw = data.current_weather;
        const d = data.daily;

        const nowDesc = weatherCodeMap[cw.weathercode] || "天氣";
        weatherNowEl.textContent = `東京現在：${nowDesc}，約 ${cw.temperature}°C`;

        const todayUv = d.uv_index_max?.[0];
        const todaySunset = d.sunset?.[0]?.substring(11, 16) || "—";
        const todaySnow = d.snowfall_sum?.[0] || 0;
        const todayPop = d.precipitation_probability_max?.[0] || 0;

        weatherExtraEl.innerHTML = `
          <div class="weather-grid-item">🌇 今日日落時間：<span>${todaySunset}</span></div>
          <div class="weather-grid-item">🌞 今日 UV 最大值：約 <span>${todayUv != null ? todayUv.toFixed(1) : "—"}</span></div>
          <div class="weather-grid-item">❄️ 今日下雪機率：約 <span>${todaySnow > 0 ? todayPop + "%" : todayPop + "%"}</span></div>
          <div class="weather-grid-item">🌡️ 體感：依風速與濕度可能略有不同</div>
        `;

        // 週預報
        weatherWeekBody.innerHTML = "";
        const len = (d.time || []).length;
        for (let i = 0; i < len; i++) {
          const date = d.time[i];
          const code = d.weathercode[i];
          const maxT = d.temperature_2m_max[i];
          const minT = d.temperature_2m_min[i];
          const sunset = d.sunset[i]?.substring(11, 16) || "—";
          const uv = d.uv_index_max[i];
          const snow = d.snowfall_sum[i] || 0;
          const pop = d.precipitation_probability_max[i] || 0;
          const desc = weatherCodeMap[code] || "—";

          const snowStr = snow > 0 ? `${pop}%（可能）` : `${pop}%`;

          const tr = document.createElement("tr");
          tr.innerHTML = `
            <td>${date}</td>
            <td>${desc}</td>
            <td>${maxT.toFixed(1)}° / ${minT.toFixed(1)}°</td>
            <td>${sunset}</td>
            <td>${uv != null ? uv.toFixed(1) : "—"}</td>
            <td>${snowStr}</td>
            <td>${aqiValue != null ? aqiValue : "—"}</td>
          `;
          weatherWeekBody.appendChild(tr);
        }
      } catch (e) {
        console.error(e);
        weatherNowEl.textContent = "東京天氣更新失敗，請稍後再試。";
        weatherWeekBody.innerHTML = `<tr><td colspan="7">天氣資料暫時無法取得</td></tr>`;
      }
    }
    loadWeather();

    // === 記帳（本機陣列，不上傳） ===
    const expDate = document.getElementById("expDate");
    const expTime = document.getElementById("expTime");
    const expName = document.getElementById("expName");
    const expAmount = document.getElementById("expAmount");
    const expCurrency = document.getElementById("expCurrency");
    const expNote = document.getElementById("expNote");
    const expImg = document.getElementById("expImg");
    const expSubmitBtn = document.getElementById("expSubmitBtn");
    const expenseListEl = document.getElementById("expenseList");

    let expenses = [];

    function renderExpenses() {
      if (!expenses.length) {
        expenseListEl.textContent = "目前尚無記帳紀錄。";
        return;
      }
      expenseListEl.innerHTML = "";
      expenses.forEach((e, idx) => {
        const div = document.createElement("div");
        div.className = "expense-item";
        div.innerHTML = `
          <div class="expense-header">
            <div>
              <div class="expense-title">${e.name || "(未命名)"}</div>
              <div class="small">${e.date || ""} ${e.time || ""}</div>
            </div>
            <div class="expense-amount">
              ${e.amount.toLocaleString()} ${e.currency}
            </div>
          </div>
          ${e.note ? `<div class="small">備註：${e.note}</div>` : ""}
          ${e.photoInfo ? `<div class="small">${e.photoInfo}</div>` : ""}
        `;
        expenseListEl.appendChild(div);
      });
    }

    expSubmitBtn.addEventListener("click", () => {
      const totalSize = Array.from(expImg.files || []).reduce((sum,f)=>sum+f.size,0);
      const max = 10 * 1024 * 1024; // 10MB
      if (totalSize > max) {
        alert("照片總大小超過 10MB，請刪減或壓縮後再試。");
        return;
      }
      const amount = parseFloat(expAmount.value || "0");
      if (!expName.value.trim() || !amount) {
        alert("請至少填寫項目名稱與金額。");
        return;
      }
      const photoInfo = expImg.files.length ? `照片：${expImg.files.length} 張（僅存在本機，未上傳）` : "";

      expenses.push({
        date: expDate.value,
        time: expTime.value,
        name: expName.value.trim(),
        amount,
        currency: expCurrency.value,
        note: expNote.value.trim(),
        photoInfo
      });

      expDate.value = "";
      expTime.value = "";
      expName.value = "";
      expAmount.value = "";
      expNote.value = "";
      expImg.value = "";

      renderExpenses();
    });

    // === 行前準備清單 ===
    const prepInput = document.getElementById("prepInput");
    const prepAddBtn = document.getElementById("prepAddBtn");
    const prepListEl = document.getElementById("prepList");

    let prepItems = [
      { text: "護照 / 身分證 / 駕照", done: false },
      { text: "現金 / 信用卡 / IC 卡（Suica / PASMO）", done: false },
      { text: "手機、充電線、行動電源", done: false },
      { text: "國際轉接頭", done: false },
      { text: "常備藥品（腸胃藥、止痛藥、暈車藥）", done: false }
    ];

    function renderPrep() {
      prepListEl.innerHTML = "";
      prepItems.forEach((item, idx) => {
        const row = document.createElement("div");
        row.className = "prep-item";

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = item.done;
        checkbox.addEventListener("change", () => {
          item.done = checkbox.checked;
          renderPrep();
        });

        const span = document.createElement("div");
        span.className = "prep-text" + (item.done ? " done" : "");
        span.textContent = item.text;

        const editBtn = document.createElement("button");
        editBtn.className = "tiny-btn";
        editBtn.textContent = "編輯";
        editBtn.addEventListener("click", () => {
          const newText = prompt("編輯項目內容：", item.text);
          if (newText !== null) {
            item.text = newText.trim();
            renderPrep();
          }
        });

        const delBtn = document.createElement("button");
        delBtn.className = "tiny-btn danger";
        delBtn.textContent = "刪除";
        delBtn.addEventListener("click", () => {
          if (confirm("確定刪除此項目？")) {
            prepItems.splice(idx, 1);
            renderPrep();
          }
        });

        row.appendChild(checkbox);
        row.appendChild(span);
        row.appendChild(editBtn);
        row.appendChild(delBtn);
        prepListEl.appendChild(row);
      });
    }

    prepAddBtn.addEventListener("click", () => {
      const text = prepInput.value.trim();
      if (!text) return;
      prepItems.push({ text, done: false });
      prepInput.value = "";
      renderPrep();
    });

    // === 購物清單 ===
    const shopName = document.getElementById("shopName");
    const shopAmount = document.getElementById("shopAmount");
    const shopCurrency = document.getElementById("shopCurrency");
    const shopNote = document.getElementById("shopNote");
    const shopImg = document.getElementById("shopImg");
    const shopAddBtn = document.getElementById("shopAddBtn");
    const shopListEl = document.getElementById("shopList");

    let shopItems = [];

    function renderShop() {
      shopListEl.innerHTML = "";
      if (!shopItems.length) {
        shopListEl.innerHTML = `<div class="small">目前尚無購物清單項目。</div>`;
        return;
      }
      shopItems.forEach((item, idx) => {
        const row = document.createElement("div");
        row.className = "shop-item";

        const textDiv = document.createElement("div");
        textDiv.className = "shop-text";
        textDiv.innerHTML =
          `<b>${item.name || "(未命名)"}</b>` +
          ` <span class="tag">${item.amount.toLocaleString()} ${item.currency}</span>` +
          (item.note ? `<div class="small">備註：${item.note}</div>` : "") +
          (item.photoInfo ? `<div class="small">${item.photoInfo}</div>` : "");

        const editBtn = document.createElement("button");
        editBtn.className = "tiny-btn";
        editBtn.textContent = "編輯";
        editBtn.addEventListener("click", () => {
          const newName = prompt("品項名稱：", item.name);
          if (newName === null) return;
          const newAmount = prompt("金額（目前：" + item.amount + "）：", item.amount);
          if (newAmount === null) return;
          const newNote = prompt("備註：", item.note || "");
          item.name = newName.trim();
          item.amount = parseFloat(newAmount || "0") || 0;
          item.note = (newNote || "").trim();
          renderShop();
        });

        const delBtn = document.createElement("button");
        delBtn.className = "tiny-btn danger";
        delBtn.textContent = "刪除";
        delBtn.addEventListener("click", () => {
          if (confirm("確定刪除此購物項目？")) {
            shopItems.splice(idx, 1);
            renderShop();
          }
        });

        row.appendChild(textDiv);
        row.appendChild(editBtn);
        row.appendChild(delBtn);
        shopListEl.appendChild(row);
      });
    }

    shopAddBtn.addEventListener("click", () => {
      const totalSize = Array.from(shopImg.files || []).reduce((sum,f)=>sum+f.size,0);
      const max = 10 * 1024 * 1024;
      if (totalSize > max) {
        alert("購物清單照片總大小超過 10MB，請刪減或壓縮後再試。");
        return;
      }
      if (!shopName.value.trim()) {
        alert("請至少填寫品項名稱。");
        return;
      }
      const amount = parseFloat(shopAmount.value || "0") || 0;
      const photoInfo = shopImg.files.length ? `照片：${shopImg.files.length} 張（僅存在本機，未上傳）` : "";

      shopItems.push({
        name: shopName.value.trim(),
        amount,
        currency: shopCurrency.value,
        note: shopNote.value.trim(),
        photoInfo
      });

      shopName.value = "";
      shopAmount.value = "";
      shopNote.value = "";
      shopImg.value = "";

      renderShop();
    });

    // 初始渲染
    renderPrep();
    renderShop();
    renderExpenses();
  </script>
</body>
</html>
