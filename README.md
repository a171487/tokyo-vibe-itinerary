<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tokyo Trip Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-4">

<div class="max-w-3xl mx-auto bg-white p-4 rounded-xl shadow">

<nav class="flex space-x-4 border-b mb-4">
  <button onclick="showTab('home')" class="py-2 px-3 font-bold">首頁</button>
  <button onclick="showTab('itinerary')" class="py-2 px-3 font-bold">行程</button>
  <button onclick="showTab('account')" class="py-2 px-3 font-bold">記帳</button>
  <button onclick="showTab('list')" class="py-2 px-3 font-bold">清單</button>
</nav>

<div id="home" class="tab">
  <h2 class="text-2xl font-bold mb-2">即時匯率</h2>
  <p>顯示匯率 / 免稅額提示</p>

  <h2 class="text-2xl font-bold mt-6 mb-2">天氣</h2>
  <p>東京天氣資訊</p>

  <h2 class="text-2xl font-bold mt-6 mb-2">富士山能見度</h2>
  <p>能見度指數顯示</p>

  <h2 class="text-2xl font-bold mt-6 mb-2">緊急電話</h2>
  <ul class="list-disc ml-6">
    <li>110 警察</li>
    <li>119 救護車 / 火警</li>
    <li>台灣駐日辦事處專線</li>
    <li>OHDr. 中文線上門診</li>
  </ul>

  <h2 class="text-2xl font-bold mt-6 mb-2">常用日語</h2>
  <ul class="list-disc ml-6">
    <li>すみません（不好意思）</li>
    <li>ありがとう（謝謝）</li>
  </ul>
</div>

<div id="itinerary" class="tab hidden">
  <h2 class="text-2xl font-bold mb-4">旅程行程</h2>

  <h3 class="font-bold text-xl mt-4">12/26（五） 上野</h3>
  <ul class="ml-6 list-disc">
    <li>14:20 抵達成田機場 T2</li>
    <li>16:00 飯店 Check-in</li>
    <li>18:00 晚餐：阿美橫丁</li>
    <li>20:00 購物：無印良品、OS Drug</li>
  </ul>

  <h3 class="font-bold text-xl mt-4">12/27（六） 東京 / 銀座</h3>
  <ul class="ml-6 list-disc">
    <li>09:00 Tricolore coffee 早餐</li>
    <li>12:00 牛たんの檸檬 有楽町店</li>
    <li>15:00 MARLOWE 布丁</li>
    <li>20:30 花火 / 惠比壽燈光秀</li>
  </ul>

  <h3 class="font-bold text-xl mt-4">12/28（日） 富士山</h3>
  <ul class="ml-6 list-disc">
    <li>08:00 集合</li>
    <li>10:30 新倉山淺間公園</li>
    <li>11:45 日川時計店</li>
    <li>12:30 忍野八海</li>
    <li>15:20 大石公園</li>
    <li>18:50 返回東京</li>
  </ul>

  <h3 class="font-bold text-xl mt-4">12/29（一） 東京 / 澀谷</h3>
  <ul class="ml-6 list-disc">
    <li>11:30 壽喜燒 Juni Ten</li>
    <li>14:30 東急 Plaza 表參道</li>
    <li>19:30 中目黑</li>
  </ul>

  <h3 class="font-bold text-xl mt-4">12/30（二） 新宿 / 秋葉原</h3>
  <ul class="ml-6 list-disc">
    <li>11:00 NEWoMan TAKANAWA</li>
    <li>18:30 二木菓子</li>
  </ul>

  <h3 class="font-bold text-xl mt-4">12/31（三） 成田</h3>
  <ul class="ml-6 list-disc">
    <li>09:30 成田山新勝寺</li>
    <li>10:30 成田山表參道</li>
    <li>11:30 成田夢牧場</li>
    <li>12:30 成田機場</li>
  </ul>
</div>

<div id="account" class="tab hidden">
  <h2 class="text-2xl font-bold mb-2">記帳</h2>
  <p>可自行擴充記帳功能</p>
</div>

<div id="list" class="tab hidden">
  <h2 class="text-2xl font-bold mb-2">清單</h2>
  <p>旅行購物 / 檢查清單</p>
</div>

</div>

<script>
function showTab(id){
  document.querySelectorAll('.tab').forEach(t => t.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
</script>

</body>
</html>
