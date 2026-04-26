
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

        .wiki-section {
            margin-top: 30px;
            background: #161616;
            border-radius: 12px;
            padding: 20px;
            border: 1px solid #222;
        }

        .wiki-section h3 {
            font-size: 16px;
            color: #ff0050;
            margin-bottom: 15px;
        }

        .wiki-tabs {
            display: flex;
            gap: 8px;
            margin-bottom: 15px;
            overflow-x: auto;
            padding-bottom: 5px;
        }

        .wiki-tab {
            background: #222;
            border: none;
            border-radius: 8px;
            padding: 10px 14px;
            font-size: 13px;
            font-weight: 600;
            color: #888;
            cursor: pointer;
            white-space: nowrap;
            flex-shrink: 0;
        }

        .wiki-tab.active {
            background: #ff0050;
            color: #fff;
        }

        .wiki-answer {
            display: none;
        }

        .wiki-answer.active {
            display: block;
        }

        .wiki-btn {
            width: 100%;
            padding: 14px;
            font-size: 14px;
            font-weight: 600;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            background: #222;
            color: #fff;
            margin-bottom: 10px;
            text-align: left;
            transition: all 0.15s;
        }

        .wiki-btn:hover, .wiki-btn:active {
            background: #ff0050;
            transform: scale(0.98);
        }

        @media (max-width: 480px) {
            body {
                padding: 12px;
            }

            h1 {
                font-size: 20px;
            }

            .stats-number {
                font-size: 28px;
            }

            .btn-main {
                padding: 18px;
                font-size: 16px;
            }
        }

        @media (max-width: 360px) {
            h1 {
                font-size: 18px;
            }

            .stats-number {
                font-size: 24px;
            }

            .btn-main {
                padding: 16px;
                font-size: 15px;
            }
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

        <button class="btn-main" id="mainBtn" onclick="generateAndCopy()">
            📋 Сгенерировать и копировать
        </button>

        <button class="btn-reset" onclick="resetUsed()">↻ Сбросить использованные</button>

        <div class="wiki-section">
            <h3>📚 Википедия — быстрые ответы</h3>

            <div class="wiki-tabs">
                <button class="wiki-tab active" onclick="switchTab('greeting')">👋 Приветствие</button>
                <button class="wiki-tab" onclick="switchTab('offer')">💼 Предложение</button>
                <button class="wiki-tab" onclick="switchTab('objections')">🛡️ Возражения</button>
                <button class="wiki-tab" onclick="switchTab('closing')">✅ Закрытие</button>
            </div>

            <div id="greeting" class="wiki-answer active">
                <button class="wiki-btn" onclick="copyWikiText(this)">Приветствие 1</button>
                <div class="wiki-text">Добрый день, Я (имя), HR-менеджер студии ONLINE MODELS. Посоветовавшись с коллегами, мы решили, что не можем не предложить вам поучаствовать в digital-проекте. Эта вакансия связана с рекламой, обзорами различных товаров, ведением лайф-эфиров.
Естественно, ваши усердия оплачиваются, обучение предоставляем бесплатно. Если вы согласны, буду рассказывать дальше ☺️</div>
            </div>

            <div id="offer" class="wiki-answer">
                <button class="wiki-btn" onclick="copyWikiText(this)">После «я согласна»</button>
                <div class="wiki-text">Отлично! Как я написал выше, работа заключается в рекламировании различных товаров, зачастую работа со съёмками, трансляциями. Оплата ежедневная, не фиксированная, так что "потолка" по заработку нет, большая часть зависит от ваших усилий, но наша команда 24/7 на связи + обучение. Может быть вы желаете уточнить некоторые моменты? ☺️</div>
            </div>

            <div id="objections" class="wiki-answer">
                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Что входит в обязанности?</button>
                <div class="wiki-text">Мы делаем акцент на проведении трансляций, для большего охвата аудитории, следовательно для большего заработка.
В ваши обязанности будет входить:
1. Соблюдение минимального графика
2. Пунктуальность и уважение к оператору.
Подробнее вам расскажет на собеседовании наша HR-девушка, уже в живом формате досконально проработает с вами все детали, покажет презентацию, и если необходимо, ответит на ваши вопросы. Если вас все устроит - назначим первый стажировочный день и закрепим за вами личного оператора ☺️</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Что именно я буду рекламировать?</button>
                <div class="wiki-text">Не смогу сказать вам точно, так как продукция зачастую разная, но чаще всего из категории "косметика".
Оплата складывается исходя из просмотров и часов, проведённых в эфире, не могу назвать вам конкретные цифры за день, но в среднем за месяц новички получают от $900-1000 (пишу в долларах, так как компания рассматривает кандидатов со всего мира, но средства будут поступать вам на счёт в валюте вашей страны).
Если у вас больше нет вопросов, предлагаю записаться на онлайн-собеседование ☺️</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Обучение бесплатное?</button>
                <div class="wiki-text">Да, включая как теоретическую часть, на основе собеседования, так и практическую, на основе стажировочных смен.
Запомните, если у вас кто-то будет требовать деньги - это не мы. Наша компания не берет денег ни за обучение, ни за работу - все абсолютно бесплатно.</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Как формируется оплата?</button>
                <div class="wiki-text">Оплата складывается исходя из просмотров и часов, проведённых в эфире.
Если у вас больше нет вопросов, предлагаю записаться на онлайн-собеседование, где наша HR-девушка уже в живом формате досконально проработает с вами все детали, покажет презентацию, и если вас все устроит - назначим первый стажировочный день ☺️</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Вы мошенник?</button>
                <div class="wiki-text">Понимаю вашу реакцию. Чтобы развеять сомнения, могу предоставить официальный сайт нашей компании. После ознакомления с ним с радостью отвечу на ваши вопросы.</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Я в этом ничего не понимаю</button>
                <div class="wiki-text">В этом нет проблемы. Опыт в данной сфере не обязателен. Мы предоставляем полный пакет обучения, включающий теоретическую и практическую части.
Обучение полностью бесплатное — от вас требуется только желание ☺️</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 Для чего нужен номер телефона?</button>
                <div class="wiki-text">Номер телефона необходим исключительно на случай, если в день собеседования интервьюер не сможет связаться с вами в Telegram. В крайнем случае он сможет вам позвонить. В других целях номер использоваться не будет.</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">👩 У вас есть девушки, которые уже работают?</button>
                <div class="wiki-text">Да, конечно. Однако мы не можем делиться контактами или профилями сотрудниц — ранее поступали жалобы на массовые сообщения и спам.
Зато согласованные примеры вам обязательно покажут на собеседовании 😉</div>
            </div>

            <div id="closing" class="wiki-answer">
                <button class="wiki-btn" onclick="copyWikiText(this)">Закрытие на собеседование</button>
                <div class="wiki-text">Отлично!
Если вас все устраивает, то предлагаю записаться на онлайн-собеседование, там наша HR-девушка уже в живом формате досконально проработает с вами все детали. После чего назначим вам первую стажировочную смену.
С вашего позволения пришлю вам анкету, благодаря которой студия оценивает возможности проведения действительно хороших съемок ☺️</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">📋 Анкета</button>
                <div class="wiki-text">— страна проживания:
— имя:
— возраст:
— номер телефона:
— Условия проживания: (с парнем/с родителями/одна)
— Модель устройства: телефон/пк + модель
— наушники: (есть/нет)
— График: (минимум 4 дня в неделю и 6 часов в день)
— телеграм (@ник)</div>

                <button class="wiki-btn" onclick="copyWikiText(this)">После анкеты</button>
                <div class="wiki-text">Как только заполните анкету, если по ней вопросов не будет, назначим собеседование, на удобную для вас дату и время ☺️</div>
            </div>
        </div>
    </div>

    <div class="copied-toast" id="toast">✓ Скопировано!</div>
    <div class="limit-toast" id="limitToast">⚠️ Все сообщения использованы! Нажмите сброс</div>

    <script>
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
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личку — у нас есть предложение",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личку — у нас есть предложение",
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — хотим предложить сотрудничество",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — хотим предложить сотрудничество",
            "Здравствуйте! Ваш профиль заинтересовал нашу команду. Напишите в личные сообщения — есть предложение по работе",
            "Добрый день! Ваш профиль заинтересовал нашу команду. Напишите в личные сообщения — есть предложение по работе",
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в личные сообщения — рассмотрим сотрудничество",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в личные сообщения — рассмотрим сотрудничество",
            "Здравствуйте! Ваш профиль привлёк наше внимание. Напишите в личные сообщения — есть предложение",
            "Добрый день! Ваш профиль привлёк наше внимание. Напишите в личные сообщения — есть предложение",
            "Здравствуйте! Ваш профиль понравился нашей компании. Напишите в ЛС — обсудим сотрудничество",
            "Добрый день! Ваш профиль понравился нашей компании. Напишите в ЛС — обсудим сотрудничество",
            "Здравствуйте! Ваш профиль впечатлил нас. Напишите в личные сообщения — есть предложение по работе",
            "Добрый день! Ваш профиль впечатлил нас. Напишите в личные сообщения — есть предложение по работе"
        ];

        const USED_KEY = 'usedMessageIndices76';
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
        }

        function resetUsed() {
            localStorage.removeItem(USED_KEY);
            showToast('✓ Сброшено! Все 76 сообщений доступны');
        }

        function getAvailableIndices() {
            const used = getUsedIndices();
            return messages.map((_, i) => i).filter(i => !used.includes(i));
        }

        async function generateAndCopy() {
            const available = getAvailableIndices();

            if (available.length === 0) {
                showToastLimit();
                return;
            }

            const index = available[Math.floor(Math.random() * available.length)];
            currentMessage = messages[index];

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
            incrementStats();

            setTimeout(() => {
                window.location.href = 'tiktok://';
            }, 500);
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

        function switchTab(tabId) {
            document.querySelectorAll('.wiki-tab').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.wiki-answer').forEach(a => a.classList.remove('active'));
            document.querySelector(`.wiki-tab[onclick="switchTab('${tabId}')"]`).classList.add('active');
            document.getElementById(tabId).classList.add('active');
        }

        function copyWikiText(btn) {
            const textDiv = btn.nextElementSibling;
            const text = textDiv.textContent.trim();

            navigator.clipboard.writeText(text).then(() => {
                showToast('✓ Скопировано!');
            }).catch(() => {
                const ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
                showToast('✓ Скопировано!');
            });
        }

        updateStats();
    </script>
</body>
</html>
