<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Генератор сообщений</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: var(--tg-theme-bg-color, #f5f5f5);
            color: var(--tg-theme-text-color, #000);
            min-height: 100vh;
            padding: 16px;
            padding-bottom: 80px;
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
            color: var(--tg-theme-hint-color, #666);
        }
        
        .stats-bar {
            background: var(--tg-theme-secondary-bg-color, #fff);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            display: flex;
            justify-content: space-around;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
        
        .stat-item {
            text-align: center;
        }
        
        .stat-value {
            font-size: 24px;
            font-weight: 700;
            color: var(--tg-theme-button-color, #2481cc);
        }
        
        .stat-label {
            font-size: 12px;
            color: var(--tg-theme-hint-color, #666);
            margin-top: 4px;
        }
        
        .message-card {
            background: var(--tg-theme-secondary-bg-color, #fff);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 16px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            position: relative;
        }
        
        .message-label {
            font-size: 12px;
            color: var(--tg-theme-hint-color, #666);
            text-transform: uppercase;
            letter-spacing: 0.5px;
            margin-bottom: 12px;
        }
        
        .message-text {
            font-size: 17px;
            line-height: 1.5;
            color: var(--tg-theme-text-color, #333);
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
            background: var(--tg-theme-button-color, #2481cc);
            color: var(--tg-theme-button-text-color, #fff);
        }
        
        .btn-primary:active {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .btn-success {
            background: #4CAF50;
            color: #fff;
        }
        
        .btn-secondary {
            background: var(--tg-theme-secondary-bg-color, #e5e5e5);
            color: var(--tg-theme-text-color, #333);
        }
        
        .loading {
            text-align: center;
            padding: 40px;
            color: var(--tg-theme-hint-color, #666);
        }
        
        .spinner {
            width: 40px;
            height: 40px;
            border: 3px solid var(--tg-theme-hint-color, #ddd);
            border-top-color: var(--tg-theme-button-color, #2481cc);
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
        
        .bottom-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: var(--tg-theme-bg-color, #f5f5f5);
            padding: 12px 16px;
            padding-bottom: calc(12px + env(safe-area-inset-bottom));
            border-top: 1px solid rgba(0,0,0,0.1);
            display: flex;
            gap: 12px;
        }
        
        .bottom-bar .btn {
            margin-bottom: 0;
            flex: 1;
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
        
        // Устанавливаем цвета темы
        document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#f5f5f5');
        document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#000');
        document.documentElement.style.setProperty('--tg-theme-button-color', tg.themeParams.button_color || '#2481cc');
        document.documentElement.style.setProperty('--tg-theme-button-text-color', tg.themeParams.button_text_color || '#fff');
        document.documentElement.style.setProperty('--tg-theme-secondary-bg-color', tg.themeParams.secondary_bg_color || '#fff');
        document.documentElement.style.setProperty('--tg-theme-hint-color', tg.themeParams.hint_color || '#666');
        
        // Данные пользователя
        let currentMessage = '';
        let stats = {
            todayTouches: 0,
            totalTouches: 0,
            totalMessages: 0
        };
        
        // Fallback шаблоны (если бот не отвечает)
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
        
        // Загрузка статистики из initData
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
        
        // Обновление отображения статистики
        function updateStatsDisplay() {
            document.getElementById('todayTouches').textContent = stats.todayTouches;
            document.getElementById('totalTouches').textContent = stats.totalTouches;
            document.getElementById('totalMessages').textContent = stats.totalMessages;
        }
        
        // Генерация сообщения
        async function generateMessage() {
            showScreen('loadingScreen');
            
            try {
                // Пытаемся получить сообщение от бота через sendData
                tg.sendData(JSON.stringify({
                    action: 'generate'
                }));
                
                // Ждем ответа (макс 5 секунд)
                await new Promise(resolve => setTimeout(resolve, 1500));
                
                // Если нет ответа, используем fallback
                if (!currentMessage) {
                    currentMessage = fallbackTemplates[Math.floor(Math.random() * fallbackTemplates.length)];
                }
                
                showMessage(currentMessage);
                
            } catch (e) {
                // Fallback
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
                // Fallback
                const textarea = document.createElement('textarea');
                textarea.value = currentMessage;
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
            
            // Через 1 секунду генерируем новое
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
        
        // Закрытие по кнопке назад
        tg.BackButton.onClick(function() {
            tg.close();
        });
        
        // Инициализация
        loadStats();
        
        // Если есть сообщение в параметре URL, показываем его
        const urlParams = new URLSearchParams(window.location.search);
        const msgFromUrl = urlParams.get('msg');
        if (msgFromUrl) {
            currentMessage = decodeURIComponent(msgFromUrl);
            showMessage(currentMessage);
        }
    </script>
</body>
</html>
