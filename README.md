<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SecurityCheck | Проверка безопасности</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* ===== GLOBAL STYLES ===== */
        :root {
            --primary: #6366f1;
            --primary-dark: #4f46e5;
            --secondary: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --dark-bg: #0f172a;
            --dark-card: #1e293b;
            --dark-border: #334155;
            --text-primary: #f1f5f9;
            --text-secondary: #94a3b8;
            --accent: #8b5cf6;
            --gradient: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            background-color: var(--dark-bg);
            color: var(--text-primary);
            line-height: 1.6;
            min-height: 100vh;
            background-image: 
                radial-gradient(circle at 20% 80%, rgba(99, 102, 241, 0.15) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(139, 92, 246, 0.1) 0%, transparent 50%);
        }

        /* ===== CONTAINER & LAYOUT ===== */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* ===== HEADER ===== */
        .main-header {
            padding: 40px 0 30px;
            border-bottom: 1px solid var(--dark-border);
            margin-bottom: 40px;
            position: relative;
            overflow: hidden;
        }

        .header-content {
            text-align: center;
            position: relative;
            z-index: 2;
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-bottom: 20px;
        }

        .logo-icon {
            font-size: 2.5rem;
            background: var(--gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 20px rgba(99, 102, 241, 0.3));
        }

        .logo-text {
            font-size: 2.8rem;
            font-weight: 800;
            background: var(--gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: -0.5px;
        }

        .tagline {
            font-size: 1.2rem;
            color: var(--text-secondary);
            max-width: 700px;
            margin: 0 auto 25px;
            line-height: 1.7;
        }

        /* ===== TABS ===== */
        .tabs-container {
            background: var(--dark-card);
            border-radius: 16px;
            border: 1px solid var(--dark-border);
            margin-bottom: 40px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .tabs-header {
            display: flex;
            overflow-x: auto;
            scrollbar-width: none;
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--dark-border);
        }

        .tabs-header::-webkit-scrollbar {
            display: none;
        }

        .tab-btn {
            padding: 20px 30px;
            background: none;
            border: none;
            color: var(--text-secondary);
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            white-space: nowrap;
            transition: all 0.3s ease;
            position: relative;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .tab-btn i {
            font-size: 1.2rem;
            transition: all 0.3s ease;
        }

        .tab-btn:hover {
            color: var(--text-primary);
            background: rgba(99, 102, 241, 0.1);
        }

        .tab-btn.active {
            color: var(--primary);
            background: rgba(99, 102, 241, 0.15);
        }

        .tab-btn.active::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: var(--gradient);
            border-radius: 3px 3px 0 0;
        }

        .tab-btn.active i {
            transform: scale(1.1);
        }

        .tab-content {
            display: none;
            padding: 40px;
            animation: fadeIn 0.5s ease;
        }

        .tab-content.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ===== FEATURE CARDS ===== */
        .feature-card {
            background: var(--dark-card);
            border-radius: 16px;
            padding: 40px;
            border: 1px solid var(--dark-border);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            transition: all 0.3s ease;
            height: 100%;
            display: flex;
            flex-direction: column;
        }

        .feature-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
            border-color: var(--primary);
        }

        .feature-icon {
            font-size: 3rem;
            margin-bottom: 25px;
            width: 80px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 16px;
            background: rgba(99, 102, 241, 0.1);
            color: var(--primary);
        }

        .feature-card h3 {
            font-size: 1.6rem;
            margin-bottom: 15px;
            color: var(--text-primary);
            font-weight: 700;
        }

        .feature-card p {
            color: var(--text-secondary);
            margin-bottom: 30px;
            flex-grow: 1;
            line-height: 1.7;
        }

        /* ===== BUTTONS ===== */
        .btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            background: var(--gradient);
            color: white;
            padding: 16px 32px;
            border-radius: 12px;
            text-decoration: none;
            font-weight: 600;
            font-size: 1rem;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.6s;
        }

        .btn:hover::before {
            left: 100%;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(99, 102, 241, 0.3);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn i {
            font-size: 1.2rem;
        }

        /* ===== FILE UPLOAD ===== */
        .file-upload-area {
            background: var(--dark-bg);
            border: 2px dashed var(--dark-border);
            border-radius: 12px;
            padding: 60px 30px;
            text-align: center;
            margin-bottom: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .file-upload-area:hover {
            border-color: var(--primary);
            background: rgba(99, 102, 241, 0.05);
            transform: translateY(-2px);
        }

        .file-upload-area.dragover {
            border-color: var(--secondary);
            background: rgba(16, 185, 129, 0.1);
        }

        .file-upload-icon {
            font-size: 4rem;
            color: var(--text-secondary);
            margin-bottom: 20px;
            transition: all 0.3s ease;
        }

        .file-upload-area:hover .file-upload-icon {
            color: var(--primary);
            transform: scale(1.1);
        }

        .file-upload-text {
            color: var(--text-secondary);
            font-size: 1.1rem;
            margin-bottom: 10px;
        }

        .file-upload-hint {
            color: var(--text-secondary);
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .file-preview {
            background: var(--dark-bg);
            border-radius: 12px;
            padding: 20px;
            margin-top: 20px;
            display: none;
            animation: fadeIn 0.5s ease;
            border: 1px solid var(--dark-border);
        }

        .file-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .file-icon {
            font-size: 2.5rem;
            color: var(--primary);
        }

        .file-details {
            flex-grow: 1;
        }

        .file-name {
            font-weight: 600;
            margin-bottom: 5px;
            word-break: break-all;
        }

        .file-size {
            color: var(--text-secondary);
            font-size: 0.9rem;
        }

        .file-remove {
            background: none;
            border: none;
            color: var(--danger);
            font-size: 1.2rem;
            cursor: pointer;
            padding: 5px;
            border-radius: 6px;
            transition: all 0.3s ease;
        }

        .file-remove:hover {
            background: rgba(239, 68, 68, 0.1);
        }

        /* ===== PROGRESS BAR ===== */
        .progress-container {
            margin: 25px 0;
            display: none;
        }

        .progress-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.9rem;
            color: var(--text-secondary);
        }

        .progress-bar {
            height: 10px;
            background: var(--dark-border);
            border-radius: 5px;
            overflow: hidden;
            position: relative;
        }

        .progress-fill {
            height: 100%;
            background: var(--gradient);
            border-radius: 5px;
            width: 0%;
            transition: width 0.5s ease;
            position: relative;
            overflow: hidden;
        }

        .progress-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, 
                transparent 25%, 
                rgba(255, 255, 255, 0.3) 50%, 
                transparent 75%);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* ===== RESULTS ===== */
        .result-container {
            background: var(--dark-bg);
            border-radius: 16px;
            padding: 30px;
            margin-top: 30px;
            border: 1px solid var(--dark-border);
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .result-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--dark-border);
        }

        .result-title {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.3rem;
            font-weight: 700;
            color: var(--text-primary);
        }

        .result-title i {
            color: var(--primary);
        }

        .result-status {
            padding: 6px 16px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .status-safe {
            background: rgba(16, 185, 129, 0.2);
            color: var(--secondary);
            border: 1px solid rgba(16, 185, 129, 0.3);
        }

        .status-warning {
            background: rgba(245, 158, 11, 0.2);
            color: var(--warning);
            border: 1px solid rgba(245, 158, 11, 0.3);
        }

        .status-danger {
            background: rgba(239, 68, 68, 0.2);
            color: var(--danger);
            border: 1px solid rgba(239, 68, 68, 0.3);
        }

        .result-content {
            background: rgba(15, 23, 42, 0.5);
            border-radius: 12px;
            padding: 25px;
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            font-size: 0.95rem;
            line-height: 1.7;
            color: var(--text-secondary);
            max-height: 400px;
            overflow-y: auto;
            border: 1px solid var(--dark-border);
        }

        .result-content pre {
            white-space: pre-wrap;
            word-break: break-word;
        }

        .result-item {
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(148, 163, 184, 0.1);
        }

        .result-item:last-child {
            margin-bottom: 0;
            border-bottom: none;
        }

        .result-label {
            color: var(--text-primary);
            font-weight: 600;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .result-label i {
            width: 20px;
            color: var(--primary);
        }

        .result-value {
            color: var(--text-secondary);
            margin-left: 28px;
        }

        /* ===== LOADER ===== */
        .loader {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .spinner {
            width: 60px;
            height: 60px;
            border: 4px solid var(--dark-border);
            border-top: 4px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }

        .loader-text {
            color: var(--text-secondary);
            font-size: 1rem;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* ===== STATS GRID ===== */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 25px;
            margin: 40px 0;
        }

        .stat-card {
            background: var(--dark-card);
            border-radius: 16px;
            padding: 30px;
            border: 1px solid var(--dark-border);
            text-align: center;
            transition: all 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            border-color: var(--primary);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }

        .stat-icon {
            font-size: 2.5rem;
            margin-bottom: 20px;
            color: var(--primary);
        }

        .stat-number {
            font-size: 2.5rem;
            font-weight: 800;
            margin-bottom: 10px;
            background: var(--gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .stat-label {
            color: var(--text-secondary);
            font-size: 1rem;
        }

        /* ===== FOOTER ===== */
        .main-footer {
            margin-top: 60px;
            padding: 40px 0;
            border-top: 1px solid var(--dark-border);
            text-align: center;
        }

        .footer-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }

        .footer-logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.5rem;
            font-weight: 700;
            background: var(--gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .footer-text {
            color: var(--text-secondary);
            font-size: 0.95rem;
            max-width: 600px;
            line-height: 1.7;
        }

        .footer-links {
            display: flex;
            gap: 20px;
            margin-top: 10px;
        }

        .footer-link {
            color: var(--text-secondary);
            text-decoration: none;
            transition: color 0.3s ease;
            font-size: 0.9rem;
        }

        .footer-link:hover {
            color: var(--primary);
        }

        /* ===== RESPONSIVE DESIGN ===== */
        @media (max-width: 992px) {
            .container {
                padding: 0 15px;
            }
            
            .logo-text {
                font-size: 2.2rem;
            }
            
            .tab-content {
                padding: 30px 20px;
            }
            
            .feature-card {
                padding: 30px;
            }
        }

        @media (max-width: 768px) {
            .logo {
                flex-direction: column;
                gap: 10px;
            }
            
            .logo-text {
                font-size: 2rem;
            }
            
            .tabs-header {
                flex-wrap: wrap;
            }
            
            .tab-btn {
                flex: 1;
                min-width: 140px;
                justify-content: center;
                padding: 18px 20px;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .footer-links {
                flex-wrap: wrap;
                justify-content: center;
            }
        }

        @media (max-width: 480px) {
            .main-header {
                padding: 30px 0 20px;
            }
            
            .logo-text {
                font-size: 1.8rem;
            }
            
            .tagline {
                font-size: 1rem;
            }
            
            .tab-btn {
                min-width: 120px;
                padding: 15px;
                font-size: 0.9rem;
            }
            
            .feature-card {
                padding: 25px 20px;
            }
            
            .btn {
                padding: 14px 24px;
                font-size: 0.95rem;
            }
        }

        /* ===== UTILITY CLASSES ===== */
        .text-gradient {
            background: var(--gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .glow {
            filter: drop-shadow(0 0 15px rgba(99, 102, 241, 0.3));
        }

        .pulse {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        .fade-in {
            animation: fadeIn 0.8s ease;
        }
    </style>
</head>
<body>
    <!-- Main Container -->
    <div class="container">
        <!-- Header -->
        <header class="main-header">
            <div class="header-content">
                <div class="logo">
                    <i class="fas fa-shield-alt logo-icon"></i>
                    <h1 class="logo-text">SecurityCheck</h1>
                </div>
                <p class="tagline">
                    Комплексная проверка безопасности в реальном времени. 
                    Анализируйте IP-адрес, анонимность, файлы и безопасность системы 
                    с помощью передовых технологий.
                </p>
                
                <!-- Stats -->
                <div class="stats-grid">
                    <div class="stat-card">
                        <i class="fas fa-bolt stat-icon"></i>
                        <div class="stat-number">0.5с</div>
                        <div class="stat-label">Среднее время проверки</div>
                    </div>
                    <div class="stat-card">
                        <i class="fas fa-shield-check stat-icon"></i>
                        <div class="stat-number">99.8%</div>
                        <div class="stat-label">Точность обнаружения</div>
                    </div>
                    <div class="stat-card">
                        <i class="fas fa-user-friends stat-icon"></i>
                        <div class="stat-number">50K+</div>
                        <div class="stat-label">Проверок ежедневно</div>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <main>
            <!-- Tabs -->
            <div class="tabs-container">
                <!-- Tab Headers -->
                <div class="tabs-header">
                    <button class="tab-btn active" data-tab="ip-check">
                        <i class="fas fa-map-marker-alt"></i>
                        Проверка IP
                    </button>
                    <button class="tab-btn" data-tab="anonymity">
                        <i class="fas fa-user-secret"></i>
                        Анонимность
                    </button>
                    <button class="tab-btn" data-tab="file-check">
                        <i class="fas fa-shield-virus"></i>
                        Проверка файлов
                    </button>
                    <button class="tab-btn" data-tab="security">
                        <i class="fas fa-laptop-medical"></i>
                        Безопасность
                    </button>
                </div>

                <!-- Tab Contents -->
                <!-- IP Check Tab -->
                <div id="ip-check" class="tab-content active">
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-map-marker-alt"></i>
                        </div>
                        <h3>Детальный анализ IP-адреса</h3>
                        <p>
                            Получите полную информацию о вашем IP-адресе: местоположение, провайдер, 
                            тип подключения и возможные угрозы. Проверьте, не используете ли вы 
                            публичный прокси или VPN.
                        </p>
                        <button class="btn" id="check-ip-btn">
                            <i class="fas fa-search"></i>
                            Запустить проверку IP
                        </button>
                        
                        <!-- Results will be inserted here -->
                        <div id="ip-results-container"></div>
                    </div>
                </div>

                <!-- Anonymity Check Tab -->
                <div id="anonymity" class="tab-content">
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-user-secret"></i>
                        </div>
                        <h3>Проверка анонимности в сети</h3>
                        <p>
                            Узнайте, насколько вы защищены от отслеживания. Проверьте наличие 
                            цифровых отпечатков, активных трекеров и оцените уровень вашей 
                            приватности в интернете.
                        </p>
                        <button class="btn" id="check-anonymity-btn">
                            <i class="fas fa-search"></i>
                            Проверить анонимность
                        </button>
                        
                        <!-- Results will be inserted here -->
                        <div id="anonymity-results-container"></div>
                    </div>
                </div>

                <!-- File Check Tab -->
                <div id="file-check" class="tab-content">
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-shield-virus"></i>
                        </div>
                        <h3>Антивирусная проверка файлов</h3>
                        <p>
                            Загрузите файл для проверки на вирусы и вредоносное ПО. 
                            Используем 68 антивирусных движков для максимальной точности. 
                            Максимальный размер файла: 100 МБ.
                        </p>
                        
                        <!-- File Upload Area -->
                        <div class="file-upload-area" id="file-drop-area">
                            <i class="fas fa-cloud-upload-alt file-upload-icon"></i>
                            <div class="file-upload-text">
                                Перетащите файл сюда или нажмите для выбора
                            </div>
                            <div class="file-upload-hint">
                                Поддерживаются все типы файлов до 100 МБ
                            </div>
                            <input type="file" id="file-input" hidden>
                        </div>
                        
                        <!-- File Preview -->
                        <div class="file-preview" id="file-preview"></div>
                        
                        <!-- Progress Bar -->
                        <div class="progress-container" id="progress-container">
                            <div class="progress-header">
                                <span>Сканирование...</span>
                                <span id="progress-percent">0%</span>
                            </div>
                            <div class="progress-bar">
                                <div class="progress-fill" id="progress-fill"></div>
                            </div>
                        </div>
                        
                        <button class="btn" id="scan-file-btn" disabled>
                            <i class="fas fa-virus"></i>
                            Начать сканирование файла
                        </button>
                        
                        <!-- Results will be inserted here -->
                        <div id="file-results-container"></div>
                    </div>
                </div>

                <!-- Security Check Tab -->
                <div id="security" class="tab-content">
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-laptop-medical"></i>
                        </div>
                        <h3>Комплексная проверка безопасности</h3>
                        <p>
                            Проведите глубокий анализ безопасности вашей системы. 
                            Проверьте настройки браузера, обновления системы, 
                            наличие уязвимостей и рекомендации по защите.
                        </p>
                        <button class="btn" id="check-security-btn">
                            <i class="fas fa-shield-alt"></i>
                            Запустить проверку безопасности
                        </button>
                        
                        <!-- Results will be inserted here -->
                        <div id="security-results-container"></div>
                    </div>
                </div>
            </div>
        </main>

        <!-- Footer -->
        <footer class="main-footer">
            <div class="footer-content">
                <div class="footer-logo">
                    <i class="fas fa-shield-alt"></i>
                    SecurityCheck
                </div>
                <p class="footer-text">
                    SecurityCheck — это инструмент для проверки безопасности, созданный для защиты 
                    вашей приватности в интернете. Все проверки выполняются анонимно и безопасно.
                </p>
                <div class="footer-links">
                    <a href="#" class="footer-link">Политика конфиденциальности</a>
                    <a href="#" class="footer-link">Условия использования</a>
                    <a href="#" class="footer-link">Контакты</a>
                </div>
                <p class="footer-text">
                    &copy; 2024 SecurityCheck. Все права защищены.<br>
                    Информация предоставляется исключительно в ознакомительных целях.
                </p>
            </div>
        </footer>
    </div>

    <!-- JavaScript -->
    <script>
        // DOM Elements
        const tabBtns = document.querySelectorAll('.tab-btn');
        const tabContents = document.querySelectorAll('.tab-content');
        const fileDropArea = document.getElementById('file-drop-area');
        const fileInput = document.getElementById('file-input');
        const filePreview = document.getElementById('file-preview');
        const scanFileBtn = document.getElementById('scan-file-btn');
        const progressContainer = document.getElementById('progress-container');
        const progressFill = document.getElementById('progress-fill');
        const progressPercent = document.getElementById('progress-percent');

        // State
        let currentFile = null;
        let isScanning = false;

        // ===== TAB FUNCTIONALITY =====
        tabBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                const tabId = btn.getAttribute('data-tab');
                
                // Update active tab button
                tabBtns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                
                // Show active tab content
                tabContents.forEach(content => {
                    content.classList.remove('active');
                    if (content.id === tabId) {
                        content.classList.add('active');
                    }
                });
            });
        });

        // ===== IP CHECK FUNCTIONALITY =====
        document.getElementById('check-ip-btn').addEventListener('click', async () => {
            const container = document.getElementById('ip-results-container');
            showLoader(container, 'Анализируем IP-адрес...');
            
            try {
                // Get IP address
                const ipResponse = await fetch('https://api.ipify.org?format=json');
                const ipData = await ipResponse.json();
                
                // Simulate geolocation data (in production, use a proper API)
                const locationData = await simulateGeolocation(ipData.ip);
                
                // Render results
                renderIPResults(container, {
                    ip: ipData.ip,
                    ...locationData,
                    timestamp: new Date().toLocaleString('ru-RU')
                });
                
            } catch (error) {
                showError(container, 'Ошибка при получении данных об IP');
            }
        });

        // ===== ANONYMITY CHECK FUNCTIONALITY =====
        document.getElementById('check-anonymity-btn').addEventListener('click', () => {
            const container = document.getElementById('anonymity-results-container');
            showLoader(container, 'Проверяем анонимность...');
            
            // Simulate check with delay
            setTimeout(() => {
                const anonymityData = checkAnonymity();
                renderAnonymityResults(container, anonymityData);
            }, 1500);
        });

        // ===== FILE UPLOAD FUNCTIONALITY =====
        fileDropArea.addEventListener('click', () => fileInput.click());
        
        fileDropArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            fileDropArea.classList.add('dragover');
        });
        
        fileDropArea.addEventListener('dragleave', () => {
            fileDropArea.classList.remove('dragover');
        });
        
        fileDropArea.addEventListener('drop', (e) => {
            e.preventDefault();
            fileDropArea.classList.remove('dragover');
            
            if (e.dataTransfer.files.length) {
                handleFileSelect(e.dataTransfer.files[0]);
            }
        });
        
        fileInput.addEventListener('change', (e) => {
            if (e.target.files.length) {
                handleFileSelect(e.target.files[0]);
            }
        });
        
        function handleFileSelect(file) {
            // Validate file size (100MB max)
            if (file.size > 100 * 1024 * 1024) {
                alert('Файл слишком большой. Максимальный размер: 100 МБ');
                return;
            }
            
            currentFile = file;
            scanFileBtn.disabled = false;
            
            // Show file preview
            const fileSize = (file.size / 1024 / 1024).toFixed(2);
            filePreview.innerHTML = `
                <div class="file-info">
                    <i class="fas fa-file-alt file-icon"></i>
                    <div class="file-details">
                        <div class="file-name">${file.name}</div>
                        <div class="file-size">${fileSize} МБ</div>
                    </div>
                    <button class="file-remove" onclick="removeFile()">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
            `;
            filePreview.style.display = 'block';
        }
        
        window.removeFile = function() {
            currentFile = null;
            scanFileBtn.disabled = true;
            filePreview.style.display = 'none';
            fileInput.value = '';
        };

        // ===== FILE SCAN FUNCTIONALITY =====
        scanFileBtn.addEventListener('click', async () => {
            if (!currentFile || isScanning) return;
            
            isScanning = true;
            scanFileBtn.disabled = true;
            const container = document.getElementById('file-results-container');
            
            // Show progress
            progressContainer.style.display = 'block';
            progressFill.style.width = '0%';
            progressPercent.textContent = '0%';
            
            // Simulate scanning process
            simulateScan(currentFile, container);
        });

        function simulateScan(file, container) {
            let progress = 0;
            const steps = [
                'Подготовка к сканированию...',
                'Анализ структуры файла...',
                'Проверка сигнатур...',
                'Эвристический анализ...',
                'Проверка облачными антивирусами...',
                'Финальный анализ...'
            ];
            
            const interval = setInterval(() => {
                progress += Math.random() * 20;
                if (progress > 100) progress = 100;
                
                progressFill.style.width = `${progress}%`;
                progressPercent.textContent = `${Math.floor(progress)}%`;
                
                // Update status message
                const stepIndex = Math.floor(progress / (100 / steps.length));
                if (stepIndex < steps.length) {
                    showLoader(container, steps[stepIndex]);
                }
                
                if (progress >= 100) {
                    clearInterval(interval);
                    
                    // Simulate scan results
                    setTimeout(() => {
                        isScanning = false;
                        progressContainer.style.display = 'none';
                        const scanResults = simulateScanResults(file);
                        renderFileResults(container, scanResults);
                        scanFileBtn.disabled = false;
                    }, 500);
                }
            }, 300);
        }

        // ===== SECURITY CHECK FUNCTIONALITY =====
        document.getElementById('check-security-btn').addEventListener('click', () => {
            const container = document.getElementById('security-results-container');
            showLoader(container, 'Анализируем систему...');
            
            // Simulate check with delay
            setTimeout(() => {
                const securityData = checkSecurity();
                renderSecurityResults(container, securityData);
            }, 2000);
        });

       
