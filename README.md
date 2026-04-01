<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-select=none">
    <title>Ludo Cricket MTSC Pro</title>
    <link rel="manifest" href="manifest.json">
    <style>
        :root { --primary: #1565c0; --accent: #ffeb3b; --bg: #0d1117; --mtsc: #ff5722; --dark-blue: #09122a; --gold: #ffd700; }
        body { font-family: 'Segoe UI', sans-serif; text-align: center; background: linear-gradient(to bottom, #1c2e4e, #0d1117); color: white; margin: 0; padding: 0; overflow: hidden; height: 100vh; }
        .screen { display: none; position: fixed; width: 100%; height: 100%; background: var(--dark-blue); flex-direction: column; align-items: center; justify-content: center; z-index: 900; }
        .active-screen { display: flex; }
        .box { background: #161b22; width: 85%; max-width: 400px; padding: 25px; border-radius: 25px; border: 2px solid #30363d; box-shadow: 0 10px 40px rgba(0,0,0,0.6); }
        #loading { position: fixed; width: 100%; height: 100%; background: #000; display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 1000; }
        .logo { width: 130px; height: 130px; border-radius: 50%; border: 4px solid var(--accent); animation: glow 2s infinite; }
        @keyframes glow { 0%, 100% { box-shadow: 0 0 10px var(--accent); } 50% { box-shadow: 0 0 25px var(--accent); } }
        .bar { width: 200px; height: 6px; background: #222; border-radius: 10px; margin-top: 20px; overflow: hidden; }
        .fill { width: 0%; height: 100%; background: var(--primary); transition: 0.1s; }
        .btn { background: var(--primary); color: white; border: none; padding: 14px; border-radius: 50px; cursor: pointer; font-weight: bold; width: 100%; margin-top: 10px; font-size: 16px; }
        .mtsc-btn { background: var(--mtsc); }
        .player-card { padding: 10px; border-radius: 15px; border: 2px solid #333; width: 44%; background: #0d1117; }
        .p-active { border-color: #4af2ff; box-shadow: 0 0 15px rgba(74, 242, 255, 0.3); }
        #dice { font-size: 60px; margin: 15px 0; }
    </style>
</head>
<body>

    <audio id="snd-dice" src="https://www.soundjay.com/board-game/sounds/dice-roll-1.mp3"></audio>
    <audio id="snd-wicket" src="https://www.myinstants.com/media/sounds/wrong-answer-126515.mp3"></audio>
    <audio id="snd-win" src="https://www.myinstants.com/media/sounds/success-fanfare-trumpets-6185.mp3"></audio>

    <div id="loading">
        <img src="logo.png" alt="Logo" class="logo">
        <div class="bar"><div class="fill" id="load-fill"></div></div>
    </div>

    <div id="dash-screen" class="screen">
        <div class="box">
            <h3 id="hi-name">Namaste!</h3>
            <p style="color: gold; font-size: 14px;">STATE RANKING: #<span id="rank-val">482</span></p>
            <div style="display: flex; justify-content: space-around; background: #21262d; padding: 12px; border-radius: 15px; margin: 15px 0; color: var(--accent); font-weight: bold;">
                <span>💰 <span id="d-coins">0</span></span>
                <span>💎 <span id="d-diams">0</span></span>
            </div>
            <button class="btn mtsc-btn" onclick="showMTSC()">🏆 MTSC TOURNAMENT</button>
            <button class="btn" style="background: #16a34a;" onclick="toGame()">START MATCH</button>
            <button class="btn" style="background: transparent; border: 1px solid #444; font-size: 12px;" onclick="toggleAbout(true)">ℹ️ ABOUT DEVELOPER</button>
        </div>
    </div>

    <div id="game-screen" class="screen">
        <div class="box">
            <div id="status" style="background: var(--primary); padding: 10px; border-radius: 12px; margin-bottom: 15px; font-weight: bold;">Game On!</div>
            <div style="display: flex; justify-content: space-between; margin-bottom: 20px;">
                <div id="p1" class="player-card p-active"><b id="p1-n">P1</b><br>Runs: <span id="s1">0</span><br>Wkt: <span id="w1">0</span>/2</div>
                <div id="p2" class="player-card"><b>Smart Bot</b><br>Runs: <span id="s2">0</span><br>Wkt: <span id="w2">0</span>/2</div>
            </div>
            <div id="dice">🎲</div>
            <button class="btn" id="roll" onclick="play()">ROLL DICE</button>
            <button class="btn" style="background: transparent; border: 1px solid #444;" onclick="toDash()">QUIT</button>
        </div>
    </div>

    <div id="about-modal" class="screen" style="background: rgba(0,0,0,0.9);">
        <div class="box" style="background: linear-gradient(135deg, #1e3a8a, #1e1b4b);">
            <h2 style="color: var(--accent);">Abhilakh Kumar</h2>
            <p style="font-size: 14px; color: #aaa;">School: Wisdom Convent<br>Insta: @abhilakh_kushwaha</p>
            <button class="btn" onclick="toggleAbout(false)">CLOSE</button>
        </div>
    </div>

<script>
    let user = JSON.parse(localStorage.getItem('lc_user')) || { name: "Abhilakh", coins: 400, diams: 10, rank: 482 };
    let scores = [0, 0], wkts = [0, 0], turn = 0;

    // Loading Logic
    let prg = 0;
    let inv = setInterval(() => {
        prg += 2;
        document.getElementById('load-fill').style.width = prg + "%";
        if(prg >= 100) { clearInterval(inv); document.getElementById('loading').style.display = 'none'; toDash(); }
    }, 40);

    function toDash() {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
        document.getElementById('dash-screen').classList.add('active-screen');
        document.getElementById('d-coins').innerText = user.coins;
        document.getElementById('d-diams').innerText = user.diams;
        document.getElementById('rank-val').innerText = user.rank;
    }

    function toGame() {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
        document.getElementById('game-screen').classList.add('active-screen');
        scores = [0, 0]; wkts = [0, 0]; turn = 0; updateUI();
    }

    function toggleAbout(show) { document.getElementById('about-modal').style.display = show ? 'flex' : 'none'; }

    function play() {
        document.getElementById('snd-dice').play();
        const res = [1, 2, 3, 4, 6, "OUT"][Math.floor(Math.random()*6)];
        const icons = {1:"⚀", 2:"⚁", 3:"⚂", 4:"⚃", 6:"⚅", "OUT":"☝️"};
        document.getElementById('dice').innerText = icons[res];

        if(res === "OUT") {
            document.getElementById('snd-wicket').play();
            wkts[turn]++;
        } else {
            scores[turn] += res;
        }
        updateUI();

        if(res !== 6) {
            turn = 1; updateCards();
            document.getElementById('roll').disabled = true;
            setTimeout(bot, 1200);
        }
    }

    function bot() {
        document.getElementById('snd-dice').play();
        const res = [1, 2, 3, 4, 6, "OUT"][Math.floor(Math.random()*6)];
        if(res === "OUT") { document.getElementById('snd-wicket').play(); wkts[1]++; }
        else { scores[1] += res; }
        updateUI();
        if(res === 6 && wkts[1] < 2) { setTimeout(bot, 1200); return; }
        turn = 0; updateCards();
        document.getElementById('roll').disabled = false;
    }

    function updateUI() {
        document.getElementById('s1').innerText = scores[0]; document.getElementById('w1').innerText = wkts[0];
        document.getElementById('s2').innerText = scores[1]; document.getElementById('w2').innerText = wkts[1];
        if(wkts[0] >= 2 || wkts[1] >= 2) checkWin();
    }

    function updateCards() {
        document.getElementById('p1').className = turn === 0 ? 'player-card p-active' : 'player-card';
        document.getElementById('p2').className = turn === 1 ? 'player-card p-active' : 'player-card';
    }

    function checkWin() {
        if(scores[0] > scores[1]) {
            document.getElementById('snd-win').play();
            alert("Victory! Rank Improved!");
            user.rank = Math.max(1, user.rank - 5);
            user.coins += 50;
        } else {
            alert("Bot won!");
        }
        localStorage.setItem('lc_user', JSON.stringify(user));
        toDash();
    }
</script>
</body>
</html>
