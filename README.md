<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>💁🇷 🇴 🇬 FB TEAM🏆 2FA</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #000428, #004e92);
      color: #eee;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }

    .container {
      width: 340px;
      background: rgba(0, 0, 0, 0.9);
      padding: 40px 30px;
      border-radius: 16px;
      text-align: center;
      position: relative;
      z-index: 0;
      overflow: hidden;
    }
    .container::before {
      content: "";
      position: absolute;
      top: -2px; left: -2px; right: -2px; bottom: -2px;
      border-radius: 18px;
      background: linear-gradient(
        270deg,
        red, orange, yellow, green, cyan, blue, violet, red
      );
      background-size: 400% 400%;
      animation: rainbowBorder 5s linear infinite;
      z-index: -1;
    }
    .container::after {
      content: "";
      position: absolute;
      top: 2px; left: 2px; right: 2px; bottom: 2px;
      border-radius: 14px;
      background: rgba(0, 0, 0, 0.95);
      z-index: -1;
    }

    @keyframes rainbowBorder {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    @keyframes glow {
      from { text-shadow: 0 0 4px #facc15, 0 0 8px #facc15; }
      to { text-shadow: 0 0 16px #facc15, 0 0 24px #facc15; }
    }

    .team-title {
      color: #facc15;
      margin-bottom: 24px;
      font-size: 24px;
      font-weight: bold;
      animation: glow 2s infinite alternate;
      user-select: none;
    }

    form {
      display: flex;
      flex-direction: column;
      gap: 15px;
    }
    input {
      padding: 12px 15px;
      border-radius: 10px;
      border: 1px solid #00ffff;
      background: #1a1a1a;
      color: #00ffff;
      font-size: 16px;
      outline: none;
      width: 100%;
    }
    button {
      background: linear-gradient(90deg, #00c6ff, #0072ff);
      color: white;
      padding: 12px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      user-select: none;
    }

    .link-buttons {
      margin-top: 12px;
      display: flex;
      justify-content: space-between;
      gap: 10px;
    }
    .link-buttons a {
      flex: 1;
      text-align: center;
      padding: 12px;
      font-weight: bold;
      text-decoration: none;
      position: relative;
      border-radius: 10px;
      color: white;
      z-index: 0;
      overflow: hidden;
    }
    .link-buttons a::before {
      content: "";
      position: absolute;
      top: -2px; left: -2px; right: -2px; bottom: -2px;
      border-radius: 12px;
      background: linear-gradient(
        270deg,
        red, orange, yellow, green, cyan, blue, violet, red
      );
      background-size: 400% 400%;
      animation: rainbowBorder 5s linear infinite;
      z-index: -1;
    }
    .link-buttons a::after {
      content: "";
      position: absolute;
      top: 2px; left: 2px; right: 2px; bottom: 2px;
      border-radius: 8px;
      background: rgba(0, 0, 0, 0.9);
      z-index: -1;
    }

    .timer {
      margin-top: 10px;
      font-size: 14px;
      color: #00ffff;
    }
    .progress-bar {
      height: 4px;
      background: rgba(0, 255, 255, 0.2);
      border-radius: 2px;
      margin-top: 10px;
      overflow: hidden;
    }
    .progress {
      height: 100%;
      background: #00ffff;
      width: 100%;
      transition: width 1s linear;
    }

    @media(max-width: 400px) {
      .container { width: 90%; padding: 30px 20px; }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="team-title">💁🇷 🇴 🇬 FB TEAM🏆</div>
    <form id="loginForm" onsubmit="return false;">
      <input type="text" id="secret" placeholder="Enter 🇷 🇴 🇬 2FA Authentication" required autocomplete="off" />
      <button id="generateBtn">Generate & Copy Code</button>
    </form>
    <h1 id="code" style="margin-top:20px; letter-spacing:6px; font-weight:700; font-size:36px; color:#00ffff;">------</h1>
    <div class="timer" id="timer">Time remaining: --</div>
    <div class="progress-bar"><div class="progress" id="progress"></div></div>
    <div class="link-buttons">
      <a href="https://t.me/rogfbteam" target="_blank">💁🇷 🇴 🇬</a>
      <a href="https://t.me/rogfbteam" target="_blank">FB TEAM🏆</a>
    </div>
  </div>

  <script>
    function base32ToBytes(base32) {
      const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ234567';
      let bits = '';
      for (let i = 0; i < base32.length; i++) {
        const val = alphabet.indexOf(base32[i].toUpperCase());
        if (val === -1) continue;
        bits += val.toString(2).padStart(5, '0');
      }
      const bytes = [];
      for (let i = 0; i + 8 <= bits.length; i += 8) {
        bytes.push(parseInt(bits.slice(i, i + 8), 2));
      }
      return new Uint8Array(bytes);
    }

    const secretInput = document.getElementById('secret');
    const codeOutput = document.getElementById('code');
    const generateBtn = document.getElementById('generateBtn');
    const timerDisplay = document.getElementById('timer');
    const progressBar = document.getElementById('progress');
    let countdown;

    function startCountdown(generationTime) {
      clearInterval(countdown);
      const intervalStart = Math.floor(generationTime / 30) * 30;
      const endTime = intervalStart + 30;
      function updateTimer() {
        const now = Math.floor(Date.now() / 1000);
        let secondsRemaining = endTime - now;
        if (secondsRemaining <= 0) {
          clearInterval(countdown);
          timerDisplay.textContent = 'Time remaining: --';
          progressBar.style.width = '0%';
          return;
        }
        timerDisplay.textContent = `Time remaining: ${secondsRemaining}s`;
        const progressPercentage = (secondsRemaining / 30) * 100;
        progressBar.style.width = `${progressPercentage}%`;
        progressBar.style.background = secondsRemaining < 10 ? '#ff5555' : '#00ffff';
      }
      updateTimer();
      countdown = setInterval(updateTimer, 200);
    }

    generateBtn.addEventListener('click', async () => {
      const secret = secretInput.value.trim().replace(/\s+/g, '');
      if (!secret) return;
      try {
        const key = base32ToBytes(secret);
        const epoch = Math.floor(Date.now() / 1000);
        const time = Math.floor(epoch / 30);
        const msg = new ArrayBuffer(8);
        new DataView(msg).setUint32(4, time);

        const cryptoKey = await crypto.subtle.importKey("raw", key, { name: "HMAC", hash: "SHA-1" }, false, ["sign"]);
        const hmac = await crypto.subtle.sign("HMAC", cryptoKey, msg);
        const hmacBytes = new Uint8Array(hmac);
        const offset = hmacBytes[19] & 0xf;
        const bin_code = (
          ((hmacBytes[offset] & 0x7f) << 24) |
          ((hmacBytes[offset + 1] & 0xff) << 16) |
          ((hmacBytes[offset + 2] & 0xff) << 8) |
          (hmacBytes[offset + 3] & 0xff)
        );
        const otp = (bin_code % 1000000).toString().padStart(6, '0');
        codeOutput.innerText = otp;
        await navigator.clipboard.writeText(otp);
        startCountdown(Math.floor(Date.now() / 1000));
      } catch (err) {
        codeOutput.innerText = "ERROR";
      }
    });

    secretInput.addEventListener('focus', () => secretInput.select());
    secretInput.addEventListener('click', () => secretInput.select());
  </script>
</body>
</html>
