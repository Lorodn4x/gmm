<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Генератор изображений с использованием ИИ">
    <meta name="theme-color" content="#2196F3">
    <title>GPT_Project</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #2196F3;
            --primary-dark: #1976D2;
            --primary-light: #BBDEFB;
            --accent-color: #FF4081;
            --background: #0D1117;
            --card-bg: #161B22;
            --sidebar-bg: #21262D;
            --text-primary: #F0F6FC;
            --text-secondary: #8B949E;
            --error-color: #F85149;
            --success-color: #3FB950;
            --border-color: #30363D;
            --hover-bg: #1F2937;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --background: #121212;
                --card-bg: #1E1E1E;
                --text-primary: #FFFFFF;
                --text-secondary: #B0B0B0;
            }
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: var(--background);
            color: var(--text-primary);
            line-height: 1.7;
            min-height: 100vh;
            padding: 0;
            font-size: 18px;
            margin: 0;
        }

        .container {
            max-width: 100%;
            margin: 0;
            padding: 0;
        }

        .app-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin: 0;
            padding: 20px 30px;
            background: rgba(33, 38, 45, 0.8);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--border-color);
            position: relative;
            z-index: 10;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 28px;
            font-weight: 700;
            color: var(--primary-color);
            background: linear-gradient(135deg, var(--primary-color), #9C27B0);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 10px rgba(33, 150, 243, 0.2);
        }

        .logo i {
            font-size: 36px;
        }

        .main-content {
            display: grid;
            grid-template-columns: 350px 1fr;
            gap: 0;
            height: calc(100vh - 80px);
            position: relative;
        }

        @media (max-width: 1024px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }

        .sidebar {
            background: var(--sidebar-bg);
            padding: 30px;
            border-radius: 0;
            height: 100%;
            overflow-y: auto;
            box-shadow: 2px 0 12px rgba(0, 0, 0, 0.5);
            position: relative;
            z-index: 5;
            border-right: 1px solid var(--border-color);
        }

        .workspace {
            background: var(--background);
            padding: 20px;
            border-radius: 0;
            height: 100%;
            overflow-y: auto;
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-label {
            display: block;
            margin-bottom: 8px;
            color: var(--text-secondary);
            font-size: 14px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        textarea, select, input {
            width: 100%;
            padding: 16px;
            border: 1px solid var(--border-color);
            border-radius: 12px;
            font-size: 16px;
            background: var(--card-bg);
            color: var(--text-primary);
            transition: all 0.3s ease;
            height: 60px;
        }

        textarea:focus, select:focus, input:focus {
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
            background: var(--hover-bg);
        }

        .btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary-color), #9C27B0);
            color: white;
            padding: 16px 32px;
            font-size: 16px;
            font-weight: 600;
            letter-spacing: 0.5px;
            transition: all 0.4s ease;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(33, 150, 243, 0.3);
        }

        .btn-outline {
            background: transparent;
            border: 2px solid var(--primary-color);
            color: var(--primary-color);
        }

        .btn-outline:hover {
            background: var(--primary-color);
            color: white;
        }

        .gallery {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 30px;
            margin-top: 20px;
        }

        .image-card {
            position: relative;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
            aspect-ratio: 1;
            animation: fadeIn 0.5s ease forwards;
            background: var(--card-bg);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid var(--border-color);
        }

        .image-card img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.3s ease;
        }

        .image-card:hover img {
            transform: translateY(-4px);
            box-shadow: 0 12px 30px rgba(0, 0, 0, 0.3);
            border-color: var(--primary-color);
        }

        .image-actions {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 15px;
            background: rgba(33, 38, 45, 0.9);
            backdrop-filter: blur(8px);
            display: flex;
            gap: 12px;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .image-card:hover .image-actions {
            opacity: 1;
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .loading-overlay.active {
            opacity: 1;
            pointer-events: all;
        }

        .spinner {
            width: 50px;
            height: 50px;
            border: 5px solid var(--primary-light);
            border-top-color: var(--primary-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 16px 24px;
            background: var(--card-bg);
            border-radius: 12px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(8px);
            display: flex;
            align-items: center;
            gap: 10px;
            transform: translateY(100px);
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            font-size: 18px;
        }

        .toast.show {
            transform: translateY(0);
            opacity: 1;
        }

        .toast.success { border-left: 4px solid var(--success-color); }
        .toast.error { border-left: 4px solid var(--error-color); }

        .limits-info {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .limits-badge {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 10px 20px;
            background: var(--card-bg);
            color: var(--text-primary);
            border-radius: 30px;
            font-size: 14px;
            font-weight: 600;
            min-width: 120px;
            border: 1px solid var(--border-color);
            text-align: center;
            transition: all 0.3s ease;
        }

        .limits-badge:hover {
            border-color: var(--primary-color);
            transform: translateY(-2px);
        }

        .limits-badge i {
            font-size: 16px;
            color: var(--primary-color);
        }

        .limits-badge span {
            white-space: nowrap;
            text-align: center;
        }

        /* Специальный стиль для бейджа быстрого режима */
        .limits-badge[title="Быстрый режим"] span {
            padding-right: 3px;
        }

        .tooltip {
            position: relative;
            display: inline-block;
        }

        .tooltip:hover::after {
            content: attr(data-tooltip);
            position: absolute;
            bottom: 100%;
            left: 50%;
            transform: translateX(-50%);
            padding: 5px 10px;
            background: rgba(0,0,0,0.8);
            color: white;
            border-radius: 5px;
            font-size: 14px;
            white-space: nowrap;
        }

        /* Обновляем стили для кнопок режима генерации */
        .generation-modes {
            display: flex;
            gap: 8px;
            margin-bottom: 16px;
        }

        .mode-button {
            flex: 1;
            padding: 8px 12px;
            border-radius: 6px;
            border: 1px solid var(--border-color);
            background: var(--card-bg);
            color: var(--text-secondary);
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
            min-height: 40px;
        }

        .mode-button i {
            font-size: 16px;
            color: var(--text-secondary);
            transition: color 0.3s ease;
        }

        .mode-button span {
            font-weight: 500;
        }

        .mode-button small {
            display: none;
        }

        .mode-button.active {
            background: var(--primary-color);
            border-color: var(--primary-color);
            color: white;
            box-shadow: 0 2px 8px rgba(33, 150, 243, 0.3);
        }

        .mode-button.active i {
            color: white;
        }

        .mode-button:hover:not(.active) {
            border-color: var(--primary-color);
            background: var(--card-bg);
            color: var(--primary-color);
        }

        .mode-button:hover:not(.active) i {
            color: var(--primary-color);
        }

        /* Добавляем стили для модального окна */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.9);
            z-index: 1100;
            padding: 20px;
            overflow: auto;
            cursor: zoom-out;
            backdrop-filter: blur(8px);
            transition: opacity 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .modal.active {
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .modal img {
            max-width: 90%;
            max-height: 90vh;
            object-fit: contain;
            border-radius: 10px;
            cursor: default;
        }

        .image-card {
            cursor: zoom-in;
        }

        /* Добавляем кнопку закрытия */
        .modal-close {
            position: fixed;
            top: 20px;
            right: 20px;
            color: white;
            font-size: 30px;
            cursor: pointer;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        .modal-close:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.1);
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .header-controls {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .settings-btn {
            padding: 10px;
            font-size: 20px;
            width: 45px;
            height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
        }

        .settings-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            z-index: 2000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(12px);
            transition: opacity 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .settings-modal.active {
            display: flex;
        }

        .settings-content {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 0;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        .settings-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 25px;
            border-bottom: 1px solid var(--text-secondary);
            background: var(--sidebar-bg);
        }

        .settings-header h2 {
            margin: 0;
            color: var(--text-primary);
            font-size: 24px;
        }

        .close-btn {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 24px;
            cursor: pointer;
            padding: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .close-btn:hover {
            color: var(--text-primary);
            transform: scale(1.1);
        }

        .settings-body {
            padding: 20px;
        }

        .settings-group {
            margin-bottom: 20px;
        }

        .settings-label {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--text-secondary);
            font-size: 16px;
            cursor: pointer;
        }

        .settings-label input[type="checkbox"] {
            width: 20px;
            height: 20px;
            margin: 0;
            cursor: pointer;
        }

        .prompt-container {
            position: relative;
            display: flex;
            gap: 10px;
        }

        .enhance-btn {
            padding: 12px 24px;
            height: fit-content;
            white-space: nowrap;
            display: flex;
            align-items: center;
            gap: 12px;
            margin-top: 8px;
            background: linear-gradient(135deg, var(--primary-color), #9C27B0);
            color: white;
            border: none;
            border-radius: 12px;
            font-weight: 600;
            font-size: 16px;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .enhance-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(255,255,255,0.1), rgba(255,255,255,0));
            transform: translateX(-100%);
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .enhance-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(33, 150, 243, 0.3);
        }

        .enhance-btn:hover::before {
            transform: translateX(100%);
        }

        .enhance-btn:active {
            transform: translateY(1px);
            box-shadow: 0 2px 8px rgba(33, 150, 243, 0.2);
        }

        .enhance-btn i {
            font-size: 18px;
            color: white;
            transition: transform 0.4s ease;
        }

        .enhance-btn:hover i {
            transform: rotate(180deg);
        }

        #enhancedPromptContainer {
            background: var(--card-bg);
            border-radius: 10px;
            padding: 15px;
            margin-top: 15px;
            border: 1px solid var(--border-color);
            transition: all 0.3s ease;
            opacity: 0;
            transform: translateY(-10px);
        }

        #enhancedPromptContainer.visible {
            opacity: 1;
            transform: translateY(0);
        }

        #enhancedPrompt {
            width: 100%;
            min-height: 100px;
            padding: 15px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            background: var(--sidebar-bg);
            color: var(--text-primary);
            font-size: 16px;
            line-height: 1.5;
            resize: vertical;
        }

        /* Добавляем стили для скроллбара */
        ::-webkit-scrollbar {
            width: 10px;
        }

        ::-webkit-scrollbar-track {
            background: var(--background);
        }

        ::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 5px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--text-secondary);
        }

        /* Добавляем стили для ссылки разработчика */
        .developer-link {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 16px;
            background: var(--card-bg);
            color: var(--text-primary);
            border-radius: 20px;
            font-size: 16px;
            text-decoration: none;
            border: 1px solid var(--border-color);
            transition: all 0.3s ease;
        }

        .developer-link:hover {
            background: var(--primary-color);
            border-color: var(--primary-color);
            transform: translateY(-2px);
        }

        .developer-link i {
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="app-header">
            <div class="logo">
                <i class="fas fa-robot"></i>
                <span>GPT_Project</span>
            </div>
            <div class="header-controls">
                <a href="https://github.com/Lorodn4x" target="_blank" class="developer-link">
                    <i class="fab fa-github"></i>
                    <span>Lordon4x</span>
                </a>
                <button class="btn btn-outline settings-btn" onclick="toggleSettingsModal()">
                    <i class="fas fa-cog"></i>
                </button>
                <div class="limits-info">
                    <span class="limits-badge" title="Стандартный режим">
                        <i class="fas fa-image"></i>
                        <span id="imagineLimit">100,000</span>
                    </span>
                    <span class="limits-badge" title="Быстрый режим">
                        <i class="fas fa-bolt"></i>
                        <span id="imagine2Limit">1,000</span>
                    </span>
                </div>
            </div>
        </header>

        <div class="main-content">
            <aside class="sidebar">
                <div class="input-group">
                    <label class="input-label">Режим генерации</label>
                    <div class="generation-modes">
                        <button class="mode-button active" id="imagineBtn" onclick="switchEndpoint('imagine')">
                            <i class="fas fa-image"></i>
                            <span>Стандартный</span>
                            <small>Высокое качество изображений</small>
                        </button>
                        <button class="mode-button" id="imagine2Btn" onclick="switchEndpoint('imagine2')">
                            <i class="fas fa-bolt"></i>
                            <span>Быстрый</span>
                            <small>Быстрая генерация</small>
                        </button>
                    </div>
                </div>

                <div class="input-group">
                    <label class="input-label">Модель</label>
                    <select id="model">
                        <option value="stable-diffusion-xl-lightning">SD XL Lightning</option>
                        <option value="stable-diffusion-xl-base">SD XL Base</option>
                        <option value="Flux-1.1-Pro">Flux 1.1 Pro</option>
                        <option value="ideogram">Ideogram</option>
                        <option value="flux">Flux</option>
                        <option value="flux-realism">Flux Realism</option>
                        <option value="flux-anime">Flux Anime</option>
                        <option value="flux-3d">Flux 3D</option>
                        <option value="flux-disney">Flux Disney</option>
                        <option value="flux-pixel">Flux Pixel</option>
                        <option value="flux-4o">Flux 4O</option>
                        <option value="any-dark">Any Dark</option>
                    </select>
                </div>

                <div class="input-group">
                    <label class="input-label">Размер</label>
                    <select id="size">
                        <option value="1:1">1:1 Квадрат</option>
                        <option value="16:9">16:9 Ландшафт</option>
                        <option value="9:16">9:16 Портрет</option>
                        <option value="21:9">21:9 Широкий</option>
                        <option value="9:21">9:21 Высокий</option>
                        <option value="1:2">1:2 Вертикальный</option>
                        <option value="2:1">2:1 Горизонтальный</option>
                    </select>
                </div>

                <div class="input-group">
                    <label class="input-label">Разрешение</label>
                    <select id="resolution">
                        <option value="512">512px</option>
                        <option value="768">768px</option>
                        <option value="1024" selected>1024px</option>
                        <option value="1536">1536px</option>
                        <option value="2048">2048px</option>
                    </select>
                </div>

                <div class="input-group">
                    <label class="input-label">Количество изображений</label>
                    <select id="imageCount">
                        <option value="1">1 изображение</option>
                        <option value="2">2 изображения</option>
                        <option value="4" selected>4 изображения</option>
                        <option value="6">6 изображений</option>
                        <option value="8">8 изображений</option>
                    </select>
                </div>
            </aside>

            <main class="workspace">
                <div class="input-group">
                    <label class="input-label">Описание изображения</label>
                    <div class="prompt-container">
                        <textarea id="prompt" rows="4" placeholder="Опишите изображение, которое хотите сгенерировать..." min-height="200" font-size="20px" padding="20px"></textarea>
                        <button class="btn btn-outline enhance-btn" onclick="enhancePromptAndShow()">
                            <i class="fas fa-wand-magic-sparkles"></i>
                            Улучшить
                        </button>
                    </div>
                </div>

                <button class="btn btn-primary" onclick="generateImage()">
                    <i class="fas fa-magic"></i> Сгенерировать
                </button>

                <div class="gallery" id="result"></div>
            </main>
        </div>
    </div>

    <div class="loading-overlay" id="loading">
        <div class="spinner"></div>
    </div>

    <div class="toast" id="toast"></div>

    <!-- Добавляем разметку модального окна -->
    <div class="modal" id="imageModal" onclick="closeModal(event)">
        <div class="modal-close" onclick="closeModal(event)">
            <i class="fas fa-times"></i>
        </div>
        <img id="modalImage" src="" alt="">
        <div id="imageInfo" style="color: var(--text-primary); padding: 10px;"></div>
    </div>

    <!-- Добавляем модальное окно настроек -->
    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <div class="settings-header">
                <h2>Настройки</h2>
                <button class="close-btn" onclick="toggleSettingsModal()">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <div class="settings-body">
                <div class="settings-group">
                    <label class="settings-label">Модель по умолчанию</label>
                    <select id="defaultModel" onchange="updateDefaultModel(this.value)">
                        <option value="stable-diffusion-xl-lightning">SD XL Lightning</option>
                        <option value="stable-diffusion-xl-base">SD XL Base</option>
                        <option value="Flux-1.1-Pro">Flux 1.1 Pro</option>
                        <option value="ideogram">Ideogram</option>
                        <option value="flux">Flux</option>
                        <option value="flux-realism">Flux Realism</option>
                        <option value="flux-anime">Flux Anime</option>
                        <option value="flux-3d">Flux 3D</option>
                        <option value="flux-disney">Flux Disney</option>
                        <option value="flux-pixel">Flux Pixel</option>
                        <option value="flux-4o">Flux 4O</option>
                        <option value="any-dark">Any Dark</option>
                    </select>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Константы для лимитов
        const DAILY_LIMITS = {
            imagine: 100000,
            imagine2: 1000
        };

        // Состояние приложения
        let state = {
            endpoint: 'imagine',
            settings: {
                defaultModel: 'stable-diffusion-xl-lightning',
                imageCount: 4,
                size: '1:1',
                resolution: '1024',
                model: 'stable-diffusion-xl-lightning'
            },
            usageData: {
                imagine: { count: 0, date: new Date().toDateString() },
                imagine2: { count: 0, date: new Date().toDateString() }
            }
        };

        // Загрузка состояния при старте
        function loadState() {
            const savedState = localStorage.getItem('imageGeneratorState');
            if (savedState) {
                try {
                    const parsed = JSON.parse(savedState);
                    const today = new Date().toDateString();

                    // Проверяем и обновляем счетчики использования
                    if (!parsed.usageData || parsed.usageData.imagine.date !== today) {
                        parsed.usageData = {
                            imagine: { count: 0, date: today },
                            imagine2: { count: 0, date: today }
                        };
                    }

                    // Загружаем состояние с сохранением значений по умолчанию
                    state = {
                        ...state,
                        endpoint: parsed.endpoint || state.endpoint,
                        settings: {
                            ...state.settings,
                            ...(parsed.settings || {})
                        },
                        usageData: parsed.usageData
                    };

                    // Применяем сохраненные настройки к интерфейсу
                    applySettings();
                } catch (error) {
                    console.error('Ошибка при загрузке состояния:', error);
                    // В случае ошибки используем значения по умолчанию
                    localStorage.removeItem('imageGeneratorState');
                }
            }
            updateLimitsDisplay();
        }

        // Обновление отображения лимитов
        function updateLimitsDisplay() {
            const imagineLimit = document.getElementById('imagineLimit');
            const imagine2Limit = document.getElementById('imagine2Limit');
            
            if (imagineLimit && imagine2Limit) {
                imagineLimit.textContent = (DAILY_LIMITS.imagine - state.usageData.imagine.count).toLocaleString();
                imagine2Limit.textContent = (DAILY_LIMITS.imagine2 - state.usageData.imagine2.count).toLocaleString();
            }
        }

        // Сохранение состояния
        function saveState() {
            try {
                // Обновляем состояние перед сохранением
                state.settings = {
                    ...state.settings,
                    model: document.getElementById('model').value,
                    imageCount: document.getElementById('imageCount').value,
                    size: document.getElementById('size').value,
                    resolution: document.getElementById('resolution').value
                };
                
                localStorage.setItem('imageGeneratorState', JSON.stringify(state));
                updateLimitsDisplay();
            } catch (error) {
                console.error('Ошибка при сохранении состояния:', error);
                showToast('Ошибка при сохранении настроек', 'error');
            }
        }

        // Добавляем обработчики изменений для всех настроек
        function setupSettingsListeners() {
            const settingsElements = ['model', 'imageCount', 'size', 'resolution'];
            settingsElements.forEach(elementId => {
                const element = document.getElementById(elementId);
                if (element) {
                    element.addEventListener('change', () => {
                        saveState();
                        showToast('Настройки сохранены', 'success');
                    });
                }
            });
        }

        // Переключение endpoint
        function switchEndpoint(endpoint) {
            state.endpoint = endpoint;
            
            // Обновляем классы кнопок
            document.getElementById('imagineBtn').classList.toggle('active', endpoint === 'imagine');
            document.getElementById('imagine2Btn').classList.toggle('active', endpoint === 'imagine2');
            
            // Показываем уведомление о смене режима
            showToast(
                endpoint === 'imagine' 
                    ? 'Включен стандартный режим генерации' 
                    : 'Включен быстрый режим генерации'
            );
            
            saveState();
        }

        // Применение сохраненных настроек к элементам интерфейса
        function applySettings() {
            // Устанавливаем значения в селекты
            document.getElementById('model').value = state.settings.model;
            document.getElementById('defaultModel').value = state.settings.model;
            document.getElementById('imageCount').value = state.settings.imageCount;
            document.getElementById('size').value = state.settings.size;
            document.getElementById('resolution').value = state.settings.resolution;
        }

        // Обновляем функцию updateDefaultModel
        function updateDefaultModel(value) {
            state.settings.model = value;
            // Обновляем текущую модель
            document.getElementById('model').value = value;
            // Сохраняем настройки
            saveState();
            showToast('Модель по умолчанию обновлена');
        }

        // Обновляем инициализацию
        document.addEventListener('DOMContentLoaded', () => {
            loadState();
            setupSettingsListeners();
            
            // Устанавливаем активный режим при загрузке
            switchEndpoint(state.endpoint);
            
            // Добавляем поддержку горячих клавиш
            document.addEventListener('keydown', (e) => {
                if (e.ctrlKey && e.key === 'Enter') {
                    generateImage();
                }
            });
        });

        // Показать/скрыть панель настроек
        function toggleSettings() {
            const panel = document.getElementById('settingsPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
        }

        // Добавляем новые функции
        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            toast.className = `toast ${type}`;
            toast.innerHTML = `
                <i class="fas fa-${type === 'success' ? 'check-circle' : 'exclamation-circle'}"></i>
                <span>${message}</span>
            `;
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                showToast('Скопировано в буфер обмена');
            });
        }

        function downloadImage(url, filename) {
            fetch(url)
                .then(response => response.blob())
                .then(blob => {
                    const link = document.createElement('a');
                    link.href = URL.createObjectURL(blob);
                    link.download = filename;
                    link.click();
                    showToast('Изображение сохранено');
                })
                .catch(() => showToast('Ошибка при сохранении', 'error'));
        }

        // Функция открытия модального окна
        function openModal(imageUrl, imageInfo) {
            const modal = document.getElementById('imageModal');
            const modalImg = document.getElementById('modalImage');
            const infoContainer = document.getElementById('imageInfo');

            modalImg.src = imageUrl;
            modal.classList.add('active');
            document.body.style.overflow = 'hidden'; // Запрещаем прокрутку страницы

            // Отображаем информацию о изображении
            infoContainer.innerHTML = `
                <p><strong>Режим генерации:</strong> ${imageInfo.generationMode}</p>
                <p><strong>Модель:</strong> ${imageInfo.model}</p>
                <p><strong>Размер:</strong> ${imageInfo.size}</p>
                <p><strong>Разрешение:</strong> ${imageInfo.resolution}</p>
                <p><strong>Описание:</strong> ${imageInfo.prompt}</p>
            `;
        }

        // Функция закрытия модального окна
        function closeModal(event) {
            if (event) {
                event.stopPropagation();
            }
            const modal = document.getElementById('imageModal');
            modal.classList.remove('active');
            document.body.style.overflow = ''; // Возвращаем прокрутку
        }

        // Добавляем обработчик клавиши Escape для закрытия модального окна
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                closeModal();
            }
        });

        // Функция для улучшения промпта через LLM
        async function enhancePrompt(prompt) {
            try {
                const systemPrompt = "You are an expert at improving image generation prompts. " +
                    "Your task is to take a basic description in Russian and: " +
                    "1. Add artistic details and descriptions " +
                    "2. Improve composition and style " +
                    "3. Translate to English " +
                    "4. Add technical parameters for better results " +
                    "Return only the enhanced prompt in English, as a plain text without quotes or formatting.";

                const response = await fetch('https://text.pollinations.ai/openai', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        messages: [
                            { role: 'system', content: systemPrompt },
                            { role: 'user', content: prompt }
                        ],
                        model: 'gpt-3.5-turbo',
                        temperature: 0.7,
                        max_tokens: 200
                    }),
                });

                if (!response.ok) {
                    throw new Error('Ошибка при улучшении промпта');
                }

                const data = await response.json();
                if (!data || !data.choices || !data.choices[0] || !data.choices[0].message) {
                    throw new Error('Некорректный ответ от сервера');
                }

                const enhancedPrompt = data.choices[0].message.content.trim();
                console.log('Enhanced prompt:', enhancedPrompt); // Для отладки
                return enhancedPrompt;
            } catch (error) {
                console.error('Ошибка при улучшении промпта:', error);
                showToast('Ошибка при улучшении промпта: ' + error.message, 'error');
                return prompt;
            }
        }

        // Функция для показа улучшенного промпта
        async function enhancePromptAndShow() {
            const promptTextarea = document.getElementById('prompt');
            const originalPrompt = promptTextarea.value;
            
            if (!originalPrompt) {
                showToast('Пожалуйста, введите описание изображения', 'error');
                return;
            }

            document.getElementById('loading').classList.add('active');
            
            try {
                const enhancedPrompt = await enhancePrompt(originalPrompt);
                if (enhancedPrompt === originalPrompt) {
                    throw new Error('Не удалось улучшить промпт');
                }
                
                // Сохраняем текущую позицию прокрутки
                const scrollTop = promptTextarea.scrollTop;
                
                // Обновляем значение в текстовом поле
                promptTextarea.value = enhancedPrompt;
                
                // Автоматически подстраиваем высоту textarea под контент
                promptTextarea.style.height = 'auto';
                promptTextarea.style.height = promptTextarea.scrollHeight + 'px';
                
                // Восстанавливаем позицию прокрутки
                promptTextarea.scrollTop = scrollTop;
                
                showToast('Промпт улучшен и переведен', 'success');
            } catch (error) {
                console.error('Ошибка:', error);
                showToast(error.message, 'error');
            } finally {
                document.getElementById('loading').classList.remove('active');
            }
        }

        // Обновляем функцию generateImage
        async function generateImage() {
            const prompt = document.getElementById('prompt').value;
            const imageCount = parseInt(document.getElementById('imageCount').value);
            
            if (!prompt) {
                showToast('Пожалуйста, введите описание изображения', 'error');
                return;
            }

            const currentLimit = state.endpoint === 'imagine' ? 
                DAILY_LIMITS.imagine - state.usageData.imagine.count :
                DAILY_LIMITS.imagine2 - state.usageData.imagine2.count;

            if (currentLimit < imageCount) {
                showToast(`Недостаточно лимита для генерации ${imageCount} изображений`, 'error');
                return;
            }

            document.getElementById('loading').classList.add('active');

            try {
                const baseUrl = `https://api.airforce/v1/${state.endpoint}`;
                const baseParams = {
                    prompt: prompt,
                    size: document.getElementById('size').value,
                    model: document.getElementById('model').value,
                    width: document.getElementById('resolution').value,
                    height: document.getElementById('resolution').value
                };

                const promises = Array(imageCount).fill().map(() => {
                    const params = new URLSearchParams(baseParams);
                    params.append('seed', Math.floor(Math.random() * 999999999));
                    return fetch(`${baseUrl}?${params.toString()}`);
                });

                const responses = await Promise.all(promises);
                
                for (const response of responses) {
                    if (!response.ok) {
                        throw new Error('Ошибка при генерации изображения');
                    }

                    const blob = await response.blob();
                    const imageUrl = URL.createObjectURL(blob);
                    
                    // Сохраняем информацию о изображении
                    const imageInfo = {
                        generationMode: state.endpoint === 'imagine' ? 'Стандартный' : 'Быстрый',
                        model: document.getElementById('model').value,
                        size: document.getElementById('size').value,
                        resolution: document.getElementById('resolution').value,
                        prompt: prompt
                    };

                    const imageCard = document.createElement('div');
                    imageCard.className = 'image-card';
                    imageCard.innerHTML = `
                        <img src="${imageUrl}" alt="${prompt}" onclick="openModal('${imageUrl}', ${JSON.stringify(imageInfo)})">
                        <div class="image-actions">
                            <button class="btn btn-outline" onclick="copyToClipboard('${prompt}')">
                                <i class="fas fa-copy"></i>
                            </button>
                            <button class="btn btn-outline" onclick="downloadImage('${imageUrl}', 'generated-image.png')">
                                <i class="fas fa-download"></i>
                            </button>
                            <button class="btn btn-outline" onclick="openModal('${imageUrl}', ${JSON.stringify(imageInfo)})">
                                <i class="fas fa-search-plus"></i>
                            </button>
                        </div>
                    `;
                    
                    const result = document.getElementById('result');
                    if (result.firstChild) {
                        result.insertBefore(imageCard, result.firstChild);
                    } else {
                        result.appendChild(imageCard);
                    }
                }

                state.usageData[state.endpoint].count += imageCount;
                saveState();

                showToast(`Успешно сгенерировано ${imageCount} изображений`);
            } catch (error) {
                showToast(error.message, 'error');
            } finally {
                document.getElementById('loading').classList.remove('active');
            }
        }

        // Добавляем обработчик для переключателя улучшения промпта
        document.getElementById('enhancePromptToggle').addEventListener('change', function(e) {
            const container = document.getElementById('enhancedPromptContainer');
            if (!e.target.checked) {
                container.classList.remove('visible');
                setTimeout(() => {
                    container.style.display = 'none';
                }, 300);
            }
        });

        // Функция переключения модального окна настроек
        function toggleSettingsModal() {
            const modal = document.getElementById('settingsModal');
            modal.classList.toggle('active');
            if (modal.classList.contains('active')) {
                // Устанавливаем текущее значение в селект
                document.getElementById('defaultModel').value = state.settings.model;
            }
        }

        // Добавляем обработчик клавиши Escape для закрытия модального окна настроек
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                const settingsModal = document.getElementById('settingsModal');
                if (settingsModal.classList.contains('active')) {
                    toggleSettingsModal();
                }
            }
        });

        // Закрытие модального окна при клике вне его
        document.getElementById('settingsModal').addEventListener('click', (e) => {
            if (e.target === e.currentTarget) {
                toggleSettingsModal();
            }
        });
    </script>
</body>
</html> 
