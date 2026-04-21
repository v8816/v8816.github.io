
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>TikTok Messenger</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #0a0a0a;
            min-height: 100vh;
            color: #fff;
            padding: 20px;
        }
        
        .container {
            max-width: 500px;
            margin: 0 auto;
        }
        
        h1 {
            text-align: center;
            font-size: 22px;
            margin-bottom: 3px;
            color: #ff0050;
        }
        
        .subtitle {
            text-align: center;
            color: #666;
            font-size: 13px;
            margin-bottom: 25px;
        }
        
        .stats {
            text-align: center;
            padding: 12px;
            background: #161616;
            border-radius: 12px;
            margin-bottom: 15px;
        }
        
        .stats-number {
            font-size: 32px;
            font-weight: bold;
            color: #ff0050;
        }
        
        .stats-label {
            font-size: 12px;
            color: #666;
        }
        
        .progress {
            text-align: center;
            padding: 10px;
            background: #161616;
            border-radius: 12px;
            margin-bottom: 15px;
            font-size: 13px;
            color: #888;
        }
        
        .progress span {
            color: #ff0050;
            font-weight: bold;
        }
        
        .message-box {
            background: #161616;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 15px;
            min-height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            font-size: 15px;
            line-height: 1.6;
            color: #fff;
            border: 1px solid #222;
        }
        
        .message-box.empty {
            color: #444;
        }
        
        .btn-main {
            width: 100%;
            padding: 20px;
            font-size: 17px;
            font-weight: 700;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            background: linear-gradient(135deg, #ff0050, #ff4080);
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .btn-main:hover, .btn-main:active {
            opacity: 0.9;
            transform: scale(0.99);
        }
        
        .btn-main:disabled {
            background: #333;
            color: #666;
            cursor: not-allowed;
        }
        
        .btn-reset {
            width: 100%;
            padding: 14px;
            font-size: 13px;
            border: 1px solid #333;
            border-radius: 12px;
            cursor: pointer;
            background: transparent;
            color: #666;
            margin-top: 10px;
        }
        
        .btn-reset:hover {
            border-color: #ff0050;
            color: #ff0050;
        }
        
        .emoji-row {
            display: flex;
            gap: 8px;
            margin-bottom: 15px;
            overflow-x: auto;
            padding: 5px 0;
        }
        
        .emoji-btn {
            background: #222;
            border: none;
            border-radius: 10px;
            padding: 12px 16px;
            font-size: 22px;
            cursor: pointer;
            flex-shrink: 0;
            transition: all 0.15s;
        }
        
        .emoji-btn:hover, .emoji-btn:active {
            background: #ff0050;
            transform: scale(1.1);
        }
        
        .copied-toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: #00d26a;
            color: #fff;
            padding: 14px 28px;
            border-radius: 50px;
            font-weight: 600;
            font-size: 14px;
            opacity: 0;
            transition: all 0.25s;
            z-index: 1000;
        }
        
        .copied-toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }

        .limit-toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: #ff0050;
            color: #fff;
            padding: 14px 28px;
            border-radius: 50px;
            font-weight: 600;
            font-size: 14px;
            opacity: 0;
            transition: all 0.25s;
            z-index: 1000;
        }
        
        .limit-toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }

        .history {
            margin-top: 25px;
        }
        
        .history h3 {
            font-size: 12px;
            color: #444;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .history-item {
            background: #161616;
            padding: 12px;
            border-radius: 10px;
            margin-bottom: 8px;
            font-size: 13px;
            color: #888;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .history-item .time {
            font-size: 11px;
            color: #555;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎯 TikTok Messenger</h1>
        <p class="subtitle">Готовые сообщения для рекрутинга</p>
        
        <div class="stats">
            <div class="stats-number" id="msgCount">0</div>
            <div class="stats-label">отправлено сегодня</div>
        </div>
        
        <div class="progress">Осталось: <span id="remainingCount">0</span>/<span id="totalCount">0</span> уникальных сообщений</div>
        
        <label style="display: block; font-size: 11px; color: #666; margin-bottom: 8px;">Добавить смайлик:</label>
        
        <div class="emoji-row">
            <button class="emoji-btn" onclick="addEmoji('😊')">😊</button>
            <button class="emoji-btn" onclick="addEmoji('👋')">👋</button>
            <button class="emoji-btn" onclick="addEmoji('✌️')">✌️</button>
            <button class="emoji-btn" onclick="addEmoji('👍')">👍</button>
            <button class="emoji-btn" onclick="addEmoji('🙌')">🙌</button>
            <button class="emoji-btn" onclick="addEmoji('✨')">✨</button>
            <button class="emoji-btn" onclick="addEmoji('🎉')">🎉</button>
            <button class="emoji-btn" onclick="addEmoji('💫')">💫</button>
        </div>
        
        <div class="message-box empty" id="messageBox">
            Нажмите кнопку ниже
        </div>
        
        <button class="btn-main" id="mainBtn" onclick="generateAndCopy()">
            📋 Сгенерировать и копировать
        </button>
        
        <button class="btn-reset" onclick="resetUsed()">↻ Сбросить использованные (если закончились)</button>
        
        <div class="history">
            <h3>📜 История</h3>
            <div id="historyList"></div>
        </div>
    </div>
    
    <div class="copied-toast" id="toast">✓ Скопировано!</div>
    <div class="limit-toast" id="limitToast">⚠️ Все сообщения использованы! Нажмите сброс</div>

    <script>
        // 70 уникальных сообщений (без "Привет", только "Здравствуйте" и "Добрый день")
        const messages = [
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Пишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пишите в ЛС — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Пишите в ЛС — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в ЛС — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пишите в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Пишите в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите, пожалуйста, в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пожалуйста, напишите в личные сообщения — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Пожалуйста, напишите в личные сообщения — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите нам в личные сообщения — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите нам в личные сообщения — у нас есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пишите в ЛС — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Пишите в ЛС — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Пишите в ЛС — у нас есть интересное предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Пишите в ЛС — у нас есть интересное предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть интересное предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть интересное предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Ждем вас в личных сообщениях — есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Ждем вас в личных сообщениях — есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — хотим сделать предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — хотим сделать предложение",
            
            "Здравствуйте! Ваш профиль привлёк внимание нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль привлёк внимание нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль заинтересовал нашу компанию. Напишите в личные сообщения — есть предложение",
            "Добрый день! Ваш профиль заинтересовал нашу компанию. Напишите в личные сообщения — есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть интересное предложение по сотрудничеству",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — есть интересное предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Будем рады сообщению в ЛС — есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Будем рады сообщению в ЛС — есть предложение",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть вам предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — у нас есть вам предложение",
            
            "Здравствуйте! Ваш профиль заинтересовал нашу компанию. Напишите в личные сообщения — у нас есть предложение для вас",
            "Добрый день! Ваш профиль заинтересовал нашу компанию. Напишите в личные сообщения — у нас есть предложение для вас",
            
            "Здравствуйте! Ваш профиль впечатлил нашу компанию. Напишите в личные сообщения — есть предложение по сотрудничеству",
            "Добрый день! Ваш профиль впечатлил нашу компанию. Напишите в личные сообщения — есть предложение по сотрудничеству",
            
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в лички — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в лички — у нас есть предложение"
        ];
        
        const USED_KEY = 'usedMessageIndices70';
        let currentMessage = '';
        
        function getUsedIndices() {
            const saved = localStorage.getItem(USED_KEY);
            return saved ? JSON.parse(saved) : [];
        }
        
        function markUsed(index) {
            const used = getUsedIndices();
            if (!used.includes(index)) {
                used.push(index);
                localStorage.setItem(USED_KEY, JSON.stringify(used));
            }
            updateProgress();
        }
        
        function resetUsed() {
            localStorage.removeItem(USED_KEY);
            currentMessage = '';
            document.getElementById('messageBox').textContent = 'Нажмите кнопку ниже';
            document.getElementById('messageBox').classList.add('empty');
            document.getElementById('messageBox').style.color = '';
            updateProgress();
            showToast('✓ Сброшено! Все 70 сообщений доступны');
        }
        
        function updateProgress() {
            const used = getUsedIndices().length;
            document.getElementById('remainingCount').textContent = messages.length - used;
            document.getElementById('totalCount').textContent = messages.length;
        }
        
        function getAvailableIndices() {
            const used = getUsedIndices();
            return messages.map((_, i) => i).filter(i => !used.includes(i));
        }
        
        async function generateAndCopy() {
            const available = getAvailableIndices();
            
            if (available.length === 0) {
                document.getElementById('messageBox').textContent = '⚠️ Все сообщения использованы! Нажмите сбросить ниже';
                document.getElementById('messageBox').classList.remove('empty');
                document.getElementById('messageBox').style.color = '#ff0050';
                showToastLimit();
                return;
            }
            
            document.getElementById('messageBox').style.color = '';
            
            const index = available[Math.floor(Math.random() * available.length)];
            const baseMessage = messages[index];
            
            const emojis = ['😊', '👋', '✌️', '👍', '🙌', '✨', '🎉', '💫', '🤗', '💖', '🌟', '😉'];
            const randomEmoji = emojis[Math.floor(Math.random() * emojis.length)];
            currentMessage = `${baseMessage} ${randomEmoji}`;
            
            document.getElementById('messageBox').textContent = currentMessage;
            document.getElementById('messageBox').classList.remove('empty');
            
            markUsed(index);
            
            try {
                await navigator.clipboard.writeText(currentMessage);
            } catch {
                const ta = document.createElement('textarea');
                ta.value = currentMessage;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
            }
            
            showToast('✓ Скопировано!');
            saveToHistory();
            incrementStats();
            
            setTimeout(() => {
                window.location.href = 'tiktok://';
            }, 500);
        }
        
        function addEmoji(emoji) {
            if (!currentMessage || currentMessage.startsWith('⚠️')) return;
            currentMessage += ` ${emoji}`;
            document.getElementById('messageBox').textContent = currentMessage;
            navigator.clipboard.writeText(currentMessage);
            showToast('✓ Скопировано с смайликом!');
        }
        
        function showToast(text) {
            const toast = document.getElementById('toast');
            toast.textContent = text;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 1800);
        }
        
        function showToastLimit() {
            const toast = document.getElementById('limitToast');
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }
        
        function saveToHistory() {
            let history = JSON.parse(localStorage.getItem('msgHistory') || '[]');
            history.unshift({
                text: currentMessage.substring(0, 45) + (currentMessage.length > 45 ? '...' : ''),
                time: new Date().toLocaleTimeString('ru-RU', {hour: '2-digit', minute:'2-digit'})
            });
            if (history.length > 10) history = history.slice(0, 10);
            localStorage.setItem('msgHistory', JSON.stringify(history));
            renderHistory();
        }
        
        function renderHistory() {
            const history = JSON.parse(localStorage.getItem('msgHistory') || '[]');
            const container = document.getElementById('historyList');
            container.innerHTML = history.length === 0 
                ? '<div class="history-item">История пуста</div>'
                : history.map(h => `
                    <div class="history-item">
                        <span>${h.text}</span>
                        <span class="time">${h.time}</span>
                    </div>
                `).join('');
        }
        
        function incrementStats() {
            const today = new Date().toDateString();
            const stats = JSON.parse(localStorage.getItem('msgStats') || '{}');
            if (stats.date !== today) {
                stats.date = today;
                stats.count = 0;
            }
            stats.count++;
            localStorage.setItem('msgStats', JSON.stringify(stats));
            updateStats();
        }
        
        function updateStats() {
            const today = new Date().toDateString();
            const stats = JSON.parse(localStorage.getItem('msgStats') || '{}');
            document.getElementById('msgCount').textContent = stats.date === today ? stats.count : 0;
        }
        
        updateStats();
        updateProgress();
        renderHistory();
    </script>
</body>
</html>
