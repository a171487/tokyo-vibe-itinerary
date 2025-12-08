<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>東京旅遊助理 App</title>
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <meta name="theme-color" content="#020617" />

  <style>
    :root {
      color-scheme: dark;
      --bg-main: #020617;
      --bg-card: rgba(15,23,42,0.96);
      --bg-card-soft: rgba(15,23,42,0.86);
      --border-subtle: rgba(51,65,85,0.9);
      --accent: #22c55e;
      --accent-soft: rgba(34,197,94,0.12);
      --accent-strong: rgba(34,197,94,0.25);
      --text-main: #e5e7eb;
      --text-muted: #9ca3af;
      --danger: #f97373;
      --danger-soft: rgba(239,68,68,0.18);
    }

    * {
      box-sizing: border-box;
    }

    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #022c22 0, #020617 45%, #000 100%);
      color: var(--text-main);
      -webkit-font-smoothing: antialiased;
    }

    body {
      display: flex;
      align-items: stretch;
      justify-content: center;
      padding: 10px 0;
    }

    .app {
      width: 100%;
      max-width: 540px;
      height: 100vh;
      max-height: 900px;
      background: radial-gradient(circle at 0 0, #064e3b 0, #020617 40%, #000 100%);
      border-radius: 26px;
      border: 1px solid rgba(148,163,184,0.35);
      box-shadow:
        0 18px 60px rgba(0,0,0,0.9),
        0 0 0 1px rgba(15,23,42,0.9);
      display: flex;
      flex-direction: column;
      padding: 10px 9px 12px;
      position: relative;
      overflow: hidden;
    }

    .status-bar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 2px 6px 6px;
      font-size: 12px;
      color: #9ca3af;
    }

    .status-bar-left {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .status-dot {
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: #22c55e;
      box-shadow: 0 0 6px rgba(34,197,94,0.8);
    }

    .status-bar-right {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .status-pill {
      border-radius: 999px;
      padding: 2px 8px;
      border: 1px solid rgba(148,163,184,0.5);
      background: rgba(15,23,42,0.85);
      display: flex;
      align-items: center;
      gap: 4px;
      font-size: 11px;
    }

    header {
      padding: 6px 4px 4px;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .app-title-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
    }

    .app-title {
      font-size: 18px;
      font-weight: 700;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .app-subtitle {
      font-size: 12px;
      color: #9ca3af;
    }

    .pill {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      border-radius: 999px;
      padding: 4px 10px;
      font-size: 11px;
      border: 1px solid rgba(148,163,184,0.4);
      background: rgba(15,23,42,0.95);
      color: #e5e7eb;
      white-space: nowrap;
    }

    .pill-accent {
      border-color: rgba(34,197,94,0.6);
      background: rgba(22,163,74,0.15);
      color: #bbf7d0;
    }

    .tab-bar {
      display: flex;
      gap: 6px;
      padding: 4px;
      border-radius: 999px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(31,41,55,0.9);
      margin: 2px 2px 0;
    }

    .tab {
      flex: 1;
      border-radius: 999px;
      padding: 6px 4px;
      font-size: 13px;
      border: none;
      background: transparent;
      color: #9ca3af;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 4px;
      cursor: pointer;
      transition: background 0.18s ease, color 0.18s ease, transform 0.08s ease;
    }

    .tab-icon {
      font-size: 14px;
    }

    .tab.active {
      background: radial-gradient(circle at top, rgba(34,197,94,0.35), rgba(22,101,52,0.95));
      color: #ecfdf5;
      box-shadow:
        0 0 0 1px rgba(22,163,74,0.85),
        0 10px 22px rgba(22,163,74,0.7);
      transform: translateY(-0.5px);
    }

    .tab:not(.active):active {
      transform: scale(0.98);
      background: rgba(31,41,55,0.9);
    }

    main {
      flex: 1;
      margin-top: 6px;
      position: relative;
      overflow-y: auto;
      overflow-x: hidden;
    }

    .tab-page {
      display: none;
      padding: 4px 4px 18px;
      animation: fadeIn 0.25s ease-out;
    }

    .tab-page.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateX(6px); }
      to { opacity: 1; transform: translateX(0); }
    }

    .grid {
      display: grid;
      grid-template-columns: minmax(0, 1fr);
      gap: 10px;
    }

    .grid-2 {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 8px;
    }

    @media (min-width: 480px) {
      .grid {
        grid-template-columns: minmax(0,1.4fr);
      }
    }

    .card {
      background: var(--bg-card);
      border-radius: 18px;
      border: 1px solid var(--border-subtle);
      padding: 10px 11px 10px;
      box-shadow:
        0 18px 40px rgba(15,23,42,0.9),
        0 0 0 1px rgba(15,23,42,0.9);
      position: relative;
    }

    .card-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      margin-bottom: 8px;
    }

    .card-header h2 {
      margin: 0;
      font-size: 16px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .chip {
      font-size: 11px;
      border-radius: 999px;
      padding: 2px 8px;
      border: 1px solid rgba(148,163,184,0.6);
      background: rgba(15,23,42,0.9);
      color: #e5e7eb;
      white-space: nowrap;
    }

    .chip-accent {
      border-color: rgba(34,197,94,0.7);
      background: rgba(22,163,74,0.2);
      color: #bbf7d0;
    }

    .chip-danger {
      border-color: rgba(239,68,68,0.75);
      background: rgba(127,29,29,0.6);
      color: #fecaca;
    }

    .label {
      font-size: 13px;
      margin-bottom: 2px;
      color: #cbd5f5;
    }

    .small-text {
      font-size: 12px;
      line-height: 1.5;
      color: var(--text-muted);
    }

    .big-number {
      font-size: 22px;
      font-weight: 700;
      margin-top: 2px;
    }

    .input-row {
      display: flex;
      align-items: center;
      gap: 6px;
      margin-top: 2px;
    }

    input, select, textarea {
      width: 100%;
      border-radius: 12px;
      border: 1px solid rgba(55,65,81,0.9);
      background: rgba(15,23,42,0.95);
      color: #e5e7eb;
      padding: 6px 9px;
      font-size: 13px;
      outline: none;
    }

    input::placeholder,
    textarea::placeholder {
      color: #6b7280;
    }

    input:focus, textarea:focus, select:focus {
      border-color: rgba(34,197,94,0.75);
      box-shadow: 0 0 0 1px rgba(34,197,94,0.7);
    }

    textarea {
      min-height: 58px;
      resize: vertical;
    }

    .btn-row {
      display: flex;
      gap: 6px;
      margin-top: 8px;
    }

    button.primary {
      flex: 1;
      border-radius: 999px;
      border: none;
      background: linear-gradient(135deg, #22c55e, #16a34a);
      color: #022c22;
      font-weight: 600;
      padding: 7px 10px;
      font-size: 13px;
      cursor: pointer;
      box-shadow:
        0 10px 25px rgba(34,197,94,0.75),
        0 0 0 1px rgba(6,95,70,0.8);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 4px;
    }

    button.primary:active {
      transform: translateY(1px);
      box-shadow:
        0 4px 14px rgba(22,163,74,0.8),
        0 0 0 1px rgba(6,95,70,0.9);
    }

    button.secondary {
      flex: 1;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.9);
      background: rgba(15,23,42,0.95);
      color: #e5e7eb;
      padding: 6px 10px;
      font-size: 13px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 4px;
    }

    button.secondary:active {
      background: rgba(31,41,55,0.95);
      transform: translateY(1px);
    }

    button.danger {
      border-radius: 999px;
      border: 1px solid rgba(248,113,113,0.9);
      background: var(--danger-soft);
      color: #fecaca;
      padding: 6px 10px;
      font-size: 12px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    button.danger:active {
      background: rgba(248,113,113,0.3);
    }

    button:disabled {
      opacity: 0.6;
      cursor: default;
      box-shadow: none;
      transform: none;
    }

    .badge-danger {
      color: #fecaca;
      padding: 0 5px;
      border-radius: 999px;
      background: rgba(248,113,113,0.28);
      border: 1px solid rgba(248,113,113,0.6);
      font-size: 11px;
    }

    .accent-link {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      font-size: 12px;
      color: #bbf7d0;
      text-decoration: none;
      border-radius: 999px;
      padding: 4px 9px;
      background: var(--accent-soft);
      border: 1px solid rgba(34,197,94,0.6);
      margin-top: 4px;
    }

    .accent-link:hover {
      background: var(--accent-strong);
    }

    .accent-link:active {
      transform: translateY(1px);
    }

    .list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 8px;
    }

    .item-card {
      border-radius: 14px;
      border: 1px solid rgba(55,65,81,0.9);
      background: var(--bg-card-soft);
      padding: 8px 9px;
    }

    .item-header {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      gap: 6px;
      margin-bottom: 4px;
    }

    .item-title {
      font-size: 13px;
      font-weight: 600;
    }

    .item-meta {
      font-size: 11px;
      color: var(--text-muted);
      text-align: right;
    }

    .photos {
      display: flex;
      gap: 6px;
      margin-top: 4px;
      flex-wrap: wrap;
    }

    .photos img {
      width: 60px;
      height: 60px;
      object-fit: cover;
      border-radius: 10px;
      border: 1px solid rgba(55,65,81,0.9);
    }

    .video-wrapper {
      position: relative;
      padding-bottom: 56.25%;
      height: 0;
      overflow: hidden;
      border-radius: 14px;
      border: 1px solid rgba(55,65,81,0.9);
      background: #000;
    }

    .video-wrapper iframe {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      border: 0;
      border-radius: 14px;
    }

    .checklist-item {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 6px 8px;
      border-radius: 12px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(55,65,81,0.9);
    }

    .checklist-item.completed {
      opacity: 0.7;
      text-decoration: line-through;
    }

    .checklist-item input[type="checkbox"] {
      width: 16px;
      height: 16px;
      accent-color: #22c55e;
    }

    .badge {
      font-size: 11px;
      border-radius: 999px;
      padding: 1px 6px;
      border: 1px solid rgba(148,163,184,0.6);
      color: #e5e7eb;
      background: rgba(15,23,42,0.95);
    }

    .badge-green {
      border-color: rgba(34,197,94,0.75);
      background: rgba(22,163,74,0.25);
      color: #bbf7d0;
    }

    .badge-warning {
      border-color: rgba(234,179,8,0.9);
      background: rgba(234,179,8,0.25);
      color: #fef9c3;
    }

    .flex-between {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
    }

    .pill-soft {
      border-radius: 999px;
      padding: 2px 8px;
      border: 1px solid rgba(148,163,184,0.6);
      font-size: 11px;
      color: #e5e7eb;
      background: rgba(15,23,42,0.9);
      white-space: nowrap;
    }

    .map-img {
      width: 100%;
      border-radius: 14px;
      border: 1px solid rgba(55,65,81,0.9);
      margin-top: 6px;
    }

    .fuji-thumb {
      width: 100%;
      border-radius: 14px;
      border: 1px solid rgba(55,65,81,0.9);
    }

    .phrase-group-title {
      font-size: 14px;
      font-weight: 600;
      margin: 8px 0 4px;
      color: #bbf7d0;
    }

    .phrase-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 6px;
      margin-bottom: 4px;
      padding: 6px 7px;
      border-radius: 10px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(55,65,81,0.9);
      font-size: 14px;
    }

    .phrase-text {
      flex: 1;
      min-width: 0;
    }

    .icon-btn {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.7);
      background: rgba(15,23,42,0.9);
      color: #e5e7eb;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      font-size: 15px;
      cursor: pointer;
    }

    .icon-btn:active {
      transform: translateY(1px);
      background: rgba(31,41,55,0.95);
    }

    footer {
      padding: 4px 6px 0;
      font-size: 11px;
      color: #6b7280;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
    }

    .footer-right {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .dot-row {
      display: flex;
      gap: 3px;
    }

    .dot {
      width: 4px;
      height: 4px;
      border-radius: 999px;
      background: rgba(75,85,99,0.9);
    }

    .dot-active {
      width: 8px;
      background: #22c55e;
      box-shadow: 0 0 6px rgba(34,197,94,0.85);
    }

    .swipe-hint {
      font-size: 11px;
      color: #9ca3af;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 6px;
      font-size: 11px;
    }

    th, td {
      border-bottom: 1px solid rgba(31,41,55,0.9);
      padding: 4px 3px;
      text-align: left;
      white-space: nowrap;
    }

    th {
      font-weight: 600;
      color: #cbd5f5;
    }

    tbody {
      font-size: 11px;
    }

    .table-scroll {
      overflow-x: auto;
      margin-top: 6px;
    }

    .table-scroll::-webkit-scrollbar {
      height: 4px;
    }

    .table-scroll::-webkit-scrollbar-thumb {
      background: rgba(148,163,184,0.7);
      border-radius: 999px;
    }

    .file-input {
      font-size: 11px;
      padding: 4px 6px;
    }

    .row-space {
      margin-top: 6px;
    }

    .section-title {
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .badge-outline {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      border-radius: 999px;
      padding: 2px 6px;
      border: 1px dashed rgba(148,163,184,0.7);
      font-size: 11px;
      color: #e5e7eb;
    }

    .pill-soft-green {
      border-radius: 999px;
      padding: 2px 6px;
      border: 1px solid rgba(34,197,94,0.7);
      background: rgba(22,163,74,0.18);
      font-size: 11px;
      color: #bbf7d0;
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="status-bar">
      <div class="status-bar-left">
        <div class="status-dot"></div>
        <span>東京旅遊助理 App</span>
      </div>
      <div class="status-bar-right">
        <div class="status-pill">
          <span>🕒</span>
          <span id="statusTime">--:--</span>
        </div>
        <div class="status-pill">
          <span>☁️ Firebase</span>
          <span id="firebaseStatus">未連線</span>
        </div>
      </div>
    </div>

    <header>
      <div class="app-title-row">
        <div>
          <div class="app-title">
            <span>東京旅遊 Dashboard</span>
            <span style="font-size:18px;">🗼</span>
          </div>
          <div class="app-subtitle">
            2024/12/26–12/31  東京・富士山・成田｜匯率・天氣・行程・記帳一次掌握
          </div>
        </div>
        <div class="pill pill-accent">
          <span>Trip Ready</span>
          <span>✅</span>
        </div>
      </div>

      <div class="tab-bar" id="tabBar">
        <button class="tab active" data-tab="home">
          <span class="tab-icon">🏠</span><span>首頁</span>
        </button>
        <button class="tab" data-tab="itinerary">
          <span class="tab-icon">🗺️</span><span>行程</span>
        </button>
        <button class="tab" data-tab="expense">
          <span class="tab-icon">💰</span><span>記帳</span>
        </button>
        <button class="tab" data-tab="lists">
          <span class="tab-icon">🧾</span><span>清單</span>
        </button>
      </div>
    </header>

    <main id="mainContainer">
      <!-- 首頁 -->
      <section class="tab-page active" id="home">
        <div class="grid">
          <!-- 匯率試算 -->
          <section class="card">
            <div class="card-header">
              <h2>匯率試算 <span style="font-size:18px;">💱</span></h2>
              <span class="chip">手動輸入匯率</span>
            </div>
            <label class="label">1 日圓 ＝ 幾元台幣？</label>
            <input type="number" id="rateTwdPerJpy" step="0.0001" placeholder="例如：0.22" />
            <div class="grid grid-2 mt-6">
              <div>
                <label class="label">日圓金額（JPY）</label>
                <input type="number" id="rateJpyInput" placeholder="例如：5000" />
              </div>
              <div>
                <label class="label">換算結果（TWD）</label>
                <div class="big-number" id="rateResult">—</div>
                <div class="small-text">依上方匯率即時計算</div>
              </div>
            </div>
            <div class="grid grid-2 mt-6">
              <div>
                <label class="label">台幣金額（TWD）</label>
                <input type="number" id="rateTwdInput" placeholder="例如：1000" />
              </div>
              <div>
                <label class="label">換算結果（JPY）</label>
                <div class="big-number" id="rateResultJpy">—</div>
                <div class="small-text">依上方匯率即時計算</div>
              </div>
            </div>
          </section>

          <!-- 天氣 + 空氣品質 -->
          <section class="card">
            <div class="card-header">
              <h2>東京天氣 <span>🌤️</span></h2>
              <span class="chip" id="weatherUpdated">更新中…</span>
            </div>
            <div id="currentWeather" class="small-text">
              正在取得東京即時天氣…
            </div>
            <div id="currentAir" class="small-text" style="margin-top:6px;">
              正在取得空氣品質與 PM2.5…
            </div>
            <div class="table-scroll">
              <table>
                <thead>
                  <tr>
                    <th>日期</th>
                    <th>天氣</th>
                    <th>最高</th>
                    <th>最低</th>
                    <th>日落</th>
                    <th>AQI</th>
                    <th>下雪？</th>
                  </tr>
                </thead>
                <tbody id="weeklyWeatherBody">
                  <tr><td colspan="7">讀取中…</td></tr>
                </tbody>
              </table>
            </div>
          </section>

          <!-- 富士山 -->
          <section class="card">
            <div class="card-header">
              <h2>富士山直播 <span>🗻</span></h2>
              <span class="chip">YouTube 直播</span>
            </div>
            <div class="small-text">
              內嵌兩個官方 YouTube 即時影像，點擊可全螢幕觀看。若無法播放，建議改用 YouTube App 開啟。
            </div>
            <div class="mt-6">
              <div class="label">山中湖 LIVE</div>
              <div class="video-wrapper">
                <iframe src="https://www.youtube.com/embed/bdUbACCWmoY"
                        title="Mt. Fuji Live 1"
                        frameborder="0"
                        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                        allowfullscreen></iframe>
              </div>
            </div>
            <div class="mt-4">
              <div class="label">富士山景 LIVE</div>
              <div class="video-wrapper">
                <iframe src="https://www.youtube.com/embed/Gn2CJjzY068"
                        title="Mt. Fuji Live 2"
                        frameborder="0"
                        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                        allowfullscreen></iframe>
              </div>
            </div>
            <div class="mt-6">
              <span class="label">富士山能見度預報（官方網站）</span>
              <a class="accent-link" href="https://fuji-san.info/zh-tw/index.html" target="_blank" rel="noopener">
                🔗 開啟 fuji-san.info 能見度預報
              </a>
            </div>
          </section>

          <!-- 緊急電話 -->
          <section class="card">
            <div class="card-header">
              <h2>緊急電話 & 線上醫療 <span>🚑</span></h2>
            </div>
            <div class="grid">
              <div>
                <div class="pill">📞 日本緊急電話</div>
                <ul class="small-text" style="margin:6px 0 0 16px;padding:0;">
                  <li>110：警察</li>
                  <li>119：救護車 / 火警</li>
                </ul>
              </div>
              <div>
                <div class="pill">🏛️ 台灣駐日辦事處</div>
                <ul class="small-text" style="margin:6px 0 0 16px;padding:0;">
                  <li>台北駐日經濟文化代表處</li>
                  <li>電話：+81-3-3280-7811</li>
                  <li>急難：+81-80-1009-5909</li>
                </ul>
              </div>
            </div>
            <div class="mt-6">
              <div class="pill">🩺 OHDr. 中文線上門診</div>
              <div class="small-text mt-4">
                點擊下方連結，直接加入 <strong>OHDr. LINE 中文版官方帳號</strong>，旅途中可用中文線上看診。
              </div>
              <a class="accent-link" href="https://line.me/R/ti/p/@406vicce" target="_blank" rel="noopener">
                🔗 加入 OHDr. 中文版 LINE 官方帳號
              </a>
            </div>
          </section>

          <!-- 飯店資訊（移到首頁） -->
          <section class="card">
            <div class="card-header">
              <h2>飯店資訊 <span>🏨</span></h2>
            </div>
            <div class="small-text">
              <div><strong>飯店名稱：</strong>上野站前1號遊客酒店（Hotel New Ueno No.1 Tourist）</div>
              <div class="mt-4">
                <strong>地址：</strong>東京都, 東京, Taito-ku Higashiueno 2-18-18, 日本
              </div>
              <div class="mt-4">
                <strong>Check-in：</strong>依飯店規定（多為 15:00 起）<br />
                <strong>Check-out：</strong> <span class="badge-danger">10:00 之前</span>
              </div>
              <div class="mt-4">
                <a class="accent-link"
                   href="https://maps.google.com/?q=Taito-ku%20Higashiueno%202-18-18%20Hotel"
                   target="_blank" rel="noopener">
                  📍 在 Google Maps 開啟飯店位置
                </a>
              </div>
            </div>
          </section>

          <!-- 常用日語 -->
          <section class="card">
            <div class="card-header">
              <h2>常用日語 <span>🗣️</span></h2>
              <span class="chip">點擊複製 + 播放語音</span>
            </div>

            <div class="small-text">
              點擊文字可複製日文句子，右側喇叭按鈕會用瀏覽器語音讀出（需開啟音量）。
            </div>

            <div class="phrase-group-title">🍽️ 餐廳</div>
            <div class="phrase-row" data-phrase="すみません、予約しています。">
              <div class="phrase-text">不好意思，我有預約。<br><span class="small-text">すみません、予約しています。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
            <div class="phrase-row" data-phrase="おすすめは何ですか。">
              <div class="phrase-text">有推薦的料理嗎？<br><span class="small-text">おすすめは何ですか。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
            <div class="phrase-row" data-phrase="会計をお願いします。">
              <div class="phrase-text">我要結帳。<br><span class="small-text">会計をお願いします。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>

            <div class="phrase-group-title">🚃 交通</div>
            <div class="phrase-row" data-phrase="この電車は上野駅に行きますか。">
              <div class="phrase-text">這班電車有到上野站嗎？<br><span class="small-text">この電車は上野駅に行きますか。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
            <div class="phrase-row" data-phrase="○○駅までいくらですか。">
              <div class="phrase-text">到○○站車資多少？<br><span class="small-text">○○駅までいくらですか。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>

            <div class="phrase-group-title">🧾 購物</div>
            <div class="phrase-row" data-phrase="これ、免税にできますか。">
              <div class="phrase-text">這個可以免稅嗎？<br><span class="small-text">これ、免税にできますか。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
            <div class="phrase-row" data-phrase="同じものをもう一つください。">
              <div class="phrase-text">請再給我一個一樣的。<br><span class="small-text">同じものをもう一つください。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>

            <div class="phrase-group-title">🚨 緊急</div>
            <div class="phrase-row" data-phrase="助けてください。">
              <div class="phrase-text">請幫幫我。<br><span class="small-text">助けてください。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
            <div class="phrase-row" data-phrase="日本語があまり話せません。">
              <div class="phrase-text">我不太會說日文。<br><span class="small-text">日本語があまり話せません。</span></div>
              <button class="icon-btn copy-btn">📋</button>
              <button class="icon-btn speak-btn">🔊</button>
            </div>
          </section>
        </div>
      </section>

      <!-- 行程 -->
      <section class="tab-page" id="itinerary">
        <div class="grid">

          <!-- 每日行程 -->
          <section class="card">
            <div class="card-header">
              <h2>行程總覽 <span>📅</span></h2>
              <span class="chip">含備註 & 預算</span>
            </div>

            <div id="itineraryList" class="list"></div>
          </section>
        </div>
      </section>

      <!-- 記帳 -->
      <section class="tab-page" id="expense">
        <div class="grid">
          <section class="card">
            <div class="card-header">
              <h2>旅費記帳 <span>💳</span></h2>
              <span class="chip" id="expenseStatus">同步中…</span>
            </div>
            <form id="expenseForm">
              <div class="grid grid-2">
                <div>
                  <label class="label">日期</label>
                  <input type="date" name="expenseDate" />
                </div>
                <div>
                  <label class="label">幣別</label>
                  <select name="expenseCurrency">
                    <option value="JPY">JPY（日圓）</option>
                    <option value="TWD">TWD（台幣）</option>
                  </select>
                </div>
              </div>
              <div class="row-space">
                <label class="label">項目</label>
                <input type="text" name="expenseTitle" placeholder="例如：晚餐、車票、伴手禮" />
              </div>
              <div class="row-space">
                <label class="label">金額</label>
                <input type="number" name="expenseAmount" step="1" placeholder="例如：1200" />
              </div>
              <div class="row-space">
                <label class="label">備註</label>
                <textarea name="expenseNote" placeholder="可記錄店名、誰先墊錢等"></textarea>
              </div>
              <div class="row-space">
                <label class="label">照片（最多 3 張）</label>
                <input type="file" name="expensePhotos" class="file-input" accept="image/*" multiple />
                <div class="small-text">
                  建議壓縮照片後再上傳，避免容量過大導致同步失敗。
                </div>
              </div>
              <div class="btn-row">
                <button type="submit" class="primary" id="expenseSubmitBtn">
                  <span>新增記帳</span> <span>➕</span>
                </button>
              </div>
            </form>

            <div class="row-space">
              <div class="section-title">記帳紀錄</div>
              <div id="expenseList" class="list small-text">
                <div class="small-text">載入中…</div>
              </div>
            </div>
          </section>
        </div>
      </section>

      <!-- 清單 -->
      <section class="tab-page" id="lists">
        <div class="grid">
          <!-- 旅遊檢查清單 -->
          <section class="card">
            <div class="card-header">
              <h2>旅遊檢查清單 <span>✅</span></h2>
              <span class="chip">可多人一起勾選</span>
            </div>
            <div id="checklistContainer" class="list small-text">
              <div class="small-text">載入中…</div>
            </div>
          </section>

          <!-- 購物清單 -->
          <section class="card">
            <div class="card-header">
              <h2>購物清單 <span>🛍️</span></h2>
            </div>
            <form id="shoppingForm">
              <div class="row-space">
                <label class="label">品項名稱</label>
                <input type="text" name="shoppingTitle" placeholder="例如：藥妝、防曬、零食禮盒" />
              </div>
              <div class="grid grid-2">
                <div>
                  <label class="label">預算金額</label>
                  <input type="number" name="shoppingAmount" step="1" placeholder="例如：3000" />
                </div>
                <div>
                  <label class="label">幣別</label>
                  <select name="shoppingCurrency">
                    <option value="JPY">JPY（日圓）</option>
                    <option value="TWD">TWD（台幣）</option>
                  </select>
                </div>
              </div>
              <div class="row-space">
                <label class="label">備註</label>
                <textarea name="shoppingNote" placeholder="可記錄買給誰、想買的品牌等"></textarea>
              </div>
              <div class="row-space">
                <label class="label">照片（最多 3 張）</label>
                <input type="file" name="shoppingPhotos" class="file-input" accept="image/*" multiple />
              </div>
              <div class="btn-row">
                <button class="primary" type="submit" id="shoppingSubmitBtn">
                  <span>新增購物項目</span> <span>➕</span>
                </button>
              </div>
            </form>

            <div class="row-space">
              <div class="section-title">購物清單</div>
              <div id="shoppingList" class="list small-text">
                <div class="small-text">載入中…</div>
              </div>
            </div>
          </section>
        </div>
      </section>
    </main>

    <footer>
      <div class="swipe-hint">
        <span>👆 左右滑動／點上方頁籤切換</span>
      </div>
      <div class="footer-right">
        <span>Dark Mode</span>
        <div class="dot-row">
          <div class="dot dot-active"></div>
          <div class="dot"></div>
          <div class="dot"></div>
        </div>
      </div>
    </footer>
  </div>

  <!-- Firebase & App Script -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import {
      getFirestore,
      collection,
      getDocs,
      addDoc,
      updateDoc,
      deleteDoc,
      doc
    } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "AIzaSyAqFH3-in0fR8d4JAL_d7Mns0RuKcKgI6Y",
      authDomain: "tokyotravelapp-d35b1.firebaseapp.com",
      projectId: "tokyotravelapp-d35b1",
      storageBucket: "tokyotravelapp-d35b1.firebasestorage.app",
      messagingSenderId: "1049741111304",
      appId: "1:1049741111304:web:27bf74acebdca721e3a1bd"
    };

    let app, db;
    const firebaseStatusEl = document.getElementById("firebaseStatus");

    try {
      app = initializeApp(firebaseConfig);
      db = getFirestore(app);
      firebaseStatusEl.textContent = "已連線";
      firebaseStatusEl.style.color = "#bbf7d0";
    } catch (e) {
      console.error(e);
      firebaseStatusEl.textContent = "連線失敗";
      firebaseStatusEl.style.color = "#fecaca";
    }

    function updateClock() {
      const el = document.getElementById("statusTime");
      const now = new Date();
      const hh = String(now.getHours()).padStart(2, "0");
      const mm = String(now.getMinutes()).padStart(2, "0");
      el.textContent = `${hh}:${mm}`;
    }
    updateClock();
    setInterval(updateClock, 30_000);

    const tabs = document.querySelectorAll(".tab");
    const pages = document.querySelectorAll(".tab-page");
    let activeTab = "home";

    function setActiveTab(tabId) {
      activeTab = tabId;
      tabs.forEach(t => {
        t.classList.toggle("active", t.dataset.tab === tabId);
      });
      pages.forEach(p => {
        p.classList.toggle("active", p.id === tabId);
      });
    }

    tabs.forEach(tab => {
      tab.addEventListener("click", () => {
        setActiveTab(tab.dataset.tab);
      });
    });

    let startX = 0;
    let isSwiping = false;

    const mainEl = document.getElementById("mainContainer");

    mainEl.addEventListener("touchstart", (e) => {
      if (e.touches.length !== 1) return;
      startX = e.touches[0].clientX;
      isSwiping = true;
    });

    mainEl.addEventListener("touchmove", (e) => {
      if (!isSwiping) return;
    });

    mainEl.addEventListener("touchend", (e) => {
      if (!isSwiping) return;
      const endX = e.changedTouches[0].clientX;
      const diff = endX - startX;
      isSwiping = false;

      if (Math.abs(diff) < 40) return;

      const order = ["home", "itinerary", "expense", "lists"];
      const idx = order.indexOf(activeTab);
      if (idx === -1) return;

      if (diff < 0 && idx < order.length - 1) {
        setActiveTab(order[idx + 1]);
      } else if (diff > 0 && idx > 0) {
        setActiveTab(order[idx - 1]);
      }
    });

    // ---------------- 匯率試算 ----------------
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
        rateResultJpy.textContent = jpy.toFixed(0) + " 円";
      } else {
        rateResultJpy.textContent = "—";
      }
    }

    rateTwdPerJpy.addEventListener("input", () => {
      updateFromJpy();
      updateFromTwd();
    });
    rateJpyInput.addEventListener("input", updateFromJpy);
    rateTwdInput.addEventListener("input", updateFromTwd);

    // ---------------- 天氣資料 ----------------
    const weatherUpdated = document.getElementById("weatherUpdated");
    const currentWeatherEl = document.getElementById("currentWeather");
    const currentAirEl = document.getElementById("currentAir");
    const weeklyWeatherBody = document.getElementById("weeklyWeatherBody");

    const TOKYO_LAT = 35.6895;
    const TOKYO_LON = 139.6917;

    const weatherCodeMap = {
      0: "晴朗",
      1: "大致晴朗",
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

    async function fetchWeather() {
      const wUrl =
        `https://api.open-meteo.com/v1/forecast?latitude=${TOKYO_LAT}&longitude=${TOKYO_LON}` +
        `&current=temperature_2m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min,sunset_time,snowfall_sum&timezone=Asia%2FTokyo`;
      const aUrl =
        `https://air-quality-api.open-meteo.com/v1/air-quality?latitude=${TOKYO_LAT}&longitude=${TOKYO_LON}` +
        `&current=european_aqi,pm2_5&timezone=Asia%2FTokyo`;

      let aqi = null;
      let pm25 = null;

      try {
        const aRes = await fetch(aUrl);
        if (aRes.ok) {
          const aData = await aRes.json();
          const current = aData.current || {};
          aqi = current.european_aqi ?? (Array.isArray(current.european_aqi) ? current.european_aqi[0] : null);
          pm25 = current.pm2_5 ?? (Array.isArray(current.pm2_5) ? current.pm2_5[0] : null);

          if (aqi != null) {
            let levelLabel = "";
            if (aqi <= 50) levelLabel = "（良好）";
            else if (aqi <= 100) levelLabel = "（普通）";
            else if (aqi <= 150) levelLabel = "（對敏感族群不健康）";
            else levelLabel = "（空氣品質較差，注意防護）";

            currentAirEl.innerHTML =
              `空氣品質 AQI：<strong>${aqi}</strong> ${levelLabel}<br>PM2.5：約 <strong>${pm25 != null ? pm25.toFixed(1) : "—"}</strong> μg/m³`;
          } else {
            currentAirEl.textContent = "空氣品質：暫時無法取得資料";
          }
        } else {
          currentAirEl.textContent = "空氣品質：暫時無法取得資料";
        }
      } catch (err) {
        console.error("AQ error", err);
        currentAirEl.textContent = "空氣品質：暫時無法取得資料";
      }

      try {
        const wRes = await fetch(wUrl);
        if (!wRes.ok) throw new Error("weather fetch failed");
        const wData = await wRes.json();

        const now = wData.current || {};
        const daily = wData.daily || {};
        const times = daily.time || [];

        const wText = weatherCodeMap[now.weather_code] || "天氣";

        currentWeatherEl.innerHTML =
          `<div class="flex-between">
            <div>
              <div style="font-size:18px;font-weight:600;">東京現在：${wText}</div>
              <div class="mt-4 small-text">氣溫約 <strong>${now.temperature_2m ?? "—"}°C</strong></div>
            </div>
          </div>`;

        weatherUpdated.textContent = "已更新";

        weeklyWeatherBody.innerHTML = "";
        for (let i = 0; i < times.length; i++) {
          const date = times[i];
          const code = daily.weather_code?.[i];
          const maxT = daily.temperature_2m_max?.[i];
          const minT = daily.temperature_2m_min?.[i];
          const sunsetRaw = daily.sunset_time?.[i] || "";
          const sunset = sunsetRaw ? sunsetRaw.substring(11, 16) : "—";
          const snow = daily.snowfall_sum?.[i] ?? 0;
          const hasSnow = snow > 0 ? "可能" : "否";
          const desc = weatherCodeMap[code] || "—";

          const tr = document.createElement("tr");
          tr.innerHTML = `
            <td>${date}</td>
            <td>${desc}</td>
            <td>${maxT != null ? maxT.toFixed(1) + "°" : "—"}</td>
            <td>${minT != null ? minT.toFixed(1) + "°" : "—"}</td>
            <td>${sunset}</td>
            <td>${aqi ?? "—"}</td>
            <td>${hasSnow}</td>
          `;
          weeklyWeatherBody.appendChild(tr);
        }

        if (!times.length) {
          weeklyWeatherBody.innerHTML = `<tr><td colspan="7">暫時沒有預報資料</td></tr>`;
        }
      } catch (e) {
        console.error("Weather error", e);
        weatherUpdated.textContent = "更新失敗";
        weeklyWeatherBody.innerHTML = `<tr><td colspan="7">天氣資料暫時無法取得</td></tr>`;
      }
    }

    // ---------------- 常用日語（複製＋語音） ----------------
    function setupPhrases() {
      const rows = document.querySelectorAll(".phrase-row");
      rows.forEach(row => {
        const jp = row.getAttribute("data-phrase") || "";
        const copyBtn = row.querySelector(".copy-btn");
        const speakBtn = row.querySelector(".speak-btn");

        if (copyBtn) {
          copyBtn.addEventListener("click", async () => {
            try {
              await navigator.clipboard.writeText(jp);
              copyBtn.textContent = "✅";
              setTimeout(() => (copyBtn.textContent = "📋"), 800);
            } catch (e) {
              alert("複製失敗，請手動長按選取。");
            }
          });
        }

        if (speakBtn) {
          speakBtn.addEventListener("click", () => {
            if (!("speechSynthesis" in window)) {
              alert("此瀏覽器不支援語音播放。");
              return;
            }
            const utter = new SpeechSynthesisUtterance(jp);
            utter.lang = "ja-JP";
            speechSynthesis.cancel();
            speechSynthesis.speak(utter);
          });
        }
      });
    }

    // ---------------- 行程資料 ----------------
    const itineraryListEl = document.getElementById("itineraryList");

    const itineraryData = [
      {
        date: "12/26（五） 上野",
        items: [
          {
            time: "14:20",
            title: "抵達成田機場 (NRT) T2",
            desc: "抵達成田國際機場第 2 航廈，辦理入境、提領行李以及兌換外幣與交通票券。",
            hours: "機場 24 小時營運",
            map: "https://maps.google.com/?q=Narita%20Airport%20Terminal%202"
          },
          {
            time: "16:00",
            title: "飯店 Check-in：上野站前1號遊客酒店",
            desc: "前往上野站附近飯店放行李、稍作休息，熟悉周邊環境與便利商店位置。",
            hours: "一般 Check-in 約 15:00 起，依飯店公告為準",
            map: "https://maps.google.com/?q=Taito-ku%20Higashiueno%202-18-18%20Hotel"
          },
          {
            time: "18:00",
            title: "晚餐：阿美橫丁周邊",
            desc: "阿美橫丁商店街有各式居酒屋、拉麵、丼飯與小吃，是感受下班後庶民氛圍的好地方。",
            hours: "多數店家約 11:00–23:00（依各店為準）",
            map: "https://maps.google.com/?q=Ameyoko%20Ueno"
          },
          {
            time: "20:00",
            title: "購物：無印良品 上野丸井店、OS Drug 藥妝店",
            desc: "採買日常小物與藥妝，適合順便把伴手禮與旅途會用到的用品一次買齊。",
            hours: "百貨與藥妝多為 10:00–21:00（依店家公告）",
            map: "https://maps.google.com/?q=Muji%20Ueno%20Marui"
          }
        ]
      },
      {
        date: "12/27（六） 東京・銀座",
        items: [
          {
            time: "09:00",
            title: "Tricolore Coffee（早餐：蘋果派、閃電泡芙）",
            desc: "銀座老牌咖啡館，西式甜點與咖啡皆很有水準，適合慢慢享用早餐。",
            hours: "一般營業時間約 8:00–22:00（依店家為準）",
            map: "https://maps.google.com/?q=Tricolore%20Coffee%20Ginza"
          },
          {
            time: "12:00",
            title: "牛たんの檸檬 有楽町店",
            desc: "主打厚切牛舌套餐，搭配白飯與小菜，是非常飽足的一餐。",
            hours: "午餐時段多為 11:00–15:00，晚餐 17:00–22:00 左右",
            map: "https://maps.google.com/?q=%E7%89%9B%E3%81%9F%E3%82%93%E3%81%AE%E6%AA%AC%E6%9C%89%E6%A5%BD%E7%94%BA"
          },
          {
            time: "15:00",
            title: "MARLOWE 焦糖布丁",
            desc: "知名玻璃杯布丁品牌，焦糖香氣濃郁，也是人氣伴手禮選擇。",
            hours: "多數門市約 10:00–20:00（依店鋪公告）",
            map: "https://maps.google.com/?q=MARLOWE%20Pudding%20Tokyo"
          },
          {
            time: "20:30",
            title: "東京・台場海濱公園花火／惠比壽花園廣場燈光秀",
            desc: "晚上欣賞海景煙火或冬季燈飾，感受東京夜景與浪漫氣氛。",
            hours: "花火／燈飾多為晚上舉行，依官方公告為準",
            map: "https://maps.google.com/?q=Odaiba%20Seaside%20Park"
          }
        ]
      },
      {
        date: "12/28（日） 富士山一日遊",
        items: [
          {
            time: "08:00",
            title: "東京站 丸之內南口集合",
            desc: "在東京站丸之內南口集合，搭乘一日遊巴士前往富士山周邊景點。",
            hours: "依旅行社集合時間為主",
            map: "https://maps.google.com/?q=Tokyo%20Station%20Marunouchi%20South"
          },
          {
            time: "10:30",
            title: "新倉山淺間公園",
            desc: "經典「五重塔＋富士山」構圖地點，天氣好時可以拍到明信片等級美照。",
            hours: "公園全天開放，夜間請注意安全",
            map: "https://maps.google.com/?q=Arakurayama%20Sengen%20Park"
          },
          {
            time: "11:45",
            title: "日川時計店",
            desc: "位於富士吉田的在地鐘錶老店，近年因網路分享而成為人氣拍照點。",
            hours: "多為 9:00–18:00（依店家為準）",
            map: "https://maps.google.com/?q=%E6%97%A5%E5%B7%9D%E6%99%82%E8%A8%88%E5%BA%97"
          },
          {
            time: "12:30",
            title: "忍野八海（含午餐）",
            desc: "湧泉池被列為日本名水之一，可一邊散步一邊享用蕎麥麵、烤仙貝等在地小吃。",
            hours: "店家多為 9:00–17:00 左右",
            map: "https://maps.google.com/?q=Oshino%20Hakkai"
          },
          {
            time: "15:20",
            title: "大石公園",
            desc: "河口湖畔賞花與拍攝富士山的熱門地點，視季節可見薰衣草、波斯菊等花海。",
            hours: "公園全天開放，咖啡廳多至傍晚",
            map: "https://maps.google.com/?q=Oishi%20Park%20Kawaguchiko"
          },
          {
            time: "18:50",
            title: "返回東京市區",
            desc: "傍晚返回東京市區，結束富士山周邊一日遊行程。",
            hours: "依旅行社行程表為主",
            map: "https://maps.google.com/?q=Tokyo"
          }
        ]
      },
      {
        date: "12/29（一） 東京・澀谷",
        items: [
          {
            time: "11:30",
            title: "壽喜燒：Sukiyaki Juni Ten",
            desc: "可品嚐日式壽喜燒或和牛鍋物，適合中午補充體力。",
            hours: "多為 11:00–15:00、17:00–22:00",
            map: "https://maps.google.com/?q=Sukiyaki%20Juni%20Ten"
          },
          {
            time: "14:30",
            title: "東急 Plaza 表參道原宿",
            desc: "時尚百貨與選物店林立，頂樓露台也很適合拍照與休息。",
            hours: "多為 11:00–21:00（依百貨公告）",
            map: "https://maps.google.com/?q=Tokyu%20Plaza%20Omotesando%20Harajuku"
          },
          {
            time: "19:30",
            title: "中目黑散步",
            desc: "沿著目黑川散步，周邊咖啡館與小店林立，晚上氣氛悠閒。",
            hours: "街區自由散步，部分店家營業至 22:00 左右",
            map: "https://maps.google.com/?q=Nakameguro"
          }
        ]
      },
      {
        date: "12/30（二） 新宿・秋葉原",
        items: [
          {
            time: "11:00",
            title: "NEWoMan / 高島屋周邊逛街",
            desc: "新宿車站南口週邊百貨林立，可集中採買服飾、雜貨與生活用品。",
            hours: "百貨多為 10:00–20:30（依各館公告）",
            map: "https://maps.google.com/?q=NEWoMan%20Shinjuku"
          },
          {
            time: "18:30",
            title: "二木菓子（買伴手禮）",
            desc: "知名零食批發店，適合大量採買糖果餅乾帶回台灣。",
            hours: "多為 10:00–20:00 左右",
            map: "https://maps.google.com/?q=Futaki%20Gashi"
          }
        ]
      },
      {
        date: "12/31（三） 成田市",
        items: [
          {
            time: "09:30",
            title: "成田山新勝寺",
            desc: "有悠久歷史的寺院，也是日本新年的參拜熱門地點之一。",
            hours: "境內多為清晨至傍晚開放",
            map: "https://maps.google.com/?q=Naritasan%20Shinshoji"
          },
          {
            time: "10:30",
            title: "成田山表參道",
            desc: "兩旁老舖林立，可品嚐鰻魚飯、和菓子與日式點心。",
            hours: "店家多為 10:00–17:00 左右",
            map: "https://maps.google.com/?q=Naritasan%20Omotesando"
          },
          {
            time: "11:30",
            title: "成田夢牧場 門前店",
            desc: "以牛奶與霜淇淋聞名的小店，適合簡單下午茶或點心時間。",
            hours: "多為 10:00–17:00（依店家為準）",
            map: "https://maps.google.com/?q=Narita%20Yume%20Bokujou%20Monzen"
          },
          {
            time: "12:30",
            title: "成田機場 (NRT) 辦理登機",
            desc: "預留足夠時間辦理退稅、托運行李與安檢，準備返程回台灣。",
            hours: "建議國際線起飛前 2–3 小時抵達機場",
            map: "https://maps.google.com/?q=Narita%20Airport"
          }
        ]
      }
    ];

    function renderItinerary() {
      itineraryListEl.innerHTML = "";
      itineraryData.forEach(day => {
        const wrap = document.createElement("div");
        wrap.className = "item-card";

        const header = document.createElement("div");
        header.className = "item-header";
        header.innerHTML = `<div class="item-title">${day.date}</div>`;
        wrap.appendChild(header);

        day.items.forEach(item => {
          const row = document.createElement("div");
          row.className = "mt-4 small-text";
          row.innerHTML =
            `<div>
              <div><strong>${item.time}</strong>　${item.title}</div>
              ${item.desc ? `<div class="mt-2">${item.desc}</div>` : ""}
              ${item.hours ? `<div class="mt-2"><strong>營業時間：</strong>${item.hours}</div>` : ""}
            </div>
            <div class="flex mt-4">
              <button class="secondary" type="button">導航</button>
            </div>`;
          const btn = row.querySelector("button");
          btn.addEventListener("click", () => {
            window.open(item.map, "_blank");
          });
          wrap.appendChild(row);
        });

        itineraryListEl.appendChild(wrap);
      });
    }

    // ---------------- Helpers for Firestore lists ----------------
    function fileListToBase64Array(fileInput, maxCount = 3) {
      const files = Array.from(fileInput.files || []).slice(0, maxCount);
      if (!files.length) return Promise.resolve([]);

      let tooLarge = false;
      const readers = files.map(
        file =>
          new Promise((resolve, reject) => {
            const maxSize = 300 * 1024;
            if (file.size > maxSize) {
              tooLarge = true;
              return resolve(null);
            }
            const reader = new FileReader();
            reader.onload = () => resolve(reader.result);
            reader.onerror = reject;
            reader.readAsDataURL(file);
          })
      );

      return Promise.all(readers).then(results => {
        const filtered = results.filter(Boolean);
        if (tooLarge) {
          alert("有部分照片檔案過大（超過約 300KB），已自動略過未上傳，以免同步失敗。");
        }
        return filtered;
      });
    }

    // ---------------- 記帳 Firestore ----------------
    async function setupExpense() {
      const col = collection(db, "expenses");
      const form = document.getElementById("expenseForm");
      const listEl = document.getElementById("expenseList");
      const status = document.getElementById("expenseStatus");
      const submitBtn = document.getElementById("expenseSubmitBtn");
      let editingId = null;

      async function load() {
        status.textContent = "同步中…";
        const snap = await getDocs(col);
        listEl.innerHTML = "";
        const docsArr = [];
        snap.forEach(d => docsArr.push({ id: d.id, ...d.data() }));
        docsArr.sort((a,b) => (a.date || "").localeCompare(b.date || ""));
        docsArr.forEach(d => {
          const div = document.createElement("div");
          div.className = "item-card";
          const header = document.createElement("div");
          header.className = "item-header";
          header.innerHTML = `
            <div>
              <div class="item-title">${d.title || "(未命名)"}</div>
              <div class="small-text">${d.date || ""}</div>
            </div>
            <div class="item-meta">
              <div>${(d.amount || 0).toLocaleString()} ${d.currency || ""}</div>
            </div>
          `;
          div.appendChild(header);

          if (d.note) {
            const note = document.createElement("div");
            note.className = "small-text";
            note.textContent = d.note;
            div.appendChild(note);
          }

          if (Array.isArray(d.photos) && d.photos.length) {
            const photosDiv = document.createElement("div");
            photosDiv.className = "photos";
            d.photos.forEach(url => {
              const img = document.createElement("img");
              img.src = url;
              photosDiv.appendChild(img);
            });
            div.appendChild(photosDiv);
          }

          const btnRow = document.createElement("div");
          btnRow.className = "btn-row";
          const editBtn = document.createElement("button");
          editBtn.className = "secondary";
          editBtn.type = "button";
          editBtn.textContent = "編輯";
          const delBtn = document.createElement("button");
          delBtn.className = "danger";
          delBtn.type = "button";
          delBtn.textContent = "刪除";
          btnRow.appendChild(editBtn);
          btnRow.appendChild(delBtn);
          div.appendChild(btnRow);

          editBtn.addEventListener("click", () => {
            editingId = d.id;
            form.expenseDate.value = d.date || "";
            form.expenseTitle.value = d.title || "";
            form.expenseAmount.value = d.amount || "";
            form.expenseCurrency.value = d.currency || "JPY";
            form.expenseNote.value = d.note || "";
            submitBtn.textContent = "更新記帳";
          });

          delBtn.addEventListener("click", async () => {
            if (!confirm("確定要刪除此筆記帳？")) return;
            await deleteDoc(doc(db, "expenses", d.id));
            await load();
          });

          listEl.appendChild(div);
        });
        status.textContent = "已同步";
      }

      form.addEventListener("submit", async (e) => {
        e.preventDefault();
        submitBtn.disabled = true;
        const isEditing = !!editingId;
        submitBtn.textContent = isEditing ? "更新中…" : "新增中…";

        try {
          const data = {
            date: form.expenseDate.value || "",
            title: form.expenseTitle.value || "",
            amount: parseFloat(form.expenseAmount.value) || 0,
            currency: form.expenseCurrency.value,
            note: form.expenseNote.value || ""
          };

          const photos = await fileListToBase64Array(form.expensePhotos, 3);
          if (photos.length) data.photos = photos;

          if (editingId) {
            const ref = doc(db, "expenses", editingId);
            await updateDoc(ref, data);
          } else {
            await addDoc(col, data);
          }

          editingId = null;
          form.reset();
          await load();
        } catch (err) {
          console.error(err);
          alert("記帳資料儲存時發生錯誤，可能是照片檔案過大或網路不穩，請稍後再試。");
        } finally {
          submitBtn.disabled = false;
          submitBtn.textContent = "新增記帳";
        }
      });

      await load();
    }

    // ---------------- 購物清單 Firestore ----------------
    async function setupShopping() {
      const col = collection(db, "shopping");
      const form = document.getElementById("shoppingForm");
      const listEl = document.getElementById("shoppingList");
      const submitBtn = document.getElementById("shoppingSubmitBtn");
      let editingId = null;

      async function load() {
        const snap = await getDocs(col);
        listEl.innerHTML = "";
        const docsArr = [];
        snap.forEach(d => docsArr.push({id:d.id, ...d.data()}));
        docsArr.forEach(d => {
          const div = document.createElement("div");
          div.className = "item-card";
          const header = document.createElement("div");
          header.className = "item-header";
          header.innerHTML = `
            <div>
              <div class="item-title">${d.title || "(未命名)"}</div>
              <div class="small-text">${d.note || ""}</div>
            </div>
            <div class="item-meta">
              <div>${(d.amount || 0).toLocaleString()} ${d.currency || ""}</div>
            </div>
          `;
          div.appendChild(header);

          if (Array.isArray(d.photos) && d.photos.length) {
            const photosDiv = document.createElement("div");
            photosDiv.className = "photos";
            d.photos.forEach(url => {
              const img = document.createElement("img");
              img.src = url;
              photosDiv.appendChild(img);
            });
            div.appendChild(photosDiv);
          }

          const btnRow = document.createElement("div");
          btnRow.className = "btn-row";
          const editBtn = document.createElement("button");
          editBtn.className = "secondary";
          editBtn.type = "button";
          editBtn.textContent = "編輯";
          const delBtn = document.createElement("button");
          delBtn.className = "danger";
          delBtn.type = "button";
          delBtn.textContent = "刪除";
          btnRow.appendChild(editBtn);
          btnRow.appendChild(delBtn);
          div.appendChild(btnRow);

          editBtn.addEventListener("click", () => {
            editingId = d.id;
            form.shoppingTitle.value = d.title || "";
            form.shoppingAmount.value = d.amount || "";
            form.shoppingCurrency.value = d.currency || "JPY";
            form.shoppingNote.value = d.note || "";
            submitBtn.textContent = "更新購物項目";
          });

          delBtn.addEventListener("click", async () => {
            if (!confirm("確定要刪除此購物項目？")) return;
            await deleteDoc(doc(db, "shopping", d.id));
            await load();
          });

          listEl.appendChild(div);
        });
      }

      form.addEventListener("submit", async (e) => {
        e.preventDefault();
        submitBtn.disabled = true;
        const isEditing = !!editingId;
        submitBtn.textContent = isEditing ? "更新中…" : "新增中…";

        try {
          const data = {
            title: form.shoppingTitle.value || "",
            amount: parseFloat(form.shoppingAmount.value) || 0,
            currency: form.shoppingCurrency.value,
            note: form.shoppingNote.value || ""
          };

          const photos = await fileListToBase64Array(form.shoppingPhotos, 3);
          if (photos.length) data.photos = photos;

          if (editingId) {
            await updateDoc(doc(db, "shopping", editingId), data);
          } else {
            await addDoc(col, data);
          }

          editingId = null;
          form.reset();
          await load();
        } catch (err) {
          console.error(err);
          alert("購物清單儲存時發生錯誤，可能是照片檔案過大或網路不穩，請稍後再試。");
        } finally {
          submitBtn.disabled = false;
          submitBtn.textContent = "新增購物項目";
        }
      });

      await load();
    }

    // ---------------- 旅遊檢查清單 Firestore ----------------
    async function setupChecklist() {
      const col = collection(db, "checklist");
      const container = document.getElementById("checklistContainer");

      const defaultItems = [
        "護照 / 身分證 / 駕照",
        "信用卡 / 現金 / Suica / PASMO",
        "機票 / 行程確認信",
        "手機・充電線・行動電源",
        "藥品（腸胃藥、止痛藥、暈車藥）",
        "雨具（摺疊傘或雨衣）",
        "保暖衣物（圍巾、手套、帽子）",
        "國際轉接頭",
        "相機 / 記憶卡",
        "隱形眼鏡 / 眼鏡"
      ];

      async function ensureSeed() {
        const snap = await getDocs(col);
        if (!snap.empty) return;
        for (const title of defaultItems) {
          await addDoc(col, { title, done: false });
        }
      }

      async function load() {
        const snap = await getDocs(col);
        container.innerHTML = "";
        const docsArr = [];
        snap.forEach(d => docsArr.push({id:d.id, ...d.data()}));
        docsArr.forEach(d => {
          const item = document.createElement("div");
          item.className = "checklist-item" + (d.done ? " completed" : "");
          const checkbox = document.createElement("input");
          checkbox.type = "checkbox";
          checkbox.checked = d.done;
          const label = document.createElement("div");
          label.className = "small-text";
          label.textContent = d.title;

          checkbox.addEventListener("change", async () => {
            await updateDoc(doc(db, "checklist", d.id), { done: checkbox.checked });
            if (checkbox.checked) {
              item.classList.add("completed");
            } else {
              item.classList.remove("completed");
            }
          });

          item.appendChild(checkbox);
          item.appendChild(label);
          container.appendChild(item);
        });
      }

      await ensureSeed();
      await load();
    }

    // ---------------- App Init ----------------
    async function init() {
      try {
        await fetchWeather();
      } catch (e) {
        console.error("weather init error", e);
      }

      setupPhrases();

      if (db) {
        await Promise.all([
          setupExpense(),
          setupShopping(),
          setupChecklist()
        ]);
      }
      renderItinerary();
    }

    init();
  </script>
</body>
</html>
