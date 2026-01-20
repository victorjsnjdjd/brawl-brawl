<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Brawl Stars - Pro Edition</title>
    <style>
        :root { --brawl-yellow: #ffcc00; --win: #00ffcc; --lost: #ff4141; --ui-bg: rgba(0,0,0,0.7); }
        body { margin: 0; overflow: hidden; background: #000; font-family: 'Arial Black', sans-serif; color: white; user-select: none; }

        /* --- UI MENU --- */
        .overlay { position: fixed; inset: 0; z-index: 1000; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: 0.6s cubic-bezier(0.4, 0, 0.2, 1); }
        #menu { background: radial-gradient(circle, #1a2a6c, #b21f1f); }
        .hide { opacity: 0; pointer-events: none; transform: translateY(-50px); }

        #name-input { padding: 15px; border-radius: 15px; border: 3px solid var(--brawl-yellow); background: #222; color: white; font-size: 20px; text-align: center; margin-bottom: 20px; outline: none; }

        .selector { display: flex; gap: 15px; margin-bottom: 30px; }
        .card { 
            padding: 20px; border: 5px solid rgba(255,255,255,0.1); border-radius: 20px; 
            cursor: pointer; background: rgba(0,0,0,0.4); width: 130px; height: 150px;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: 0.2s;
        }
        .card.selected { border-color: var(--brawl-yellow); background: rgba(255,204,0,0.2); transform: scale(1.05); }
        .card div { font-size: 55px; margin-bottom: 5px; }

        .btn { padding: 18px 60px; font-size: 28px; background: var(--brawl-yellow); border: none; border-radius: 50px; cursor: pointer; font-weight: bold; box-shadow: 0 6px 0 #947600; text-transform: uppercase; }

        /* --- HUD & FEED --- */
        #kill-feed { position: fixed; bottom: 20px; left: 20px; z-index: 500; display: flex; flex-direction: column; gap: 5px; }
        .kill-msg { background: var(--ui-bg); padding: 8px 15px; border-radius: 5px; font-size: 14px; animation: slideIn 0.3s forwards; border-left: 4px solid var(--lost); }
        @keyframes slideIn { from { transform: translateX(-100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }

        .game-info { position: fixed; top: 20px; right: 20px; background: var(--ui-bg); padding: 10px 20px; border-radius: 30px; border: 2px solid white; font-size: 18px; z-index: 500; }

        /* --- GAME ENGINE --- */
        #game-view { display: none; }
        #camera { position: absolute; will-change: transform; }
        #arena { width: 3000px; height: 3000px; background: #4caf50; background-image: repeating-linear-gradient(rgba(0,0,0,0.05) 0 1px, transparent 1px 100px), repeating-linear-gradient(90deg, rgba(0,0,0,0.05) 0 1px, transparent 1px 100px); }
        
        .brawler { position: absolute; width: 60px; height: 60px; z-index: 50; transition: opacity 0.3s; }
        .name-tag { position: absolute; top: -50px; width: 100%; text-align: center; font-size: 14px; text-shadow: 2px 2px 0 black; }
        .circle { width: 56px; height: 56px; border-radius: 50%; border: 4px solid #000; display: flex; align-items: center; justify-content: center; font-size: 35px; }
        .hp-box { position: absolute; top: -30px; width: 60px; height: 8px; background: #000; border: 1px solid #fff; border-radius: 4px; overflow: hidden; }
        .hp-bar { height: 100%; background: #4bff4b; width: 100%; }

        .box { position: absolute; width: 60px; height: 60px; background: #795548; border: 4px solid #3e2723; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 30px; }
        .drop { position: absolute; width: 25px; height: 25px; background: #00ffcc; border-radius: 50%; border: 2px solid #fff; box-shadow: 0 0 15px #00ffcc; z-index: 40; }
        .bullet { position: absolute; background: #fff; border-radius: 50%; border: 2px solid #000; z-index: 60; }

        #gas-svg { position: absolute; inset: 0; pointer-events: none; z-index: 100; }
        #countdown { position: fixed; inset: 0; z-index: 1100; display: none; align-items: center; justify-content: center; font-size: 150px; color: var(--brawl-yellow); -webkit-text-stroke: 4px black; }
        #end-screen { display: none; background: rgba(0,0,0,0.9); z-index: 2000; text-align: center; }
    </style>
</head>
<body onclick="startCtx()">

    <div id="menu" class="overlay">
        <h1 style="margin: 0 0 20px 0;">BRAWL ROYALE</h1>
        <input type="text" id="name-input" placeholder="Seu Nome" maxlength="12" value="Player">
        <div class="selector">
            <div class="card selected" onclick="sel('Shelly','🤠',this)"><div>🤠</div><h4>Shelly</h4></div>
            <div class="card" onclick="sel('Colt','🔫',this)"><div>🔫</div><h4>Colt</h4></div>
            <div class="card" onclick="sel('Kit','🐱',this)"><div>🐱</div><h4>Kit</h4></div>
        </div>
        <button class="btn" onclick="play()">JOGAR</button>
    </div>

    <div id="kill-feed"></div>
    <div id="countdown">3</div>
    
    <div id="game-view">
        <div class="game-info">VIVOS: <span id="alive-count">10</span></div>
        <div id="camera">
            <div id="arena">
                <svg id="gas-svg">
                    <mask id="m"><rect width="3000" height="3000" fill="white"/><circle id="sz" cx="1500" cy="1500" r="1500" fill="black"/></mask>
                    <rect width="3000" height="3000" fill="rgba(50, 255, 0, 0.45)" mask="url(#m)"/>
                </svg>
            </div>
        </div>
    </div>

    <div id="end-screen" class="overlay">
        <h1 id="end-title" style="font-size: 60px;"></h1>
        <h2 id="end-stats" style="font-size: 30px; color: var(--win);"></h2>
        <button class="btn" style="margin-top: 30px;" onclick="location.reload()">MENU</button>
    </div>

    <script>
        let AC = null;
        let pSet = {n:'Shelly', i:'🤠'}, players = [], boxes = [], drops = [], keys = {};
        let running = false, canMove = false, gasR = 1500, alive = 0;
        let pName = "Player";

        function startCtx() { if(!AC) AC = new (window.AudioContext || window.webkitAudioContext)(); if(AC.state==='suspended') AC.resume(); }
        function sfx(f, t, d, v=0.08) { if(!AC) return; const o=AC.createOscillator(), g=AC.createGain(); o.type=t; o.frequency.setValueAtTime(f, AC.currentTime); g.gain.setValueAtTime(v, AC.currentTime); g.gain.linearRampToValueAtTime(0, AC.currentTime+d); o.connect(g); g.connect(AC.destination); o.start(); o.stop(AC.currentTime+d); }

        function sel(n, i, el) { sfx(500, 'sine', 0.05); pSet={n, i}; document.querySelectorAll('.card').forEach(c=>c.classList.remove('selected')); el.classList.add('selected'); }

        function play() {
            pName = document.getElementById('name-input').value || "Player";
            sfx(800, 'sine', 0.1);
            document.getElementById('menu').classList.add('hide');
            setTimeout(() => {
                document.getElementById('menu').style.display='none';
                document.getElementById('game-view').style.display='block';
                setup();
            }, 600);
        }

        function setup() {
            players = [spawn(pName, pSet.i, 1500, 1500, false)];
            const bNames = ["Mortis","Leon","Crow","Bull","El Primo","Piper","Poco","Spike","Gale"];
            const bIcons = ["🎩","🦎","🦅","🐂","🥊","☂️","🎸","🌵","❄️"];
            for(let i=0; i<9; i++) players.push(spawn(bNames[i], bIcons[i], Math.random()*2400+300, Math.random()*2400+300, true));
            for(let i=0; i<25; i++) spawnBox(Math.random()*2600+200, Math.random()*2600+200);

            const cd = document.getElementById('countdown');
            cd.style.display='flex';
            let t=3;
            let itv = setInterval(() => {
                t--; sfx(400, 'sine', 0.1);
                cd.innerText = t || "BRAWL!";
                if(t < 0) { clearInterval(itv); cd.style.display='none'; canMove=true; running=true; }
            }, 1000);
            requestAnimationFrame(loop);
        }

        function spawn(name, icon, x, y, isBot) {
            const el = document.createElement('div'); el.className = 'brawler';
            el.innerHTML = `<div class="name-tag">${name}</div><div class="hp-box"><div class="hp-bar"></div></div><div class="circle" style="background:${isBot?'#ff4b4b':'#0084ff'}">${icon}</div>`;
            document.getElementById('arena').appendChild(el);
            return { name, icon, x, y, hp:4500, mHp:4500, cubes:0, isBot, el, lastS:0, lastD:0, tx:x, ty:y };
        }

        function spawnBox(x, y) {
            const el = document.createElement('div'); el.className = 'box'; el.innerHTML = '📦';
            el.style.left = x+'px'; el.style.top = y+'px';
            document.getElementById('arena').appendChild(el);
            boxes.push({x, y, hp:2000, el});
        }

        function addKillFeed(victor, loser) {
            const feed = document.getElementById('kill-feed');
            const msg = document.createElement('div');
            msg.className = 'kill-msg';
            msg.innerText = `${victor} detonou ${loser}`;
            feed.appendChild(msg);
            setTimeout(() => msg.remove(), 4000);
        }

        window.onkeydown = e => keys[e.key.toLowerCase()] = true;
        window.onkeyup = e => keys[e.key.toLowerCase()] = false;
        window.onmousedown = e => { if(canMove) attack(players[0], Math.atan2(e.clientY-window.innerHeight/2, e.clientX-window.innerWidth/2)); };

        function attack(p, ang) {
            if(Date.now() - p.lastS < (p.n === 'Kit' ? 450 : 850)) return;
            p.lastS = Date.now();
            sfx(150, 'square', 0.1, 0.05);

            const b = document.createElement('div'); b.className = 'bullet';
            b.style.cssText = `width:16px; height:16px; left:${p.x+22}px; top:${p.y+22}px;`;
            document.getElementById('arena').appendChild(b);
            let bx=p.x+22, by=p.y+22, d=0;
            const iv = setInterval(() => {
                bx += Math.cos(ang)*22; by += Math.sin(ang)*22; d += 22;
                b.style.left = bx+'px'; b.style.top = by+'px';
                if(hit(p, bx, by, 40) || d > 500) { clearInterval(iv); b.remove(); }
            }, 16);
        }

        function hit(p, x, y, r) {
            let h = false;
            players.forEach(t => {
                if(t !== p && t.hp > 0 && Math.hypot(x-(t.x+30), y-(t.y+30)) < r) {
                    t.hp -= 700 + p.cubes*150; t.lastD = Date.now(); h = true;
                    if(t.hp <= 0) addKillFeed(p.name, t.name);
                }
            });
            boxes.forEach(box => {
                if(box.hp > 0 && Math.hypot(x-(box.x+30), y-(box.y+30)) < r) {
                    box.hp -= 700 + p.cubes*150;
                    if(box.hp <= 0) { sfx(100, 'sine', 0.2); box.el.remove(); spawnDrop(box.x+15, box.y+15); }
                    h = true;
                }
            });
            return h;
        }

        function spawnDrop(x, y) {
            const el = document.createElement('div'); el.className = 'drop';
            el.style.left = x+'px'; el.style.top = y+'px';
            document.getElementById('arena').appendChild(el);
            drops.push({x, y, el});
        }

        function loop() {
            if(!running) { if(players[0]) updateCam(players[0]); requestAnimationFrame(loop); return; }
            const p = players[0];

            if(canMove) {
                if(keys['w'] && p.y > 0) p.y -= 4.5; if(keys['s'] && p.y < 2940) p.y += 4.5;
                if(keys['a'] && p.x > 0) p.x -= 4.5; if(keys['d'] && p.x < 2940) p.x += 4.5;
                gasR -= 0.35;
                document.getElementById('sz').setAttribute('r', Math.max(0, gasR));
            }

            alive = 0;
            players.forEach(b => {
                if(b.hp <= 0) { if(b.el.style.display !== 'none') { b.el.style.display='none'; if(b===p) finish(false); } return; }
                alive++;

                // DANO DO GÁS
                if(Math.hypot(b.x+30-1500, b.y+30-1500) > gasR) {
                    b.hp -= 15;
                    if(b.hp <= 0) addKillFeed("Gás Tóxico", b.name);
                }

                // MOVIMENTO BOTS LENTO E INDEPENDENTE
                if(b.isBot && canMove) {
                    let box = boxes.find(bx => bx.hp > 0 && Math.hypot(bx.x-b.x, bx.y-b.y) < 400);
                    let target = box || players.find(o => o!==b && o.hp > 0 && Math.hypot(o.x-b.x, o.y-b.y) < 450);
                    
                    if(target) {
                        let dist = Math.hypot(target.x-b.x, target.y-b.y);
                        b.x += (target.x-b.x)/dist * 2.5; b.y += (target.y-b.y)/dist * 2.5;
                        if(dist < 300) attack(b, Math.atan2(target.y-b.y, target.x-b.x));
                    } else {
                        if(Math.hypot(b.x-b.tx, b.y-b.ty) < 40) { b.tx=Math.random()*2600+200; b.ty=Math.random()*2600+200; }
                        let d = Math.hypot(b.tx-b.x, b.ty-b.y);
                        b.x += (b.tx-b.x)/d * 2.2; b.y += (b.ty-b.y)/d * 2.2;
                    }
                }

                drops.forEach((d, i) => {
                    if(Math.hypot(b.x+30-d.x, b.y+30-d.y) < 50) {
                        b.cubes++; b.hp = Math.min(b.mHp+400, b.hp+800); b.mHp+=400; d.el.remove(); drops.splice(i,1);
                        if(!b.isBot) sfx(900, 'sine', 0.1);
                    }
                });

                b.el.style.transform = `translate(${b.x}px, ${b.y}px)`;
                b.el.querySelector('.hp-bar').style.width = (b.hp/b.mHp*100)+'%';
            });

            document.getElementById('alive-count').innerText = alive;
            if(alive <= 1 && p.hp > 0) finish(true);
            updateCam(p);
            requestAnimationFrame(loop);
        }

        function updateCam(p) {
            camX += (p.x - camX) * 0.1; camY += (p.y - camY) * 0.1;
            document.getElementById('camera').style.transform = `translate(${window.innerWidth/2 - camX - 30}px, ${window.innerHeight/2 - camY - 30}px)`;
        }

        let camX = 1500, camY = 1500;

        function finish(win) {
            running = false;
            const scr = document.getElementById('end-screen');
            scr.style.display = 'flex';
            document.getElementById('end-title').innerText = win ? "VITÓRIA!" : "ELIMINADO";
            document.getElementById('end-title').style.color = win ? "var(--win)" : "var(--lost)";
            document.getElementById('end-stats').innerText = win ? "Você é o Craque!" : "Tente novamente";
            sfx(win ? 1000 : 200, 'sine', 0.5);
        }
    </script>
</body>
</html>
