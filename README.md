  <!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Qora Mushuk: Tungi Jangchi / Black Cat / Черный Кот</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            font-family: 'Courier New', Courier, monospace;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #0d0e15;
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #game-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            max-width: 1200px;
            max-height: 800px;
            background: #12141d;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.8);
            border: 4px solid #2a2e3d;
            border-radius: 8px;
            overflow: hidden;
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
            image-rendering: pixelated;
        }

        /* UI Overlays */
        .ui-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
        }

        /* HUD Top Bar */
        .hud-top {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            pointer-events: auto;
        }

        .stats-box {
            background: rgba(15, 17, 26, 0.85);
            border: 2px solid #4a5168;
            border-radius: 8px;
            padding: 12px 16px;
            min-width: 220px;
        }

        .bar-container {
            width: 100%;
            height: 14px;
            background: #222533;
            border-radius: 7px;
            margin: 6px 0 10px 0;
            overflow: hidden;
            border: 1px solid #4a5168;
        }

        .bar-fill {
            height: 100%;
            width: 100%;
            transition: width 0.2s ease;
        }

        #hp-bar { background: linear-gradient(90deg, #ff3b30, #ff6b60); }
        #exp-bar { background: linear-gradient(90deg, #34c759, #30d158); }

        .stat-text {
            font-size: 14px;
            font-weight: bold;
            color: #d1d5db;
            display: flex;
            justify-content: space-between;
        }

        /* Top Right Buttons */
        .top-right-controls {
            display: flex;
            gap: 10px;
        }

        .icon-btn {
            background: #2a2e3d;
            border: 2px solid #4a5168;
            color: #fff;
            width: 44px;
            height: 44px;
            border-radius: 8px;
            font-size: 20px;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: all 0.2s;
        }

        .icon-btn:hover {
            background: #3a3f54;
            border-color: #6366f1;
            transform: scale(1.05);
        }

        /* Modals & Screens */
        .modal {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(8, 9, 14, 0.88);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            pointer-events: auto;
            z-index: 10;
            backdrop-filter: blur(4px);
        }

        .hidden { display: none !important; }

        .modal-card {
            background: #181b26;
            border: 3px solid #6366f1;
            border-radius: 12px;
            padding: 30px;
            width: 90%;
            max-width: 480px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(0,0,0,0.7);
        }

        h1 {
            font-size: 28px;
            color: #a5b4fc;
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 1px;
            text-shadow: 2px 2px 0px #000;
        }

        p {
            font-size: 14px;
            color: #9ca3af;
            margin-bottom: 20px;
            line-height: 1.5;
        }

        .btn-main {
            background: #4f46e5;
            color: white;
            border: none;
            padding: 12px 28px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 4px 0 #3730a3;
            margin: 6px;
        }

        .btn-main:hover {
            background: #6366f1;
            transform: translateY(-2px);
            box-shadow: 0 6px 0 #3730a3;
        }

        .btn-main:active {
            transform: translateY(2px);
            box-shadow: 0 0 0 #3730a3;
        }

        /* Settings Specifics */
        .settings-section {
            margin: 20px 0;
            text-align: left;
        }

        .settings-label {
            font-size: 14px;
            color: #d1d5db;
            margin-bottom: 8px;
            display: block;
        }

        .btn-group {
            display: flex;
            gap: 8px;
            margin-bottom: 16px;
        }

        .btn-option {
            flex: 1;
            background: #282c3d;
            border: 2px solid #4a5168;
            color: #9ca3af;
            padding: 10px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
            transition: all 0.2s;
        }

        .btn-option.active {
            background: #4f46e5;
            border-color: #818cf8;
            color: white;
        }

        /* Upgrade Cards Grid */
        .upgrade-grid {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-top: 15px;
            width: 100%;
        }

        .upgrade-card {
            background: #232736;
            border: 2px solid #4a5168;
            border-radius: 8px;
            padding: 12px 16px;
            display: flex;
            align-items: center;
            gap: 16px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
        }

        .upgrade-card:hover {
            border-color: #a5b4fc;
            background: #2e3448;
            transform: scale(1.02);
        }

        .upgrade-icon {
            font-size: 28px;
            background: #181b26;
            width: 48px;
            height: 48px;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            border: 1px solid #4a5168;
        }

        /* Controls Help List */
        .controls-list {
            background: #12141d;
            border-radius: 8px;
            padding: 12px;
            margin: 15px 0;
            text-align: left;
            font-size: 13px;
            color: #d1d5db;
        }

        .controls-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px;
        }
        .controls-item:last-child { margin-bottom: 0; }
        .key {
            background: #2a2e3d;
            padding: 2px 6px;
            border-radius: 4px;
            border: 1px solid #4a5168;
            color: #a5b4fc;
            font-size: 11px;
        }

        /* Mobile On-Screen Controls */
        #mobile-controls {
            position: absolute;
            bottom: 20px;
            left: 20px;
            right: 20px;
            display: none;
            justify-content: space-between;
            align-items: flex-end;
            pointer-events: none;
        }

        .touch-joystick-area {
            width: 120px;
            height: 120px;
            background: rgba(255, 255, 255, 0.08);
            border: 2px dashed rgba(255, 255, 255, 0.2);
            border-radius: 60px;
            pointer-events: auto;
            position: relative;
        }

        .touch-action-btns {
            display: flex;
            gap: 12px;
            pointer-events: auto;
        }

        .touch-btn {
            width: 58px;
            height: 58px;
            background: rgba(79, 70, 229, 0.6);
            border: 2px solid rgba(165, 180, 252, 0.8);
            border-radius: 29px;
            color: white;
            font-size: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(2px);
        }

        .touch-btn:active {
            background: rgba(99, 102, 241, 0.9);
            transform: scale(0.95);
        }

        @media (max-width: 768px) {
            #mobile-controls { display: flex; }
        }
    </style>
</head>
<body>

    <div id="game-container">
        <canvas id="gameCanvas"></canvas>

        <!-- HUD Layer -->
        <div class="ui-layer">
            <div class="hud-top">
                <div class="stats-box">
                    <div class="stat-text">
                        <span id="lbl-hp">HP</span>
                        <span id="hp-text">100 / 100</span>
                    </div>
                    <div class="bar-container">
                        <div id="hp-bar" class="bar-fill"></div>
                    </div>
                    
                    <div class="stat-text">
                        <span id="lbl-lvl">Daraja: 1</span>
                        <span id="wave-text">To'lqin: 1</span>
                    </div>
                    <div class="bar-container">
                        <div id="exp-bar" class="bar-fill" style="width: 0%;"></div>
                    </div>
                </div>

                <div class="top-right-controls">
                    <button class="icon-btn" id="btn-settings" title="Sozlamalar">⚙️</button>
                </div>
            </div>
        </div>

        <!-- Mobile Controls -->
        <div id="mobile-controls">
            <div class="touch-joystick-area" id="joystick"></div>
            <div class="touch-action-btns">
                <div class="touch-btn" id="btn-touch-dash">⚡</div>
                <div class="touch-btn" id="btn-touch-shadow">💥</div>
                <div class="touch-btn" id="btn-touch-attack">🐾</div>
            </div>
        </div>

        <!-- Start Screen Modal -->
        <div id="start-modal" class="modal">
            <div class="modal-card">
                <h1 id="title-main">Qora Mushuk</h1>
                <p id="desc-main">Zombilar, Skeletlar va Ulkan Bosslar armiyasiga qarshi tungi jangga tayyorlaning!</p>

                <div class="controls-list">
                    <div class="controls-item"><span id="ctrl-move">Harakatlanish:</span> <span class="key">WASD / Oklar</span></div>
                    <div class="controls-item"><span id="ctrl-attack">Panja Zarbası:</span> <span class="key">Sichqon / Bo'shliq</span></div>
                    <div class="controls-item"><span id="ctrl-dash">Tezkor Toshish:</span> <span class="key">Shift / R-Click</span></div>
                    <div class="controls-item"><span id="ctrl-skill">Soya Portlashi:</span> <span class="key">Q / E</span></div>
                </div>

                <button class="btn-main" id="btn-start">O'yinni Boshlash</button>
            </div>
        </div>

        <!-- Settings Modal -->
        <div id="settings-modal" class="modal hidden">
            <div class="modal-card">
                <h1 id="title-settings">Sozlamalar</h1>

                <div class="settings-section">
                    <span class="settings-label" id="lbl-sound-setting">Ovoz:</span>
                    <div class="btn-group">
                        <button class="btn-option active" id="btn-sound-on">🔊 Ovoz ON</button>
                        <button class="btn-option" id="btn-sound-off">🔇 Bez Zvuk</button>
                    </div>

                    <span class="settings-label" id="lbl-lang-setting">Til / Language / Язык:</span>
                    <div class="btn-group">
                        <button class="btn-option" id="btn-lang-uz">O'zbek</button>
                        <button class="btn-option" id="btn-lang-en">English</button>
                        <button class="btn-option" id="btn-lang-ru">Русский</button>
                    </div>
                </div>

                <button class="btn-main" id="btn-close-settings">Yopish</button>
            </div>
        </div>

        <!-- Level Up Modal -->
        <div id="upgrade-modal" class="modal hidden">
            <div class="modal-card">
                <h1 id="title-upgrade">Daraja Otdi!</h1>
                <p id="desc-upgrade">Yangi mahorat yoki kuchaytirishni tanlang:</p>
                <div class="upgrade-grid" id="upgrade-list">
                    <!-- Dynamic Upgrade Cards -->
                </div>
            </div>
        </div>

        <!-- Game Over Modal -->
        <div id="gameover-modal" class="modal hidden">
            <div class="modal-card">
                <h1 id="title-gameover" style="color: #ef4444;">Mag'lubiyat!</h1>
                <p id="desc-gameover">Qora Mushuk jasurlik bilan jang qildi.</p>
                <p><span id="lbl-score">Yengilgan dushmanlar:</span> <strong id="final-kills" style="color: #a5b4fc;">0</strong></p>
                <button class="btn-main" id="btn-restart">Qayta Boshlash</button>
            </div>
        </div>
    </div>

    <script>
        /* --- TRANSLATIONS DICTIONARY --- */
        const TRANSLATIONS = {
            uz: {
                titleMain: "Qora Mushuk",
                descMain: "Zombilar, Skeletlar va Ulkan Bosslar armiyasiga qarshi tungi jangga tayyorlaning!",
                ctrlMove: "Harakatlanish:",
                ctrlAttack: "Panja Zarbası:",
                ctrlDash: "Tezkor Toshish:",
                ctrlSkill: "Soya Portlashi:",
                btnStart: "O'yinni Boshlash",
                titleSettings: "Sozlamalar",
                lblSoundSetting: "Ovoz:",
                lblLangSetting: "Til / Language / Язык:",
                btnSoundOn: "🔊 Ovoz ON",
                btnSoundOff: "🔇 Bez Zvuk",
                btnCloseSettings: "Yopish",
                titleUpgrade: "Daraja O'tdi!",
                descUpgrade: "Yangi mahorat yoki kuchaytirishni tanlang:",
                titleGameOver: "Mag'lubiyat!",
                descGameOver: "Qora Mushuk jasurlik bilan jang qildi.",
                lblScore: "Yengilgan dushmanlar:",
                btnRestart: "Qayta Boshlash",
                lblHp: "HP",
                lblLvl: "Daraja",
                lblWave: "To'lqin",
                upgrades: {
                    clawDamage: { name: "Otkir Panjalar", desc: "Zarba kuchini +25% oshiradi" },
                    attackSpeed: { name: "Tezkor Hujum", desc: "Hujum tezligini +20% oshiradi" },
                    moveSpeed: { name: "Tungi Chaqqonlik", desc: "Yugurish tezligini +15% oshiradi" },
                    heal: { name: "Sutli Davo", desc: "Sog'liqni (HP) 50% tiklaydi" },
                    shadowPower: { name: "Soya Qudrati", desc: "Portlash doirasini va zararining kuchini oshiradi" }
                }
            },
            en: {
                titleMain: "Black Cat",
                descMain: "Prepare for night combat against armies of Zombies, Skeletons, and Giant Bosses!",
                ctrlMove: "Movement:",
                ctrlAttack: "Claw Strike:",
                ctrlDash: "Quick Dash:",
                ctrlSkill: "Shadow Burst:",
                btnStart: "Start Game",
                titleSettings: "Settings",
                lblSoundSetting: "Sound:",
                lblLangSetting: "Language:",
                btnSoundOn: "🔊 Sound ON",
                btnSoundOff: "🔇 Mute",
                btnCloseSettings: "Close",
                titleUpgrade: "Level Up!",
                descUpgrade: "Select a new skill or enhancement:",
                titleGameOver: "Game Over!",
                descGameOver: "The Black Cat fought bravely.",
                lblScore: "Enemies Defeated:",
                btnRestart: "Play Again",
                lblHp: "HP",
                lblLvl: "Level",
                lblWave: "Wave",
                upgrades: {
                    clawDamage: { name: "Sharp Claws", desc: "Increases attack damage by +25%" },
                    attackSpeed: { name: "Rapid Attack", desc: "Increases attack speed by +20%" },
                    moveSpeed: { name: "Night Agility", desc: "Increases movement speed by +15%" },
                    heal: { name: "Milk Recovery", desc: "Restores 50% of Health (HP)" },
                    shadowPower: { name: "Shadow Power", desc: "Increases burst area and magic damage" }
                }
            },
            ru: {
                titleMain: "Черный Кот",
                descMain: "Приготовьтесь к ночному бою против армий Зомби, Скелетов и Гигантских Боссов!",
                ctrlMove: "Движение:",
                ctrlAttack: "Удар Лапой:",
                ctrlDash: "Рывок:",
                ctrlSkill: "Взрыв Тэни:",
                btnStart: "Начать Игру",
                titleSettings: "Настройки",
                lblSoundSetting: "Звук:",
                lblLangSetting: "Язык:",
                btnSoundOn: "🔊 Вкл Звук",
                btnSoundOff: "🔇 Без Звука",
                btnCloseSettings: "Закрыть",
                titleUpgrade: "Новый Уровень!",
                descUpgrade: "Выберите новое умение или улучшение:",
                titleGameOver: "Поражение!",
                descGameOver: "Черный Кот сражался храбро.",
                lblScore: "Побеждено врагов:",
                btnRestart: "Играть Снова",
                lblHp: "Здоровье",
                lblLvl: "Уровень",
                lblWave: "Волна",
                upgrades: {
                    clawDamage: { name: "Острые Когти", desc: "Увеличивает урон атаки на +25%" },
                    attackSpeed: { name: "Быстрая Атака", desc: "Увеличивает скорость атаки на +20%" },
                    moveSpeed: { name: "Ночная Ловкость", desc: "Увеличивает скорость бега на +15%" },
                    heal: { name: "Восстановление", desc: "Восстанавливает 50% Здоровья (HP)" },
                    shadowPower: { name: "Сила Тэни", desc: "Увеличивает радиус и урон магии" }
                }
            }
        };

        /* --- AUDIO SYNTHESIZER (Web Audio API) --- */
        class SoundFX {
            constructor() {
                this.ctx = null;
                this.muted = false;
            }

            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }

            play(type) {
                if (this.muted || !this.ctx) return;

                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.connect(gain);
                gain.connect(this.ctx.destination);

                const now = this.ctx.currentTime;

                if (type === 'scratch') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(400, now);
                    osc.frequency.exponentialRampToValueAtTime(100, now + 0.1);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.1);
                    osc.start(now);
                    osc.stop(now + 0.1);
                } else if (type === 'dash') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(200, now);
                    osc.frequency.exponentialRampToValueAtTime(600, now + 0.15);
                    gain.gain.setValueAtTime(0.2, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.15);
                    osc.start(now);
                    osc.stop(now + 0.15);
                } else if (type === 'shadow') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(150, now);
                    osc.frequency.exponentialRampToValueAtTime(40, now + 0.4);
                    gain.gain.setValueAtTime(0.5, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.4);
                    osc.start(now);
                    osc.stop(now + 0.4);
                } else if (type === 'hit') {
                    osc.type = 'square';
                    osc.frequency.setValueAtTime(120, now);
                    osc.frequency.exponentialRampToValueAtTime(30, now + 0.08);
                    gain.gain.setValueAtTime(0.2, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.08);
                    osc.start(now);
                    osc.stop(now + 0.08);
                } else if (type === 'levelup') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(300, now);
                    osc.frequency.setValueAtTime(450, now + 0.1);
                    osc.frequency.setValueAtTime(600, now + 0.2);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.35);
                    osc.start(now);
                    osc.stop(now + 0.35);
                }
            }
        }

        const sfx = new SoundFX();

        /* --- GAME ENGINE & STATE --- */
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let currentLang = 'uz';
        let gameState = 'START'; // START, PLAYING, UPGRADE, GAMEOVER, PAUSED_SETTINGS
        let killsCount = 0;
        let currentWave = 1;

        // Resize Canvas
        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Input Management
        const keys = {};
        const mouse = { x: canvas.width / 2, y: canvas.height / 2, down: false };

        window.addEventListener('keydown', e => {
            keys[e.code] = true;
            if (e.code === 'KeyE' || e.code === 'KeyQ') triggerShadowSkill();
            if (e.code === 'ShiftLeft' || e.code === 'ShiftRight') triggerDash();
        });
        window.addEventListener('keyup', e => keys[e.code] = false);

        canvas.addEventListener('mousemove', e => {
            const rect = canvas.getBoundingClientRect();
            mouse.x = e.clientX - rect.left;
            mouse.y = e.clientY - rect.top;
        });

        canvas.addEventListener('mousedown', e => {
            if (e.button === 0) mouse.down = true;
            if (e.button === 2) triggerDash();
        });
        canvas.addEventListener('mouseup', e => { if (e.button === 0) mouse.down = false; });
        canvas.addEventListener('contextmenu', e => e.preventDefault());

        /* --- PARTICLES & EFFECTS --- */
        let particles = [];
        let projectiles = [];
        let floatingTexts = [];

        class Particle {
            constructor(x, y, color, size, vx, vy, life) {
                this.x = x; this.y = y;
                this.color = color;
                this.size = size;
                this.vx = vx; this.vy = vy;
                this.life = life;
                this.maxLife = life;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.life--;
            }

            draw() {
                ctx.fillStyle = this.color;
                ctx.globalAlpha = Math.max(0, this.life / this.maxLife);
                ctx.fillRect(Math.round(this.x), Math.round(this.y), this.size, this.size);
                ctx.globalAlpha = 1.0;
            }
        }

        class FloatingText {
            constructor(x, y, text, color) {
                this.x = x; this.y = y;
                this.text = text;
                this.color = color;
                this.life = 40;
            }
            update() {
                this.y -= 0.8;
                this.life--;
            }
            draw() {
                ctx.fillStyle = this.color;
                ctx.font = 'bold 14px monospace';
                ctx.globalAlpha = Math.max(0, this.life / 40);
                ctx.fillText(this.text, this.x, this.y);
                ctx.globalAlpha = 1.0;
            }
        }

        /* --- PLAYER CLASS (QORA MUSHUK) --- */
        class Player {
            constructor() {
                this.x = canvas.width / 2;
                this.y = canvas.height / 2;
                this.size = 18;
                this.speed = 3.2;
                this.maxHp = 100;
                this.hp = 100;
                this.level = 1;
                this.exp = 0;
                this.nextExp = 50;

                // Stats & Upgrades
                this.damage = 25;
                this.attackCooldown = 0;
                this.attackSpeed = 22; // frames
                this.shadowRadius = 110;
                this.shadowDamage = 60;

                // Dash
                this.dashCooldown = 0;
                this.isDashing = false;
                this.dashTimer = 0;

                this.angle = 0;
            }

            update() {
                // Movement Input
                let dx = 0, dy = 0;
                if (keys['KeyW'] || keys['ArrowUp']) dy -= 1;
                if (keys['KeyS'] || keys['ArrowDown']) dy += 1;
                if (keys['KeyA'] || keys['ArrowLeft']) dx -= 1;
                if (keys['KeyD'] || keys['ArrowRight']) dx += 1;

                // Normalize movement
                if (dx !== 0 && dy !== 0) {
                    dx *= 0.7071;
                    dy *= 0.7071;
                }

                // Dash Logic
                let currentSpeed = this.speed;
                if (this.isDashing) {
                    currentSpeed = this.speed * 2.8;
                    this.dashTimer--;
                    if (this.dashTimer <= 0) this.isDashing = false;

                    // Dash trail particle
                    particles.push(new Particle(
                        this.x + (Math.random() - 0.5) * 10,
                        this.y + (Math.random() - 0.5) * 10,
                        '#818cf8', 4, 0, 0, 15
                    ));
                }

                this.x += dx * currentSpeed;
                this.y += dy * currentSpeed;

                // Screen Constraints
                this.x = Math.max(20, Math.min(canvas.width - 20, this.x));
                this.y = Math.max(20, Math.min(canvas.height - 20, this.y));

                // Face Mouse Position
                this.angle = Math.atan2(mouse.y - this.y, mouse.x - this.x);

                // Auto/Manual Attack Trigger
                if (this.attackCooldown > 0) this.attackCooldown--;
                if (this.dashCooldown > 0) this.dashCooldown--;

                if ((mouse.down || keys['Space']) && this.attackCooldown <= 0) {
                    this.attack();
                }
            }

            attack() {
                this.attackCooldown = this.attackSpeed;
                sfx.play('scratch');

                // Scratch Arc Effect
                const scratchAngle = this.angle;
                const range = 55;

                for (let i = -0.5; i <= 0.5; i += 0.2) {
                    const pAngle = scratchAngle + i;
                    const px = this.x + Math.cos(pAngle) * range;
                    const py = this.y + Math.sin(pAngle) * range;
                    particles.push(new Particle(px, py, '#e0e7ff', 3, Math.cos(pAngle) * 2, Math.sin(pAngle) * 2, 12));
                }

                // Deal damage to enemies in cone
                enemies.forEach(enemy => {
                    const edx = enemy.x - this.x;
                    const edy = enemy.y - this.y;
                    const dist = Math.hypot(edx, edy);

                    if (dist < range + enemy.size) {
                        const enemyAngle = Math.atan2(edy, edx);
                        let diff = Math.abs(this.angle - enemyAngle);
                        if (diff > Math.PI) diff = 2 * Math.PI - diff;

                        if (diff < 0.9) { // ~50 degrees cone
                            enemy.takeDamage(this.damage);
                        }
                    }
                });
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.angle);

                // Pixel Art Black Cat Rendering
                // Tail
                ctx.strokeStyle = '#1e1b4b';
                ctx.lineWidth = 4;
                ctx.beginPath();
                ctx.moveTo(-10, 0);
                ctx.quadraticCurveTo(-18, Math.sin(Date.now() * 0.01) * 8, -22, -4);
                ctx.stroke();

                // Body
                ctx.fillStyle = '#0f172a'; // Deep Black/Dark Navy
                ctx.fillRect(-12, -10, 22, 20);

                // Head
                ctx.fillStyle = '#1e293b';
                ctx.fillRect(4, -9, 14, 18);

                // Ears
                ctx.fillStyle = '#475569';
                ctx.beginPath();
                ctx.moveTo(8, -9); ctx.lineTo(13, -15); ctx.lineTo(13, -9);
                ctx.moveTo(8, 9); ctx.lineTo(13, 15); ctx.lineTo(13, 9);
                ctx.fill();

                // Glowing Eyes
                ctx.fillStyle = '#38bdf8'; // Cyan Bright Eyes
                ctx.fillRect(12, -6, 3, 3);
                ctx.fillRect(12, 3, 3, 3);

                ctx.restore();
            }

            addExp(amount) {
                this.exp += amount;
                if (this.exp >= this.nextExp) {
                    this.exp -= this.nextExp;
                    this.level++;
                    this.nextExp = Math.round(this.nextExp * 1.4);
                    sfx.play('levelup');
                    triggerUpgradeModal();
                }
                updateHUD();
            }
        }

        function triggerDash() {
            if (player.dashCooldown <= 0) {
                player.isDashing = true;
                player.dashTimer = 10;
                player.dashCooldown = 60; // 1 second
                sfx.play('dash');
            }
        }

        function triggerShadowSkill() {
            if (player.dashCooldown > 30) return; // Share partial cooldown or trigger freely
            sfx.play('shadow');

            // Burst Particles
            for (let i = 0; i < 36; i++) {
                const ang = (i / 36) * Math.PI * 2;
                const vx = Math.cos(ang) * 5;
                const vy = Math.sin(ang) * 5;
                particles.push(new Particle(player.x, player.y, '#c084fc', 5, vx, vy, 20));
            }

            // Damage nearby
            enemies.forEach(enemy => {
                const dist = Math.hypot(enemy.x - player.x, enemy.y - player.y);
                if (dist < player.shadowRadius) {
                    enemy.takeDamage(player.shadowDamage);
                }
            });
        }

        /* --- ENEMIES CLASSES (ZOMBI, SKELET, BOSS) --- */
        let enemies = [];

        class Enemy {
            constructor(x, y, type, waveMultiplier) {
                this.x = x; this.y = y;
                this.type = type; // 'zombie', 'skeleton', 'boss'

                if (type === 'zombie') {
                    this.size = 16;
                    this.speed = 1.4 + Math.random() * 0.4;
                    this.hp = 40 * waveMultiplier;
                    this.color = '#22c55e';
                    this.expVal = 15;
                    this.damage = 10;
                } else if (type === 'skeleton') {
                    this.size = 14;
                    this.speed = 1.8;
                    this.hp = 30 * waveMultiplier;
                    this.color = '#e2e8f0';
                    this.expVal = 20;
                    this.damage = 8;
                    this.shootCooldown = 120;
                } else if (type === 'boss') {
                    this.size = 36;
                    this.speed = 0.9;
                    this.hp = 400 * waveMultiplier;
                    this.color = '#ef4444';
                    this.expVal = 150;
                    this.damage = 25;
                    this.shootCooldown = 90;
                }
                this.maxHp = this.hp;
            }

            update() {
                const dx = player.x - this.x;
                const dy = player.y - this.y;
                const dist = Math.hypot(dx, dy);

                // Move towards player
                if (dist > 0) {
                    this.x += (dx / dist) * this.speed;
                    this.y += (dy / dist) * this.speed;
                }

                // Melee Collision with player
                if (dist < this.size + player.size) {
                    if (!player.isDashing) {
                        player.hp -= this.damage * 0.05; // Continuous contact damage
                        updateHUD();
                        if (player.hp <= 0) gameOver();
                    }
                }

                // Ranged Enemy Logic (Skeleton / Boss)
                if (this.type === 'skeleton' || this.type === 'boss') {
                    this.shootCooldown--;
                    if (this.shootCooldown <= 0) {
                        this.shootCooldown = this.type === 'boss' ? 70 : 140;
                        // Fire Arrow / Projectile
                        const pAngle = Math.atan2(player.y - this.y, player.x - this.x);
                        projectiles.push(new Projectile(
                            this.x, this.y,
                            Math.cos(pAngle) * 3.5, Math.sin(pAngle) * 3.5,
                            this.type === 'boss' ? '#f43f5e' : '#cbd5e1',
                            this.type === 'boss' ? 15 : 8
                        ));
                    }
                }
            }

            takeDamage(amount) {
                this.hp -= amount;
                sfx.play('hit');
                floatingTexts.push(new FloatingText(this.x, this.y - 10, Math.round(amount), '#f2f2f2'));

                // Hit particles
                for (let i = 0; i < 4; i++) {
                    particles.push(new Particle(
                        this.x, this.y, this.color, 3,
                        (Math.random() - 0.5) * 3, (Math.random() - 0.5) * 3, 10
                    ));
                }

                if (this.hp <= 0) {
                    killsCount++;
                    player.addExp(this.expVal);
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);

                if (this.type === 'zombie') {
                    // Pixel Zombie
                    ctx.fillStyle = '#15803d';
                    ctx.fillRect(-this.size, -this.size, this.size * 2, this.size * 2);
                    ctx.fillStyle = '#ef4444'; // Red eyes
                    ctx.fillRect(-6, -4, 3, 3); ctx.fillRect(3, -4, 3, 3);
                } else if (this.type === 'skeleton') {
                    // Pixel Skeleton
                    ctx.fillStyle = '#cbd5e1';
                    ctx.fillRect(-this.size, -this.size, this.size * 2, this.size * 2);
                    ctx.fillStyle = '#0f172a'; // Black eye sockets
                    ctx.fillRect(-5, -4, 4, 4); ctx.fillRect(2, -4, 4, 4);
                } else if (this.type === 'boss') {
                    // Giant Boss
                    ctx.fillStyle = '#9f1239';
                    ctx.fillRect(-this.size, -this.size, this.size * 2, this.size * 2);
                    ctx.fillStyle = '#facc15'; // Yellow Crown / Horns
                    ctx.fillRect(-18, -this.size - 8, 36, 8);
                    ctx.fillStyle = '#000';
                    ctx.fillRect(-10, -8, 6, 6); ctx.fillRect(4, -8, 6, 6);
                }

                // Health Bar over Enemy
                if (this.hp < this.maxHp) {
                    ctx.fillStyle = '#1e293b';
                    ctx.fillRect(-15, -this.size - 10, 30, 4);
                    ctx.fillStyle = '#22c55e';
                    ctx.fillRect(-15, -this.size - 10, (this.hp / this.maxHp) * 30, 4);
                }

                ctx.restore();
            }
        }

        /* --- ENEMY PROJECTILES --- */
        class Projectile {
            constructor(x, y, vx, vy, color, damage) {
                this.x = x; this.y = y;
                this.vx = vx; this.vy = vy;
                this.color = color;
                this.damage = damage;
                this.life = 180;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.life--;

                // Check collision with player
                const dist = Math.hypot(this.x - player.x, this.y - player.y);
                if (dist < player.size + 4) {
                    if (!player.isDashing) {
                        player.hp -= this.damage;
                        updateHUD();
                        sfx.play('hit');
                        if (player.hp <= 0) gameOver();
                    }
                    this.life = 0; // Destroy projectile
                }
            }

            draw() {
                ctx.fillStyle = this.color;
                ctx.fillRect(this.x - 3, this.y - 3, 6, 6);
            }
        }

        /* --- SPAWN WAVE SYSTEM --- */
        let waveTimer = 0;

        function handleSpawning() {
            waveTimer++;
            if (waveTimer % 180 === 0) { // Every ~3 seconds
                const waveMult = 1 + (currentWave - 1) * 0.2;
                const count = Math.min(12, 2 + Math.floor(currentWave * 1.2));

                for (let i = 0; i < count; i++) {
                    // Random Position Outside Screen
                    let sx, sy;
                    if (Math.random() < 0.5) {
                        sx = Math.random() < 0.5 ? -20 : canvas.width + 20;
                        sy = Math.random() * canvas.height;
                    } else {
                        sx = Math.random() * canvas.width;
                        sy = Math.random() < 0.5 ? -20 : canvas.height + 20;
                    }

                    const isSkeleton = Math.random() < 0.35;
                    enemies.push(new Enemy(sx, sy, isSkeleton ? 'skeleton' : 'zombie', waveMult));
                }
            }

            // Boss Wave (Every 5 waves)
            if (waveTimer % 1800 === 0) { // Every ~30 seconds wave progresses
                currentWave++;
                updateHUD();

                if (currentWave % 5 === 0) {
                    // Spawn Boss
                    enemies.push(new Enemy(canvas.width / 2, -40, 'boss', 1 + currentWave * 0.3));
                }
            }
        }

        /* --- UI & LANGUAGE UPDATES --- */
        function setLanguage(lang) {
            currentLang = lang;
            const t = TRANSLATIONS[lang];

            document.getElementById('title-main').innerText = t.titleMain;
            document.getElementById('desc-main').innerText = t.descMain;
            document.getElementById('ctrl-move').innerText = t.ctrlMove;
            document.getElementById('ctrl-attack').innerText = t.ctrlAttack;
            document.getElementById('ctrl-dash').innerText = t.ctrlDash;
            document.getElementById('ctrl-skill').innerText = t.ctrlSkill;
            document.getElementById('btn-start').innerText = t.btnStart;

            document.getElementById('title-settings').innerText = t.titleSettings;
            document.getElementById('lbl-sound-setting').innerText = t.lblSoundSetting;
            document.getElementById('lbl-lang-setting').innerText = t.lblLangSetting;
            document.getElementById('btn-sound-on').innerText = t.btnSoundOn;
            document.getElementById('btn-sound-off').innerText = t.btnSoundOff;
            document.getElementById('btn-close-settings').innerText = t.btnCloseSettings;

            document.getElementById('title-upgrade').innerText = t.titleUpgrade;
            document.getElementById('desc-upgrade').innerText = t.descUpgrade;

            document.getElementById('title-gameover').innerText = t.titleGameOver;
            document.getElementById('desc-gameover').innerText = t.descGameOver;
            document.getElementById('lbl-score').innerText = t.lblScore;
            document.getElementById('btn-restart').innerText = t.btnRestart;

            document.getElementById('lbl-hp').innerText = t.lblHp;

            // Highlight Active Language Button
            document.getElementById('btn-lang-uz').classList.toggle('active', lang === 'uz');
            document.getElementById('btn-lang-en').classList.toggle('active', lang === 'en');
            document.getElementById('btn-lang-ru').classList.toggle('active', lang === 'ru');

            updateHUD();
        }

        function updateHUD() {
            const t = TRANSLATIONS[currentLang];
            document.getElementById('hp-text').innerText = `${Math.max(0, Math.ceil(player.hp))} / ${player.maxHp}`;
            document.getElementById('hp-bar').style.width = `${Math.max(0, (player.hp / player.maxHp) * 100)}%`;

            document.getElementById('lbl-lvl').innerText = `${t.lblLvl}: ${player.level}`;
            document.getElementById('wave-text').innerText = `${t.lblWave}: ${currentWave}`;
            document.getElementById('exp-bar').style.width = `${Math.min(100, (player.exp / player.nextExp) * 100)}%`;
        }

        /* --- UPGRADE MODAL SYSTEM --- */
        function triggerUpgradeModal() {
            gameState = 'UPGRADE';
            const listContainer = document.getElementById('upgrade-list');
            listContainer.innerHTML = '';

            const t = TRANSLATIONS[currentLang].upgrades;
            const availableKeys = Object.keys(t);
            // Pick 3 random distinct upgrades
            const shuffled = availableKeys.sort(() => 0.5 - Math.random()).slice(0, 3);

            shuffled.forEach(key => {
                const item = t[key];
                const card = document.createElement('div');
                card.className = 'upgrade-card';

                let icon = '⚔️';
                if (key === 'attackSpeed') icon = '⚡';
                if (key === 'moveSpeed') icon = '🐾';
                if (key === 'heal') icon = '🥛';
                if (key === 'shadowPower') icon = '💥';

                card.innerHTML = `
                    <div class="upgrade-icon">${icon}</div>
                    <div>
                        <div style="font-weight: bold; color: #a5b4fc; font-size: 15px;">${item.name}</div>
                        <div style="font-size: 12px; color: #9ca3af; margin-top: 4px;">${item.desc}</div>
                    </div>
                `;

                card.addEventListener('click', () => applyUpgrade(key));
                listContainer.appendChild(card);
            });

            document.getElementById('upgrade-modal').classList.remove('hidden');
        }

        function applyUpgrade(key) {
            if (key === 'clawDamage') player.damage *= 1.25;
            if (key === 'attackSpeed') player.attackSpeed = Math.max(8, player.attackSpeed * 0.8);
            if (key === 'moveSpeed') player.speed *= 1.15;
            if (key === 'heal') player.hp = Math.min(player.maxHp, player.hp + player.maxHp * 0.5);
            if (key === 'shadowPower') {
                player.shadowRadius += 25;
                player.shadowDamage += 25;
            }

            document.getElementById('upgrade-modal').classList.add('hidden');
            gameState = 'PLAYING';
            updateHUD();
        }

        /* --- GAME OVER --- */
        function gameOver() {
            gameState = 'GAMEOVER';
            document.getElementById('final-kills').innerText = killsCount;
            document.getElementById('gameover-modal').classList.remove('hidden');
        }

        /* --- RESET / START GAME --- */
        let player = new Player();

        function resetGame() {
            player = new Player();
            enemies = [];
            particles = [];
            projectiles = [];
            floatingTexts = [];
            killsCount = 0;
            currentWave = 1;
            waveTimer = 0;
            updateHUD();
        }

        /* --- MAIN GAME LOOP --- */
        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw Background Grid (Retro Pixel Style)
            ctx.strokeStyle = '#181b26';
            ctx.lineWidth = 1;
            const gridSize = 40;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            if (gameState === 'PLAYING') {
                handleSpawning();

                // Update & Draw Player
                player.update();
                player.draw();

                // Update & Draw Enemies
                for (let i = enemies.length - 1; i >= 0; i--) {
                    enemies[i].update();
                    enemies[i].draw();
                    if (enemies[i].hp <= 0) enemies.splice(i, 1);
                }

                // Update & Draw Projectiles
                for (let i = projectiles.length - 1; i >= 0; i--) {
                    projectiles[i].update();
                    projectiles[i].draw();
                    if (projectiles[i].life <= 0) projectiles.splice(i, 1);
                }

                // Update & Draw Particles
                for (let i = particles.length - 1; i >= 0; i--) {
                    particles[i].update();
                    particles[i].draw();
                    if (particles[i].life <= 0) particles.splice(i, 1);
                }

                // Update & Draw Floating Texts
                for (let i = floatingTexts.length - 1; i >= 0; i--) {
                    floatingTexts[i].update();
                    floatingTexts[i].draw();
                    if (floatingTexts[i].life <= 0) floatingTexts.splice(i, 1);
                }
            } else if (gameState === 'UPGRADE' || gameState === 'PAUSED_SETTINGS') {
                // Still draw entities in background when paused/upgrading
                player.draw();
                enemies.forEach(e => e.draw());
            }

            requestAnimationFrame(gameLoop);
        }

        /* --- EVENT LISTENERS FOR BUTTONS --- */
        document.getElementById('btn-start').addEventListener('click', () => {
            sfx.init();
            document.getElementById('start-modal').classList.add('hidden');
            resetGame();
            gameState = 'PLAYING';
        });

        document.getElementById('btn-restart').addEventListener('click', () => {
            document.getElementById('gameover-modal').classList.add('hidden');
            resetGame();
            gameState = 'PLAYING';
        });

        // Settings Modal Listeners
        const settingsModal = document.getElementById('settings-modal');
        let previousState = 'START';

        document.getElementById('btn-settings').addEventListener('click', () => {
            if (gameState !== 'PAUSED_SETTINGS') {
                previousState = gameState;
                gameState = 'PAUSED_SETTINGS';
                settingsModal.classList.remove('hidden');
            }
        });

        document.getElementById('btn-close-settings').addEventListener('click', () => {
            settingsModal.classList.add('hidden');
            gameState = previousState;
        });

        // Mute / Unmute Controls
        const btnSoundOn = document.getElementById('btn-sound-on');
        const btnSoundOff = document.getElementById('btn-sound-off');

        btnSoundOn.addEventListener('click', () => {
            sfx.muted = false;
            btnSoundOn.classList.add('active');
            btnSoundOff.classList.remove('active');
        });

        btnSoundOff.addEventListener('click', () => {
            sfx.muted = true;
            btnSoundOff.classList.add('active');
            btnSoundOn.classList.remove('active');
        });

        // Language Buttons Listeners
        document.getElementById('btn-lang-uz').addEventListener('click', () => setLanguage('uz'));
        document.getElementById('btn-lang-en').addEventListener('click', () => setLanguage('en'));
        document.getElementById('btn-lang-ru').addEventListener('click', () => setLanguage('ru'));

        // Mobile Touch Listeners
        document.getElementById('btn-touch-attack').addEventListener('touchstart', (e) => { e.preventDefault(); player.attack(); });
        document.getElementById('btn-touch-dash').addEventListener('touchstart', (e) => { e.preventDefault(); triggerDash(); });
        document.getElementById('btn-touch-shadow').addEventListener('touchstart', (e) => { e.preventDefault(); triggerShadowSkill(); });

        // Initialize Default Language and Start Loop
        setLanguage('uz');
        requestAnimationFrame(gameLoop);
    </script>
</body>
</html>
