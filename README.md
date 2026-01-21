<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Brawl Stars - Memória Ativada</title>
    <style>
        :root { --rare: #00a2ff; --epic: #a332ff; --leg: #ffff00; --y: #ffcc00; --gas: rgba(139, 0, 255, 0.5); }
        body { margin: 0; overflow: hidden; background: #000; font-family: 'Arial Black', sans-serif; color: white; user-select: none; }

        /* --- TELAS --- */
        #end-screen { position: fixed; inset: 0; z-index: 500; background: rgba(0,0,0,0.95); display: none; flex-direction: column; align-items: center; justify-content: center; backdrop-filter: blur(15px); }
        #end-title { font-size: 80px; font-style: italic; }
        
        #profile-display { position: absolute; top: 20px; left: 20px; z-index: 60; background: linear-gradient(135deg, #000, #222); padding: 12px 30px; border-radius: 15px 50px 50px 15px; border-left: 6px solid var(--rare); cursor: pointer; display: flex; align-items: center; gap: 15px; border-top: 1px solid #444; }
        .card.selected-blue { border-color: var(--rare) !important; box-shadow: 0 0 20px var(--rare); background: #001a33; }

        #countdown { position: fixed; inset: 0; z-index: 300; display: none; align-items: center; justify-content: center; font-size: 180px; font-style: italic; font-weight: 900; }
        .pop { animation: popAnim 0.8s ease-out; }
        @keyframes popAnim { 0% { transform: scale(0.3); opacity:0; } 50% { transform: scale(1.2); opacity:1; } 100% { transform: scale(1); opacity:0; } }

        #lobby { position: fixed; inset: 0; z-index: 50; background: radial-gradient(circle, #1a2a6c, #000); display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .play-btn { padding: 25px 80px; font-size: 40px; background: var(--y); color: #000; border-radius: 20px; cursor: pointer; border: none; box-shadow: 0 8px 0 #947600; font-style: italic; }
        .overlay { position: fixed; inset: 0; z-index: 200; background: rgba(0,0,0,0.9); display: none; align-items: center; justify-content: center; }
        .window { background: #1a1a1a; padding: 30px; border-radius: 40px; width: 85%; max-height: 80vh; display: flex; flex-direction: column; border: 4px solid #333; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(110px, 1fr)); gap: 15px; overflow-y: auto; padding: 20px; background: #000; border-radius: 20px; }
        .card { background: #222; border: 4px solid #444; border-radius: 15px; padding: 15px; cursor: pointer; text-align: center; }

        /* --- ARENA --- */
        #game { display: none; }
        #arena { width: 3000px; height: 3000px; background: #3e8e41; position: relative; background-image: radial-gradient(#2d6a2f 12%, transparent 13%); background-size: 60px 60px; }
        .brawler { position: absolute; width: 65px; height: 65px; z-index: 10; }
        .b-name { position: absolute; top: -50px; width: 160px; left: -47px; text-align: center; font-size: 14px; font-weight: bold; text-shadow: 2px 2px #000; }
        .circle { width: 100%; height: 100%; border-radius: 50%; border: 4px solid #000; display: flex; align-items: center; justify-content: center; font-size: 35px; }
        .hp { position: absolute; top: -25px; width: 100%; height: 10px; background: #000; border-radius: 5px; border: 1.5px solid #fff; overflow: hidden; }
        .hp-in { height: 100%; background: #0f0; width: 100%; }
        #gas { position: absolute; inset: 0; border: 0px solid var(--gas); pointer-events: none; z-index: 5; box-shadow: inset 0 0 120px 60px var(--gas); }
    </style>
</head>
<body onclick="initAudio()">

    <div id="lobby">
        <div id="profile-display" onclick="openMenu('win-profile')">
            <div style="font-size: 40px;" id="p-icon-v">🤠</div>
            <div>
                <div id="p-name-v">JOGADOR</div>
                <div style="color:var(--y)">🏆 <span id="t-val">0</span></div>
            </div>
        </div>
        <div id="main-preview" style="font-size: 180px;">🤠</div>
        <h1 id="main-b-name">SHELLY</h1>
        <p id="main-b-type" style="color: var(--rare); font-size: 18px; margin-top: -20px;">ATAQUE: LEQUE</p>
        <button class="play-btn" onclick="startSequence()">JOGAR</button>
        <button onclick="openMenu('win-b')" style="margin-top:20px; color:white; background:none; border:2px solid #fff; border-radius:50px; padding:10px 40px; cursor:pointer;">BRAWLERS</button>
    </div>

    <div id="countdown">3</div>

    <div id="end-screen">
        <div id="end-title">VITÓRIA!</div>
        <div id="end-trophies">+10 Troféus</div>
        <button class="play-btn" onclick="location.reload()" style="font-size:25px">VOLTAR AO LOBBY</button>
    </div>

    <div id="win-b" class="overlay">
        <div class="window">
            <h2 style="text-align:center; color:var(--y)">SELECIONE SEU BRAWLER</h2>
            <div class="grid" id="b-grid"></div>
            <button class="play-btn" style="font-size:20px; align-self:center; margin-top:20px;" onclick="closeAll()">PRONTO</button>
        </div>
    </div>

    <div id="win-profile" class="overlay">
        <div class="window" style="max-width:450px;">
            <h2>EDITAR PERFIL</h2>
            <input type="text" id="nick-in" placeholder="NICKNAME" maxlength="12" style="padding:15px; font-size:20px; width:90%; text-align:center;">
            <div class="grid" style="grid-template-columns: repeat(4, 1fr); margin-top:20px;" id="icon-grid"></div>
            <button class="play-btn" style="font-size:20px; margin-top:20px;" onclick="saveProfile()">SALVAR</button>
        </div>
    </div>

    <div id="game">
        <div id="cam" style="position:absolute; inset:0; overflow:hidden;">
            <div id="arena"><div id="gas"></div></div>
        </div>
    </div>

    <script>
        let AC, running=false, players=[], keys={}, gasSize=0;

        // --- CARREGAR DADOS SALVOS ---
        let user = { 
            n: localStorage.getItem('bs_nick') || "VOCÊ", 
            i: localStorage.getItem('bs_icon') || "🤠", 
            t: parseInt(localStorage.getItem('br_t')) || 0 
        };
        let sel = JSON.parse(localStorage.getItem('bs_selected')) || { n: "Shelly", i: "🤠", type: "spread" };

        const rawNames = ["Shelly","Colt","Brock","El Primo","Poco","Rosa","Mortis","Tara","Gene","Max","Leon","Crow","Spike","Sandy","Amber","Meg","Surge","Moe","Kenji","Clancy"];
        const bNames = [];
        for(let i=0; i<98; i++) bNames.push(rawNames[i % rawNames.length] + (i >= rawNames.length ? " " + Math.ceil(i/rawNames.length) : ""));
        const icons = ['🤠','🔫','🦅','🌵','🤖','🦇','🥊','🎸','🦎','🔨','🃏','🛠️','🧞','🏹','🧪','📦','💎','👑','🔥','🌊'];

        // Atualizar Lobby com dados salvos
        function updateLobbyUI() {
            document.getElementById('t-val').innerText = user.t;
            document.getElementById('p-name-v').innerText = user.n;
            document.getElementById('p-icon-v').innerText = user.i;
            document.getElementById('main-preview').innerText = sel.i;
            document.getElementById('main-b-name').innerText = sel.n.toUpperCase();
            document.getElementById('main-b-type').innerText = "ATAQUE: " + (sel.type === "spread" ? "LEQUE" : sel.type === "sniper" ? "PRECISÃO" : "RAJADA");
            document.getElementById('nick-in').value = user.n;
        }

        function initAudio() { if(!AC) AC = new AudioContext(); }
        function playS(f, d, t="sine", v=0.1) {
            if(!AC) return;
            let o = AC.createOscillator(), g = AC.createGain();
            o.type = t; o.frequency.setValueAtTime(f, AC.currentTime);
            o.frequency.exponentialRampToValueAtTime(10, AC.currentTime + d);
            g.gain.setValueAtTime(v, AC.currentTime);
            g.gain.exponentialRampToValueAtTime(0.0001, AC.currentTime + d);
            o.connect(g); g.connect(AC.destination); o.start(); o.stop(AC.currentTime + d);
        }

        // --- INTERFACE ---
        const bG = document.getElementById('b-grid');
        bNames.forEach((n, i) => {
            const types = ["spread", "sniper", "burst"];
            const type = types[i % 3];
            const card = document.createElement('div');
            card.className = `card ${['r-rare','r-epic','r-leg'][i%3]}`;
            card.innerHTML = `<div style="font-size:40px">${icons[i%icons.length]}</div><small>${n}</small>`;
            card.onclick = () => {
                playS(600, 0.1, "square");
                sel = { n, i: icons[i%icons.length], type };
                localStorage.setItem('bs_selected', JSON.stringify(sel)); // SALVAR BRAWLER
                updateLobbyUI();
                closeAll();
            };
            bG.appendChild(card);
        });

        const iG = document.getElementById('icon-grid');
        icons.forEach(icon => {
            const d = document.createElement('div'); d.className='card'; d.innerText=icon; d.style.fontSize="30px";
            if(icon === user.i) d.classList.add('selected-blue');
            d.onclick = function() {
                playS(900, 0.1, "sine", 0.2);
                document.querySelectorAll('#icon-grid .card').forEach(c => c.classList.remove('selected-blue'));
                this.classList.add('selected-blue');
                user.i = icon;
                localStorage.setItem('bs_icon', icon); // SALVAR ÍCONE
            };
            iG.appendChild(d);
        });

        function openMenu(id) { playS(400,0.1); document.getElementById(id).style.display='flex'; }
        function closeAll() { document.querySelectorAll('.overlay').forEach(o=>o.style.display='none'); }
        
        function saveProfile() {
            user.n = document.getElementById('nick-in').value || "VOCÊ";
            localStorage.setItem('bs_nick', user.n); // SALVAR NICK
            updateLobbyUI();
            closeAll();
        }

        // --- GAMEPLAY ---
        function startSequence() {
            initAudio();
            document.getElementById('lobby').style.display='none';
            const cd = document.getElementById('countdown');
            cd.style.display='flex';
            const s = ["3","2","1","BRAWL!"];
            s.forEach((t, i) => {
                setTimeout(() => {
                    cd.innerText = t; cd.className = "pop";
                    playS(300 + (i*100), 0.4, t==="BRAWL!"?"sawtooth":"square", 0.2);
                    setTimeout(()=>cd.className="", 900);
                    if(t==="BRAWL!") setTimeout(()=>{ cd.style.display='none'; startGame(); }, 800);
                }, i*1000);
            });
        }

        function spawn(name, icon, x, y, isBot, type) {
            const el = document.createElement('div'); el.className='brawler';
            el.innerHTML = `<div class="b-name" style="color:${isBot?'red':'#0f0'}">${name}</div><div class="hp"><div class="hp-in"></div></div><div class="circle" style="background:${isBot?'#800':'#006eff'}">${icon}</div>`;
            document.getElementById('arena').appendChild(el);
            return { name, x, y, hp:5000, el, isBot, vx:(Math.random()-0.5)*6, vy:(Math.random()-0.5)*6, lastA:0, type };
        }

        function startGame() {
            document.getElementById('game').style.display='block';
            players = [spawn(user.n, sel.i, 1500, 1500, false, sel.type)];
            for(let i=0; i<9; i++) {
                const bType = ["spread", "sniper", "burst"][Math.floor(Math.random()*3)];
                players.push(spawn(bNames[Math.floor(Math.random()*bNames.length)], icons[i%icons.length], Math.random()*2000+500, Math.random()*2000+500, true, bType));
            }
            running = true; loop();
        }

        function fireBullet(p, ang, speed=30, distMax=800) {
            playS(p.isBot?150:1100, 0.15, "sawtooth", 0.04);
            const b = document.createElement('div'); 
            b.style.cssText=`position:absolute; width:16px; height:16px; border-radius:50%; background:${p.isBot?'#fff':'#ff0'}; z-index:100; box-shadow: 0 0 10px ${p.isBot?'#fff':'#ff0'};`;
            let bx=p.x+24, by=p.y+24, d=0; 
            document.getElementById('arena').appendChild(b);
            const iv = setInterval(() => {
                bx += Math.cos(ang)*speed; by += Math.sin(ang)*speed; d+=speed;
                b.style.left=bx+'px'; b.style.top=by+'px';
                players.forEach(t => { 
                    if(t!==p && t.hp>0 && Math.hypot(bx-(t.x+32), by-(t.y+32))<40){ 
                        t.hp-=1000; d=9999; 
                    }
                });
                if(d>distMax){ clearInterval(iv); b.remove(); }
            }, 20);
        }

        function attack(p, ang) {
            if(Date.now() - p.lastA < 1000) return; 
            p.lastA = Date.now();
            if(p.type === "spread") {
                [ang-0.25, ang, ang+0.25].forEach(a => fireBullet(p, a, 25, 500));
            } else if(p.type === "burst") {
                fireBullet(p, ang, 35, 700);
                setTimeout(() => fireBullet(p, ang, 35, 700), 150);
                setTimeout(() => fireBullet(p, ang, 35, 700), 300);
            } else {
                fireBullet(p, ang, 45, 950);
            }
        }

        function loop() {
            if(!running) return;
            const p = players[0];
            const speed = 10;
            gasSize += 0.4; document.getElementById('gas').style.borderWidth = gasSize + 'px';
            
            if(keys['w'] || keys['arrowup']) p.y -= speed;
            if(keys['s'] || keys['arrowdown']) p.y += speed;
            if(keys['a'] || keys['arrowleft']) p.x -= speed;
            if(keys['d'] || keys['arrowright']) p.x += speed;

            players.forEach(b => {
                if(b.hp <= 0) { b.el.style.display='none'; return; }
                if(b.x < gasSize || b.x > 3000-gasSize || b.y < gasSize || b.y > 3000-gasSize) b.hp -= 30;
                if(b.isBot) {
                    b.x += b.vx; b.y += b.vy;
                    if(b.x < gasSize + 150 || b.x > 2850-gasSize) b.vx *= -1;
                    if(b.y < gasSize + 150 || b.y > 2850-gasSize) b.vy *= -1;
                    if(Math.hypot(p.x-b.x, p.y-b.y) < 600 && Math.random() < 0.02) attack(b, Math.atan2(p.y-b.y, p.x-b.x));
                }
                b.el.style.left = b.x+'px'; b.el.style.top = b.y+'px';
                b.el.querySelector('.hp-in').style.width = (b.hp/5000*100)+'%';
            });

            if(p.hp <= 0) { running=false; showEnd(false); }
            else if(players.filter(v=>v.hp>0).length === 1) { running=false; showEnd(true); }
            else {
                document.getElementById('arena').style.transform = `translate(${window.innerWidth/2-p.x-32}px, ${window.innerHeight/2-p.y-32}px)`;
                requestAnimationFrame(loop);
            }
        }

        function showEnd(win) {
            const screen = document.getElementById('end-screen');
            screen.style.display = 'flex';
            document.getElementById('end-title').innerText = win ? "VITÓRIA!" : "DERROTA...";
            document.getElementById('end-title').style.color = win ? "var(--y)" : "red";
            document.getElementById('end-trophies').innerText = win ? "+10 Troféus" : "-5 Troféus";
            user.t = win ? user.t + 10 : Math.max(0, user.t - 5);
            localStorage.setItem('br_t', user.t);
        }

        window.onkeydown = e => keys[e.key.toLowerCase()] = true;
        window.onkeyup = e => keys[e.key.toLowerCase()] = false;
        window.onmousedown = (e) => { if(running) attack(players[0], Math.atan2(e.clientY-window.innerHeight/2, e.clientX-window.innerWidth/2)); };
        
        // Inicializar UI
        updateLobbyUI();
    </script>
</body>
</html>
