[index.html](https://github.com/user-attachments/files/25325653/index.html)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <link rel="icon" href="https://cdn-icons-png.flaticon.com">
    <title>IT Empire Pro v3.2 Final</title>
    <link href="https://fonts.googleapis.com" rel="stylesheet">
    <style>
        :root {
            --bg: #0b0e14; --panel: #151921; --border: #2d333f;
            --accent: #00a3ff; --money: #00ff94; --code: #70ff00; --danger: #ff4b4b;
        }
        * { user-select: none; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        body {
            background: var(--bg); color: #e1e1e1; font-family: 'Inter', sans-serif;
            margin: 0; display: flex; height: 100vh; overflow: hidden;
        }
        #side {
            width: 360px; background: var(--panel); border-right: 1px solid var(--border);
            padding: 25px; display: flex; flex-direction: column; gap: 15px; overflow-y: auto;
        }
        .stat-box { border-left: 3px solid var(--accent); padding-left: 15px; margin-bottom: 10px; }
        .label { font-size: 10px; text-transform: uppercase; color: #888; letter-spacing: 1px; }
        .val { font-family: 'JetBrains Mono', monospace; font-size: 24px; font-weight: 700; }
        
        /* Кнопки помощников */
        .upgrade-card {
            background: #1c222d; border: 1px solid var(--border); padding: 12px; border-radius: 6px;
            cursor: pointer; transition: 0.1s; margin-bottom: 8px; outline: none;
        }
        .upgrade-card:hover:not(.disabled) {
            animation: blink-fast 0.1s infinite;
            border-color: var(--accent);
        }
        @keyframes blink-fast {
            0% { opacity: 1; } 50% { opacity: 0.8; background: #2d333f; } 100% { opacity: 1; }
        }
        .upgrade-card.disabled { opacity: 0.2; cursor: not-allowed; filter: grayscale(1); }
        
        #main { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative; padding: 40px; }
        
        /* Терминал */
        .terminal {
            width: 100%; max-width: 600px; height: 250px; background: #000; border: 1px solid var(--border);
            border-radius: 12px; padding: 25px; font-family: 'JetBrains Mono', monospace;
            cursor: pointer; box-shadow: 0 15px 40px rgba(0,0,0,0.6); margin-bottom: 30px;
        }
        #cursor { animation: blink 1s infinite; border-right: 10px solid var(--accent); }
        @keyframes blink { 50% { opacity: 0; } }

        /* Проекты */
        #project-area {
            width: 100%; max-width: 600px; background: #1c222d; border: 1px solid var(--accent);
            border-radius: 8px; padding: 20px; display: flex; justify-content: space-between; align-items: center;
        }
        .progress-bg { width: 100%; background: #000; height: 10px; border-radius: 5px; margin-top: 10px; overflow: hidden; }
        #progress-bar { width: 0%; height: 100%; background: var(--accent); transition: 0.3s; }

        .btn-ui {
            background: #2d333f; border: 1px solid var(--border); color: white; padding: 10px;
            border-radius: 4px; cursor: pointer; font-size: 11px; transition: 0.2s; width: 100%;
            margin-bottom: 5px; text-transform: uppercase;
        }
        #sell-container { margin-top: auto; text-align: center; }
        .sell-hint { font-size: 10px; color: #666; margin-top: 5px; font-style: italic; }

        #settings-panel {
            position: absolute; bottom: 120px; right: 25px; background: #1c222d;
            padding: 20px; border: 1px solid var(--accent); border-radius: 8px; display: none;
            box-shadow: 0 0 30px rgba(0,0,0,0.5); width: 220px; z-index: 100;
        }
    </style>
</head>
<body>

<div id="side">
    <div style="font-weight: 800; color: var(--accent); margin-bottom: 10px;">CORE_OS // V3.2_FINAL</div>
    
    <div class="stat-box">
        <div class="label" data-key="balance">Баланс</div>
        <div class="val" style="color: var(--money);">$<span id="cash">0</span></div>
    </div>
    
    <div class="stat-box">
        <div class="label" data-key="lines">Строки кода</div>
        <div class="val" id="code">0</div>
        <div class="label" style="margin-top:5px"><span data-key="production">Доход</span>: <span id="cps">0</span> loc/s</div>
    </div>

    <div id="upgrades-container"></div>

    <div id="sell-container">
        <button class="btn-ui" style="background: var(--money); color: #000; font-weight: bold; height: 50px; margin-bottom: 0;" onclick="sellCode()" data-key="sellBtn">
            ВЫПУСТИТЬ РЕЛИЗ
        </button>
        <div class="sell-hint" data-key="sellHint">(Минимум 10 строк кода)</div>
        <button class="btn-ui" style="margin-top: 15px;" onclick="toggleSettings()" data-key="settingsBtn">⚙️ Настройки</button>
    </div>
</div>

<div id="main">
    <div class="terminal" onclick="clickCode()">
        <div style="color: #444;">// SYSTEM_ONLINE</div>
        <div style="color: var(--code); margin-top: 20px; font-size: 18px;">><span id="term-text">...</span><span id="cursor"></span></div>
    </div>

    <div id="project-area">
        <div style="flex: 1; padding-right: 20px;">
            <div class="label" id="proj-name">Текущий проект: Ожидание...</div>
            <div class="progress-bg"><div id="progress-bar"></div></div>
            <div class="label" style="margin-top: 5px;"><span id="proj-progress">0</span>% / <span id="proj-reward">$0</span></div>
        </div>
        <button class="btn-ui" style="width: 120px; background: var(--accent); color: #000; font-weight: bold;" onclick="startProject()" id="proj-btn">НОВЫЙ ЗАКАЗ</button>
    </div>
</div>

<div id="settings-panel">
    <div class="label" style="margin-bottom:10px" data-key="langSelect">Язык / Language</div>
    <div style="display:flex; gap:5px; margin-bottom:15px;">
        <button class="btn-ui" onclick="setLang('ru')">RU</button>
        <button class="btn-ui" onclick="setLang('en')">EN</button>
    </div>
    <button class="btn-ui" style="color: var(--danger); border-color: var(--danger);" onclick="resetGame()" data-key="resetBtn">Сбросить всё</button>
</div>

<script>
    const i18n = {
        ru: {
            balance: "Баланс", lines: "Строки кода", production: "Доход",
            sellBtn: "ВЫПУСТИТЬ РЕЛИЗ", sellHint: "(Минимум 10 строк кода)", settingsBtn: "⚙️ Настройки",
            langSelect: "Смена языка", term: "Пишем код для проекта...",
            resetBtn: "Удалить империю", confirmReset: "Вы уверены? Весь прогресс будет потерян!",
            intern: "Джун-разработчик", senior: "Сеньор-помидор", ai: "GPT-Бот", server: "Нейро-сервер", ceo: "Техдиректор",
            proj_btn: "НОВЫЙ ЗАКАЗ", proj_wait: "Текущий проект: Ожидание..."
        },
        en: {
            balance: "Balance", lines: "Lines of Code", production: "Production",
            sellBtn: "PUSH TO PRODUCTION", sellHint: "(Min. 10 lines of code)", settingsBtn: "⚙️ Settings",
            langSelect: "Language Settings", term: "Coding the project...",
            resetBtn: "Reset Empire", confirmReset: "Are you sure? All progress will be lost!",
            intern: "Intern Developer", senior: "Senior Architect", ai: "AI Copilot", server: "Neural Server", ceo: "CTO",
            proj_btn: "NEW TASK", proj_wait: "Project: Idle..."
        }
    };

    const PROJECTS = [
        { name: {ru: "Лендинг для пиццерии", en: "Pizza Landing"}, target: 60, reward: 250 },
        { name: {ru: "Бот для Telegram", en: "Telegram Bot"}, target: 250, reward: 1200 },
        { name: {ru: "Мобильное приложение", en: "Mobile App"}, target: 1200, reward: 6000 },
        { name: {ru: "Криптобиржа", en: "Crypto Exchange"}, target: 6000, reward: 35000 }
    ];

    const INITIAL_STATE = {
        money: 0, code: 0, click: 1, auto: 0, lang: 'ru',
        upgs: {
            intern: { cost: 15, power: 1, count: 0 },
            senior: { cost: 120, power: 10, count: 0 },
            ai: { cost: 500, power: 45, count: 0 },
            server: { cost: 2000, power: 200, count: 0 },
            ceo: { cost: 10000, power: 1200, count: 0 }
        },
        activeProject: null,
        projectProgress: 0
    };

    let state = JSON.parse(localStorage.getItem('it_emp_final')) || JSON.parse(JSON.stringify(INITIAL_STATE));

    function initUpgrades() {
        const container = document.getElementById('upgrades-container');
        container.innerHTML = '';
        for (let id in state.upgs) {
            const card = document.createElement('div');
            card.id = `card-${id}`; card.className = 'upgrade-card';
            card.onclick = () => buy(id);
            card.innerHTML = `
                <div class="label" style="color:var(--money)">$<span id="cost-${id}">0</span></div>
                <div style="font-weight:700; font-size:13px"><span id="name-${id}"></span> (<span id="count-${id}">0</span>)</div>
                <div class="label">+${state.upgs[id].power} loc/s</div>`;
            container.appendChild(card);
        }
    }

    function startProject() {
        if (state.activeProject) return;
        state.activeProject = PROJECTS[Math.floor(Math.random() * PROJECTS.length)];
        state.projectProgress = 0;
        updateUI();
        save();
    }

    function clickCode() {
        state.code += state.click;
        if (state.activeProject) {
            state.projectProgress += state.click;
            checkProject();
        }
        updateUI();
    }

    function checkProject() {
        if (state.activeProject && state.projectProgress >= state.activeProject.target) {
            state.money += state.activeProject.reward;
            alert((state.lang === 'ru' ? "ПРОЕКТ ЗАВЕРШЕН! Прибыль: $" : "PROJECT COMPLETE! Reward: $") + state.activeProject.reward);
            state.activeProject = null;
            state.projectProgress = 0;
            save();
        }
    }

    function sellCode() {
        if (state.code < 10) return;
        state.money += Math.floor(state.code * 0.25);
        state.code = 0;
        save(); updateUI();
    }

    function buy(id) {
        let u = state.upgs[id];
        if (state.money >= u.cost) {
            state.money -= u.cost;
            u.count++;
            state.auto += u.power;
            u.cost = Math.round(u.cost * 1.35);
            save(); updateUI();
        }
    }

    function toggleSettings() {
        const p = document.getElementById('settings-panel');
        p.style.display = p.style.display === 'block' ? 'none' : 'block';
    }

    function setLang(l) { state.lang = l; updateUI(); save(); }
    
    function resetGame() {
        if (confirm(i18n[state.lang].confirmReset)) {
            localStorage.removeItem('it_emp_final');
            location.reload();
        }
    }

    function save() { localStorage.setItem('it_emp_final', JSON.stringify(state)); }

    function updateUI() {
        const t = i18n[state.lang];
        document.getElementById('cash').innerText = Math.floor(state.money).toLocaleString();
        document.getElementById('code').innerText = Math.floor(state.code).toLocaleString();
        document.getElementById('cps').innerText = state.auto.toLocaleString();
        document.getElementById('term-text').innerText = state.activeProject ? t.term : t.proj_wait;

        document.querySelectorAll('[data-key]').forEach(el => {
            el.innerText = t[el.getAttribute('data-key')];
        });

        // UI Проектов
        const pBtn = document.getElementById('proj-btn');
        if (state.activeProject) {
            pBtn.disabled = true; pBtn.style.opacity = 0.5;
            document.getElementById('proj-name').innerText = state.activeProject.name[state.lang];
            document.getElementById('proj-reward').innerText = "$" + state.activeProject.reward;
            let pct = Math.min(100, Math.floor((state.projectProgress / state.activeProject.target) * 100));
            document.getElementById('progress-bar').style.width = pct + "%";
            document.getElementById('proj-progress').innerText = pct;
        } else {
            pBtn.disabled = false; pBtn.style.opacity = 1;
            document.getElementById('proj-name').innerText = t.proj_wait;
            document.getElementById('progress-bar').style.width = "0%";
            document.getElementById('proj-progress').innerText = "0";
            document.getElementById('proj-reward').innerText = "$0";
        }

        // Обновление кнопок (без пересоздания)
        for (let id in state.upgs) {
            const u = state.upgs[id];
            const card = document.getElementById(`card-${id}`);
            document.getElementById(`cost-${id}`).innerText = u.cost.toLocaleString();
            document.getElementById(`count-${id}`).innerText = u.count;
            document.getElementById(`name-${id}`).innerText = t[id];
            if (state.money < u.cost) card.classList.add('disabled');
            else card.classList.remove('disabled');
        }
    }

    // Случайные события (раз в 3 минуты)
    setInterval(() => {
        if (Math.random() > 0.4) {
            const isBug = Math.random() > 0.6;
            if (isBug) {
                state.code = Math.floor(state.code * 0.7);
                alert("SYSTEM ERROR: Critical bug fixed. 30% of code lost.");
            } else {
                state.money += 2000;
                alert("INVESTMENT: You received a $2,000 grant!");
            }
            updateUI();
            save();
        }
    }, 180000);

    // Основной цикл
    setInterval(() => {
        if (state.auto > 0) {
            state.code += state.auto / 10;
            if (state.activeProject) {
                state.projectProgress += state.auto / 10;
                checkProject();
            }
        }
        updateUI();
    }, 100);

    initUpgrades();
    updateUI();
</script>
</body>
</html>
