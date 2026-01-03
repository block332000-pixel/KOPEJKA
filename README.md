<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Full Messenger</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .messenger-container {
            width: 100%;
            max-width: 1400px;
            height: 90vh;
            background-color: white;
            border-radius: 20px;
            display: flex;
            overflow: hidden;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
        }

        .sidebar {
            width: 400px;
            background-color: #f8f9fa;
            border-right: 1px solid #e9ecef;
            display: flex;
            flex-direction: column;
        }

        .sidebar-tabs {
            display: flex;
            background-color: white;
            border-bottom: 1px solid #e9ecef;
        }

        .tab-btn {
            flex: 1;
            padding: 18px 0;
            background: none;
            border: none;
            cursor: pointer;
            font-size: 1rem;
            color: #666;
            transition: all 0.3s;
            position: relative;
        }

        .tab-btn.active {
            color: #1a2980;
            font-weight: 600;
        }

        .tab-btn.active::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 3px;
            background-color: #1a2980;
        }

        .tab-content {
            display: none;
            flex: 1;
            overflow-y: auto;
        }

        .tab-content.active {
            display: block;
        }

        .user-profile {
            padding: 25px;
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            color: white;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .user-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #1a2980;
            font-size: 1.8rem;
            border: 3px solid rgba(255, 255, 255, 0.3);
        }

        .user-info h3 {
            font-size: 1.3rem;
            margin-bottom: 5px;
        }

        .user-info p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .user-status {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-top: 5px;
        }

        .status-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #4cd137;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .list-header {
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #e9ecef;
        }

        .list-header h3 {
            font-size: 1.4rem;
            color: #333;
        }

        .search-box {
            padding: 15px 20px;
            border-bottom: 1px solid #e9ecef;
        }

        .search-input {
            width: 100%;
            padding: 12px 20px;
            border: 1px solid #ddd;
            border-radius: 25px;
            font-size: 0.95rem;
            outline: none;
            transition: border-color 0.3s;
            background-color: white;
        }

        .search-input:focus {
            border-color: #1a2980;
        }

        .contacts-list, .chats-list, .groups-list {
            flex: 1;
            overflow-y: auto;
        }

        .contact, .chat-item, .group-item {
            display: flex;
            align-items: center;
            padding: 15px 20px;
            cursor: pointer;
            transition: background-color 0.2s;
            border-bottom: 1px solid #f1f1f1;
        }

        .contact:hover, .chat-item:hover, .group-item:hover {
            background-color: #e9ecef;
        }

        .contact.active, .chat-item.active {
            background-color: #e0e7ff;
            border-left: 4px solid #1a2980;
        }

        .contact-avatar, .chat-avatar, .group-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-right: 15px;
            font-size: 1.3rem;
            position: relative;
        }

        .online-indicator {
            position: absolute;
            bottom: 2px;
            right: 2px;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background-color: #4cd137;
            border: 2px solid white;
        }

        .contact-info, .chat-info, .group-info {
            flex: 1;
            min-width: 0;
        }

        .contact-info h4, .chat-info h4, .group-info h4 {
            font-size: 1rem;
            color: #333;
            margin-bottom: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .contact-info p, .chat-info p, .group-info p {
            font-size: 0.85rem;
            color: #666;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .chat-time, .contact-time {
            font-size: 0.8rem;
            color: #888;
            margin-left: 10px;
        }

        .unread-count {
            background-color: #ff4757;
            color: white;
            font-size: 0.8rem;
            padding: 2px 8px;
            border-radius: 12px;
            margin-left: 10px;
        }

        .chat-area {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .chat-header {
            padding: 20px 25px;
            border-bottom: 1px solid #e9ecef;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: white;
        }

        .chat-header-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .chat-header-avatar {
            width: 55px;
            height: 55px;
            border-radius: 50%;
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.4rem;
        }

        .chat-header-info h3 {
            font-size: 1.3rem;
            color: #333;
            margin-bottom: 5px;
        }

        .chat-header-info p {
            font-size: 0.9rem;
            color: #666;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .chat-actions {
            display: flex;
            gap: 15px;
        }

        .action-btn {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            border: none;
            background-color: #f8f9fa;
            color: #666;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .action-btn:hover {
            background-color: #1a2980;
            color: white;
            transform: translateY(-2px);
        }

        .video-btn {
            background-color: #1a2980;
            color: white;
        }

        .chat-messages {
            flex: 1;
            padding: 25px;
            overflow-y: auto;
            background-color: #f8f9fa;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .message {
            max-width: 70%;
            padding: 15px 20px;
            border-radius: 20px;
            position: relative;
            word-wrap: break-word;
        }

        .message.sent {
            align-self: flex-end;
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            color: white;
            border-bottom-right-radius: 5px;
        }

        .message.received {
            align-self: flex-start;
            background-color: white;
            color: #333;
            border-bottom-left-radius: 5px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }

        .message-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }

        .message-sender {
            font-weight: 600;
            font-size: 0.9rem;
        }

        .message-time {
            font-size: 0.75rem;
            opacity: 0.8;
        }

        .message-text {
            font-size: 1rem;
            line-height: 1.5;
        }

        .voice-message {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background-color: rgba(255, 255, 255, 0.2);
            border-radius: 15px;
            margin-top: 10px;
        }

        .voice-play-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: white;
            color: #1a2980;
            border: none;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .voice-play-btn:hover {
            transform: scale(1.1);
        }

        .voice-waveform {
            flex: 1;
            height: 40px;
            background: linear-gradient(90deg, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0.4) 100%);
            border-radius: 20px;
            position: relative;
            overflow: hidden;
        }

        .voice-waveform::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            width: 60%;
            background: linear-gradient(90deg, rgba(255,255,255,0.9) 0%, rgba(255,255,255,0.6) 100%);
            animation: wave 2s infinite linear;
        }

        @keyframes wave {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .voice-duration {
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.9);
        }

        .message-input-area {
            padding: 20px 25px;
            border-top: 1px solid #e9ecef;
            display: flex;
            gap: 15px;
            align-items: center;
            background-color: white;
        }

        .input-actions {
            display: flex;
            gap: 10px;
        }

        .input-btn {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background-color: #f8f9fa;
            color: #666;
            border: none;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .input-btn:hover {
            background-color: #1a2980;
            color: white;
        }

        .record-btn.recording {
            background-color: #ff4757;
            color: white;
            animation: pulse-red 1s infinite;
        }

        @keyframes pulse-red {
            0% { box-shadow: 0 0 0 0 rgba(255, 71, 87, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(255, 71, 87, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 71, 87, 0); }
        }

        .message-input-container {
            flex: 1;
            position: relative;
        }

        .message-input {
            width: 100%;
            padding: 16px 25px;
            border: 1px solid #ddd;
            border-radius: 30px;
            font-size: 1rem;
            outline: none;
            transition: border-color 0.3s;
            background-color: #f8f9fa;
        }

        .message-input:focus {
            border-color: #1a2980;
            background-color: white;
        }

        .send-btn {
            width: 55px;
            height: 55px;
            border-radius: 50%;
            background: linear-gradient(135deg, #1a2980 0%, #26d0ce 100%);
            color: white;
            border: none;
            cursor: pointer;
            font-size: 1.3rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(26, 41, 128, 0.3);
        }

        .send-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(26, 41, 128, 0.4);
        }

        .video-call-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.95);
            z-index: 1000;
            flex-direction: column;
            padding: 20px;
        }

        .video-call-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: white;
            padding: 20px;
            background-color: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            margin-bottom: 20px;
        }

        .call-info h3 {
            font-size: 1.5rem;
            margin-bottom: 5px;
        }

        .call-info p {
            font-size: 1rem;
            color: #ccc;
        }

        .call-timer {
            font-size: 1.8rem;
            font-weight: 600;
            color: #4cd137;
        }

        .video-grid {
            flex: 1;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .video-item {
            background-color: #222;
            border-radius: 15px;
            overflow: hidden;
            position: relative;
        }

        .video-element {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .video-label {
            position: absolute;
            bottom: 15px;
            left: 15px;
            color: white;
            background-color: rgba(0, 0, 0, 0.6);
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
        }

        .call-controls {
            display: flex;
            justify-content: center;
            gap: 30px;
            padding: 20px;
        }

        .call-btn {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            border: none;
            cursor: pointer;
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .call-btn:hover {
            transform: scale(1.1);
        }

        .end-call-btn {
            background-color: #ff4757;
            color: white;
        }

        .mute-btn, .video-toggle-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
        }

        .mute-btn.active, .video-toggle-btn.active {
            background-color: #4cd137;
        }

        @media (max-width: 1200px) {
            .messenger-container {
                height: 95vh;
            }
            
            .sidebar {
                width: 350px;
            }
        }

        @media (max-width: 992px) {
            .sidebar {
                width: 300px;
            }
            
            .chat-header {
                padding: 15px 20px;
            }
        }

        .empty-chat {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: #888;
            text-align: center;
            padding: 40px;
            background-color: #f8f9fa;
        }

        .empty-chat i {
            font-size: 5rem;
            margin-bottom: 20px;
            color: #ddd;
        }

        .empty-chat h3 {
            font-size: 1.8rem;
            margin-bottom: 10px;
        }

        .empty-chat p {
            font-size: 1rem;
            max-width: 400px;
            line-height: 1.5;
        }
    </style>
</head>
<body>
    <div class="messenger-container">
        <div class="sidebar">
            <div class="user-profile">
                <div class="user-avatar" id="current-user-avatar">Я</div>
                <div class="user-info">
                    <h3 id="current-user-name">Алексей Петров</h3>
                    <div class="user-status">
                        <div class="status-dot"></div>
                        <p>В сети</p>
                    </div>
                </div>
            </div>
            
            <div class="sidebar-tabs">
                <button class="tab-btn active" data-tab="chats">
                    <i class="fas fa-comments"></i> Чаты
                </button>
                <button class="tab-btn" data-tab="contacts">
                    <i class="fas fa-users"></i> Контакты
                </button>
                <button class="tab-btn" data-tab="groups">
                    <i class="fas fa-user-friends"></i> Группы
                </button>
            </div>
            
            <div class="tab-content active" id="chats-tab">
                <div class="search-box">
                    <input type="text" class="search-input" placeholder="Поиск чатов...">
                </div>
                <div class="chats-list" id="chats-list"></div>
            </div>
            
            <div class="tab-content" id="contacts-tab">
                <div class="list-header">
                    <h3>Все контакты</h3>
                    <button class="action-btn" id="add-contact-btn">
                        <i class="fas fa-user-plus"></i>
                    </button>
                </div>
                <div class="search-box">
                    <input type="text" class="search-input" placeholder="Поиск контактов...">
                </div>
                <div class="contacts-list" id="contacts-list"></div>
            </div>
            
            <div class="tab-content" id="groups-tab">
                <div class="list-header">
                    <h3>Группы</h3>
                    <button class="action-btn" id="create-group-btn">
                        <i class="fas fa-plus"></i>
                    </button>
                </div>
                <div class="search-box">
                    <input type="text" class="search-input" placeholder="Поиск групп...">
                </div>
                <div class="groups-list" id="groups-list"></div>
            </div>
        </div>
        
        <div class="chat-area">
            <div class="empty-chat" id="empty-chat">
                <i class="far fa-comments"></i>
                <h3>Выберите чат</h3>
                <p>Выберите чат из списка слева, чтобы начать общение</p>
                <p style="margin-top: 20px; color: #1a2980;">
                    <i class="fas fa-video"></i> Доступны видеозвонки<br>
                    <i class="fas fa-microphone"></i> Поддержка голосовых сообщений
                </p>
            </div>
            
            <div class="active-chat" id="active-chat" style="display: none;">
                <div class="chat-header">
                    <div class="chat-header-left">
                        <div class="chat-header-avatar" id="chat-header-avatar">ИИ</div>
                        <div class="chat-header-info">
                            <h3 id="chat-header-name">Иван Иванов</h3>
                            <p id="chat-header-status">
                                <span class="status-dot" style="background-color: #4cd137;"></span>
                                <span id="status-text">В сети</span>
                            </p>
                        </div>
                    </div>
                    
                    <div class="chat-actions">
                        <button class="action-btn" id="voice-call-btn" title="Голосовой вызов">
                            <i class="fas fa-phone"></i>
                        </button>
                        <button class="action-btn video-btn" id="video-call-btn" title="Видеозвонок">
                            <i class="fas fa-video"></i>
                        </button>
                        <button class="action-btn" id="chat-info-btn" title="Информация о чате">
                            <i class="fas fa-info-circle"></i>
                        </button>
                    </div>
                </div>
                
                <div class="chat-messages" id="chat-messages"></div>
                
                <div class="message-input-area">
                    <div class="input-actions">
                        <button class="input-btn" id="attach-btn" title="Прикрепить файл">
                            <i class="fas fa-paperclip"></i>
                        </button>
                        <button class="input-btn" id="emoji-btn" title="Эмодзи">
                            <i class="far fa-smile"></i>
                        </button>
                        <button class="input-btn record-btn" id="record-btn" title="Записать голосовое сообщение">
                            <i class="fas fa-microphone"></i>
                        </button>
                    </div>
                    
                    <div class="message-input-container">
                        <input type="text" class="message-input" id="message-input" placeholder="Введите сообщение...">
                    </div>
                    
                    <button class="send-btn" id="send-btn">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        </div>
        
        <div class="video-call-container" id="video-call-container">
            <div class="video-call-header">
                <div class="call-info">
                    <h3 id="call-with-name">Видеозвонок с Иваном Ивановым</h3>
                    <p id="call-status">Идет разговор...</p>
                </div>
                <div class="call-timer" id="call-timer">00:00</div>
            </div>
            
            <div class="video-grid" id="video-grid">
                <div class="video-item">
                    <video class="video-element" id="local-video" autoplay muted></video>
                    <div class="video-label">Вы (Я)</div>
                </div>
                <div class="video-item">
                    <video class="video-element" id="remote-video" autoplay></video>
                    <div class="video-label" id="remote-user-label">Иван Иванов</div>
                </div>
            </div>
            
            <div class="call-controls">
                <button class="call-btn mute-btn" id="mute-btn" title="Выключить микрофон">
                    <i class="fas fa-microphone"></i>
                </button>
                <button class="call-btn video-toggle-btn" id="video-toggle-btn" title="Выключить камеру">
                    <i class="fas fa-video"></i>
                </button>
                <button class="call-btn end-call-btn" id="end-call-btn" title="Завершить звонок">
                    <i class="fas fa-phone-slash"></i>
                </button>
            </div>
        </div>
    </div>

    <script>
        const currentUser = {
            id: 1,
            name: "Алексей Петров",
            avatar: "АП",
            online: true
        };

        const allUsers = [
            { id: 2, name: "Иван Иванов", avatar: "ИИ", online: true },
            { id: 3, name: "Мария Смирнова", avatar: "МС", online: true },
            { id: 4, name: "Дмитрий Козлов", avatar: "ДК", online: false },
            { id: 5, name: "Ольга Новикова", avatar: "ОН", online: true },
            { id: 6, name: "Сергей Васильев", avatar: "СВ", online: false },
            { id: 7, name: "Анна Кузнецова", avatar: "АК", online: true },
            { id: 8, name: "Павел Орлов", avatar: "ПО", online: false },
            { id: 9, name: "Екатерина Волкова", avatar: "ЕВ", online: true }
        ];

        const groups = [
            {
                id: 1,
                name: "Рабочая группа",
                avatar: "РГ",
                members: [1, 2, 3, 5],
                lastMessage: "Совещание в 15:00",
                time: "10:30",
                unread: 3
            },
            {
                id: 2,
                name: "Друзья",
                avatar: "ДР",
                members: [1, 4, 6, 7],
                lastMessage: "Встречаемся в субботу",
                time: "Вчера",
                unread: 0
            }
        ];

        const chats = [
            {
                id: 1,
                type: 'private',
                userId: 2,
                lastMessage: "Привет! Как дела?",
                time: "10:30",
                unread: 2,
                messages: [
                    { id: 1, text: "Привет! Как дела?", time: "10:30", sender: 2, type: 'text' },
                    { id: 2, text: "Привет! Всё отлично, спасибо!", time: "10:32", sender: 1, type: 'text' },
                    { id: 3, text: "Что нового?", time: "10:33", sender: 2, type: 'text' },
                    { id: 4, text: "Готовлю новый проект. Как твои успехи?", time: "10:35", sender: 1, type: 'text' },
                    { id: 5, text: "Тоже работаю над интересной задачей. Надо встретиться на следующей неделе!", time: "10:40", sender: 2, type: 'text' },
                    { id: 6, text: "", time: "11:15", sender: 2, type: 'voice', duration: "1:25" }
                ]
            },
            {
                id: 2,
                type: 'private',
                userId: 3,
                lastMessage: "Отправила тебе файл с отчетом",
                time: "Вчера",
                unread: 0,
                messages: [
                    { id: 1, text: "Привет! Отправила тебе файл с отчетом", time: "15:45", sender: 3, type: 'text' },
                    { id: 2, text: "Спасибо, получила. Посмотрю сегодня вечером.", time: "16:20", sender: 1, type: 'text' },
                    { id: 3, text: "Хорошо, жду твоих комментариев!", time: "16:22", sender: 3, type: 'text' }
                ]
            },
            {
                id: 3,
                type: 'private',
                userId: 4,
                lastMessage: "Завтра в 18:00 на совещании",
                time: "Пн",
                unread: 0,
                messages: [
                    { id: 1, text: "Напоминаю: завтра в 18:00 на совещании", time: "Пн 14:30", sender: 4, type: 'text' },
                    { id: 2, text: "Спасибо, не забуду. Буду с презентацией.", time: "Пн 15:10", sender: 1, type: 'text' },
                    { id: 3, text: "Отлично, жду!", time: "Пн 15:12", sender: 4, type: 'text' }
                ]
            },
            {
                id: 4,
                type: 'group',
                groupId: 1,
                lastMessage: "Совещание в 15:00",
                time: "10:30",
                unread: 3,
                messages: [
                    { id: 1, text: "Напоминаю: совещание в 15:00", time: "09:15", sender: 2, type: 'text' },
                    { id: 2, text: "Я буду", time: "09:20", sender: 3, type: 'text' },
                    { id: 3, text: "Я тоже", time: "09:25", sender: 1, type: 'text' },
                    { id: 4, text: "", time: "10:00", sender: 5, type: 'voice', duration: "0:45" }
                ]
            }
        ];

        let activeChat = null;
        let isRecording = false;
        let callTimer = null;
        let callSeconds = 0;

        document.addEventListener('DOMContentLoaded', function() {
            initApp();
            setupEventListeners();
        });

        function initApp() {
            document.getElementById('current-user-avatar').textContent = currentUser.avatar;
            document.getElementById('current-user-name').textContent = currentUser.name;
            
            renderChats();
            renderContacts();
            renderGroups();
            setupTabs();
        }

        function setupEventListeners() {
            document.getElementById('send-btn').addEventListener('click', sendMessage);
            document.getElementById('message-input').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') sendMessage();
            });
            
            document.getElementById('video-call-btn').addEventListener('click', startVideoCall);
            document.getElementById('voice-call-btn').addEventListener('click', startVoiceCall);
            document.getElementById('end-call-btn').addEventListener('click', endCall);
            
            const recordBtn = document.getElementById('record-btn');
            recordBtn.addEventListener('click', toggleRecording);
            
            document.getElementById('mute-btn').addEventListener('click', toggleMute);
            document.getElementById('video-toggle-btn').addEventListener('click', toggleVideo);
            document.getElementById('add-contact-btn').addEventListener('click', addContact);
            document.getElementById('create-group-btn').addEventListener('click', createGroup);
        }

        function setupTabs() {
            const tabBtns = document.querySelectorAll('.tab-btn');
            const tabContents = document.querySelectorAll('.tab-content');
            
            tabBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const tabId = this.getAttribute('data-tab');
                    
                    tabBtns.forEach(b => b.classList.remove('active'));
                    tabContents.forEach(c => c.classList.remove('active'));
                    
                    this.classList.add('active');
                    document.getElementById(`${tabId}-tab`).classList.add('active');
                });
            });
        }

        function renderChats() {
            const chatsList = document.getElementById('chats-list');
            chatsList.innerHTML = '';
            
            chats.forEach(chat => {
                let chatInfo;
                let avatar;
                let name;
                
                if (chat.type === 'private') {
                    const user = allUsers.find(u => u.id === chat.userId);
                    chatInfo = user;
                    avatar = user.avatar;
                    name = user.name;
                } else {
                    const group = groups.find(g => g.id === chat.groupId);
                    chatInfo = group;
                    avatar = group.avatar;
                    name = group.name;
                }
                
                const chatElement = document.createElement('div');
                chatElement.className = 'chat-item';
                chatElement.dataset.id = chat.id;
                
                chatElement.innerHTML = `
                    <div class="chat-avatar">
                        ${avatar}
                        ${chatInfo.online ? '<div class="online-indicator"></div>' : ''}
                    </div>
                    <div class="chat-info">
                        <h4>${name}</h4>
                        <p>${chat.lastMessage}</p>
                    </div>
                    <div class="chat-time">${chat.time}</div>
                    ${chat.unread > 0 ? `<div class="unread-count">${chat.unread}</div>` : ''}
                `;
                
                chatElement.addEventListener('click', () => openChat(chat.id));
                chatsList.appendChild(chatElement);
            });
        }

        function renderContacts() {
            const contactsList = document.getElementById('contacts-list');
            contactsList.innerHTML = '';
            
            allUsers.forEach(user => {
                if (user.id === currentUser.id) return;
                
                const contactElement = document.createElement('div');
                contactElement.className = 'contact';
                contactElement.dataset.id = user.id;
                
                contactElement.innerHTML = `
                    <div class="contact-avatar">
                        ${user.avatar}
                        ${user.online ? '<div class="online-indicator"></div>' : ''}
                    </div>
                    <div class="contact-info">
                        <h4>${user.name}</h4>
                        <p>${user.online ? 'В сети' : 'Был(а) недавно'}</p>
                    </div>
                    <button class="action-btn start-chat-btn" data-id="${user.id}" title="Начать чат">
                        <i class="fas fa-comment"></i>
                    </button>
                `;
                
                contactsList.appendChild(contactElement);
                
                contactElement.querySelector('.start-chat-btn').addEventListener('click', function(e) {
                    e.stopPropagation();
                    startNewChat(parseInt(this.getAttribute('data-id')));
                });
            });
        }

        function renderGroups() {
            const groupsList = document.getElementById('groups-list');
            groupsList.innerHTML = '';
            
            groups.forEach(group => {
                const groupElement = document.createElement('div');
                groupElement.className = 'group-item';
                groupElement.dataset.id = group.id;
                
                groupElement.innerHTML = `
                    <div class="group-avatar">
                        ${group.avatar}
                    </div>
                    <div class="group-info">
                        <h4>${group.name}</h4>
                        <p>Участников: ${group.members.length}</p>
                    </div>
                    <div class="chat-time">${group.time}</div>
                    ${group.unread > 0 ? `<div class="unread-count">${group.unread}</div>` : ''}
                `;
                
                groupElement.addEventListener('click', () => openGroupChat(group.id));
                groupsList.appendChild(groupElement);
            });
        }

        function openChat(chatId) {
            activeChat = chats.find(c => c.id === chatId);
            if (!activeChat) return;
            
            document.querySelectorAll('.chat-item').forEach(item => {
                item.classList.remove('active');
                if (parseInt(item.dataset.id) === chatId) {
                    item.classList.add('active');
                }
            });
            
            document.getElementById('empty-chat').style.display = 'none';
            document.getElementById('active-chat').style.display = 'flex';
            
            if (activeChat.type === 'private') {
                const user = allUsers.find(u => u.id === activeChat.userId);
                if (user) {
                    document.getElementById('chat-header-avatar').textContent = user.avatar;
                    document.getElementById('chat-header-name').textContent = user.name;
                    document.getElementById('status-text').textContent = user.online ? 'В сети' : 'Был(а) недавно';
                    
                    const statusDot = document.querySelector('#chat-header-status .status-dot');
                    statusDot.style.backgroundColor = user.online ? '#4cd137' : '#888';
                }
            } else {
                const group = groups.find(g => g.id === activeChat.groupId);
                if (group) {
                    document.getElementById('chat-header-avatar').textContent = group.avatar;
                    document.getElementById('chat-header-name').textContent = group.name;
                    document.getElementById('status-text').textContent = `Участников: ${group.members.length}`;
                    document.querySelector('#chat-header-status .status-dot').style.display = 'none';
                }
            }
            
            renderMessages(activeChat.messages);
            document.getElementById('message-input').focus();
            
            if (activeChat.unread > 0) {
                activeChat.unread = 0;
                renderChats();
            }
        }

        function openGroupChat(groupId) {
            const groupChat = chats.find(c => c.type === 'group' && c.groupId === groupId);
            if (groupChat) {
                openChat(groupChat.id);
            } else {
                alert('Чат с этой группой еще не создан');
            }
        }

        function renderMessages(messages) {
            const messagesContainer = document.getElementById('chat-messages');
            messagesContainer.innerHTML = '';
            
            messages.forEach(message => {
                const messageElement = document.createElement('div');
                const isSent = message.sender === currentUser.id;
                messageElement.className = `message ${isSent ? 'sent' : 'received'}`;
                
                let senderName = '';
                if (!isSent && activeChat.type === 'group') {
                    const user = allUsers.find(u => u.id === message.sender);
                    senderName = user ? user.name : 'Неизвестный';
                }
                
                let messageContent = '';
                if (message.type === 'text') {
                    messageContent = `<div class="message-text">${message.text}</div>`;
                } else if (message.type === 'voice') {
                    messageContent = `
                        <div class="voice-message">
                            <button class="voice-play-btn">
                                <i class="fas fa-play"></i>
                            </button>
                            <div class="voice-waveform"></div>
                            <div class="voice-duration">${message.duration}</div>
                        </div>
                    `;
                }
                
                messageElement.innerHTML = `
                    ${senderName ? `<div class="message-sender">${senderName}</div>` : ''}
                    <div class="message-header">
                        <div></div>
                        <div class="message-time">${message.time}</div>
                    </div>
                    ${messageContent}
                `;
                
                messagesContainer.appendChild(messageElement);
            });
            
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        function sendMessage() {
            const input = document.getElementById('message-input');
            const messageText = input.value.trim();
            
            if (!messageText || !activeChat) return;
            
            const newMessage = {
                id: Date.now(),
                text: messageText,
                time: getCurrentTime(),
                sender: currentUser.id,
                type: 'text'
            };
            
            activeChat.messages.push(newMessage);
            activeChat.lastMessage = messageText;
            activeChat.time = 'Только что';
            
            renderMessages(activeChat.messages);
            input.value = '';
            input.focus();
            renderChats();
            
            setTimeout(() => {
                const replies = [
                    "Интересно, расскажи подробнее",
                    "Понял, спасибо за информацию",
                    "Давай обсудим это завтра",
                    "Хорошо, я учту",
                    "Отличная идея!"
                ];
                
                const randomReply = replies[Math.floor(Math.random() * replies.length)];
                const replyMessage = {
                    id: Date.now() + 1,
                    text: randomReply,
                    time: getCurrentTime(),
                    sender: activeChat.type === 'private' ? activeChat.userId : allUsers[Math.floor(Math.random() * allUsers.length)].id,
                    type: 'text'
                };
                
                activeChat.messages.push(replyMessage);
                activeChat.lastMessage = randomReply;
                activeChat.time = 'Только что';
                
                renderMessages(activeChat.messages);
                renderChats();
            }, 1000 + Math.random() * 2000);
        }

        function startNewChat(userId) {
            const existingChat = chats.find(c => c.type === 'private' && c.userId === userId);
            
            if (existingChat) {
                openChat(existingChat.id);
            } else {
                const user = allUsers.find(u => u.id === userId);
                if (!user) return;
                
                const newChat = {
                    id: chats.length + 1,
                    type: 'private',
                    userId: userId,
                    lastMessage: "Чат создан",
                    time: "Только что",
                    unread: 0,
                    messages: []
                };
                
                chats.push(newChat);
                renderChats();
                openChat(newChat.id);
            }
        }

        function toggleRecording() {
            isRecording = !isRecording;
            const recordBtn = document.getElementById('record-btn');
            
            if (isRecording) {
                recordBtn.classList.add('recording');
                recordBtn.innerHTML = '<i class="fas fa-stop"></i>';
                recordBtn.title = "Остановить запись";
                
                setTimeout(() => {
                    if (isRecording) {
                        toggleRecording();
                        sendVoiceMessage();
                    }
                }, 3000);
            } else {
                recordBtn.classList.remove('recording');
                recordBtn.innerHTML = '<i class="fas fa-microphone"></i>';
                recordBtn.title = "Записать голосовое сообщение";
            }
        }

        function sendVoiceMessage() {
            if (!activeChat) return;
            
            const voiceMessage = {
                id: Date.now(),
                text: "",
                time: getCurrentTime(),
                sender: currentUser.id,
                type: 'voice',
                duration: "0:45"
            };
            
            activeChat.messages.push(voiceMessage);
            activeChat.lastMessage = "Голосовое сообщение";
            activeChat.time = 'Только что';
            
            renderMessages(activeChat.messages);
            renderChats();
        }

        function startVideoCall() {
            if (!activeChat) {
                alert('Выберите чат для звонка');
                return;
            }
            
            let callWithName = '';
            if (activeChat.type === 'private') {
                const user = allUsers.find(u => u.id === activeChat.userId);
                callWithName = user ? user.name : 'Пользователь';
            } else {
                const group = groups.find(g => g.id === activeChat.groupId);
                callWithName = group ? group.name : 'Группа';
            }
            
            document.getElementById('call-with-name').textContent = `Видеозвонок с ${callWithName}`;
            document.getElementById('remote-user-label').textContent = callWithName;
            
            document.getElementById('video-call-container').style.display = 'flex';
            startCallTimer();
            simulateVideo();
        }

        function startVoiceCall() {
            if (!activeChat) {
                alert('Выберите чат для звонка');
                return;
            }
            
            alert('Голосовой звонок запущен (имитация)');
        }

        function endCall() {
            document.getElementById('video-call-container').style.display = 'none';
            stopCallTimer();
            document.getElementById('mute-btn').classList.remove('active');
            document.getElementById('video-toggle-btn').classList.remove('active');
        }

        function startCallTimer() {
            callSeconds = 0;
            updateCallTimer();
            callTimer = setInterval(() => {
                callSeconds++;
                updateCallTimer();
            }, 1000);
        }

        function stopCallTimer() {
            if (callTimer) {
                clearInterval(callTimer);
                callTimer = null;
            }
        }

        function updateCallTimer() {
            const minutes = Math.floor(callSeconds / 60);
            const seconds = callSeconds % 60;
            document.getElementById('call-timer').textContent = 
                `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function toggleMute() {
            const muteBtn = document.getElementById('mute-btn');
            muteBtn.classList.toggle('active');
            muteBtn.innerHTML = muteBtn.classList.contains('active') ? 
                '<i class="fas fa-microphone-slash"></i>' : 
                '<i class="fas fa-microphone"></i>';
        }

        function toggleVideo() {
            const videoBtn = document.getElementById('video-toggle-btn');
            videoBtn.classList.toggle('active');
            videoBtn.innerHTML = videoBtn.classList.contains('active') ? 
                '<i class="fas fa-video-slash"></i>' : 
                '<i class="fas fa-video"></i>';
        }

        function simulateVideo() {
            const localVideo = document.getElementById('local-video');
            const remoteVideo = document.getElementById('remote-video');
            
            const canvas = document.createElement('canvas');
            canvas.width = 640;
            canvas.height = 480;
            const ctx = canvas.getContext('2d');
            
            function drawVideo(color, element) {
                ctx.fillStyle = color;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                ctx.fillStyle = 'white';
                ctx.font = '48px Arial';
                ctx.textAlign = 'center';
                ctx.fillText('Видео поток', canvas.width/2, canvas.height/2);
                const stream = canvas.captureStream(25);
                element.srcObject = stream;
            }
            
            drawVideo('#1a2980', localVideo);
            drawVideo('#26d0ce', remoteVideo);
        }

        function addContact() {
            const name = prompt('Введите имя нового контакта:');
            if (name && name.trim()) {
                const newId = allUsers.length + 1;
                const initials = name.split(' ').map(n => n[0]).join('').toUpperCase();
                
                const newUser = {
                    id: newId,
                    name: name.trim(),
                    avatar: initials.substring(0, 2),
                    online: true
                };
                
                allUsers.push(newUser);
                renderContacts();
                alert(`Контакт "${name}" добавлен!`);
            }
        }

        function createGroup() {
            const name = prompt('Введите название группы:');
            if (name && name.trim()) {
                const newId = groups.length + 1;
                const initials = name.split(' ').map(n => n[0]).join('').toUpperCase().substring(0, 2);
                
                const newGroup = {
                    id: newId,
                    name: name.trim(),
                    avatar: initials,
                    members: [currentUser.id, 2, 3],
                    lastMessage: "Группа создана",
                    time: "Только что",
                    unread: 0
                };
                
                groups.push(newGroup);
                
                const newChat = {
                    id: chats.length + 1,
                    type: 'group',
                    groupId: newId,
                    lastMessage: "Группа создана",
                    time: "Только что",
                    unread: 0,
                    messages: []
                };
                
                chats.push(newChat);
                
                renderGroups();
                renderChats();
                alert(`Группа "${name}" создана!`);
            }
        }

        function getCurrentTime() {
            const now = new Date();
            return `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        }
    </script>
</body>
</html>
