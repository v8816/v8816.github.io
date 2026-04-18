<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="color-scheme" content="light dark">
    <title>Генератор сообщений</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        :root {
            --bg-color: #f5f5f5;
            --text-color: #000000;
            --secondary-bg: #ffffff;
            --button-color: #2481cc;
            --button-text: #ffffff;
            --hint-color: #666666;
            --border-color: rgba(0,0,0,0.1);
            --card-shadow: rgba(0,0,0,0.08);
            --error-color: #ff4444;
            --success-color: #4CAF50;
        }
        
        @media (prefers-color-scheme: dark) {
            :root {
                --bg-color: #1a1a1a;
                --text-color: #ffffff;
                --secondary-bg: #2d2d2d;
                --button-color: #3390ec;
                --button-text: #ffffff;
                --hint-color: #888888;
                --border-color: rgba(255,255,255,0.1);
                --card-shadow: rgba(0,0,0,0.3);
            }
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: var(--tg-theme-bg-color, var(--bg-color));
            color: var(--tg-theme-text-color, var(--text-color));
            min-height: 100vh;
            padding: 16px;
            padding-bottom: 100px;
            transition: background 0.3s, color 0.3s;
        }
        
        .header {
            text-align: center;
            margin-bottom: 20px;
        }
        
        .header h1 {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 8px;
        }
        
        .header p {
            font-size: 14px;
            color: var(--tg-theme-hint-color, var(--hint-color));
        }
        
        .stats-bar {
            background: var(--tg-theme-secondary-bg-color, var(--secondary-bg));
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            display: flex;
            justify-content: space-around;
            box-shadow: 0 1px 3px var(--card-shadow);
        }
        
        .stat-item {
            text-align: center;
        }
        
        .stat-value {
            font-size: 24px;
            font-weight: 700;
            color: var(--tg-theme-button-color, var(--button-color));
        }
        
        .stat-label {
            font-size: 12px;
            color: var(--tg-theme-hint-color, var(--hint-color));
            margin-top: 4px;
        }
        
        .message-card {
            background: var(--tg-theme-secondary-bg-color, var(--secondary-bg));
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 16px;
            box-shadow: 0 2px 8px var(--card-shadow);
            border: 1px solid var(--border-color);
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        
        .message-label {
            font-size: 12px;
            color: var(--tg-theme-hint-color, var(--hint-color));
            text-transform: uppercase;
            letter-spacing: 0.5px;
            margin-bottom: 12px;
        }
        
        .message-text {
            font-size: 17px;
            line-height: 1.5;
            color: var(--tg-theme-text-color, var(--text-color));
            word-wrap: break-word;
        }
        
        .message-placeholder {
            color: var(--tg-theme-hint-color, var(--hint-color));
            font-style: italic;
        }
        
        .btn {
            width: 100%;
            padding: 16px 24px;
            font-size: 16px;
            font-weight: 600;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }
        
        .btn-primary {
            background: var(--tg-theme-button-color, var(--button-color));
            color: var(--tg-theme-button-text-color, var(--button-text));
        }
        
        .btn-primary:active:not(:disabled) {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .btn-success {
            background: var(--success-color);
            color: #fff;
        }
        
        .btn-success:active:not(:disabled) {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .btn-secondary {
            background: var(--tg-theme-secondary-bg-color, var(--secondary-bg));
            color: var(--tg-theme-text-color, var(--text-color));
            border: 1px solid var(--border-color);
        }
        
        .error-message {
            background: rgba(255, 68, 68, 0.1);
            border: 1px solid var(--error-color);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            color: var(--error-color);
            text-align: center;
        }
        
        .loading {
            text-align: center;
            padding: 40px;
            color: var(--tg-theme-hint-color, var(--hint-color));
        }
        
        .spinner {
            width: 40px;
            height: 40px;
            border: 3px solid var(--tg-theme-hint-color, #ddd);
            border-top-color: var(--tg-theme-button-color, var(--button-color));
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 16px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .success-animation {
            text-align: center;
            padding: 20px;
        }
        
        .success-icon {
            font-size: 64px;
            margin-bottom: 16px;
        }
        
        .hidden {
            display: none !important;
        }
        
        .settings-section {
            background: var(--tg-theme-secondary-bg-color, var(--secondary-bg));
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            border: 1px solid var(--border-color);
        }
        
        .settings-title {
            font-size: 14px;
            font-weight: 600;
            margin-bottom: 12px;
            color: var(--tg-theme-text-color, var(--text-color));
        }
        
        .input-field {
            width: 100%;
            padding: 12px 16px;
            font-size: 15px;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            background: var(--tg-theme-bg-color, var(--bg-color));
            color: var(--tg-theme-text-color, var(--text-color));
            margin-bottom: 12px;
            font-family: inherit;
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--tg-theme-button-color, var(--button-color));
        }
        
        .toggle-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
        }
        
        .toggle-label {
            font-size: 15px;
            color: var(--tg-theme-text-color, var(--text-color));
        }
        
        .toggle-switch {
            position: relative;
            width: 50px;
            height: 28px;
            background: var(--tg-theme-hint-color, #ccc);
            border-radius: 14px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .toggle-switch.active {
            background: var(--tg-theme-button-color, var(--button-color));
        }
        
        .toggle-switch::after {
            content: '';
            position: absolute;
            top: 2px;
            left: 2px;
            width: 24px;
            height: 24px;
            background: white;
            border-radius: 50%;
            transition: transform 0.3s;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        
        .toggle-switch.active::after {
            transform: translateX(22px);
        }
    </style>
</head>
<body>
    <!-- Главный экран -->
    <div id="mainScreen">
        <div class="header">
            <h1>🤖 Генератор сообщений</h1>
            <p>NVIDIA AI + Real-time генерация</p>
        </div>
        
        <div class="stats-bar">
            <div class="stat-item">
                <div class="stat-value" id="todayTouches">0</div>
                <div class="stat-label">Сегодня</div>
            </div>
            <div class="stat-item">
                <div class="stat-value" id="totalTouches">0</div>
                <div class="stat-label">Всего</div>
            </div>
            <div class="stat-item">
                <div class="stat-value" id="totalMessages">0</div>
                <div class="stat-label">Сообщений</div>
            </div>
        </div>
        
        <!-- API Key настройки -->
        <div class="settings-section" id="apiSettings">
            <div class="settings-title">🔑 Настройки NVIDIA API</div>
            <input type="password" class="input-field" id="apiKeyInput" placeholder="Введите NVIDIA API Key">
            <div class="toggle-row">
                <span class="toggle-label">Сохранить ключ</span>
                <div class="toggle-switch" id="saveKeyToggle" onclick="toggleSaveKey()"></div>
            </div>
        </div>
        
        <div id="errorContainer"></div>
        
        <div class="message-card">
            <div class="message-label">💬 Сообщение для отправки</div>
            <div class="message-text" id="messageText">
                <span class="message-placeholder">Нажмите "Сгенерировать" для создания сообщения через NVIDIA AI</span>
            </div>
        </div>
        
        <button class="btn btn-primary" id="generateBtn" onclick="generateMessage()">
            ✨ Сгенерировать (NVIDIA AI)
        </button>
        
        <button class="btn btn-success hidden" id="copyBtn" onclick="copyAndNext()" disabled>
            📋 Копировать и получить новое
        </button>
        
        <button class="btn btn-secondary" id="syncBtn" onclick="syncWithBot()">
            🔄 Синхронизировать с ботом
        </button>
    </div>
    
    <!-- Экран загрузки -->
    <div id="loadingScreen" class="hidden">
        <div class="loading">
            <div class="spinner"></div>
            <p id="loadingText">Подключаемся к NVIDIA API...</p>
            <p style="font-size: 12px; color: var(--hint-color); margin-top: 8px;">Модель: nemotron-3-super-120b-a12b</p>
        </div>
    </div>
    
    <!-- Экран успеха -->
    <div id="successScreen" class="hidden">
        <div class="success-animation">
            <div class="success-icon">✅</div>
            <h2>Скопировано!</h2>
            <p>+1 к касаниям</p>
        </div>
    </div>

    <script>
        // Инициализация Telegram Web App
        const tg = window.Telegram.WebApp;
        tg.ready();
        tg.expand();
        
        // Конфигурация
        const NVIDIA_API_URL = 'https://integrate.api.nvidia.com/v1';
        const MODEL = 'nvidia/nemotron-3-super-120b-a12b';
        
        // Состояние
        let currentMessage = '';
        let stats = {
            todayTouches: 0,
            totalTouches: 0,
            totalMessages: 0
        };
        let apiKey = localStorage.getItem('nvidia_api_key') || '';
        let saveKeyEnabled = localStorage.getItem('save_api_key') === 'true';
        
        // Применение темы
        function applyTheme() {
            const theme = tg.themeParams;
            const root = document.documentElement;
            
            if (theme.bg_color) root.style.setProperty('--tg-theme-bg-color', theme.bg_color);
            if (theme.text_color) root.style.setProperty('--tg-theme-text-color', theme.text_color);
            if (theme.secondary_bg_color) root.style.setProperty('--tg-theme-secondary-bg-color', theme.secondary_bg_color);
            if (theme.button_color) root.style.setProperty('--tg-theme-button-color', theme.button_color);
            if (theme.button_text_color) root.style.setProperty('--tg-theme-button-text-color', theme.button_text_color);
            if (theme.hint_color) root.style.setProperty('--tg-theme-hint-color', theme.hint_color);
            
            tg.setHeaderColor(theme.bg_color || '#f5f5f5');
            tg.setBackgroundColor(theme.bg_color || '#f5f5f5');
        }
        
        applyTheme();
        tg.onEvent('themeChanged', applyTheme);
        
        // Инициализация UI
        function initUI() {
            document.getElementById('apiKeyInput').value = apiKey;
            if (saveKeyEnabled && apiKey) {
                document.getElementById('saveKeyToggle').classList.add('active');
            }
            
            // Загружаем статистику из localStorage
            const savedStats = localStorage.getItem('stats');
            if (savedStats) {
                stats = JSON.parse(savedStats);
                updateStatsDisplay();
            }
            
            // Загружаем статистику от бота
            loadStatsFromBot();
        }
        
        // Переключение сохранения ключа
        function toggleSaveKey() {
            saveKeyEnabled = !saveKeyEnabled;
            const toggle = document.getElementById('saveKeyToggle');
            toggle.classList.toggle('active', saveKeyEnabled);
            localStorage.setItem('save_api_key', saveKeyEnabled);
            
            if (saveKeyEnabled) {
                localStorage.setItem('nvidia_api_key', document.getElementById('apiKeyInput').value);
            } else {
                localStorage.removeItem('nvidia_api_key');
            }
        }
        
        // Сохранение API ключа
        document.getElementById('apiKeyInput').addEventListener('input', (e) => {
            apiKey = e.target.value;
            if (saveKeyEnabled) {
                localStorage.setItem('nvidia_api_key', apiKey);
            }
        });
        
        // Загрузка статистики от бота
        function loadStatsFromBot() {
            try {
                const initData = tg.initDataUnsafe;
                if (initData.start_param) {
                    const params = JSON.parse(decodeURIComponent(initData.start_param));
                    if (params.stats) {
                        stats = { ...stats, ...params.stats };
                        saveStats();
                        updateStatsDisplay();
                    }
                }
            } catch (e) {
                console.log('Нет данных от бота');
            }
        }
        
        // Сохранение статистики
        function saveStats() {
            localStorage.setItem('stats', JSON.stringify(stats));
        }
        
        // Обновление отображения статистики
        function updateStatsDisplay() {
            document.getElementById('todayTouches').textContent = stats.todayTouches;
            document.getElementById('totalTouches').textContent = stats.totalTouches;
            document.getElementById('totalMessages').textContent = stats.totalMessages;
        }
        
        // Показать ошибку
        function showError(text) {
            const container = document.getElementById('errorContainer');
            container.innerHTML = `<div class="error-message">⚠️ ${text}</div>`;
            setTimeout(() => container.innerHTML = '', 5000);
        }
        
        // РЕАЛЬНАЯ генерация через NVIDIA API
        async function generateMessage() {
            if (!apiKey) {
                showError('Введите NVIDIA API Key в настройках!');
                document.getElementById('apiSettings').scrollIntoView({ behavior: 'smooth' });
                return;
            }
            
            showScreen('loadingScreen');
            document.getElementById('loadingText').textContent = 'Подключаемся к NVIDIA API...';
            
            const prompt = `Напиши ОЧЕНЬ КОРОТКОЕ сообщение на русском языке (максимум 20 слов).

ОБЯЗАТЕЛЬНАЯ структура:
1. НАША КОМПАНИЯ оценила профиль
2. Есть предложение  
3. Призыв написать В ЛИЧНЫЕ СООБЩЕНИЯ (или "в директ")
4. Один смайл в конце

ПРАВИЛЬНЫЕ примеры:
"Ваш профиль понравился нашей компании, приготовили для вас предложение - пишите в личные сообщения 😊"
"Наша компания оценила ваш профиль, есть интересное предложение - напишите в директ 💎"  
"Ваш профиль понравился нашей компании, есть выгодное предложение - пишите в личные сообщения ✨"

ОШИБКИ которые НЕЛЬЗЯ:
- "организация" - пиши только "компания"
- "привлек внимание" без указания чьё (нашей компании)
- "пишите в личные" без слова "сообщения"

Всегда пиши: "наша компания"!

Вариант:`;
            
            try {
                document.getElementById('loadingText').textContent = 'Генерация через nemotron-3-super-120b-a12b...';
                
                const response = await fetch(`${NVIDIA_API_URL}/chat/completions`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`
                    },
                    body: JSON.stringify({
                        model: MODEL,
                        messages: [{ role: 'user', content: prompt }],
                        temperature: 0.8,
                        top_p: 0.9,
                        max_tokens: 100,
                        stream: false
                    })
                });
                
                if (!response.ok) {
                    const error = await response.json();
                    throw new Error(error.error?.message || `HTTP ${response.status}`);
                }
                
                const data = await response.json();
                let message = data.choices[0].message.content.trim();
                
                // Очистка
                message = message.replace(/\*\*/g, '').replace(/\*/g, '').replace(/__/g, '');
                message = message.replace(/"/g, '').replace(/'/g, '').trim();
                message = message.replace(/\s+/g, ' ');
                
                // Проверка смайла
                const emojis = ['😊', '💎', '✨', '🌟', '🔥', '💫', '🚀', '⭐', '🎯', '💡', '💌', '😍', '🤩', '💖', '🎁', '🏆'];
                const hasEmoji = emojis.some(e => message.slice(-10).includes(e));
                if (!hasEmoji) {
                    message += ` ${emojis[Math.floor(Math.random() * emojis.length)]}`;
                }
                
                currentMessage = message;
                stats.totalMessages++;
                saveStats();
                updateStatsDisplay();
                
                // Отправляем статистику боту
                syncWithBot();
                
                showMessage(currentMessage);
                
            } catch (error) {
                console.error('Ошибка NVIDIA API:', error);
                showError(`Ошибка API: ${error.message}. Проверьте ключ.`);
                showScreen('mainScreen');
            }
        }
        
        // Показать сообщение
        function showMessage(message) {
            document.getElementById('messageText').textContent = message;
            document.getElementById('messageText').classList.remove('message-placeholder');
            document.getElementById('generateBtn').classList.add('hidden');
            document.getElementById('copyBtn').classList.remove('hidden');
            document.getElementById('copyBtn').disabled = false;
            showScreen('mainScreen');
        }
        
        // Копировать и следующее
        async function copyAndNext() {
            if (!currentMessage) return;
            
            document.getElementById('copyBtn').disabled = true;
            
            try {
                await navigator.clipboard.writeText(currentMessage);
            } catch (e) {
                const textarea = document.createElement('textarea');
                textarea.value = currentMessage;
                textarea.style.position = 'fixed';
                textarea.style.opacity = '0';
                document.body.appendChild(textarea);
                textarea.select();
                document.execCommand('copy');
                document.body.removeChild(textarea);
            }
            
            tg.HapticFeedback.notificationOccurred('success');
            
            // Обновляем статистику
            stats.todayTouches++;
            stats.totalTouches++;
            saveStats();
            updateStatsDisplay();
            
            // Отправляем боту
            tg.sendData(JSON.stringify({
                action: 'copied',
                message: currentMessage,
                stats: stats
            }));
            
            showScreen('successScreen');
            
            setTimeout(() => {
                currentMessage = '';
                document.getElementById('generateBtn').classList.remove('hidden');
                document.getElementById('copyBtn').classList.add('hidden');
                document.getElementById('messageText').innerHTML = '<span class="message-placeholder">Нажмите "Сгенерировать" для создания сообщения через NVIDIA AI</span>';
                showScreen('mainScreen');
            }, 1500);
        }
        
        // Синхронизация с ботом
        function syncWithBot() {
            tg.sendData(JSON.stringify({
                action: 'sync',
                stats: stats
            }));
        }
        
        // Показать экран
        function showScreen(screenId) {
            ['mainScreen', 'loadingScreen', 'successScreen'].forEach(id => {
                document.getElementById(id).classList.add('hidden');
            });
            document.getElementById(screenId).classList.remove('hidden');
        }
        
        // Кнопка назад
        tg.BackButton.onClick(() => tg.close());
        
        // Инициализация
        initUI();
        
        // Слушаем сообщения от бота
        tg.onEvent('message', (event) => {
            try {
                const data = JSON.parse(event.data);
                if (data.stats) {
                    stats = { ...stats, ...data.stats };
                    saveStats();
                    updateStatsDisplay();
                }
            } catch (e) {
                console.log('Сообщение от бота:', event.data);
            }
        });
    </script>
</body>
</html>
