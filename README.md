<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>東京旅遊助理 App v4</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --primary: #166534; /* 墨綠 */
      --primary-light: #22c55e;
      --bg: #0f172a;
      --card: #0b1120;
      --text: #e5e7eb;
      --muted: #9ca3af;
      --accent: #facc15;
      --danger: #f97373;
      --border: #1f2937;
      --shadow-soft: 0 10px 30px rgba(0,0,0,0.45);
      --radius-lg: 18px;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #022c22 0, #020617 45%, #000 100%);
      color: var(--text);
      font-size: 18px;
    }

    .app {
      max-width: 540px;
      margin: 0 auto;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      padding: 12px 8px 20px;
    }

    header {
      padding: 10px 14px 6px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
    }

    header h1 {
      font-size: 22px;
      margin: 0;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    header h1 span.emoji {
      font-size: 24px;
    }

    header .subtitle {
      font-size: 12px;
      color: var(--muted);
    }

    .tabs {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 6px;
      margin: 6px 4px 10px;
      padding: 4px;
      background: rgba(15,23,42,0.9);
      border-radius: 999px;
      box-shadow: 0 12px 30px rgba(0,0,0,0.6);
      position: sticky;
      top: 0;
      z-index: 10;
      backdrop-filter: blur(12px);
    }

    .tab-btn {
      border: none;
      border-radius: 999px;
      padding: 6px 2px;
      font-size: 13px;
      background: transparent;
      color: var(--muted);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 4px;
      cursor: pointer;
    }

    .tab-btn span.icon {
      font-size: 14px;
    }

    .tab-btn.active {
      background: linear-gradient(135deg, var(--primary), var(--primary-light));
      color: #ecfdf5;
      font-weight: 600;
    }

    main {
      flex: 1;
      margin-top: 6px;
      position: relative;
      overflow: hidden;
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
      from { opacity: 0; transform: translateY(4px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .grid {
      display: grid;
      gap: 10px;
    }

    .grid-2 {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .card {
      background: radial-gradient(circle at top left, rgba(34,197,94,0.10), rgba(15,23,42,0.9));
      border-radius: var(--radius-lg);
      padding: 12px 12px 10px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(31,41,55,0.9);
    }

    .card-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
      gap: 8px;
    }

    .card-header h2 {
      font-size: 17px;
      margin: 0;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .chip {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 11px;
      padding: 2px 7px;
      border-radius: 999px;
      background: rgba(15,23,42,0.8);
      border: 1px solid rgba(55,65,81,0.9);
      color: var(--muted);
    }

    .label {
      display: block;
      font-size: 13px;
      color: var(--muted);
      margin-bottom: 4px;
    }

    input[type="text"],
    input[type="number"],
    input[type="date"],
    textarea,
    select {
      width: 100%;
      padding: 8px 9px;
      border-radius: 12px;
      border: 1px solid rgba(55,65,81,0.9);
      background: rgba(15,23,42,0.95);
      color: var(--text);
      font-size: 15px;
      outline: none;
    }

    textarea {
      resize: vertical;
      min-height: 60px;
    }

    input::placeholder,
    textarea::placeholder {
      color: #6b7280;
    }

    button.primary {
      width: 100%;
      margin-top: 6px;
      padding: 9px 10px;
      border-radius: 999px;
      border: none;
      background: linear-gradient(135deg, var(--primary), var(--primary-light));
      color: #ecfdf5;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
      box-shadow: 0 12px 25px rgba(16,185,129,0.5);
    }

    button.secondary {
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid rgba(75,85,99,0.9);
      background: rgba(15,23,42,0.9);
      color: var(--muted);
      font-size: 12px;
      cursor: pointer;
    }

    button.danger {
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(220,38,38,0.9);
      background: rgba(127,29,29,0.8);
      color: #fee2e2;
      font-size: 12px;
      cursor: pointer;
    }

    .small-text {
      font-size: 12px;
      color: var(--muted);
    }

    .big-number {
      font-size: 24px;
      font-weight: 700;
    }

    .flex {
      display: flex;
      gap: 8px;
      align-items: center;
      flex-wrap: wrap;
    }

    .flex-between {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
    }

    .mt-4 { margin-top: 4px; }
    .mt-6 { margin-top: 6px; }
    .mt-8 { margin-top: 8px; }

    .table-scroll {
      overflow-x: auto;
      margin-top: 6px;
    }

    table {
      border-collapse: collapse;
      width: 100%;
      min-width: 520px;
      font-size: 13px;
    }

    th, td {
      border: 1px solid rgba(55,65,81,0.9);
      padding: 4px 6px;
      text-align: center;
    }

    th {
      background: rgba(15,23,42,0.95);
      position: sticky;
      top: 0;
      z-index: 1;
    }

    img.map-img,
    img.fuji-thumb {
      width: 100%;
      border-radius: 14px;
      border: 1px solid rgba(55,65,81,0.9);
      display: block;
    }

    .pill {
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 11px;
      background: rgba(15,23,42,0.9);
      color: var(--muted);
      border: 1px solid rgba(55,65,81,0.9);
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    .list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 6px;
    }

    .item-card {
      border-radius: 14px;
      padding: 8px 10px;
      background: rgba(15,23,42,0.95);
      border: 1px solid rgba(55,65,81,0.9);
    }

    .item-header {
      display: flex;
      justify-content: space-between;
      gap: 8px;
      align-items: center;
    }

    .item-title {
      font-size: 15px;
      font-weight: 600;
    }

    .tag {
      font-size: 11px;
      padding: 1px 7px;
      border-radius: 999px;
      background: rgba(15,118,110,0.8);
      color: #a7f3d0;
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
      width: 18px;
      height: 18px;
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
    }

    .icon-btn {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      border: none;
      background: rgba(31,41,55,0.95);
      color: #e5e7eb;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      cursor: pointer;
    }

    .icon-btn.copy {
      font-size: 15px;
    }

    .accent-link {
      color: #a5f3fc;
      text-decoration: none;
      font-size: 14px;
    }
    .accent-link:hover { text-decoration: underline; }

    .badge-danger {
      background: rgba(127,29,29,0.9);
      color: #fecaca;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 11px;
    }

    .badge-ok {
      background: rgba(22,101,52,0.9);
      color: #bbf7d0;
      padding: 2px 8px;
      border-radius: 999px;
      font-size: 11px;
    }

    @media (max-width: 400px) {
      body { font-size: 17px; }
      header h1 { font-size: 20px; }
      .tab-btn { font-size: 12px; }
    }
  </style>
</head>
<body>
<div class="app" id="app">
  <header>
    <div>
      <h1><span class="emoji">🗼</span>東京旅遊助理</h1>
      <div class="subtitle">行程 · 記帳 · 檢查清單 · 富士山</div>
    </div>
    <div class="chip">
      <span>主題</span> <span style="width:10px;height:10px;border-radius:999px;background:#15803d;"></span>
    </div>
  </header>

  <nav class="tabs">
    <button class="tab-btn active" data-tab="home"><span class="icon">🏠</span>首頁</button>
    <button class="tab-btn" data-tab="itinerary"><span class="icon">🗺️</span>行程</button>
    <button class="tab-btn" data-tab="account"><span class="icon">💰</span>記帳</button>
    <button class="tab-btn" data-tab="lists"><span class="icon">📝</span>清單</button>
  </nav>

  <main id="main">
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
          <div class="mt-6 small-text" id="currentAir">
            空氣品質：讀取中…
          </div>
          <div class="mt-6">
            <div class="small-text" style="margin-bottom:4px;">未來一週預報（含日落 / AQI / 是否可能下雪）</div>
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
          </div>
        </section>

        <!-- 富士山 -->
        <section class="card">
          <div class="card-header">
            <h2>富士山直播縮圖 <span>🗻</span></h2>
            <span class="chip">Cloudflare Worker</span>
          </div>
          <a href="https://fuji-san.info/zh-tw/index.html" target="_blank" rel="noopener">
            <img src="https://fuji-proxy.a171487.workers.dev/" alt="富士山即時影像縮圖" class="fuji-thumb" />
          </a>
          <div class="small-text mt-4">
            縮圖每次開啟頁面時即時抓取。點擊圖片前往富士山即時影像與詳細資訊。
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
                <li>駐日代表處：+81-3-3280-7811</li>
                <li>急難：+81-80-1009-5909</li>
              </ul>
            </div>
          </div>
          <div class="mt-6">
            <div class="pill">🩺 OHDr. 中文線上門診</div>
            <div class="small-text mt-4">
              點擊下方連結，直接加入 **OHDr. LINE 中文版官方帳號**，旅途中可用中文線上看診。
            </div>
            <a class="accent-link" href="https://line.me/R/ti/p/@406vicce" target="_blank" rel="noopener">
              🔗 加入 OHDr. 中文版 LINE 官方帳號
            </a>
          </div>
        </section>

        <!-- 常用日語 -->
        <section class="card">
          <div class="card-header">
            <h2>常用日語 <span>🗣️</span></h2>
            <span class="chip">點擊發音 & 複製</span>
          </div>
          <div id="phrases"></div>
        </section>
      </div>
    </section>

    <!-- 行程 -->
    <section class="tab-page" id="itinerary">
      <div class="grid">
        <!-- 地鐵與 JR、Skyliner -->
        <section class="card">
          <div class="card-header">
            <h2>交通路線圖 <span>🚇</span></h2>
          </div>
          <div class="grid">
            <div>
              <div class="label">東京地鐵路線圖（示意）</div>
              <a href="https://www.gotokyo.org/shared/img/2023_tokyo_subway_map_en.png" target="_blank" rel="noopener">
                <img class="map-img" src="https://www.gotokyo.org/shared/img/2023_tokyo_subway_map_en.png" alt="Tokyo Subway Map" />
              </a>
              <div class="small-text mt-4">
                ↑ 點擊可放大檢視。官方完整版 PDF 可至 Tokyo Metro 官網下載。
              </div>
            </div>
            <div>
              <div class="label">JR 主要路線圖</div>
              <a href="https://ontheworldmap.com/japan/city/tokyo/tokyo-jr-map.jpg" target="_blank" rel="noopener">
                <img class="map-img" src="https://ontheworldmap.com/japan/city/tokyo/tokyo-jr-map.jpg" alt="Tokyo JR Map" />
              </a>
            </div>
            <div>
              <div class="label">Skyliner 路線示意</div>
              <a href="https://www.keisei.co.jp/keisei/tetudou/skyliner/cn/image/skyliner_route_map.png" target="_blank" rel="noopener">
                <img class="map-img" src="https://www.keisei.co.jp/keisei/tetudou/skyliner/cn/image/skyliner_route_map.png" alt="Skyliner Route Map" />
              </a>
            </div>
          </div>
        </section>

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
    <section class="tab-page" id="account">
      <div class="grid">
        <section class="card">
          <div class="card-header">
            <h2>記帳本 <span>💰</span></h2>
            <span class="chip">Firebase 同步</span>
          </div>

          <form id="expenseForm">
            <label class="label">日期</label>
            <input type="date" id="expenseDate" required />

            <label class="label mt-6">項目名稱</label>
            <input type="text" id="expenseTitle" placeholder="例如：晚餐、伴手禮" required />

            <div class="grid grid-2 mt-6">
              <div>
                <label class="label">金額</label>
                <input type="number" id="expenseAmount" step="0.01" required />
              </div>
              <div>
                <label class="label">幣別</label>
                <select id="expenseCurrency">
                  <option value="JPY">日圓（JPY）</option>
                  <option value="TWD">台幣（TWD）</option>
                </select>
              </div>
            </div>

            <label class="label mt-6">備註</label>
            <textarea id="expenseNote" placeholder="可記錄消費內容、店名等"></textarea>

            <label class="label mt-6">照片（最多 3 張）</label>
            <input type="file" id="expensePhotos" accept="image/*" multiple />

            <button type="submit" class="primary" id="expenseSubmitBtn">新增記帳</button>
            <div class="small-text mt-4">
              ✓ 新增 / 編輯 / 刪除都會自動同步到 Firebase，換手機或重新整理也不會不見。
            </div>
          </form>
        </section>

        <section class="card">
          <div class="card-header">
            <h2>記帳列表</h2>
            <span class="chip" id="expenseStatus">讀取中…</span>
          </div>
          <div id="expenseList" class="list"></div>
        </section>
      </div>
    </section>

    <!-- 清單 / 檢查清單 / 購物 / 飯店 -->
    <section class="tab-page" id="lists">
      <div class="grid">
        <!-- 旅遊檢查清單 -->
        <section class="card">
          <div class="card-header">
            <h2>旅遊檢查清單 <span>📋</span></h2>
            <span class="chip">多人同步</span>
          </div>
          <form id="checklistForm">
            <label class="label">新增待辦項目</label>
            <input type="text" id="checklistInput" placeholder="例如：護照、外幣、轉接頭…" />
            <button class="primary" type="submit">新增項目</button>
          </form>
          <div id="checklist" class="list mt-6"></div>
        </section>

        <!-- 購物清單 -->
        <section class="card">
          <div class="card-header">
            <h2>購物清單 <span>🛍️</span></h2>
            <span class="chip">含金額 & 照片</span>
          </div>
          <form id="shoppingForm">
            <label class="label">品項名稱</label>
            <input type="text" id="shoppingTitle" placeholder="例如：藥妝、餅乾、伴手禮…" required />

            <div class="grid grid-2 mt-6">
              <div>
                <label class="label">預算金額</label>
                <input type="number" id="shoppingAmount" step="0.01" />
              </div>
              <div>
                <label class="label">幣別</label>
                <select id="shoppingCurrency">
                  <option value="JPY">日圓（JPY）</option>
                  <option value="TWD">台幣（TWD）</option>
                </select>
              </div>
            </div>

            <label class="label mt-6">備註</label>
            <textarea id="shoppingNote" placeholder="例如：哪間店比較便宜、要買幾盒"></textarea>

            <label class="label mt-6">照片（最多 3 張）</label>
            <input type="file" id="shoppingPhotos" accept="image/*" multiple />

            <button type="submit" class="primary" id="shoppingSubmitBtn">新增購物項目</button>
          </form>

          <div id="shoppingList" class="list mt-6"></div>
        </section>

        <!-- 飯店資訊 -->
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
      </div>
    </section>
  </main>
</div>

<!-- 主程式：Firebase + UI 邏輯 -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.1/firebase-app.js";
  import {
    getFirestore,
    collection,
    addDoc,
    getDocs,
    updateDoc,
    deleteDoc,
    doc
  } from "https://www.gstatic.com/firebasejs/10.13.1/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "AIzaSyAqFH3-in0fR8d4JAL_d7Mns0RuKcKgI6Y",
    authDomain: "tokyotravelapp-d35b1.firebaseapp.com",
    projectId: "tokyotravelapp-d35b1",
    storageBucket: "tokyotravelapp-d35b1.firebasestorage.app",
    messagingSenderId: "1049741111304",
    appId: "1:1049741111304:web:27bf74acebdca721e3a1bd"
  };

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);

  // ---------------- Tab switch & swipe ----------------
  const tabButtons = Array.from(document.querySelectorAll(".tab-btn"));
  const pages = Array.from(document.querySelectorAll(".tab-page"));
  let currentTabIndex = 0;

  function setActiveTab(index) {
    currentTabIndex = index;
    const id = tabButtons[index].dataset.tab;
    tabButtons.forEach(btn => btn.classList.toggle("active", btn.dataset.tab === id));
    pages.forEach(p => p.classList.toggle("active", p.id === id));
  }

  tabButtons.forEach((btn, idx) => {
    btn.addEventListener("click", () => setActiveTab(idx));
  });

  // swipe
  let touchStartX = null;
  const mainEl = document.getElementById("main");
  mainEl.addEventListener("touchstart", e => {
    touchStartX = e.touches[0].clientX;
  }, {passive: true});

  mainEl.addEventListener("touchend", e => {
    if (touchStartX == null) return;
    const dx = e.changedTouches[0].clientX - touchStartX;
    if (Math.abs(dx) > 60) {
      if (dx < 0 && currentTabIndex < tabButtons.length - 1) setActiveTab(currentTabIndex + 1);
      if (dx > 0 && currentTabIndex > 0) setActiveTab(currentTabIndex - 1);
    }
    touchStartX = null;
  });

  // ---------------- 匯率試算 ----------------
  const rateTwdPerJpy = document.getElementById("rateTwdPerJpy");
  const rateJpyInput = document.getElementById("rateJpyInput");
  const rateResult = document.getElementById("rateResult");

  function updateRateResult() {
    const r = parseFloat(rateTwdPerJpy.value);
    const j = parseFloat(rateJpyInput.value);
    if (!isNaN(r) && !isNaN(j)) {
      const twd = j * r;
      rateResult.textContent = twd.toFixed(0) + " 元";
    } else {
      rateResult.textContent = "—";
    }
  }
  rateTwdPerJpy.addEventListener("input", updateRateResult);
  rateJpyInput.addEventListener("input", updateRateResult);

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
    try {
      const wUrl =
        `https://api.open-meteo.com/v1/forecast?latitude=${TOKYO_LAT}&longitude=${TOKYO_LON}` +
        `&current=temperature_2m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min,sunset_time,snowfall_sum&timezone=Asia%2FTokyo`;
      const aUrl =
        `https://air-quality-api.open-meteo.com/v1/air-quality?latitude=${TOKYO_LAT}&longitude=${TOKYO_LON}` +
        `&current=european_aqi,pm2_5&timezone=Asia%2FTokyo`;

      const [wRes, aRes] = await Promise.all([fetch(wUrl), fetch(aUrl)]);
      const wData = await wRes.json();
      const aData = await aRes.json();

      const now = wData.current;
      const daily = wData.daily;
      const aqi = aData?.current?.european_aqi;
      const pm25 = aData?.current?.pm2_5;

      const wText = weatherCodeMap[now.weather_code] || "天氣";
      currentWeatherEl.innerHTML =
        `<div class="flex-between">
          <div>
            <div style="font-size:18px;font-weight:600;">東京現在：${wText}</div>
            <div class="mt-4 small-text">體感溫度約 <strong>${now.temperature_2m}°C</strong></div>
          </div>
        </div>`;

      if (aqi != null) {
        let levelLabel = "";
        if (aqi <= 50) levelLabel = "（良好）";
        else if (aqi <= 100) levelLabel = "（普通）";
        else if (aqi <= 150) levelLabel = "（對敏感族群不健康）";
        else levelLabel = "（空氣品質較差，注意防護）";

        currentAirEl.innerHTML =
          `空氣品質 AQI：<strong>${aqi}</strong> ${levelLabel}<br>PM2.5：約 <strong>${pm25?.toFixed(1) ?? "—"}</strong> μg/m³`;
      } else {
        currentAirEl.textContent = "空氣品質：暫時無法取得資料";
      }

      weatherUpdated.textContent = "已更新";

      // weekly
      weeklyWeatherBody.innerHTML = "";
      for (let i = 0; i < daily.time.length; i++) {
        const date = daily.time[i];
        const code = daily.weather_code[i];
        const maxT = daily.temperature_2m_max[i];
        const minT = daily.temperature_2m_min[i];
        const sunset = daily.sunset_time[i].substring(11, 16);
        const snow = daily.snowfall_sum[i];
        const hasSnow = snow && snow > 0 ? "可能" : "否";
        const desc = weatherCodeMap[code] || "—";

        const tr = document.createElement("tr");
        tr.innerHTML = `
          <td>${date}</td>
          <td>${desc}</td>
          <td>${maxT.toFixed(1)}°</td>
          <td>${minT.toFixed(1)}°</td>
          <td>${sunset}</td>
          <td>${aqi ?? "—"}</td>
          <td>${hasSnow}</td>
        `;
        weeklyWeatherBody.appendChild(tr);
      }
    } catch (e) {
      console.error(e);
      weatherUpdated.textContent = "更新失敗";
      weeklyWeatherBody.innerHTML = `<tr><td colspan="7">天氣資料暫時無法取得</td></tr>`;
    }
  }

  // ---------------- 常用日語 ----------------
  const phraseData = [
    {
      title: "餐廳",
      items: [
        {zh:"不好意思，請問有空位嗎？", jp:"すみません、空いている席はありますか？"},
        {zh:"請給我菜單。", jp:"メニューをお願いします。"},
        {zh:"我想點這個。", jp:"これをお願いします。"},
        {zh:"可以幫我打包嗎？", jp:"テイクアウトできますか？"},
        {zh:"可以幫我少一點鹽嗎？", jp:"塩を少なめにしてもらえますか？"}
      ]
    },
    {
      title: "交通",
      items: [
        {zh:"到上野站要怎麼去？", jp:"上野駅にはどう行けばいいですか？"},
        {zh:"請問這班車有到成田機場嗎？", jp:"この電車は成田空港まで行きますか？"},
        {zh:"一張到新宿的車票，謝謝。", jp:"新宿までの切符を一枚お願いします。"},
        {zh:"請問在哪裡換車？", jp:"どこで乗り換えますか？"}
      ]
    },
    {
      title: "緊急",
      items: [
        {zh:"請幫我叫救護車。", jp:"救急車を呼んでください。"},
        {zh:"我身體不舒服。", jp:"体の具合が悪いです。"},
        {zh:"護照不見了。", jp:"パスポートをなくしました。"},
        {zh:"最近的警察局在哪裡？", jp:"一番近い交番はどこですか？"}
      ]
    },
    {
      title: "購物",
      items: [
        {zh:"這個多少錢？", jp:"これはいくらですか？"},
        {zh:"可以免稅嗎？", jp:"免税できますか？"},
        {zh:"可以幫我包裝成禮物嗎？", jp:"プレゼント用に包装してもらえますか？"},
        {zh:"有沒有其他顏色／尺寸？", jp:"ほかの色／サイズはありますか？"}
      ]
    },
    {
      title: "基本會話",
      items: [
        {zh:"你好／早安／晚安", jp:"こんにちは／おはようございます／こんばんは"},
        {zh:"謝謝。", jp:"ありがとうございます。"},
        {zh:"不好意思。", jp:"すみません。"},
        {zh:"對不起。", jp:"ごめんなさい。"},
        {zh:"可以說慢一點嗎？", jp:"もう少しゆっくり話してもらえますか？"}
      ]
    }
  ];

  const phrasesEl = document.getElementById("phrases");

  function renderPhrases() {
    phraseData.forEach(group => {
      const title = document.createElement("div");
      title.className = "phrase-group-title";
      title.textContent = "▸ " + group.title;
      phrasesEl.appendChild(title);

      group.items.forEach(item => {
        const row = document.createElement("div");
        row.className = "phrase-row";
        const textDiv = document.createElement("div");
        textDiv.className = "phrase-text";
        textDiv.innerHTML = `<div>${item.zh}</div><div class="small-text">${item.jp}</div>`;
        const speakBtn = document.createElement("button");
        speakBtn.className = "icon-btn";
        speakBtn.innerHTML = "🔊";
        speakBtn.addEventListener("click", () => {
          const u = new SpeechSynthesisUtterance(item.jp);
          u.lang = "ja-JP";
          speechSynthesis.speak(u);
        });
        const copyBtn = document.createElement("button");
        copyBtn.className = "icon-btn copy";
        copyBtn.innerHTML = "📋";
        copyBtn.addEventListener("click", async () => {
          try {
            await navigator.clipboard.writeText(item.jp);
            copyBtn.innerHTML = "✅";
            setTimeout(() => (copyBtn.innerHTML = "📋"), 800);
          } catch {
            alert("剪貼簿權限被阻擋，請手動複製。");
          }
        });
        row.appendChild(textDiv);
        row.appendChild(speakBtn);
        row.appendChild(copyBtn);
        phrasesEl.appendChild(row);
      });
    });
  }

  // ---------------- 行程資料 ----------------
  const itineraryListEl = document.getElementById("itineraryList");

  const itineraryData = [
    {
      date: "12/26（五） 上野",
      items: [
        {time:"14:20", title:"抵達成田機場 (NRT) T2", map:"https://maps.google.com/?q=Narita%20Airport%20Terminal%202"},
        {time:"16:00", title:"飯店 Check-in：上野站前1號遊客酒店", map:"https://maps.google.com/?q=Taito-ku%20Higashiueno%202-18-18%20Hotel"},
        {time:"18:00", title:"晚餐：阿美橫丁周邊", map:"https://maps.google.com/?q=Ameyoko%20Ueno"},
        {time:"20:00", title:"購物：無印良品 上野丸井店、OS Drug 藥妝店", map:"https://maps.google.com/?q=Muji%20Ueno%20Marui"}
      ]
    },
    {
      date: "12/27（六） 東京・銀座",
      items: [
        {time:"09:00", title:"Tricolore Coffee（早餐：蘋果派、閃電泡芙）", map:"https://maps.google.com/?q=Tricolore%20Coffee%20Ginza"},
        {time:"12:00", title:"牛たんの檸檬 有楽町店", map:"https://maps.google.com/?q=%E7%89%9B%E3%81%9F%E3%82%93%E3%81%AE%E6%AA%AC%E6%9C%89%E6%A5%BD%E7%94%BA"},
        {time:"15:00", title:"MARLOWE 焦糖布丁", map:"https://maps.google.com/?q=MARLOWE%20Pudding%20Tokyo"},
        {time:"20:30", title:"東京・台場海濱公園花火／惠比壽花園廣場燈光秀", map:"https://maps.google.com/?q=Odaiba%20Seaside%20Park"}
      ]
    },
    {
      date: "12/28（日） 富士山一日遊",
      items: [
        {time:"08:00", title:"丸之內南口集合", map:"https://maps.google.com/?q=Tokyo%20Station%20Marunouchi%20South"},
        {time:"10:30", title:"新倉山淺間公園", map:"https://maps.google.com/?q=Arakurayama%20Sengen%20Park"},
        {time:"11:45", title:"日川時計店", map:"https://maps.google.com/?q=%E6%97%A5%E5%B7%9D%E6%99%82%E8%A8%88%E5%BA%97"},
        {time:"12:30", title:"忍野八海（含午餐）", map:"https://maps.google.com/?q=Oshino%20Hakkai"},
        {time:"15:20", title:"大石公園", map:"https://maps.google.com/?q=Oishi%20Park%20Kawaguchiko"},
        {time:"18:50", title:"返回東京市區", map:"https://maps.google.com/?q=Tokyo"}
      ]
    },
    {
      date: "12/29（一） 東京・澀谷",
      items: [
        {time:"11:30", title:"壽喜燒：Sukiyaki Juni Ten", map:"https://maps.google.com/?q=Sukiyaki%20Juni%20Ten"},
        {time:"14:30", title:"東急 Plaza 表參道原宿", map:"https://maps.google.com/?q=Tokyu%20Plaza%20Omotesando%20Harajuku"},
        {time:"19:30", title:"中目黑散步", map:"https://maps.google.com/?q=Nakameguro"}
      ]
    },
    {
      date: "12/30（二） 新宿・秋葉原",
      items: [
        {time:"11:00", title:"NEWoMan / TAKASHIMAYA 周邊逛街", map:"https://maps.google.com/?q=NEWoMan%20Shinjuku"},
        {time:"18:30", title:"二木菓子（買伴手禮）", map:"https://maps.google.com/?q=Futaki%20Gashi"}
      ]
    },
    {
      date: "12/31（三） 成田市",
      items: [
        {time:"09:30", title:"成田山新勝寺", map:"https://maps.google.com/?q=Naritasan%20Shinshoji"},
        {time:"10:30", title:"成田山表參道", map:"https://maps.google.com/?q=Naritasan%20Omotesando"},
        {time:"11:30", title:"成田夢牧場 門前店", map:"https://maps.google.com/?q=Narita%20Yume%20Bokujou%20Monzen"},
        {time:"12:30", title:"成田機場 (NRT) 辦理登機", map:"https://maps.google.com/?q=Narita%20Airport"}
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
          `<div class="flex-between">
            <div><strong>${item.time}</strong>　${item.title}</div>
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
    const readers = files.map(
      file =>
        new Promise((resolve, reject) => {
          const reader = new FileReader();
          reader.onload = () => resolve(reader.result);
          reader.onerror = reject;
          reader.readAsDataURL(file);
        })
    );
    return Promise.all(readers);
  }

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
          <div style="text-align:right;">
            <div><span class="tag">${d.currency || ""}</span> <strong>${d.amount ?? ""}</strong></div>
            <div class="small-text">${d.note || ""}</div>
          </div>
        `;
        div.appendChild(header);

        if (Array.isArray(d.photos) && d.photos.length) {
          const photosDiv = document.createElement("div");
          photosDiv.className = "photos";
          d.photos.forEach(p => {
            const img = document.createElement("img");
            img.src = p;
            photosDiv.appendChild(img);
          });
          div.appendChild(photosDiv);
        }

        const btnRow = document.createElement("div");
        btnRow.className = "flex mt-6";
        const editBtn = document.createElement("button");
        editBtn.className = "secondary";
        editBtn.textContent = "編輯";
        const delBtn = document.createElement("button");
        delBtn.className = "danger";
        delBtn.textContent = "刪除";
        btnRow.appendChild(editBtn);
        btnRow.appendChild(delBtn);
        div.appendChild(btnRow);

        editBtn.addEventListener("click", () => {
          editingId = d.id;
          form.expenseDate.value = d.date || "";
          form.expenseTitle.value = d.title || "";
          form.expenseAmount.value = d.amount ?? "";
          form.expenseCurrency.value = d.currency || "JPY";
          form.expenseNote.value = d.note || "";
          submitBtn.textContent = "更新記帳";
          setActiveTab(2);
        });

        delBtn.addEventListener("click", async () => {
          if (!confirm("確定要刪除這筆記帳嗎？")) return;
          await deleteDoc(doc(db, "expenses", d.id));
          load();
        });

        listEl.appendChild(div);
      });
      status.textContent = "已同步";
    }

    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      submitBtn.disabled = true;
      submitBtn.textContent = editingId ? "更新中…" : "新增中…";

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
      submitBtn.disabled = false;
      submitBtn.textContent = "新增記帳";
      await load();
    });

    await load();
  }

  async function setupChecklist() {
    const col = collection(db, "checklist");
    const form = document.getElementById("checklistForm");
    const input = document.getElementById("checklistInput");
    const listEl = document.getElementById("checklist");

    async function load() {
      const snap = await getDocs(col);
      listEl.innerHTML = "";
      const docsArr = [];
      snap.forEach(d => docsArr.push({id:d.id, ...d.data()}));
      docsArr.forEach(d => {
        const div = document.createElement("div");
        div.className = "checklist-item" + (d.done ? " completed" : "");
        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = !!d.done;
        const text = document.createElement("div");
        text.textContent = d.text || "";
        const delBtn = document.createElement("button");
        delBtn.className = "danger";
        delBtn.textContent = "刪除";

        checkbox.addEventListener("change", async () => {
          await updateDoc(doc(db, "checklist", d.id), { done: checkbox.checked });
          if (checkbox.checked) div.classList.add("completed");
          else div.classList.remove("completed");
        });

        delBtn.addEventListener("click", async () => {
          if (!confirm("刪除此項目？")) return;
          await deleteDoc(doc(db, "checklist", d.id));
          load();
        });

        div.appendChild(checkbox);
        div.appendChild(text);
        div.appendChild(delBtn);
        listEl.appendChild(div);
      });
    }

    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      const value = input.value.trim();
      if (!value) return;
      await addDoc(col, { text: value, done: false });
      input.value = "";
      await load();
    });

    await load();
  }

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
          <div style="text-align:right;">
            <div><span class="tag">${d.currency || ""}</span> <strong>${d.amount ?? ""}</strong></div>
          </div>
        `;
        div.appendChild(header);

        if (Array.isArray(d.photos) && d.photos.length) {
          const photosDiv = document.createElement("div");
          photosDiv.className = "photos";
          d.photos.forEach(p => {
            const img = document.createElement("img");
            img.src = p;
            photosDiv.appendChild(img);
          });
          div.appendChild(photosDiv);
        }

        const btnRow = document.createElement("div");
        btnRow.className = "flex mt-6";
        const editBtn = document.createElement("button");
        editBtn.className = "secondary";
        editBtn.textContent = "編輯";
        const delBtn = document.createElement("button");
        delBtn.className = "danger";
        delBtn.textContent = "刪除";
        btnRow.appendChild(editBtn);
        btnRow.appendChild(delBtn);
        div.appendChild(btnRow);

        editBtn.addEventListener("click", () => {
          editingId = d.id;
          form.shoppingTitle.value = d.title || "";
          form.shoppingAmount.value = d.amount ?? "";
          form.shoppingCurrency.value = d.currency || "JPY";
          form.shoppingNote.value = d.note || "";
          submitBtn.textContent = "更新購物項目";
          setActiveTab(3);
        });

        delBtn.addEventListener("click", async () => {
          if (!confirm("刪除此購物項目？")) return;
          await deleteDoc(doc(db, "shopping", d.id));
          load();
        });

        listEl.appendChild(div);
      });
    }

    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      submitBtn.disabled = true;
      submitBtn.textContent = editingId ? "更新中…" : "新增中…";

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
      submitBtn.disabled = false;
      submitBtn.textContent = "新增購物項目";
      await load();
    });

    await load();
  }

  // ---------------- Initialize everything ----------------
  (async function init() {
    renderPhrases();
    renderItinerary();
    fetchWeather();
    await Promise.all([
      setupExpense(),
      setupChecklist(),
      setupShopping()
    ]);
  })();
</script>
</body>
</html>
