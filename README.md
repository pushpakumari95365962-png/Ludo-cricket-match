<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ludo Cricket MTSC Pro</title>
    <style>
        :root { --primary: #1565c0; --accent: #ffeb3b; --bg: #0d1117; --mtsc: #ff5722; }
        body { font-family: 'Segoe UI', sans-serif; text-align: center; background-color: var(--bg); color: white; margin: 0; padding: 0; overflow-x: hidden; }
        
        /* 1. Loading Screen */
        #loading-screen { position: fixed; width: 100%; height: 100%; background: #000; display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 100; }
        .loader-bar { width: 200px; height: 10px; background: #333; border-radius: 5px; margin-top: 20px; overflow: hidden; }
        .loader-fill { width: 0%; height: 100%; background: var(--primary); transition: 0.1s; }

        /* 2. Name Entry Screen */
        #name-screen { display: none; position: fixed; width: 100%; height: 100%; background: var(--bg); z-index: 90; flex-direction: column; align-items: center; justify-content: center; }
        input[type="text"] { padding: 12px; border-radius: 8px; border: none; width: 250px; margin-bottom: 15px; font-size: 16px; text-align: center; }

        /* 3. Game Container (Main Home Page) */
        #game-container { display: none; padding: 15px; }
        .game-box { background: #161b22; max-width: 420px; margin: auto; padding: 20px; border-radius: 25px; border: 2px solid #30363d; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        
        .game-logo { width: 120px; height: 120px; margin-bottom: 10px; border-radius: 50%; border: 3px solid var(--accent); object-fit: cover; }
        .wallet { display: flex; justify-content: space-around; background: #21262d; padding: 10px; border-radius: 15px; margin-bottom: 15px; }
        .status-bar { background: var(--primary); color: white; padding: 12px; border-radius: 12px; font-weight: bold; margin-bottom: 15px; }
        
        .players-container { display: flex; justify-content: space-around; margin-bottom: 15px; }
        .player-card { padding: 12px; border-radius: 15px; border: 2px solid #30363d; width: 42%; background: #0d1117; }
        .active { border-color: #4af2ff; box-shadow: 0 0 15px rgba(74, 242, 255, 0.2); }
        
        button { background: linear-gradient(45deg, #1565c0, #00d2ff); color: white; border: none; padding: 15px; border-radius: 50px; cursor: pointer; font-weight: bold; width: 100%; transition: 0.2s; }
        button:active { transform: scale(0.98); }
        .emoji-btn { width: auto; background: #21262d; padding: 8px; font-size: 20px; margin: 5px; }
    </style>
</head>
<body>

    <div id="loading-screen">
        <img src="logo.png" alt="Logo" width="150">
        <h2 style="color: var(--accent);">Loading Ludo Cricket...</h2>
        <div class="loader-bar"><div class="loader-fill" id="fill"></div></div>
    </div>

    <div id="name-screen">
        <img src="logo.png" alt="Logo" width="100">
        <h1 style="color: var(--accent);">Enter Your Name</h1>
        <input type="text" id="player-name-input" placeholder="Apna naam likhein..." maxlength="10">
        <button style="width: 200px;" onclick="startGame()">START GAME</button>
    </div>

    <div id="game-container">
        <div class="game-box">
            <img src="logo.png" alt="Logo" class="game-logo">
            <h3 id="welcome-user" style="margin: 5px 0; color: var(--accent);">Welcome!</h3>
            
            <div class="wallet">
                <span>💰 <span id="coins">400</span></span>
                <span>💎 <span id="diamonds">10</span></span>
            </div>

            <div id="status-msg" class="status-bar">Ready to Play?</div>

            <div class="players-container">
                <div id="p1-card" class="player-card active">
                    <strong id="display-p1-name">P1</strong><br>
                    <b>Runs: <span id="p1-runs">0</span>/50</b><br>
                    <small>Wkts: <span id="p1-wickets">0</span>/2</small>
                </div>
                <div id="p2-card" class="player-card">
                    <strong>Computer</strong><br>
                    <b>Runs: <span id="p2-runs">0</span>/50</b><br>
                    <small>Wkts: <span id="p2-wickets">0</span>/2</small>
                </div>
            </div>

            <div style="font-size: 60px; margin: 10px;" id="dice-display">🎲</div>
            <button onclick="playTurn()">ROLL DICE</button>

            <div style="margin-top: 15px;">
                <button class="emoji-btn" onclick="sendEmoji('🔥')">🔥</button>
                <button class="emoji-btn" onclick="sendEmoji('😂')">😂</button>
                <button class="emoji-btn" onclick="sendEmoji('🏆')">🏆</button>
            </div>
        </div>
    </div>

<script>
    // 1. Loading Logic
    let progress = 0;
    let fill = document.getElementById('fill');
    let loadInterval = setInterval(() => {
        progress += 2;
        fill.style.width = progress + "%";
        if(progress >= 100) {
            clearInterval(loadInterval);
            document.getElementById('loading-screen').style.display = 'none';
            document.getElementById('name-screen').style.display = 'flex';
        }
    }, 50);

    // 2. Start Game Logic
    let playerName = "Player 1";
    function startGame() {
        let input = document.getElementById('player-name-input').value;
        if(input.trim() !== "") playerName = input;
        
        document.getElementById('display-p1-name').innerText = playerName;
        document.getElementById('welcome-user').innerText = "Namaste, " + playerName + "!";
        document.getElementById('name-screen').style.display = 'none';
        document.getElementById('game-container').style.display = 'block';
        <link rel="manifest" href="manifest.json">
    }

    // 3. Core Game Logic (Simplified for Home Page)
    let scores = [0, 0], wickets = [0, 0], wallet = {coins: 400, diamonds: 10}, currentPlayer = 0;

    function playTurn() {
        const outcomes = 

    
