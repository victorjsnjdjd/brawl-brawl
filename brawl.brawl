<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Brawl Stars Pro</title>
    <style>
        :root { 
            --rare: #00a2ff; --epic: #a332ff; --leg: #ffff00; --mythic: #ff4444; --y: #ffcc00; 
            --gas: rgba(139, 0, 255, 0.75); --bg: #1a2a6c;
        }
        body { margin: 0; overflow: hidden; background: #000; font-family: 'Arial Black', sans-serif; color: white; user-select: none; touch-action: none; }

        /* --- LOBBY DESIGN --- */
        #lobby { position: fixed; inset: 0; z-index: 100; background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d); background-size: 400% 400%; animation: gradientBG 15s ease infinite; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        @keyframes gradientBG { 0% {background-position: 0% 50%} 50% {background-position: 100% 50%} 100% {background-position: 0% 50%} }

        #profile-card { position: absolute; top: 20px; left: 20px; background: rgba(0,0,0,0.6); padding: 10px 20px; border-radius: 50px; border: 3px solid #fff; display: flex; align-items: center; gap: 15px; cursor: pointer; transition: 0.2s; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
        #profile-card:hover { transform: scale(1.05); background: rgba(0,0,0,0.8); }

        .play-btn { padding: 25px 100px; font-size: 45px; background: linear-gradient(#ffcc00, #ff8800); color: #000; border-radius: 25px; border: none; box-shadow: 0 10px 0 #947600; font-style: italic; cursor: pointer; transition: 0.1s; }
        .play-btn:active { transform: translateY(5px); box-shadow: 0 5px 0 #947600; }

        /* --- MODAIS --- */
        .modal { position: fixed; inset: 0; z-index: 500; background: rgba(0,0,0,0.9); display: none; align-items: center; justify-content: center; backdrop-filter: blur(10px); }
        .window { background: #222; width: 90%; max-height: 80vh; border-radius: 30px; border: 5px solid #444; padding: 20px; display: flex; flex-direction: column; overflow: hidden; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(120px, 1fr)); gap: 15px; overflow-y: auto; padding: 10px; }
        .b-card { background: #333; border-radius: 20px; padding: 15px; text-align: center; border-bottom: 5px solid #000; transition: 0.2s; cursor: pointer; }
        .b-card:active { transform: scale(0.9); }

        /* --- JOGO --- */
        #game { display: none; }
        #arena { width: 3000px; height: 3000px; background: #3e8e41; position: relative; background-image: radial-gradient(rgba(0,0,0,0.2) 15%, transparent 16%); background-size: 80px 80px; will-change: transform; }
        #gas { position: absolute; inset: 0; border: 0px solid var(--gas); pointer-events: none; z-index: 50; box-shadow: inset 0 0 150px 80px var(--gas); }
        
        /* MIRA */
        .laser { position: absolute; height: 6px; background: rgba(255,255,255,0.3); border-radius: 10px; transform-origin: 0 50%; pointer-events: none; display: none; z-index: 40; border: 1px solid rgba(255,255,255,0.5); }

        /* CONTAGEM */
        #countdown { position: fixed; inset: 0; z-index: 1000; display: none; align-items: center; justify-content: center; pointer-events: none; }
        .cnt { font-size: 200px; font-weight: 900; animation: pop 0.9s ease-out; text-shadow: 0 10px 0 #000; }
        @keyframes pop { 0% { transform: scale(0); opacity: 0; } 50% { transform: scale(1.2); opacity: 1; } 100% { transform: scale(1); opacity: 0; } }

        /* CONTROLES */
        .joy { position: fixed; bottom: 60px; width: 150px; height: 150px; background: rgba(255,255,255,0.1); border-radius: 50%; border: 3px solid rgba(255,255,255,0.3); z-index: 600; }
        .stick { position: absolute; top: 50%; left: 50%; width: 70px; height: 70px; background: #fff; border-radius: 50%; transform: translate(-50%, -50%); opacity: 0.6; }
    </style>
</head>
<body onclick="initAudio()">

    <div id="lobby">
        <div id="profile-card" onclick="openModal('modal-profile')">
            <div id="p-icon" style="font-size: 50px;">🤠</div>
            <div>
                <b id="p-name" style="font-size: 20px;">PLAYER</b><br>
                <span style="color:var(--y)">🏆 <span id="p-trophies">0</span></span>
            </div>
        </div>
        <div id="brawler-preview" style="font-size: 200px; filter: drop-shadow(0 20px 30px rgba(0,0,0,0.5));">🤠</div>
        <h1 id="brawler-name" style="font-size: 60px; margin: 10px 0; font-style: italic;">SHELLY</h1>
        <button class="play-btn" onclick="startSequence()">JOGAR</button>
        <button onclick="openModal('modal-brawlers')" style="margin-top: 20px; background: rgba(0,0,0,0.5); border: 2px solid #fff; color: white; padding: 10px 40px; border-radius: 50px; cursor: pointer;">BRAWLERS</button>
    </div>

    <div id="modal-brawlers" class="modal">
        <div class="window">
            <h2 style="text-align:center; color: var(--y)">MEUS BRAWLERS</h2>
            <div class="grid" id="b-grid"></div>
            <button class="play-btn" style="font-size: 20px; margin-top: 20px;" onclick="closeModals()">FECHAR</button>
        </div>
    </div>

    <div id="modal-profile" class="modal">
        <div class="window" style="max-width: 400px;">
            <h2 style="text-align:center;">EDITAR PERFIL</h2>
            <input type="text" id="name-input" placeholder="NOME" style="width: 100%; padding: 15px; border-radius: 10px; border: none; font-size: 20px; text-align: center;">
            <div class="grid" style="grid-template-columns: repeat(3, 1fr); margin-top: 15px;" id="icon-grid"></div>
            <button class="play-btn" style="font-size: 20px; margin-top: 20px;" onclick="saveProfile()">SALVAR</button>
        </div>
    </div>

    <div id="game">
        <div id="cam" style="position:fixed; inset:0; overflow:hidden;">
            <div id="arena">
                <div id="laser-line" class="laser"></div>
                <div id="gas"></div>
            </div>
        </div>
        <div id="joy-move" class="joy" style="left: 60px;"><div class="stick" id="stick-move"></div></div>
        <div id="joy-aim" class="joy" style="right: 60px;"><div class="stick" id="stick-aim"></div></div>
        <div id="countdown"></div>
    </div>

    <script>
        let AC, running=false, players=[], gasSize=0;
        let user = { n: localStorage.getItem('bs_n')||"VOCÊ", i: localStorage.getItem('bs_i')||"🤠", t: parseInt(localStorage.getItem('bs_t'))||0 };
        let sel = { n: "Shelly", i: "🤠", type: "spread", color: "#00a2ff" };

        const bData = [
            {n:"Shelly", i:"🤠", t:"spread", c:"var(--rare)"}, {n:"Colt", i:"🔫", t:"burst", c:"var(--rare)"},
            {n:"El Primo", i:"🥊", t:"burst", c:"var(--rare)"}, {n:"Poco", i:"🎸", t:"spread", c:"var(--rare)"},
            {n:"Mortis", i:"🦇", t:"burst", c:"var(--mythic)"}, {n:"Tara", i:"🃏", t:"spread", c:"var(--mythic)"},
            {n:"Leon", i:"🦎", t:"burst", c:"var(--leg)"}, {n:"Crow", i:"🐦", t:"burst", c:"var(--leg)"},
            {n:"Spike", i:"🌵", t:"spread", c:"var(--leg)"}, {n:"Sandy", i:"⏳", t:"spread", c:"var(--leg)"}
        ];

        // --- AUDIO ---
        function initAudio() { if(!AC) AC = new (window.AudioContext || window.webkitAudioContext)(); }
        function playS(f, d, t="sine", v=0.1) {
            if(!AC) return;
            let o = AC.createOscillator(), g = AC.createGain();
            o.type = t; o.frequency.value = f;
            g.gain.setValueAtTime(v, AC.currentTime);
            g.gain.exponentialRampToValueAtTime(0.0001, AC.currentTime + d);
            o.connect(g); g.connect(AC.destination);
            o.start(); o.stop(AC.currentTime + d);
        }

        // --- UI LOGIC ---
        function openModal(id) { playS(500,0.1); document.getElementById(id).style.display='flex'; }
        function closeModals() { document.querySelectorAll('.modal').forEach(m=>m.style.display='none'); }

        const bGrid = document.getElementById('b-grid');
        bData.forEach(b => {
            const d = document.createElement('div'); d.className = 'b-card';
            d.style.borderColor = b.c;
            d.innerHTML = `<div style="font-size:50px">${b.i}</div><b style="color:${b.c}">${b.n}</b>`;
            d.onclick = () => { playS(600,0.1); sel=b; updateUI(); closeModals(); };
            bGrid.appendChild(d);
        });

        const iGrid = document.getElementById('icon-grid');
        ["🤠","🤖","💀","⭐","🔥","🃏"].forEach(i => {
            const d = document.createElement('div'); d.className = 'b-card'; d.innerHTML = i;
            d.onclick = () => { playS(800,0.1); user.i = i; };
            iGrid.appendChild(d);
        });

        function saveProfile() {
            user.n = document.getElementById('name-input').value || user.n;
            localStorage.setItem('bs_n', user.n);
            localStorage.setItem('bs_i', user.i);
            updateUI(); closeModals();
        }

        function updateUI() {
            document.getElementById('p-name').innerText = user.n;
            document.getElementById('p-icon').innerText = user.i;
            document.getElementById('p-trophies').innerText = user.t;
            document.getElementById('brawler-preview').innerText = sel.i;
            document.getElementById('brawler-name').innerText = sel.n.toUpperCase();
        }

        // --- GAMEPLAY ---
        let moveInput = {x:0, y:0}, aimAng = null;
        function setupJoy(id, stickId, isAim) {
            const area = document.getElementById(id);
            const stick = document.getElementById(stickId);
            let active = false;
            const handle = (e) => {
                if(!active) return;
                const rect = area.getBoundingClientRect();
                const t = e.touches ? e.touches[0] : e;
                const dx = t.clientX - (rect.left + 75), dy = t.clientY - (rect.top + 75);
                const dist = Math.min(65, Math.hypot(dx, dy));
                const ang = Math.atan2(dy, dx);
                stick.style.transform = `translate(calc(-50% + ${Math.cos(ang)*dist}px), calc(-50% + ${Math.sin(ang)*dist}px))`;
                if(isAim) {
                    aimAng = ang;
                    const laser = document.getElementById('laser-line');
                    laser.style.display = dist > 20 ? 'block' : 'none';
                    laser.style.left = (players[0].x + 35) + 'px';
                    laser.style.top = (players[0].y + 35) + 'px';
                    laser.style.transform = `rotate(${ang}rad) scaleX(200)`;
                } else {
                    moveInput = {x: (Math.cos(ang)*dist)/65, y: (Math.sin(ang)*dist)/65};
                }
            };
            area.addEventListener('touchstart', (e) => { active=true; handle(e); });
            area.addEventListener('mousedown', (e) => { active=true; handle(e); });
            window.addEventListener('mousemove', handle);
            window.addEventListener('touchmove', handle);
            window.addEventListener('mouseup', () => { 
                if(isAim && active && aimAng !== null) { attack(players[0], aimAng); document.getElementById('laser-line').style.display='none'; }
                active=false; stick.style.transform='translate(-50%,-50%)'; if(!isAim) moveInput={x:0, y:0};
            });
            window.addEventListener('touchend', () => { 
                if(isAim && active && aimAng !== null) { attack(players[0], aimAng); document.getElementById('laser-line').style.display='none'; }
                active=false; stick.style.transform='translate(-50%,-50%)'; if(!isAim) moveInput={x:0, y:0};
            });
        }

        function startSequence() {
            initAudio();
            document.getElementById('lobby').style.display = 'none';
            document.getElementById('game').style.display = 'block';
            const cd = document.getElementById('countdown');
            cd.style.display = 'flex';
            const s = ["3","2","1","BRAWL!"];
            s.forEach((t, i) => {
                setTimeout(() => {
                    cd.innerHTML = `<div class="cnt" style="color:${i===3?'var(--y)':'#fff'}">${t}</div>`;
                    playS(400 + (i*100), 0.3, "square");
                    if(i === 3) setTimeout(() => { cd.style.display='none'; startGame(); }, 800);
                }, i * 1000);
            });
        }

        function startGame() {
            players = [spawn(user.n, sel.i, 1500, 1500, false, sel.type, "#2980b9")];
            for(let i=0; i<7; i++) players.push(spawn("BOT "+(i+1), bData[i%bData.length].i, Math.random()*2000+500, Math.random()*2000+500, true, "spread", "#c0392b"));
            setupJoy('joy-move', 'stick-move', false);
            setupJoy('joy-aim', 'stick-aim', true);
            running = true; loop();
        }

        function spawn(n, i, x, y, bot, type, color) {
            const el = document.createElement('div'); el.className='brawler';
            el.style.cssText = `position:absolute; width:70px; height:70px; z-index:10;`;
            el.innerHTML = `<div style="position:absolute;top:-25px;width:100%;height:10px;background:#000;border:2px solid #fff;border-radius:5px;overflow:hidden;"><div class="hp-in" style="height:100%;background:#0f0;width:100%"></div></div><div class="circle" style="background:${color}">${i}</div><small style="display:block;text-align:center;">${n}</small>`;
            document.getElementById('arena').appendChild(el);
            return { x, y, hp:5000, el, bot, type, lastA:0 };
        }

        function attack(p, ang) {
            if(Date.now() - p.lastA < 800) return;
            p.lastA = Date.now();
            playS(p.bot?150:1200, 0.1, "sawtooth", 0.05);
            const bCount = p.type === "spread" ? 3 : 1;
            for(let i=0; i<bCount; i++) fireBullet(p, ang + (i - (bCount-1)/2)*0.25);
        }

        function fireBullet(p, ang) {
            const b = document.createElement('div');
            b.style.cssText = `position:absolute; width:18px; height:18px; border-radius:50%; background:${p.bot?'#fff':'#ff0'}; box-shadow:0 0 10px #ff0; left:${p.x+26}px; top:${p.y+26}px; z-index:45;`;
            document.getElementById('arena').appendChild(b);
            let d=0, bx=p.x+26, by=p.y+26;
            const iv = setInterval(() => {
                bx += Math.cos(ang)*30; by += Math.sin(ang)*30; d+=30;
                b.style.left = bx+'px'; b.style.top = by+'px';
                players.forEach(t => {
                    if(t!==p && t.hp>0 && Math.hypot(bx-(t.x+35), by-(t.y+35)) < 40) {
                        t.hp -= 1000; d=999; playS(200,0.05,"sine",0.2);
                    }
                });
                if(d>800) { clearInterval(iv); b.remove(); }
            }, 16);
        }

        function loop() {
            if(!running) return;
            const p = players[0];
            gasSize += 0.5;
            document.getElementById('gas').style.borderWidth = gasSize + 'px';

            players.forEach(b => {
                if(b.hp <= 0) { b.el.style.display='none'; return; }
                if(!b.bot) {
                    b.x += moveInput.x * 13; b.y += moveInput.y * 13;
                } else {
                    const dist = Math.hypot(p.x-b.x, p.y-b.y);
                    if(dist < 600) {
                        b.x += (p.x > b.x ? 1 : -1) * 6; b.y += (p.y > b.y ? 1 : -1) * 6;
                        if(Math.random() < 0.02) attack(b, Math.atan2(p.y-b.y, p.x-b.x));
                    }
                }
                if(b.x < gasSize || b.x > 3000-gasSize || b.y < gasSize || b.y > 3000-gasSize) b.hp -= 30;
                b.el.style.transform = `translate(${b.x}px, ${b.y}px)`;
                b.el.querySelector('.hp-in').style.width = (b.hp/5000*100)+'%';
            });

            document.getElementById('arena').style.transform = `translate(${window.innerWidth/2 - p.x - 35}px, ${window.innerHeight/2 - p.y - 35}px)`;

            if(p.hp <= 0) { alert("DERROTA!"); location.reload(); }
            else if(players.filter(v=>v.hp>0).length === 1) { user.t += 10; localStorage.setItem('bs_t', user.t); alert("VITÓRIA!"); location.reload(); }
            else requestAnimationFrame(loop);
        }

        updateUI();
    </script>
</body>
</html>
