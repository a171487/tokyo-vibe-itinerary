<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>東京旅遊助理 v6.2</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- Supabase CDN -->
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", system-ui, sans-serif;
      background: #020617;
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
      box-shadow: 0 4px 12px rgba(0,0,0,0.5);
    }
    nav {
      display: flex;
      background: #020617;
      box-shadow: 0 2px 4px rgba(0,0,0,0.6);
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
      max-width: 820px;
      margin: 0 auto 30px;
    }
    section { display: none; }
    section.active { display: block; }

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
    .day-card-title {
      font-size: 19px;
      font-weight: 700;
      color: #bfdbfe;
      margin-bottom: 6px;
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
    button.tiny-btn, .tiny-btn {
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
    button:disabled {
      opacity: 0.6;
      cursor: default;
    }

    .rate-grid {
      display: grid;
      grid-template-columns: 1.2fr 1fr;
      gap: 10px;
      margin-top: 10px;
    }

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
    thead {
      background: #d1d5db; /* 淺灰表頭 */
    }
    th, td {
      padding: 4px 6px;
      text-align: center;
      white-space: nowrap;
    }
    th {
      color: #111827;
      border-bottom: 1px solid #9ca3af;
    }
    tbody tr:nth-child(odd) {
      background: #e5e7eb;
    }
    tbody tr:nth-child(even) {
      background: #f3f4f6;
    }
    td {
      color: #111827;
      border-bottom: 1px solid #d1d5db;
    }

    iframe { border: 0; }
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

    .prep-item, .shop-item {
      display: flex;
      align-items: flex-start;
      gap: 8px;
      padding: 6px 8px;
      border-radius: 10px;
      border: 1px solid #1f2937;
      background: #020617;
      margin-top: 6px;
      font-size: 14px;
    }
    .prep-check {
      width: 16px;
      height: 16px;
      margin-top: 4px;
    }
    .prep-text, .shop-text {
      flex: 1;
    }
    .prep-text.done, .shop-text.done {
      text-decoration: line-through;
      opacity: 0.6;
    }

    .tag {
      font-size: 12px;
      padding: 1px 6px;
      border-radius: 999px;
      background: #14532d;
      color: #bbf7d0;
      margin-left: 4px;
    }

    .jp-category {
      font-size: 15px;
      font-weight: 600;
      margin-top: 6px;
      color: #bfdbfe;
    }
    .phrase-row {
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 4px 0;
      border-bottom: 1px dashed #1f2937;
      font-size: 14px;
    }
    .phrase-main {
      flex: 1;
    }
    .phrase-jp {
      color: #a5b4fc;
      font-weight: 600;
    }

    @media (max-width: 480px) {
      body { font-size: 17px; }
      .card h2 { font-size: 19px; }
      .day-card-title { font-size: 18px; }
      nav button { font-size: 15px; padding: 8px 4px; }
    }
  </style>
</head>
<body>
<header>東京旅遊助理 v6.2</header>

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

      <div class="label" style="margin-top:8px;">未來一週（高低溫 / 日落 / UV / 下雪機率 / AQI）</div>
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
      <div style="font-size:16px;font-weight:600;">
        上野站前1號遊客酒店（Hotel Guest 1 Ueno Station）
      </div>

      <div class="label">地址</div>
      <div class="small">
        2 Chome-18-18 Higashiueno, Taito City, Tokyo 110-0015 日本
      </div>

      <div class="label">入住 / 退房</div>
      <div class="small">
        Check-in：<b>15:00</b> 後<br />
        Check-out：<b>10:00 之前</b>
      </div>

      <div class="map-embed">
        <iframe
          loading="lazy"
          src="https://www.google.com/maps?q=2+Chome-18-18+Higashiueno,+Taito+City,+Tokyo+110-0015+Japan&output=embed">
        </iframe>
      </div>
    </div>

    <!-- 常用日語 -->
    <div class="card">
      <h2>常用日語 🎧（點擊發音 / 複製）</h2>

      <div class="jp-category">🍽 餐廳</div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>請問有兩位的座位嗎？</div>
          <div class="phrase-jp">二人ですが、席は空いていますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('二人ですが、席は空いていますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('二人ですが、席は空いていますか。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>可以給我菜單嗎？</div>
          <div class="phrase-jp">メニューを見せてください。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('メニューを見せてください。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('メニューを見せてください。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>這道有沒有含酒精？</div>
          <div class="phrase-jp">この料理にアルコールは入っていますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('この料理にアルコールは入っていますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('この料理にアルコールは入っていますか。')">📋</button>
      </div>

      <div class="jp-category">🚆 交通</div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>請問這班電車有到○○嗎？</div>
          <div class="phrase-jp">この電車は○○に行きますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('この電車は○○に行きますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('この電車は○○に行きますか。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>請問要在哪一站轉車？</div>
          <div class="phrase-jp">どの駅で乗り換えればいいですか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('どの駅で乗り換えればいいですか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('どの駅で乗り換えればいいですか。')">📋</button>
      </div>

      <div class="jp-category">🆘 緊急</div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>請幫我叫救護車。</div>
          <div class="phrase-jp">救急車を呼んでください。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('救急車を呼んでください。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('救急車を呼んでください。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>我身體不舒服，哪裡可以看醫生？</div>
          <div class="phrase-jp">体調が悪いので、どこで診察を受けられますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('体調が悪いので、どこで診察を受けられますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('体調が悪いので、どこで診察を受けられますか。')">📋</button>
      </div>

      <div class="jp-category">🛍 購物</div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>這個可以免稅嗎？</div>
          <div class="phrase-jp">これは免税になりますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('これは免税になりますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('これは免税になりますか。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>可以給我收據嗎？</div>
          <div class="phrase-jp">領収書をください。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('領収書をください。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('領収書をください。')">📋</button>
      </div>

      <div class="jp-category">🏨 旅館 / 飯店</div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>我要辦理入住。</div>
          <div class="phrase-jp">チェックインをお願いします。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('チェックインをお願いします。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('チェックインをお願いします。')">📋</button>
      </div>
      <div class="phrase-row">
        <div class="phrase-main">
          <div>可以延遲退房嗎？</div>
          <div class="phrase-jp">レイトチェックアウトはできますか。</div>
        </div>
        <button class="tiny-btn" onclick="speakJP('レイトチェックアウトはできますか。')">🔊</button>
        <button class="tiny-btn" onclick="copyJP('レイトチェックアウトはできますか。')">📋</button>
      </div>
    </div>
  </section>

  <!-- 行程 -->
  <section id="plan">
    <!-- 12/26 -->
    <div class="card">
      <div class="day-card-title">12/26（五） 台北 → 東京・上野</div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">10:10</div>
          <div class="schedule-title">桃園國際機場 TPE T1（JX802 起飛）</div>
        </div>
        <div class="schedule-desc">
          搭乘星宇航空 JX802 前往東京，建議於起飛前 2–3 小時抵達機場，預留辦理托運、安檢與登機時間。
        </div>
        <div class="schedule-hours">溫馨提醒：出發前確認護照、機票、隨身行李體積與重量。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Taoyuan+International+Airport+Terminal+1">
            📍 導航到 桃園國際機場 T1
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">14:20</div>
          <div class="schedule-title">抵達成田機場 (NRT) T2</div>
        </div>
        <div class="schedule-desc">
          抵達東京成田機場第二航廈，辦理入境、領行李，準備前往市區。可在機場購買交通票券或儲值 Suica / PASMO。
        </div>
        <div class="schedule-hours">航班：JX802，實際降落時間以航空公司公告為準。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Narita+Airport+Terminal+2">
            📍 導航到 成田機場 T2
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">16:00</div>
          <div class="schedule-title">飯店 Check-in：上野站前1號遊客酒店</div>
        </div>
        <div class="schedule-desc">抵達上野站附近飯店放行李，熟悉周邊便利商店與車站出入口。</div>
        <div class="schedule-hours">Check-in：15:00 起，依飯店公告為準。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=2+Chome-18-18+Higashiueno,+Taito+City,+Tokyo+110-0015+Japan">
            📍 導航到 飯店
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">18:00</div>
          <div class="schedule-title">晚餐：阿美橫丁</div>
        </div>
        <div class="schedule-desc">阿美橫丁商店街聚集居酒屋、拉麵、海鮮丼與小吃，是感受庶民風情的好地方。</div>
        <div class="schedule-hours">營業時間：多數店家約 11:00–22:00（依各店為準）。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Ameya-Yokocho+Market+Tokyo">
            📍 導航到 阿美橫丁
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">20:00</div>
          <div class="schedule-title">購物：無印良品 上野丸井店、OS Drug 藥妝店</div>
        </div>
        <div class="schedule-desc">採買生活用品、保養品、藥妝與零食，順便觀察物價與貨品種類。</div>
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
        <div class="schedule-desc">老牌咖啡店，搭配蘋果派、閃電泡芙享用早餐，感受銀座復古氛圍。</div>
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
        <div class="schedule-desc">品嚐炭烤厚切牛舌或套餐，鹽味與檸檬風味是人氣選擇。</div>
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
        <div class="schedule-desc">以玻璃杯裝盛的焦糖布丁著名，也適合作為伴手禮帶回飯店冰起來吃。</div>
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
        <div class="schedule-desc">可依當天狀況選擇台場海濱公園欣賞花火，或前往惠比壽花園廣場看燈光秀。</div>
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
        <div class="schedule-desc">搭乘一日遊巴士前往富士山地區，建議提早抵達避免錯過集合。</div>
        <div class="schedule-hours">集合時間依旅行社行程表為準。</div>
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
        <div class="schedule-desc">經典「五重塔 + 富士山」構圖拍照點，氣候良好時景色非常壯觀。</div>
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
        <div class="schedule-desc">在地老字號鐘錶店，因富士山背景拍照而走紅，是頗具味道的小鎮街景。</div>
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
        <div class="schedule-desc">以清澈湧泉池聞名，可邊散步邊品嚐蕎麥麵、烤仙貝等在地美食。</div>
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
        <div class="schedule-desc">河口湖畔賞花與拍攝富士山的熱門地點，天氣晴朗時視野極佳。</div>
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
        <div class="schedule-desc">傍晚返程回東京站，結束一整天的富士山行程，回飯店休息或自由活動。</div>
        <div class="schedule-hours">抵達時間依交通狀況可能略有變動。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Tokyo+Station">
            📍 導航到 東京站
          </a>
        </div>
      </div>
    </div>

    <!-- 12/29 -->
    <div class="card">
      <div class="day-card-title">12/29（一） 東京・澀谷</div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">11:30</div>
          <div class="schedule-title">壽喜燒：Sukiyaki Juni Ten</div>
        </div>
        <div class="schedule-desc">品嚐日式壽喜燒，可選擇和牛套餐，適合悠閒用餐。</div>
        <div class="schedule-hours">營業時間：約 11:00–22:00，依店家公告為準。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Sukiyaki+Juni+Ten+Tokyo">
            📍 導航到 Sukiyaki Juni Ten
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">14:30</div>
          <div class="schedule-title">東急 Plaza 表參道原宿</div>
        </div>
        <div class="schedule-desc">時尚購物商場，可逛設計選物、服飾品牌與咖啡廳。</div>
        <div class="schedule-hours">營業時間：約 11:00–21:00。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Tokyu+Plaza+Omotesando+Harajuku">
            📍 導航到 東急 Plaza 表參道原宿
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">19:30</div>
          <div class="schedule-title">中目黑 散步與晚餐</div>
        </div>
        <div class="schedule-desc">沿著目黑川散步，感受夜晚街景與小店氣氛，找一間喜歡的餐廳用餐。</div>
        <div class="schedule-hours">多數店家 17:00–23:00 左右營業。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Nakameguro+Station">
            📍 導航到 中目黑站附近
          </a>
        </div>
      </div>
    </div>

    <!-- 12/30 -->
    <div class="card">
      <div class="day-card-title">12/30（二） 新宿・秋葉原</div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">11:00</div>
          <div class="schedule-title">新宿：NEWoMan / 高島屋等百貨</div>
        </div>
        <div class="schedule-desc">逛新宿車站周邊百貨與商場，採買服飾、生活雜貨與伴手禮。</div>
        <div class="schedule-hours">多數百貨 10:00–20:00 左右。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=NEWoMan+Shinjuku">
            📍 導航到 NEWoMan 新宿
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">18:30</div>
          <div class="schedule-title">二木菓子（秋葉原買伴手禮）</div>
        </div>
        <div class="schedule-desc">大型零食店，可一次購入各種糖果、餅乾與伴手禮。</div>
        <div class="schedule-hours">營業時間：約 10:00–20:00。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Futabakashi+Akihabara">
            📍 導航到 二木菓子 秋葉原
          </a>
        </div>
      </div>
    </div>

    <!-- 12/31 -->
    <div class="card">
      <div class="day-card-title">12/31（三） 成田市 & 回程</div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">09:30</div>
          <div class="schedule-title">成田山新勝寺</div>
        </div>
        <div class="schedule-desc">關東著名寺院，新年參拜人氣景點，可感受日本寺院氛圍。</div>
        <div class="schedule-hours">開門時間：約 6:00–17:00，依季節略有不同。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Naritasan+Shinshoji+Temple">
            📍 導航到 成田山新勝寺
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">10:30</div>
          <div class="schedule-title">成田山表參道</div>
        </div>
        <div class="schedule-desc">寺院前的商店街，聚集鰻魚飯老店、土產店與小吃店。</div>
        <div class="schedule-hours">店家多為 10:00–17:00 左右。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Naritasan+Omotesando">
            📍 導航到 成田山表參道
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">11:30</div>
          <div class="schedule-title">成田夢牧場 門前店</div>
        </div>
        <div class="schedule-desc">可品嚐霜淇淋、乳製品與輕食，適合當回程前的小憩。</div>
        <div class="schedule-hours">營業時間：約 10:00–17:00。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Narita+Yume+Bokujo+Omotesando">
            📍 導航到 成田夢牧場 門前店
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">12:30</div>
          <div class="schedule-title">成田機場 (NRT) 出境</div>
        </div>
        <div class="schedule-desc">預留時間辦理退稅、托運與安檢，準備搭機返家。</div>
        <div class="schedule-hours">建議起飛前約 3 小時抵達機場。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Narita+Airport+Terminal+2">
            📍 導航到 成田機場 T2
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">15:40</div>
          <div class="schedule-title">成田機場 NRT T2（JX803 起飛）</div>
        </div>
        <div class="schedule-desc">
          搭乘星宇航空 JX803 飛返桃園國際機場，請再次確認登機門與登機時間。
        </div>
        <div class="schedule-hours">溫馨提醒：起飛前 2–3 小時抵達機場，預留辦理出境與安檢時間。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Narita+Airport+Terminal+2">
            📍 導航到 成田機場 T2
          </a>
        </div>
      </div>

      <div class="schedule-item">
        <div class="schedule-header">
          <div class="schedule-time">18:45</div>
          <div class="schedule-title">抵達 桃園國際機場 TPE T1</div>
        </div>
        <div class="schedule-desc">返回台灣，結束東京旅程。可於入境後領取托運行李並通關。</div>
        <div class="schedule-hours">實際抵達時間以航班資訊為準。</div>
        <div class="schedule-nav">
          <a class="nav-link" target="_blank"
             href="https://www.google.com/maps/search/?api=1&query=Taoyuan+International+Airport+Terminal+1">
            📍 導航到 桃園國際機場 T1
          </a>
        </div>
      </div>
    </div>
  </section>

  <!-- 記帳 -->
  <section id="expense">
    <div class="card">
      <h2>旅費記帳 💰</h2>
      <div class="small">
        照片會上傳到 Supabase Storage（bucket：<b>expense-photos</b>），每筆最多 3 張、總量 10MB。
      </div>

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

      <label class="label">照片（最多 3 張 / 10MB）</label>
      <input id="expImg" type="file" accept="image/*" multiple />
      <div id="expUploadStatus" class="small"></div>

      <div class="btn-row">
        <button class="primary" id="expSubmitBtn">新增記帳</button>
      </div>
    </div>

    <div class="card">
      <h3>記帳列表</h3>
      <div id="expenseList" class="small">正在讀取雲端記帳資料…</div>
    </div>
  </section>

  <!-- 清單 -->
  <section id="list">
    <!-- 行前準備 -->
    <div class="card">
      <h2>行前準備清單 ✅</h2>
      <div class="small">勾選代表已完成（會顯示刪除線），可隨時編輯與刪除。資料會同步儲存在 Supabase。</div>

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
      <div class="small">
        照片會上傳到 Supabase Storage（bucket：<b>shopping-photos</b>），每筆最多 3 張、總量 10MB。
      </div>

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

      <label class="label">照片（最多 3 張 / 10MB）</label>
      <input id="shopImg" type="file" accept="image/*" multiple />
      <div id="shopUploadStatus" class="small"></div>

      <div class="btn-row">
        <button class="primary" id="shopAddBtn">新增購物項目</button>
      </div>

      <div id="shopList" style="margin-top:8px;"></div>
    </div>
  </section>
</main>

<script>
  // === Supabase 初始化 ===
  const { createClient } = supabase;
  const supabaseUrl = "https://jbfrwvmcvnctnfwknxtw.supabase.co";
  const supabaseKey = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpiZnJ3dm1jdm5jdG5md2tueHR3Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjUyMDI3ODYsImV4cCI6MjA4MDc3ODc4Nn0.H8vosru6poIlFcV7bGo28hRP9ukOGzh_eJ3Ba5dnXFM";
  const sb = createClient(supabaseUrl, supabaseKey);

  // === 頁籤切換（只用點擊，不使用左右滑動） ===
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

  // === 東京天氣 + 空氣品質 ===
  const weatherNowEl = document.getElementById("weatherNow");
  const weatherExtraEl = document.getElementById("weatherExtra");
  const airNowEl = document.getElementById("airNow");
  const weatherWeekBody = document.getElementById("weatherWeekBody");

  const weatherCodeMap = {
    0: "晴朗", 1: "幾乎晴朗", 2: "多雲", 3: "陰天",
    45: "有霧", 48: "霧凇", 51: "毛毛雨",
    61: "小雨", 63: "中雨", 65: "大雨",
    71: "小雪", 73: "中雪", 75: "大雪",
    95: "雷雨"
  };

  async function loadWeather() {
    const lat = 35.6895;
    const lon = 139.6917;

    const forecastUrl =
      `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}` +
      `&current_weather=true` +
      `&daily=weathercode,temperature_2m_max,temperature_2m_min,sunset,uv_index_max,precipitation_probability_max,snowfall_sum` +
      `&timezone=Asia%2FTokyo`;

    const airUrl =
      `https://air-quality-api.open-meteo.com/v1/air-quality?latitude=${lat}&longitude=${lon}` +
      `&hourly=european_aqi,pm2_5&timezone=Asia%2FTokyo&forecast_days=1`;

    let aqiValue = null;

    try {
      const airRes = await fetch(airUrl);
      if (airRes.ok) {
        const airData = await airRes.json();
        const h = airData.hourly || {};
        const aqArr = h.european_aqi || [];
        const pmArr = h.pm2_5 || [];
        if (aqArr.length) {
          const idx = aqArr.length - 1;
          aqiValue = aqArr[idx];
          const pm = pmArr[idx];
          let level = "";
          if (aqiValue <= 50) level = "（良好）";
          else if (aqiValue <= 100) level = "（普通）";
          else if (aqiValue <= 150) level = "（對敏感族群不健康）";
          else level = "（較差，注意防護）";

          airNowEl.innerHTML =
            `空氣品質 AQI：<b>${aqiValue}</b> ${level}<br>` +
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
        <div class="weather-grid-item">❄️ 今日下雪機率：約 <span>${todayPop}%${todaySnow > 0 ? "（可能有積雪）" : ""}</span></div>
        <div class="weather-grid-item">🌡️ 體感：依風速與濕度可能略有不同</div>
      `;

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

  // === 常用日語：發音 & 複製 ===
  function speakJP(text) {
    if (!("speechSynthesis" in window)) {
      alert("此瀏覽器不支援語音播放。");
      return;
    }
    const utter = new SpeechSynthesisUtterance(text);
    utter.lang = "ja-JP";
    window.speechSynthesis.speak(utter);
  }
  function copyJP(text) {
    navigator.clipboard?.writeText(text).then(
      () => {},
      () => { alert("複製失敗，請再試一次。"); }
    );
  }

  // === 共用：上傳檔案到 Storage，支援進度回報 ===
  async function uploadFilesToBucket(bucket, files, progressCallback) {
    const urls = [];
    const total = files.length || 1;
    for (let i = 0; i < files.length; i++) {
      const f = files[i];
      const path = `${Date.now()}-${Math.random().toString(36).slice(2)}-${f.name}`;
      if (progressCallback) {
        const percent = Math.round((i / total) * 100);
        progressCallback(percent);
      }
      const { error: upErr } = await sb.storage.from(bucket).upload(path, f);
      if (upErr) {
        console.error("upload error", upErr);
        throw upErr;
      }
      const { data } = sb.storage.from(bucket).getPublicUrl(path);
      urls.push(data.publicUrl);
    }
    if (progressCallback) {
      progressCallback(100);
    }
    return urls;
  }

  // === 記帳 ===
  const expDate = document.getElementById("expDate");
  const expTime = document.getElementById("expTime");
  const expName = document.getElementById("expName");
  const expAmount = document.getElementById("expAmount");
  const expCurrency = document.getElementById("expCurrency");
  const expNote = document.getElementById("expNote");
  const expImg = document.getElementById("expImg");
  const expSubmitBtn = document.getElementById("expSubmitBtn");
  const expenseListEl = document.getElementById("expenseList");
  const expUploadStatus = document.getElementById("expUploadStatus");

  async function loadExpenses() {
    const { data, error } = await sb
      .from("expenses")
      .select("*")
      .order("created_at", { ascending: false });

    if (error) {
      console.error(error);
      expenseListEl.textContent = "讀取記帳資料失敗：" + error.message;
      return;
    }
    if (!data.length) {
      expenseListEl.textContent = "目前尚無記帳紀錄。";
      return;
    }
    expenseListEl.innerHTML = "";
    data.forEach(e => {
      const div = document.createElement("div");
      div.className = "expense-item";

      const created = e.created_at ? new Date(e.created_at) : null;
      const createdStr = created
        ? created.toLocaleString("zh-TW", { timeZone: "Asia/Tokyo" })
        : "";

      const photos = [e.photo1_url, e.photo2_url, e.photo3_url]
  .filter(Boolean)
  .map(url => `
    <a href="${url}" target="_blank">
      <img src="${url}" 
           style="width:70px;height:70px;object-fit:cover;border-radius:8px;margin-right:6px;border:1px solid #555;" />
    </a>
  `)
  .join("");


      const contentHtml = `
        <div class="expense-header">
          <div>
            <div class="expense-title">${e.title || "(未命名)"}</div>
            <div class="small">${createdStr}</div>
          </div>
          <div class="expense-amount">
            ${(e.amount ?? 0).toLocaleString?.() || e.amount || 0} ${e.currency || ""}
          </div>
        </div>
        ${e.note ? `<div class="small">備註：${e.note}</div>` : ""}
        ${photos ? `<div style="margin-top:6px;">${photos}</div>` : ""}

      `;
      div.innerHTML = contentHtml;

      // 編輯 & 刪除按鈕
      const btnRow = document.createElement("div");
      btnRow.style.marginTop = "4px";
      const editBtn = document.createElement("button");
      editBtn.className = "tiny-btn";
      editBtn.textContent = "編輯";
      editBtn.addEventListener("click", async () => {
        const newTitle = prompt("項目名稱：", e.title || "");
        if (newTitle === null) return;
        const newAmtStr = prompt("金額：", e.amount || 0);
        if (newAmtStr === null) return;
        const newCurrency = prompt("幣別（例如 JPY / TWD）：", e.currency || "JPY");
        if (newCurrency === null) return;
        const newNote = prompt("備註：", e.note || "");
        const newAmt = parseFloat(newAmtStr || "0") || 0;

        const { error: upErr } = await sb.from("expenses").update({
          title: newTitle.trim(),
          amount: newAmt,
          currency: newCurrency.trim() || "JPY",
          note: (newNote || "").trim() || null
        }).eq("id", e.id);
        if (upErr) {
          alert("更新記帳資料失敗：" + upErr.message);
        } else {
          loadExpenses();
        }
      });

      const delBtn = document.createElement("button");
      delBtn.className = "tiny-btn danger";
      delBtn.textContent = "刪除";
      delBtn.style.marginLeft = "6px";
      delBtn.addEventListener("click", async () => {
        if (!confirm("確定刪除此記帳項目？")) return;
        const { error: delErr } = await sb.from("expenses").delete().eq("id", e.id);
        if (delErr) {
          alert("刪除記帳資料失敗：" + delErr.message);
        } else {
          loadExpenses();
        }
      });

      btnRow.appendChild(editBtn);
      btnRow.appendChild(delBtn);
      div.appendChild(btnRow);

      expenseListEl.appendChild(div);
    });
  }

  expSubmitBtn.addEventListener("click", async () => {
    try {
      expUploadStatus.textContent = "";
      const files = expImg.files;
      if (files.length > 3) {
        alert("最多僅能上傳 3 張照片。");
        return;
      }
      const totalSize = Array.from(files).reduce((s,f)=>s+f.size,0);
      const max = 10 * 1024 * 1024;
      if (totalSize > max) {
        alert("照片總大小超過 10MB，請刪減或壓縮後再試。");
        return;
      }
      const amount = parseFloat(expAmount.value || "0");
      if (!expName.value.trim() || !amount) {
        alert("請至少填寫項目名稱與金額。");
        return;
      }

      expSubmitBtn.disabled = true;
      expSubmitBtn.textContent = "上傳中…";

      // 日期 + 時間 合併進備註最前面
      let noteFull = (expNote.value || "").trim();
      const d = expDate.value;
      const t = expTime.value;
      if (d || t) {
        noteFull = `[${d || ""} ${t || ""}] ${noteFull}`;
      }

      let photoUrls = [];
      if (files.length) {
        expUploadStatus.textContent = "照片上傳中… 0%";
        photoUrls = await uploadFilesToBucket("expense-photos", files, (p) => {
          expUploadStatus.textContent = `照片上傳中… ${p}%`;
        });
        expUploadStatus.textContent = "照片上傳完成 ✅";
      }

      const p1 = photoUrls[0] || null;
      const p2 = photoUrls[1] || null;
      const p3 = photoUrls[2] || null;

      const { error } = await sb.from("expenses").insert({
        title: expName.value.trim(),
        amount,
        currency: expCurrency.value,
        note: noteFull || null,
        photo1_url: p1,
        photo2_url: p2,
        photo3_url: p3
      });
      if (error) {
        console.error(error);
        alert("儲存記帳資料失敗：" + error.message);
      } else {
        expDate.value = "";
        expTime.value = "";
        expName.value = "";
        expAmount.value = "";
        expNote.value = "";
        expImg.value = "";
        loadExpenses();
      }
    } catch (e) {
      console.error(e);
      alert("上傳或儲存時發生錯誤：" + (e.message || e));
    } finally {
      expSubmitBtn.disabled = false;
      expSubmitBtn.textContent = "新增記帳";
    }
  });

  // === 行前準備清單 ===
  const prepInput = document.getElementById("prepInput");
  const prepAddBtn = document.getElementById("prepAddBtn");
  const prepListEl = document.getElementById("prepList");
  let prepItems = [];

  async function loadPrep() {
    const { data, error } = await sb
      .from("prep_items")
      .select("*")
      .order("created_at", { ascending: true });

    if (error) {
      console.error(error);
      prepListEl.innerHTML = `<div class="small">讀取行前準備清單失敗：${error.message}</div>`;
      return;
    }
    prepItems = data;
    renderPrep();
  }

  function renderPrep() {
    if (!prepItems.length) {
      prepListEl.innerHTML = `<div class="small">目前尚無行前準備項目。</div>`;
      return;
    }
    prepListEl.innerHTML = "";
    prepItems.forEach(item => {
      const row = document.createElement("div");
      row.className = "prep-item";

      const checkbox = document.createElement("input");
      checkbox.type = "checkbox";
      checkbox.className = "prep-check";
      checkbox.checked = item.checked;
      checkbox.addEventListener("change", async () => {
        item.checked = checkbox.checked;
        renderPrep();
        await sb.from("prep_items").update({ checked: item.checked }).eq("id", item.id);
      });

      const span = document.createElement("div");
      span.className = "prep-text" + (item.checked ? " done" : "");
      span.textContent = item.name;

      const editBtn = document.createElement("button");
      editBtn.className = "tiny-btn";
      editBtn.textContent = "編輯";
      editBtn.addEventListener("click", async () => {
        const newText = prompt("編輯項目內容：", item.name || "");
        if (newText === null) return;
        const trimmed = newText.trim();
        if (!trimmed) return;
        item.name = trimmed;
        renderPrep();
        await sb.from("prep_items").update({ name: trimmed }).eq("id", item.id);
      });

      const delBtn = document.createElement("button");
      delBtn.className = "tiny-btn danger";
      delBtn.textContent = "刪除";
      delBtn.addEventListener("click", async () => {
        if (!confirm("確定刪除此項目？")) return;
        await sb.from("prep_items").delete().eq("id", item.id);
        prepItems = prepItems.filter(p => p.id !== item.id);
        renderPrep();
      });

      row.appendChild(checkbox);
      row.appendChild(span);
      row.appendChild(editBtn);
      row.appendChild(delBtn);
      prepListEl.appendChild(row);
    });
  }

  prepAddBtn.addEventListener("click", async () => {
    const text = prepInput.value.trim();
    if (!text) return;
    const { data, error } = await sb
      .from("prep_items")
      .insert({ name: text })
      .select()
      .single();
    if (error) {
      console.error(error);
      alert("新增行前準備項目失敗：" + error.message);
      return;
    }
    prepInput.value = "";
    prepItems.push(data);
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
  const shopUploadStatus = document.getElementById("shopUploadStatus");
  let shopItems = [];

  async function loadShop() {
    const { data, error } = await sb
      .from("shopping_items")
      .select("*")
      .order("created_at", { ascending: false });

    if (error) {
      console.error(error);
      shopListEl.innerHTML = `<div class="small">讀取購物清單失敗：${error.message}</div>`;
      return;
    }
    shopItems = data;
    renderShop();
  }

  function renderShop() {
    if (!shopItems.length) {
      shopListEl.innerHTML = `<div class="small">目前尚無購物清單項目。</div>`;
      return;
    }
    shopListEl.innerHTML = "";
    shopItems.forEach(item => {
      const row = document.createElement("div");
      row.className = "shop-item";

      const textDiv = document.createElement("div");
      textDiv.className = "shop-text";
      const amountLabel = (item.price ?? 0).toLocaleString?.() || item.price || 0;

const photos = [e.photo1_url, e.photo2_url, e.photo3_url]
  .filter(Boolean)
  .map(url => `
    <a href="${url}" target="_blank">
      <img src="${url}" 
           style="width:70px;height:70px;object-fit:cover;border-radius:8px;margin-right:6px;border:1px solid #555;" />
    </a>
  `)
  .join("");


${photos ? `<div style="margin-top:6px;">${photos}</div>` : ""}


      textDiv.innerHTML =
        `<b>${item.name || "(未命名)"}</b> <span class="tag">${amountLabel} ${item.currency || ""}</span>` +
        (item.note ? `<div class="small">備註：${item.note}</div>` : "") +
        (photos ? `<div class="small">${photos}</div>` : "");

      const editBtn = document.createElement("button");
      editBtn.className = "tiny-btn";
      editBtn.textContent = "編輯";
      editBtn.addEventListener("click", async () => {
        const newName = prompt("品項名稱：", item.name || "");
        if (newName === null) return;
        const newAmountStr = prompt("金額：", item.price || 0);
        if (newAmountStr === null) return;
        const newCur = prompt("幣別（JPY / TWD）：", item.currency || "JPY");
        if (newCur === null) return;
        const newNote = prompt("備註：", item.note || "");
        item.name = newName.trim();
        item.price = parseFloat(newAmountStr || "0") || 0;
        item.currency = newCur.trim() || "JPY";
        item.note = (newNote || "").trim();
        renderShop();
        await sb.from("shopping_items").update({
          name: item.name,
          price: item.price,
          currency: item.currency,
          note: item.note
        }).eq("id", item.id);
      });

      const delBtn = document.createElement("button");
      delBtn.className = "tiny-btn danger";
      delBtn.textContent = "刪除";
      delBtn.addEventListener("click", async () => {
        if (!confirm("確定刪除此購物項目？")) return;
        await sb.from("shopping_items").delete().eq("id", item.id);
        shopItems = shopItems.filter(s => s.id !== item.id);
        renderShop();
      });

      row.appendChild(textDiv);
      row.appendChild(editBtn);
      row.appendChild(delBtn);
      shopListEl.appendChild(row);
    });
  }

  shopAddBtn.addEventListener("click", async () => {
    try {
      shopUploadStatus.textContent = "";
      const files = shopImg.files;
      if (files.length > 3) {
        alert("最多僅能上傳 3 張照片。");
        return;
      }
      const totalSize = Array.from(files).reduce((s,f)=>s+f.size,0);
      const max = 10 * 1024 * 1024;
      if (totalSize > max) {
        alert("照片總大小超過 10MB，請刪減或壓縮後再試。");
        return;
      }
      if (!shopName.value.trim()) {
        alert("請至少填寫品項名稱。");
        return;
      }
      shopAddBtn.disabled = true;
      shopAddBtn.textContent = "上傳中…";

      let photoUrls = [];
      if (files.length) {
        shopUploadStatus.textContent = "照片上傳中… 0%";
        photoUrls = await uploadFilesToBucket("shopping-photos", files, (p) => {
          shopUploadStatus.textContent = `照片上傳中… ${p}%`;
        });
        shopUploadStatus.textContent = "照片上傳完成 ✅";
      }

      const p1 = photoUrls[0] || null;
      const p2 = photoUrls[1] || null;
      const p3 = photoUrls[2] || null;

      const { data, error } = await sb.from("shopping_items").insert({
        name: shopName.value.trim(),
        price: parseFloat(shopAmount.value || "0") || 0,
        currency: shopCurrency.value,
        note: shopNote.value.trim() || null,
        photo1_url: p1,
        photo2_url: p2,
        photo3_url: p3
      }).select().single();
      if (error) {
        console.error(error);
        alert("新增購物項目失敗：" + error.message);
        return;
      }
      shopName.value = "";
      shopAmount.value = "";
      shopNote.value = "";
      shopImg.value = "";
      shopItems.unshift(data);
      renderShop();
    } catch (e) {
      console.error(e);
      alert("上傳或儲存購物項目時發生錯誤：" + (e.message || e));
    } finally {
      shopAddBtn.disabled = false;
      shopAddBtn.textContent = "新增購物項目";
    }
  });

  // === 初始化載入雲端資料 ===
  loadExpenses();
  loadPrep();
  loadShop();
</script>
</body>
</html>
