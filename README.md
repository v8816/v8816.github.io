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
            /* Light theme defaults */
            --bg-color: #f5f5f5;
            --text-color: #000000;
            --secondary-bg: #ffffff;
            --button-color: #2481cc;
            --button-text: #ffffff;
            --hint-color: #666666;
            --border-color: rgba(0,0,0,0.1);
            --card-shadow: rgba(0,0,0,0.08);
        }
        
        /* Dark theme detection */
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
            padding-bottom: 80px;
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
            position: relative;
            border: 1px solid var(--border-color);
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
        
        .btn-primary {
            background: var(--tg-theme-button-color, var(--button-color));
            color: var(--tg-theme-button-text-color, var(--button-text));
        }
        
        .btn-primary:active {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .btn-success {
            background: #4CAF50;
            color: #fff;
        }
        
        .btn-success:active {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .btn-secondary {
            background: var(--tg-theme-secondary-bg-color, var(--secondary-bg));
            color: var(--tg-theme-text-color, var(--text-color));
            border: 1px solid var(--border-color);
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
    </style>
</head>
<body>
    <!-- Главный экран -->
    <div id="mainScreen">
        <div class="header">
            <h1>🤖 Генератор сообщений</h1>
            <p>AI создаёт уникальные продающие сообщения</p>
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
        
        <div class="message-card">
            <div class="message-label">💬 Сообщение для отправки</div>
            <div class="message-text" id="messageText">Нажмите "Сгенерировать" чтобы получить сообщение</div>
        </div>
        
        <button class="btn btn-primary" id="generateBtn" onclick="generateMessage()">
            ✨ Сгенерировать сообщение
        </button>
        
        <button class="btn btn-success hidden" id="copyBtn" onclick="copyAndNext()">
            📋 Копировать и получить новое
        </button>
    </div>
    
    <!-- Экран загрузки -->
    <div id="loadingScreen" class="hidden">
        <div class="loading">
            <div class="spinner"></div>
            <p>Генерирую сообщение через AI...</p>
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
        
        // Устанавливаем цвета темы из Telegram
        function applyTheme() {
            const theme = tg.themeParams;
            const root = document.documentElement;
            
            if (theme.bg_color) root.style.setProperty('--tg-theme-bg-color', theme.bg_color);
            if (theme.text_color) root.style.setProperty('--tg-theme-text-color', theme.text_color);
            if (theme.secondary_bg_color) root.style.setProperty('--tg-theme-secondary-bg-color', theme.secondary_bg_color);
            if (theme.button_color) root.style.setProperty('--tg-theme-button-color', theme.button_color);
            if (theme.button_text_color) root.style.setProperty('--tg-theme-button-text-color', theme.button_text_color);
            if (theme.hint_color) root.style.setProperty('--tg-theme-hint-color', theme.hint_color);
            
            // Устанавливаем цвет header Telegram
            tg.setHeaderColor(theme.bg_color || '#f5f5f5');
            tg.setBackgroundColor(theme.bg_color || '#f5f5f5');
        }
        
        // Применяем тему при загрузке
        applyTheme();
        
        // Слушаем изменения темы
        tg.onEvent('themeChanged', applyTheme);
        
        // Данные пользователя
        let currentMessage = '';
        let stats = {
            todayTouches: 0,
            totalTouches: 0,
            totalMessages: 0
        };
        
        // Fallback шаблоны
        const fallbackTemplates = [
            "Ваш профиль понравился нашей компании, приготовили для вас предложение - ждем в ЛС 😊",
            "Наша организация оценила ваш профиль, есть интересное предложение - напишите нам в лс 💎",
            "Ваш профиль понравился нам, готовы предложить сотрудничество - пишите в личные ✨",
            "Компания оценила ваш профиль, есть выгодное предложение - свяжитесь с нами в ЛС 🌟",
            "Ваш профиль в нашем ТОПе, есть предложение для вас - напишите в директ 🔥",
            "Организация оценила ваш профиль, приглашаем к сотрудничеству - ждем в лс 💫",
            "Нам понравился ваш профиль, есть интересное предложение - пишите в личные 🎯",
            "Ваш профиль привлек внимание, есть предложение - напишите нам в ЛС 💌",
        ];
        
        // Загрузка статистики
        function loadStats() {
            try {
                const initData = tg.initDataUnsafe;
                if (initData.start_param) {
                    const params = JSON.parse(decodeURIComponent(initData.start_param));
                    if (params.stats) {
                        stats = params.stats;
                        updateStatsDisplay();
                    }
                }
            } catch (e) {
                console.log('Нет данных статистики');
            }
        }
        
        // Обновление статистики
        function updateStatsDisplay() {
            document.getElementById('todayTouches').textContent = stats.todayTouches;
            document.getElementById('totalTouches').textContent = stats.totalTouches;
            document.getElementById('totalMessages').textContent = stats.totalMessages;
        }
        
        // Генерация сообщения
        async function generateMessage() {
            showScreen('loadingScreen');
            
            try {
                // Сообщаем боту что нужно сгенерировать
                tg.sendData(JSON.stringify({ action: 'generate' }));
                
                // Ждем немного
                await new Promise(resolve => setTimeout(resolve, 1500));
                
                // Если нет ответа, используем fallback
                if (!currentMessage) {
                    currentMessage = fallbackTemplates[Math.floor(Math.random() * fallbackTemplates.length)];
                }
                
                showMessage(currentMessage);
                
            } catch (e) {
                currentMessage = fallbackTemplates[Math.floor(Math.random() * fallbackTemplates.length)];
                showMessage(currentMessage);
            }
        }
        
        // Показать сообщение
        function showMessage(message) {
            document.getElementById('messageText').textContent = message;
            document.getElementById('generateBtn').classList.add('hidden');
            document.getElementById('copyBtn').classList.remove('hidden');
            showScreen('mainScreen');
        }
        
        // Копировать и получить новое
        async function copyAndNext() {
            if (!currentMessage) return;
            
            // Копируем в буфер
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
            
            // Вибрация
            tg.HapticFeedback.notificationOccurred('success');
            
            // Показываем успех
            showScreen('successScreen');
            
            // Уведомляем бота
            tg.sendData(JSON.stringify({
                action: 'copied',
                message: currentMessage
            }));
            
            // Через секунду генерируем новое
            setTimeout(() => {
                stats.todayTouches++;
                stats.totalTouches++;
                updateStatsDisplay();
                currentMessage = '';
                generateMessage();
            }, 1000);
        }
        
        // Показать экран
        function showScreen(screenId) {
            document.getElementById('mainScreen').classList.add('hidden');
            document.getElementById('loadingScreen').classList.add('hidden');
            document.getElementById('successScreen').classList.add('hidden');
            document.getElementById(screenId).classList.remove('hidden');
        }
        
        // Слушаем сообщения от бота
        tg.onEvent('message', function(event) {
            try {
                const data = JSON.parse(event.data);
                if (data.message) {
                    currentMessage = data.message;
                    if (data.stats) {
                        stats = data.stats;
                        updateStatsDisplay();
                    }
                    showMessage(currentMessage);
                }
            } catch (e) {
                console.log('Неизвестное сообщение от бота');
            }
        });
        
        // Кнопка назад
        tg.BackButton.onClick(function() {
            tg.close();
        });
        
        // Инициализация
        loadStats();
        
        // Если есть сообщение в URL
        const urlParams = new URLSearchParams(window.location.search);
        const msgFromUrl = urlParams.get('msg');
        if (msgFromUrl) {
            currentMessage = decodeURIComponent(msgFromUrl);
            showMessage(currentMessage);
        }
    </script>
</body>
</html>
