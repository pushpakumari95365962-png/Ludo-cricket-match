<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ludo Cricket MTSC Pro</title>
    <style>
        :root { --primary: #1565c0; --accent: #ffeb3b; --bg: #0d1117; --mtsc: #ff5722; }
        body { font-family: 'Segoe UI', sans-serif; text-align: center; background-color: var(--bg); color: white; margin: 0; padding: 15px; }
        
        .game-box { background: #161b22; max-width: 420px; margin: auto; padding: 20px; border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); border: 2px solid #30363d; }
        
        /* Logo Styling */
        .game-logo { width: 120px; height: 120px; margin-bottom: 10px; border-radius: 50%; border: 3px solid var(--accent); object-fit: cover; }

        .wallet { display: flex; justify-content: space-around; background: #21262d; padding: 10px; border-radius: 15px; margin-bottom: 15px; border: 1px solid #444; }
        .wallet span { font-weight: bold; color: var(--accent); }

        .mtsc-banner { background: var(--mtsc); color: white; padding: 6px; border-radius: 8px; font-size: 12px; font-weight: bold; margin-bottom: 10px; cursor: pointer; animation: blink 1.5s infinite; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.7; } 100% { opacity: 1; } }

        .status-bar { background: var(--primary); color: white; padding: 12px; border-radius: 12px; font-weight: bold; margin-bottom: 15px; min-height: 20px; }
        
        .players-container { display: flex; justify-content: space-around; margin-bottom: 15px; }
        .player-card { padding: 12px; border-radius: 15px; border: 2px solid #30363d; width: 42%; background: #0d1117; transition: 0.3s; }
        .active { border-color: #4af2ff; background-color: #1c2128; transform: scale(1.05); box-shadow: 0 0 15px rgba(74, 242, 255, 0.2); }
        
        .dice-area { font-size: 70px; margin: 15px 0; cursor: pointer; user-select: none; }
        button.main-btn { background: linear-gradient(45deg, #1565c0, #00d2ff); color: white; border: none; padding: 15px 40px; font-size: 18px; border-radius: 50px; cursor: pointer; font-weight: bold; width: 100%; box-shadow: 0 4px #0d47a1; }
        button.main-btn:active { transform: translateY(2px); box-shadow: 0 0; }

        .emoji-bar { display: flex; justify-content: center; gap: 10px; margin-top: 20px; padding-top: 15px; border-top: 1px solid #30363d; }
        .emoji-btn { background: #21262d; border: 1px solid #444; padding: 8px; border-radius: 10px; cursor: pointer; font-size: 20px; }
    </style>
</head>
<body>

<div class="game-box">
    <img src="logo.png" alt="Ludo Cricket Logo" class="game-logo">

    <div class="mtsc-banner" onclick="showMTSC()">🏆 MTSC TOURNAMENT: 50,000 COINS PRIZE!</div>
    
    <div class="wallet">
        <span>💰 Coins: <span id="coins">400</span></span>
        <span>💎 Diamonds: <span id="diamonds">10</span></span>
    </div>

    <div id="status-msg" class="status-bar">Chalo Shuru Karein!</div>

    <div class="players-container">
        <div id="p1-card" class="player-card active">
            <strong>Player 1</strong><br>
            <span style="font-size: 18px; font-weight: bold;">Runs: <span id="p1-runs">0</span>/50</span><br>
            <small>Wkts: <span id="p1-wickets">0</span>/2 | Balls: <span id="p1-balls">12</span></small>
        </div>
        <div id="p2-card" class="player-card">
            <strong>Player 2</strong><br>
            <span style="font-size: 18px; font-weight: bold;">Runs: <span id="p2-runs">0</span>/50</span><br>
            <small>Wkts: <span id="p2-wickets">0</span>/2 | Balls: <span id="p2-balls">12</span></small>
        </div>
    </div>

    <div class="dice-area" id="dice-display" onclick="playTurn()">🎲</div>
    <button class="main-btn" onclick="playTurn()">DICE ROLL</button>

    <div class="emoji-bar">
        <button class="emoji-btn" onclick="sendEmoji('🔥')">🔥</button>
        <button class="emoji-btn" onclick="sendEmoji('😂')">😂</button>
        <button class="emoji-btn" onclick="sendEmoji('🏏')">🏏</button>
    </div>
</div>

<script>
    let scores = [0, 0]; 
    let wickets = [0, 0];
    let balls = [12, 12];
    let lifeline = [true, true];
    let wallet = { coins: 400, diamonds: 10 };
    let currentPlayer = 0; 

    function showMTSC() {
        alert("Monthly Talent Search Championship (MTSC)\n\nMonthly Winner Reward:\n- 50,000 Game Coins\n- 100 Diamonds\n- Special State Rank Badge!");
    }

    function playTurn() {
        if (checkGameOver()) return;
        const outcomes = [1, 2, 3, 4, 6, "OUT"];
        let result = outcomes[Math.floor(Math.random() * outcomes.length)];
        const diceIcons = {1:"⚀", 2:"⚁", 3:"⚂", 4:"⚃", 6:"⚅", "OUT":"☝️"};
        document.getElementById("dice-display").innerHTML = diceIcons[result] || "🎲";
        let status = document.getElementById("status-msg");
        balls[currentPlayer]--;

        if (result === "OUT") {
            if (lifeline[currentPlayer]) {
                lifeline[currentPlayer] = false;
                status.innerHTML = `🛡️ OUT! Life-line used!`;
            } else {
                wickets[currentPlayer]++;
                status.innerHTML = `☝️ WICKET! Player ${currentPlayer + 1} Out!`;
                if (wickets[currentPlayer] < 2) changeTurn();
            }
        } else {
            scores[currentPlayer] += result;
            status.innerHTML = `Player ${currentPlayer + 1}: +${result} Runs!`;
            if (result !== 6) changeTurn();
            else status.innerHTML = `🚀 SIXER! Extra Turn!`;
        }
        updateUI();
    }

    function changeTurn() {
        if (!checkGameOver()) {
            currentPlayer = (currentPlayer === 0) ? 1 : 0;
            document.getElementById("p1-card").classList.toggle("active");
            document.getElementById("p2-card").classList.toggle("active");
        }
    }

    function sendEmoji(e) {
        if (wallet.diamonds > 0) {
            wallet.diamonds--;
            document.getElementById("status-msg").innerHTML = `Emoji ${e} Sent! (-1 💎)`;
            updateUI();
        } else alert("Diamonds khatam!");
    }

    function updateUI() {
        document.getElementById("coins").innerText = wallet.coins;
        document.getElementById("diamonds").innerText = wallet.diamonds;
        document.getElementById("p1-runs").innerText = scores[0];
        document.getElementById("p1-wickets").innerText = wickets[0];
        document.getElementById("p1-balls").innerText = balls[0];
        document.getElementById("p2-runs").innerText = scores[1];
        document.getElementById("p2-wickets").innerText = wickets[1];
        document.getElementById("p2-balls").innerText = balls[1];
        checkGameOver();
    }

    function checkGameOver() {
        let msg = ""; let over = false;
        if (scores[0] >= 50) { msg = "🏆 PLAYER 1 WINS! +100 Coins"; wallet.coins += 100; over = true; }
        else if (scores[1] >= 50) { msg = "🏆 PLAYER 2 WINS! +100 Coins"; wallet.coins += 100; over = true; }
        else if (wickets[0] >= 2 && wickets[1] >= 2) { msg = "MATCH OVER: All Out!"; over = true; }
        else if (balls[0] <= 0 && balls[1] <= 0) { msg = "MATCH OVER: No Balls left!"; over = true; }
        if (over) { document.getElementById("status-msg").innerHTML = msg; document.getElementById("status-msg").style.background = "#2e7d32"; return true; }
        return false;
    }
</script>
</body>
</html>
