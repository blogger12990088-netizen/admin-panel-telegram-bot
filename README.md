<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Daily Rewards</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: #000;
      color: #fff;
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      text-align: center;
      background: #1a1a1a;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(255, 215, 0, 0.3);
      width: 350px;
      border: 1px solid #333;
      max-height: 95vh;
      overflow-y: auto;
    }
    .container h1 {
      font-size: 22px;
      color: #FFD700;
      margin-bottom: 10px;
      text-shadow: 0 0 6px rgba(255, 215, 0, 0.6);
    }
    .developer {
      font-size: 12px;
      background-color: #4CAF50;
      padding: 5px 10px;
      border-radius: 5px;
      margin-bottom: 15px;
      display: inline-block;
      color: white;
    }
    .stats p {
      margin: 5px 0;
      font-size: 14px;
      color: #FFD700;
    }
    .progress-circle {
      width: 80px;
      height: 80px;
      border: 4px solid #FFD700;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 10px auto;
    }
    .progress-circle span {
      font-size: 18px;
      font-weight: bold;
      color: #FFD700;
    }
    .buttons button {
      width: 95%;
      margin: 5px 0;
      padding: 12px;
      font-size: 14px;
      border: none;
      border-radius: 6px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s;
    }
    #watch-ad-btn { background: #FFD700; color:#000; }
    #youtube-btn { background: #FF0000; color: #fff; }
    #telegram-btn { background: #0088cc; color: #fff; }
    #withdraw-btn { background: orange; color:#000; }
    #support-btn { background: #0088cc; color:#fff; display:flex; align-items:center; justify-content:center; gap:8px; }
    #support-btn img { width:20px; height:20px; }
    .buttons button:hover {
      transform: translateY(-3px);
      box-shadow: 0 5px 15px rgba(255, 215, 0, 0.4);
    }
    .withdraw-section {
      margin-top: 20px;
      display: none;
      padding: 15px;
      background-color: #111;
      border-radius: 10px;
      border: 1px solid #333;
    }
    .withdraw-section input, select {
      width: 90%;
      padding: 8px;
      margin: 5px 0;
      border-radius: 6px;
      border: none;
      text-align: center;
    }
    .refer-box {
      margin-top: 15px;
      background:#222;
      padding:10px;
      border-radius:8px;
    }
    .refer-box button {
      background:#FFD700;
      border:none;
      padding:5px 10px;
      margin-left:5px;
      border-radius:5px;
      cursor:pointer;
    }
    .history-box {
      margin-top: 20px;
      background:#222;
      padding:10px;
      border-radius:8px;
      text-align:left;
      font-size:13px;
      max-height:150px;
      overflow-y:auto;
    }
    .history-box h4 {
      margin:0 0 10px;
      color:#FFD700;
    }
    .history-item {
      margin-bottom: 6px;
      padding: 6px;
      background:#111;
      border-radius:5px;
      border:1px solid #333;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Daily Rewards</h1>
    <div class="developer">Developed by TBPYC</div>
    <p>Welcome, <span id="user-name"></span></p>
    <div class="stats">
      <p>প্রতি বিজ্ঞাপন: <span id="per-ad-rate">0.10</span> টাকা</p>
      <p>Watched Ads: <span id="watched-ads">0</span></p>
      <p>Total Ads Seen (Lifetime): <span id="lifetime-ads">0</span></p>
      <p>Balance: <span id="earned-points">0.00</span> টাকা</p>
      <p>👥 Referrals: <span id="total-refers">0</span></p>
      <p>💰 Referral Income: <span id="ref-income">0.00</span> টাকা</p>
    </div>
    <div class="progress-circle">
      <span id="ads-progress">0%</span>
    </div>
    <div class="buttons">
      <button id="watch-ad-btn" onclick="watchAd()">📺 বিজ্ঞাপন দেখুন</button>
      <button id="youtube-btn" onclick="subscribeYoutube()">▶️ Subscribe YouTube (2 টাকা)</button>
      <button id="telegram-btn" onclick="joinTelegram()">📢 Join Telegram (2 টাকা)</button>
      <button id="withdraw-btn" onclick="showWithdrawForm()">💵 Withdrawal Request</button>
      <button id="support-btn" onclick="openSupport()">
        <img src="https://upload.wikimedia.org/wikipedia/commons/8/82/Telegram_logo.svg"/> Support
      </button>
    </div>
    <div class="refer-box">
      <p>🔗 Your Referral Link (প্রতি রেফারে ৩ টাকা, Withdraw করতে মিনিমাম ৫ রেফার আবশ্যক):</p>
      <input type="text" id="ref-link" readonly>
      <button onclick="copyRefer()">Copy</button>
    </div>
    <div id="withdraw-section" class="withdraw-section">
      <h3>Withdraw</h3>
      <input type="number" id="withdraw-amount" placeholder="Minimum 100 টাকা" min="100">
      <select id="payment-method">
        <option value="Bkash">Bkash</option>
        <option value="Nagad">Nagad</option>
      </select>
      <input type="text" id="withdraw-phone" placeholder="Phone Number">
      <button onclick="withdrawPoints()">Confirm Withdraw</button>
      <p id="withdraw-status"></p>
    </div>
    <div class="history-box" id="withdraw-history">
      <h4>Withdraw History</h4>
      <div id="history-list"></div>
    </div>
  </div>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script src='//libtl.com/sdk.js' data-zone='9840613' data-sdk='show_9840613'></script>

  <script>
  const ADMIN_USER_ID = "7850506878";  
  const BOT_TOKEN = "YOUR_BOT_TOKEN_HERE";

  let MAX_DAILY_ADS = 150;
  let REWARD_PER_AD = 0.10;   // ✅ প্রতি বিজ্ঞাপন ০.১০ টাকা
  const REF_REWARD = 3.00;

  let telegramUID = "unknownUser";
  if (window.Telegram && Telegram.WebApp && Telegram.WebApp.initDataUnsafe.user) {
    telegramUID = Telegram.WebApp.initDataUnsafe.user.id;
  }

  const urlParams = new URLSearchParams(window.location.search);
  const refBy = urlParams.get('start');

  if (!localStorage.getItem('user_initialized_' + telegramUID)) {
    localStorage.setItem('watchedAdsCount_' + telegramUID, 0);
    localStorage.setItem('lifetimeAdsCount_' + telegramUID, 0);
    localStorage.setItem('earnedPoints_' + telegramUID, 0);
    localStorage.setItem('youtubeBonusTaken_' + telegramUID, false);
    localStorage.setItem('telegramBonusTaken_' + telegramUID, false);
    localStorage.setItem('withdrawHistory_' + telegramUID, JSON.stringify([]));
    localStorage.setItem('totalRefers_' + telegramUID, 0);
    localStorage.setItem('refIncome_' + telegramUID, 0);
    localStorage.setItem('user_initialized_' + telegramUID, true);

    if (refBy && refBy !== String(telegramUID)) {
        getReferralReward(refBy, telegramUID);
    }
  }

  document.getElementById("user-name").textContent = `UID: ${telegramUID}`;
  document.getElementById("ref-link").value = `https://t.me/DailyRewards999_bot?start=${telegramUID}`;
  document.getElementById("per-ad-rate").textContent = REWARD_PER_AD.toFixed(2);

  let watchedAdsCount = parseInt(localStorage.getItem("watchedAdsCount_" + telegramUID)) || 0;
  let lifetimeAdsCount = parseInt(localStorage.getItem("lifetimeAdsCount_" + telegramUID)) || 0;
  let earnedPoints = parseFloat(localStorage.getItem("earnedPoints_" + telegramUID)) || 0.00;
  let totalRefers = parseInt(localStorage.getItem("totalRefers_" + telegramUID) || 0);
  let refIncome = parseFloat(localStorage.getItem("refIncome_" + telegramUID) || 0.00);
  let youtubeBonusTaken = localStorage.getItem("youtubeBonusTaken_" + telegramUID) === "true";
  let telegramBonusTaken = localStorage.getItem("telegramBonusTaken_" + telegramUID) === "true";
  let withdrawHistory = JSON.parse(localStorage.getItem("withdrawHistory_" + telegramUID) || "[]");

  const today = new Date().toLocaleDateString();
  const savedDate = localStorage.getItem("lastDate_" + telegramUID) || "";
  if (savedDate !== today) {
    watchedAdsCount = 0;
    localStorage.setItem("watchedAdsCount_" + telegramUID, 0);
    localStorage.setItem("lastDate_" + telegramUID, today);
  }

  updateStats();
  renderHistory();
  updateReferralUI();

  function watchAd() {
    if (watchedAdsCount >= MAX_DAILY_ADS) {
      alert("❌ আজকের limit শেষ হয়েছে। আগামীকাল আবার চেষ্টা করুন!");
      return;
    }
    if (typeof show_9840613 === 'function') {
      show_9840613();
      setTimeout(() => {
        watchedAdsCount++;
        lifetimeAdsCount++;
        earnedPoints += REWARD_PER_AD;
        saveData();
        updateStats();
      }, 15000);
    }
  }

  function subscribeYoutube() {
    if (youtubeBonusTaken) { alert("❌ YouTube Bonus Already Collected!"); return; }
    window.open("https://youtube.com/@tacktipsbangla", "_blank");
    earnedPoints += 2.00;
    youtubeBonusTaken = true;
    localStorage.setItem("youtubeBonusTaken_" + telegramUID, "true");
    saveData();
    updateStats();
  }

  function joinTelegram() {
    if (telegramBonusTaken) { alert("❌ Telegram Bonus Already Collected!"); return; }
    window.open("https://t.me/EarningSupport998", "_blank");
    earnedPoints += 2.00;
    telegramBonusTaken = true;
    localStorage.setItem("telegramBonusTaken_" + telegramUID, "true");
    saveData();
    updateStats();
  }

  function openSupport() { window.open("https://t.me/Mysupportid100", "_blank"); }
  function showWithdrawForm() { document.getElementById("withdraw-section").style.display = "block"; }

  function withdrawPoints() {
    const amount = document.getElementById('withdraw-amount').value;
    const method = document.getElementById('payment-method').value;
    const phone = document.getElementById('withdraw-phone').value;

    if (amount < 100) {
      document.getElementById("withdraw-status").textContent = "❌ Minimum 100 টাকা হতে হবে।";
      return;
    }
    if (amount > earnedPoints) {
      document.getElementById("withdraw-status").textContent = "❌ পর্যাপ্ত Balance নেই।";
      return;
    }
    if (!youtubeBonusTaken || !telegramBonusTaken) {
      document.getElementById("withdraw-status").textContent = "❌ Withdraw করতে হলে অবশ্যই YouTube ও Telegram join করতে হবে।";
      return;
    }
    if (totalRefers < 5) {
      document.getElementById("withdraw-status").textContent = "❌ Withdraw করতে হলে অন্তত ৫ জনকে রেফার করতে হবে।";
      return;
    }

    const msg = `🟡 Withdraw Request 🟡
🆔 UID: ${telegramUID}
💰 Amount: ${amount} টাকা
📱 Method: ${method}
📞 Number: ${phone}
🕒 ${new Date().toLocaleString()}`;
    
    sendTelegram(msg);

    earnedPoints -= parseFloat(amount);
    withdrawHistory.push({ amount, method, phone, time: new Date().toLocaleString() });
    saveData();
    updateStats();
    renderHistory();
    document.getElementById("withdraw-status").textContent = "✅ Withdraw Request Sent!";
  }

  function renderHistory() {
    const list = document.getElementById("history-list");
    list.innerHTML = "";
    withdrawHistory.forEach(item => {
      const div = document.createElement("div");
      div.className = "history-item";
      div.textContent = `${item.time} | ${item.amount} টাকা | ${item.method} (${item.phone})`;
      list.appendChild(div);
    });
  }

  function updateStats() {
    document.getElementById("watched-ads").textContent = watchedAdsCount + "/" + MAX_DAILY_ADS;
    document.getElementById("earned-points").textContent = earnedPoints.toFixed(2);
    document.getElementById("lifetime-ads").textContent = lifetimeAdsCount;
    const percentage = Math.min((watchedAdsCount / MAX_DAILY_ADS) * 100, 100);
    document.getElementById("ads-progress").textContent = `${Math.floor(percentage)}%`;
  }

  function updateReferralUI() {
    document.getElementById("total-refers").textContent = totalRefers;
    document.getElementById("ref-income").textContent = refIncome.toFixed(2);
  }

  function saveData() {
    localStorage.setItem("watchedAdsCount_" + telegramUID, watchedAdsCount);
    localStorage.setItem("lifetimeAdsCount_" + telegramUID, lifetimeAdsCount);
    localStorage.setItem("earnedPoints_" + telegramUID, earnedPoints.toFixed(2));
    localStorage.setItem("totalRefers_" + telegramUID, totalRefers);
    localStorage.setItem("refIncome_" + telegramUID, refIncome.toFixed(2));
    localStorage.setItem("withdrawHistory_" + telegramUID, JSON.stringify(withdrawHistory));
  }

  function copyRefer() {
    const copyText = document.getElementById("ref-link");
    copyText.select();
    document.execCommand("copy");
    alert("Referral Link Copied!");
  }

  function sendTelegram(message) {
    const url = `https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`;
    fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ chat_id: ADMIN_USER_ID, text: message })
    });
  }

  function getReferralReward(referrerId, newUserUID) {
    let referrer_totalRefers = parseInt(localStorage.getItem("totalRefers_" + referrerId)) || 0;
    let referrer_refIncome = parseFloat(localStorage.getItem("refIncome_" + referrerId)) || 0.00;
    let referrer_earnedPoints = parseFloat(localStorage.getItem("earnedPoints_" + referrerId)) || 0.00;
    
    referrer_totalRefers++;
    referrer_refIncome += REF_REWARD;
    referrer_earnedPoints += REF_REWARD;
    
    localStorage.setItem("totalRefers_" + referrerId, referrer_totalRefers);
    localStorage.setItem("refIncome_" + referrerId, referrer_refIncome.toFixed(2));
    localStorage.setItem("earnedPoints_" + referrerId, referrer_earnedPoints.toFixed(2));
    
    const refMsg = `👥 New Referral Added!
🔗 Referrer UID: ${referrerId}
🆕 New User UID: ${newUserUID}
💰 Reward: ${REF_REWARD} টাকা`;
    sendTelegram(refMsg);
  }
  </script>
</body>
</html>
