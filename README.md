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
            /* 調整主背景為深灰色 */
            background-color: #111827; /* Gray 900 */
        }
        .scrollable-content {
            max-height: calc(100vh - 200px); 
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        /* 自定義捲軸樣式 (僅適用於高級感) */
        .scrollable-content::-webkit-scrollbar {
            width: 8px;
        }
        .scrollable-content::-webkit-scrollbar-thumb {
            background-color: #4f46e5; /* 靛藍色捲軸 */
            border-radius: 4px;
        }
        .scrollable-content::-webkit-scrollbar-track {
            background-color: #374151; /* 深灰色軌道 */
        }
        /* 調整 input 外觀以符合深色主題 */
        input[type="number"] {
            -moz-appearance: textfield; /* Firefox */
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none; /* Chrome, Safari, Edge */
            margin: 0;
        }
    </style>
</head>
<body class="min-h-screen p-4 sm:p-8">

    <!-- 主容器 -->
    <div class="max-w-4xl mx-auto bg-gray-800 rounded-3xl shadow-2xl shadow-gray-950/50 overflow-hidden">
        
        <!-- 標題區 (使用深靛藍或接近黑的顏色來區分) -->
        <header class="p-6 bg-slate-900 text-white shadow-xl">
            <h1 class="text-3xl font-bold mb-1 text-indigo-400">東京六天五夜精選行程</h1>
            <p class="text-slate-400">出發日期：12月26日 | 避開排隊名店與觀光人潮</p>
        </header>

        <!-- 行程內容區 -->
        <div class="flex flex-col lg:flex-row">
            
            <!-- 左側：日期導覽列 -->
            <nav id="day-navigation" class="lg:w-1/4 p-4 lg:p-6 bg-gray-900 border-b lg:border-r border-gray-700 flex lg:flex-col overflow-x-auto lg:overflow-y-auto whitespace-nowrap lg:whitespace-normal">
                <!-- 按鈕將由 JS 動態生成 -->
            </nav>

            <!-- 右側：詳細行程與天氣預報 -->
            <main class="lg:w-3/4 p-4 sm:p-6 lg:p-8">
                
                <!-- 新增：日幣匯率換算器 (TWD ↔ JPY) -->
                <div id="currency-converter" class="p-6 bg-gray-700/70 rounded-xl shadow-xl mb-6 border border-indigo-600">
                    <h3 class="text-xl font-bold text-indigo-400 mb-4 flex items-center">
                        <i data-lucide="coins" class="w-5 h-5 mr-2"></i>
                        日幣匯率換算 (TWD ↔ JPY)
                    </h3>
                    <!-- 匯率顯示/輸入區 -->
                    <div class="mb-4 p-3 bg-gray-800 rounded-lg">
                        <label for="rateInput" class="block text-sm font-medium text-gray-400">手動設定當前匯率 (1 TWD 可換取 JPY)：</label>
                        <div class="flex items-center mt-1">
                            <input type="number" id="rateInput" value="4.80" step="0.01" min="0.01" 
                                class="w-24 p-2 text-center bg-gray-600 text-yellow-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500 border-none">
                            <span class="ml-2 text-lg
