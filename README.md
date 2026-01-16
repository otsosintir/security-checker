<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SecurityCheck - Проверка безопасности онлайн</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #0f172a;
            color: #e2e8f0;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            text-align: center;
            padding: 40px 0 30px;
            border-bottom: 1px solid #334155;
            margin-bottom: 40px;
        }

        header h1 {
            font-size: 2.8rem;
            color: #38bdf8;
            margin-bottom: 10px;
        }

        header p {
            font-size: 1.1rem;
            color: #94a3b8;
            max-width: 700px;
            margin: 0 auto;
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 50px;
        }

        .feature-card {
            background-color: #1e293b;
            border-radius: 12px;
            padding: 30px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s, box-shadow 0.3s;
            border: 1px solid #334155;
        }

        .feature-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.3);
        }

        .feature-icon {
            font-size: 2.5rem;
            color: #38bdf8;
            margin-bottom: 20px;
        }

        .feature-card h3 {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: #f1f5f9;
        }

        .feature-card p {
            color: #94a3b8;
            margin-bottom: 20px;
            min-height: 60px;
        }

        .btn {
            display: inline-block;
            background-color: #3b82f6;
            color: white;
            padding: 12px 24px;
            border-radius: 8px;
            text-decoration: none;
            font-weight: 600;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
            width: 100%;
            text-align: center;
        }

        .btn:hover {
            background-color: #2563eb;
        }

        .result-area {
            background-color: #1e293b;
            border-radius: 12px;
            padding: 25px;
            margin-top: 25px;
            display: none;
            border: 1px solid #334155;
        }

        .result-area h4 {
            color: #38bdf8;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }

        .result-content {
            background-color: #0f172a;
            padding: 20px;
            border-radius: 8px;
            font-family: monospace;
            white-space: pre-wrap;
            max-height: 300px;
            overflow-y: auto;
        }

        .file-upload {
            background-color: #1e293b;
            border: 2px dashed #475569;
            border-radius: 8px;
            padding: 40px 20px;
            text-align: center;
            margin-bottom: 20px;
            cursor: pointer;
        }

        .file-upload:hover {
            border-color: #3b82f6;
        }

        .file-upload i {
            font-size: 3rem;
            color: #475569;
            margin-bottom: 15px;
        }

        .progress-bar {
            height: 8px;
            background-color: #334155;
            border-radius: 4px;
            margin-top: 15px;
            overflow: hidden;
            display: none;
        }

        .progress-fill {
            height: 100%;
            background-color: #10b981;
            width: 0%;
            transition: width 0.5s;
        }

        .status-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
        }

        .safe {
            background-color: #10b981;
        }

        .warning {
            background-color: #f59e0b;
        }

        .danger {
            background-color: #ef4444;
        }

        footer {
            text-align: center;
            padding: 30px 0;
            margin-top: 50px;
            border-top: 1px solid #334155;
            color: #94a3b8;
            font-size: 0.9rem;
        }

        .loader {
            display: none;
            border: 3px solid #334155;
            border-top: 3px solid #38bdf8;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .tab-container {
            margin-bottom: 30px;
        }

        .tabs {
            display: flex;
            border-bottom: 1px solid #334155;
            margin-bottom: 20px;
        }

        .tab {
            padding: 12px 24px;
            cursor: pointer;
            background: none;
            border: none;
            color: #94a3b8;
            font-weight: 600;
            font-size: 1rem;
            border-bottom: 3px solid transparent;
        }

        .tab.active {
            color: #38bdf8;
            border-bottom: 3px solid #38bdf8;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .info-note {
            background-color: rgba(59, 130, 246, 0.1);
            border-left: 4px solid #3b82f6;
            padding: 15px;
            margin: 20px 0;
            border-radius: 0 8px 8px 0;
            font-size: 0.95rem;
        }

        @media (max-width: 768px) {
            .features-grid {
                grid-template-columns: 1fr;
            }
            
            header h1 {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>SecurityCheck</h1>
            <p>Многофункциональный инструмент для проверки безопасности в интернете. Проверьте свой IP-адрес, анонимность, загруженные файлы и безопасность системы.</p>
        </header>

        <div class="tab-container">
            <div class="tabs">
                <button class="tab active" data-tab="ip-check">Проверка IP</button>
                <button class="tab" data-tab="anonymity">Анонимность</button>
                <button class="tab" data-tab="file-check">Проверка файлов</button>
                <button class="tab" data-tab="security">Безопасность системы</button>
            </div>

            <!-- Вкладка проверки IP -->
            <div id="ip-check" class="tab-content active">
                <div class="feature-card">
                    <div class="feature-icon">
                        <i class="fas fa-map-marker-alt"></i>
                    </div>
                    <h3>Проверка вашего IP-адреса</h3>
                    <p>Узнайте ваш текущий IP-адрес, местоположение, интернет-провайдера и другую информацию о вашем сетевом соединении.</p>
                    <button class="btn" id="check-ip-btn">Проверить IP-адрес</button>
                    
                    <div class="result-area" id="ip-result">
                        <h4>Результат проверки IP</h4>
                        <div class="result-content" id="ip-result-content">
                            <!-- Здесь будут отображаться результаты -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Вкладка проверки анонимности -->
            <div id="anonymity" class="tab-content">
                <div class="feature-card">
                    <div class="feature-icon">
                        <i class="fas fa-user-secret"></i>
                    </div>
                    <h3>Проверка вашей анонимности в сети</h3>
                    <p>Проверьте, насколько вы анонимны в интернете. Узнайте, какие данные о вас видят сайты и рекламные сети.</p>
                    <button class="btn" id="check-anonymity-btn">Проверить анонимность</button>
                    
                    <div class="result-area" id="anonymity-result">
                        <h4>Результат проверки анонимности</h4>
                        <div class="result-content" id="anonymity-result-content">
                            <!-- Здесь будут отображаться результаты -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Вкладка проверки файлов -->
            <div id="file-check" class="tab-content">
                <div class="feature-card">
                    <div class="feature-icon">
                        <i class="fas fa-shield-alt"></i>
                    </div>
                    <h3>Проверка файла на вирусы</h3>
                    <p>Загрузите файл для проверки на наличие вирусов и вредоносного кода. Максимальный размер файла: 50 МБ.</p>
                    
                    <div class="file-upload" id="file-drop-area">
                        <i class="fas fa-cloud-upload-alt"></i>
                        <p>Перетащите файл сюда или нажмите для выбора</p>
                        <input type="file" id="file-input" style="display: none;">
                    </div>
                    
                    <div class="progress-bar" id="progress-bar">
                        <div class="progress-fill" id="progress-fill"></div>
                    </div>
                    
                    <button class="btn" id="scan-file-btn" disabled>Сканировать файл</button>
                    
                    <div class="result-area" id="file-result">
                        <h4>Результат проверки файла</h4>
                        <div class="result-content" id="file-result-content">
                            <!-- Здесь будут отображаться результаты -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Вкладка проверки безопасности -->
            <div id="security" class="tab-content">
                <div class="feature-card">
                    <div class="feature-icon">
                        <i class="fas fa-laptop-code"></i>
                    </div>
                    <h3>Проверка безопасности вашего компьютера</h3>
                    <p>Проверьте настройки безопасности вашего браузера и системы на наличие уязвимостей.</p>
                    <button class="btn" id="check-security-btn">Проверить безопасность</button>
                    
                    <div class="result-area" id="security-result">
                        <h4>Результат проверки безопасности</h4>
                        <div class="result-content" id="security-result-content">
                            <!-- Здесь будут отображаться результаты -->
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="info-note">
            <p><strong>Примечание:</strong> Все проверки выполняются на стороне клиента. Мы не храним и не передаем третьим лицам ваши данные. Проверка файлов использует эмуляцию антивирусного сканирования, для реальной проверки на вирусы рекомендуется использовать специализированные антивирусные программы.</p>
        </div>

        <div class="features-grid">
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-bolt"></i>
                </div>
                <h3>Быстрые проверки</h3>
                <p>Все проверки выполняются быстро без необходимости установки дополнительного ПО.</p>
            </div>
            
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-lock"></i>
                </div>
                <h3>Конфиденциальность</h3>
                <p>Мы не храним ваши данные и файлы. Все проверки выполняются локально в вашем браузере.</p>
            </div>
            
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-mobile-alt"></i>
                </div>
                <h3>Адаптивный дизайн</h3>
                <p>Сайт работает на любых устройствах: компьютерах, планшетах и смартфонах.</p>
            </div>
        </div>

        <footer>
            <p>SecurityCheck &copy; 2023 | Инструменты для проверки безопасности в интернете</p>
            <p>Данный сайт является демонстрационным аналогом ip2.com</p>
        </footer>
    </div>

    <script>
        // Элементы DOM
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');
        const checkIpBtn = document.getElementById('check-ip-btn');
        const checkAnonymityBtn = document.getElementById('check-anonymity-btn');
        const checkSecurityBtn = document.getElementById('check-security-btn');
        const scanFileBtn = document.getElementById('scan-file-btn');
        const fileInput = document.getElementById('file-input');
        const fileDropArea = document.getElementById('file-drop-area');
        const progressBar = document.getElementById('progress-bar');
        const progressFill = document.getElementById('progress-fill');

        // Текущий выбранный файл
        let selectedFile = null;

        // Переключение вкладок
        tabs.forEach(tab => {
            tab.addEventListener('click', () => {
                const tabId = tab.getAttribute('data-tab');
                
                // Убираем активный класс у всех вкладок и контента
                tabs.forEach(t => t.classList.remove('active'));
                tabContents.forEach(content => content.classList.remove('active'));
                
                // Добавляем активный класс выбранной вкладке и контенту
                tab.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });

        // Проверка IP-адреса
        checkIpBtn.addEventListener('click', async () => {
            const resultArea = document.getElementById('ip-result');
            const resultContent = document.getElementById('ip-result-content');
            
            // Показываем индикатор загрузки
            resultContent.innerHTML = '<div class="loader"></div>Идет получение информации об IP...';
            resultArea.style.display = 'block';
            
            try {
                // Получаем информацию об IP через внешний API
                const response = await fetch('https://api.ipify.org?format=json');
                const ipData = await response.json();
                const ip = ipData.ip;
                
                // Получаем дополнительную информацию об IP (симуляция, т.к. CORS блокирует многие API)
                // В реальном приложении нужно использовать свой серверный прокси или API с поддержкой CORS
                const locationInfo = await simulateIPLocation(ip);
                
                // Форматируем результат
                resultContent.innerHTML = `
IP-адрес: <strong>${ip}</strong>

Местоположение:
  Город: ${locationInfo.city}
  Регион: ${locationInfo.region}
  Страна: ${locationInfo.country}
  Часовой пояс: ${locationInfo.timezone}

Провайдер: ${locationInfo.isp}
Тип подключения: ${locationInfo.connectionType}

Ваш IP является ${locationInfo.type === 'public' ? 'публичным' : 'приватным'}.
${locationInfo.proxy || locationInfo.vpn ? '⚠ Обнаружены признаки использования VPN/прокси' : '✓ Прямое подключение'}
                `;
                
            } catch (error) {
                resultContent.innerHTML = `Ошибка при получении данных: ${error.message}\n\nИнформация о вашем IP (симуляция):\n\nIP-адрес: 192.168.1.1 (пример)\nМестоположение: Не определено\nПровайдер: Домашняя сеть`;
            }
        });

        // Проверка анонимности
        checkAnonymityBtn.addEventListener('click', () => {
            const resultArea = document.getElementById('anonymity-result');
            const resultContent = document.getElementById('anonymity-result-content');
            
            // Показываем индикатор загрузки
            resultContent.innerHTML = '<div class="loader"></div>Проверяем вашу анонимность...';
            resultArea.style.display = 'block';
            
            // Симуляция проверки анонимности
            setTimeout(() => {
                const anonymityScore = Math.floor(Math.random() * 100);
                let status, statusClass, statusIcon;
                
                if (anonymityScore > 70) {
                    status = "Высокая анонимность";
                    statusClass = "safe";
                    statusIcon = "✓";
                } else if (anonymityScore > 40) {
                    status = "Средняя анонимность";
                    statusClass = "warning";
                    statusIcon = "⚠";
                } else {
                    status = "Низкая анонимность";
                    statusClass = "danger";
                    statusIcon = "✗";
                }
                
                resultContent.innerHTML = `
Оценка анонимности: <strong>${anonymityScore}%</strong>
Статус: <span class="status-indicator ${statusClass}"></span>${statusIcon} ${status}

Детальная информация:
✓ Разрешение экрана: ${screen.width} × ${screen.height}
✓ Язык браузера: ${navigator.language}
✓ Включены cookies: Да
✓ JavaScript включен: Да
✓ Трекеры: Обнаружено ${Math.floor(Math.random() * 5)} трекеров
✓ Фингерпринтинг: ${anonymityScore > 60 ? "Затруднен" : "Возможен"}
${navigator.doNotTrack ? "✓ Do Not Track включен" : "✗ Do Not Track не включен"}

Рекомендации:
${anonymityScore > 70 ? "• Ваша анонимность на хорошем уровне" : "• Используйте VPN для повышения анонимности"}
${navigator.doNotTrack ? "" : "• Включите опцию Do Not Track в настройках браузера"}
• Используйте расширения для блокировки трекеров (uBlock Origin, Privacy Badger)
                `;
            }, 1500);
        });

        // Проверка безопасности системы
        checkSecurityBtn.addEventListener('click', () => {
            const resultArea = document.getElementById('security-result');
            const resultContent = document.getElementById('security-result-content');
            
            // Показываем индикатор загрузки
            resultContent.innerHTML = '<div class="loader"></div>Проверяем настройки безопасности...';
            resultArea.style.display = 'block';
            
            // Симуляция проверки безопасности
            setTimeout(() => {
                const securityChecks = [
                    { name: "HTTPS соединение", result: window.location.protocol === "https:", important: true },
                    { name: "Защита от фишинга", result: Math.random() > 0.3, important: true },
                    { name: "Защита от вредоносных сайтов", result: Math.random() > 0.2, important: true },
                    { name: "Блокировка небезопасного контента", result: Math.random() > 0.4, important: false },
                    { name: "Обновления системы", result: Math.random() > 0.1, important: true },
                    { name: "Защита от трекеров", result: Math.random() > 0.5, important: false },
                    { name: "Блокировка всплывающих окон", result: Math.random() > 0.6, important: false },
                    { name: "Безопасные пароли", result: Math.random() > 0.7, important: true }
                ];
                
                let passedChecks = securityChecks.filter(check => check.result).length;
                let totalChecks = securityChecks.length;
                let securityScore = Math.round((passedChecks / totalChecks) * 100);
                
                let status, statusClass, statusIcon;
                
                if (securityScore > 80) {
                    status = "Высокий уровень безопасности";
                    statusClass = "safe";
                    statusIcon = "✓";
                } else if (securityScore > 50) {
                    status = "Средний уровень безопасности";
                    statusClass = "warning";
                    statusIcon = "⚠";
                } else {
                    status = "Низкий уровень безопасности";
                    statusClass = "danger";
                    statusIcon = "✗";
                }
                
                let checksList = "Результаты проверки:\n\n";
                securityChecks.forEach(check => {
                    const icon = check.result ? "✓" : "✗";
                    const importance = check.important ? " (важно)" : "";
                    checksList += `${icon} ${check.name}${importance}\n`;
                });
                
                resultContent.innerHTML = `
Общая оценка безопасности: <strong>${securityScore}%</strong>
Статус: <span class="status-indicator ${statusClass}"></span>${statusIcon} ${status}

${checksList}

Критические проблемы:
${securityChecks.filter(c => !c.result && c.important).length > 0 
    ? securityChecks.filter(c => !c.result && c.important).map(c => `• ${c.name}`).join('\n')
    : '✓ Критических проблем не обнаружено'}

Рекомендации:
• Обновляйте операционную систему и браузер
• Используйте антивирусное ПО
• Включайте двухфакторную аутентификацию
• Регулярно меняйте пароли
                `;
            }, 2000);
        });

        // Работа с загрузкой файлов
        fileDropArea.addEventListener('click', () => {
            fileInput.click();
        });

        fileDropArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            fileDropArea.style.borderColor = '#3b82f6';
            fileDropArea.style.backgroundColor = 'rgba(59, 130, 246, 0.1)';
        });

        fileDropArea.addEventListener('dragleave', () => {
            fileDropArea.style.borderColor = '#475569';
            fileDropArea.style.backgroundColor = '#1e293b';
        });

        fileDropArea.addEventListener('drop', (e) => {
            e.preventDefault();
            fileDropArea.style.borderColor = '#475569';
            fileDropArea.style.backgroundColor = '#1e293b';
            
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
            // Проверка размера файла (макс. 50 МБ)
            if (file.size > 50 * 1024 * 1024) {
                alert('Файл слишком большой. Максимальный размер: 50 МБ');
                return;
            }
            
            selectedFile = file;
            scanFileBtn.disabled = false;
            fileDropArea.innerHTML = `
                <i class="fas fa-file-alt"></i>
                <p><strong>${file.name}</strong></p>
                <p>${(file.size / 1024 / 1024).toFixed(2)} МБ</p>
                <p>Нажмите "Сканировать файл" для проверки</p>
            `;
            
            // Сбрасываем предыдущие результаты
            document.getElementById('file-result').style.display = 'none';
        }

        // Сканирование файла
        scanFileBtn.addEventListener('click', () => {
            if (!selectedFile) return;
            
            const resultArea = document.getElementById('file-result');
            const resultContent = document.getElementById('file-result-content');
            
            // Показываем прогресс бар
            progressBar.style.display = 'block';
            progressFill.style.width = '0%';
            resultArea.style.display = 'block';
            resultContent.innerHTML = 'Подготовка к сканированию...';
            
            // Симуляция процесса сканирования
            let progress = 0;
            const scanInterval = setInterval(() => {
                progress += Math.random() * 15;
                if (progress > 100) progress = 100;
                progressFill.style.width = `${progress}%`;
                
                if (progress < 30) {
                    resultContent.innerHTML = `Сканирование... ${Math.floor(progress)}%\nАнализ структуры файла...`;
                } else if (progress < 60) {
                    resultContent.innerHTML = `Сканирование... ${Math.floor(progress)}%\nПроверка на наличие известных сигнатур...`;
                } else if (progress < 90) {
                    resultContent.innerHTML = `Сканирование... ${Math.floor(progress)}%\nЭвристический анализ...`;
                } else if (progress >= 100) {
                    clearInterval(scanInterval);
                    simulateFileScanResult(selectedFile, resultContent);
                }
            }, 300);
        });

        // Функция симуляции результата сканирования файла
        function simulateFileScanResult(file, resultContent) {
            const fileExt = file.name.split('.').pop().toLowerCase();
            const fileSizeMB = file.size / 1024 / 1024;
            const fileName = file.name;
            
            // Определяем, будет ли файл "заражен" (для демонстрации)
            // В реальном приложении здесь должно быть обращение к антивирусному API
            const isInfected = Math.random() > 0.85;
            const threatName = isInfected ? getRandomThreatName() : null;
            const scanTime = (Math.random() * 5 + 1).toFixed(2);
            
            if (isInfected) {
                resultContent.innerHTML = `
РЕЗУЛЬТАТ СКАНИРОВАНИЯ: <span style="color:#ef4444">ОБНАРУЖЕНА УГРОЗА!</span>

Файл: ${fileName}
Размер: ${fileSizeMB.toFixed(2)} МБ
Тип: ${fileExt.toUpperCase()} файл

Обнаруженные угрозы:
✗ ${threatName}

Статус: <span class="status-indicator danger"></span>ОПАСНО
Время сканирования: ${scanTime} сек.

Рекомендуемые действия:
1. Немедленно удалите этот файл
2. Проверьте систему полноценным антивирусом
3. Не открывайте подозрительные файлы из неизвестных источников
                `;
            } else {
                resultContent.innerHTML = `
РЕЗУЛЬТАТ СКАНИРОВАНИЯ: <span style="color:#10b981">УГРОЗ НЕ ОБНАРУЖЕНО</span>

Файл: ${fileName}
Размер: ${fileSizeMB.toFixed(2)} МБ
Тип: ${fileExt.toUpperCase()} файл

Просканировано элементов: ${Math.floor(Math.random() * 5000) + 1000}
Обнаруженные угрозы: 0

Статус: <span class="status-indicator safe"></span>БЕЗОПАСНО
Время сканирования: ${scanTime} сек.

Рекомендации:
✓ Файл безопасен для использования
✓ Всегда проверяйте файлы из неизвестных источников
✓ Регулярно обновляйте антивирусные базы
                `;
            }
            
            // Скрываем прогресс бар после завершения
            setTimeout(() => {
                progressBar.style.display = 'none';
            }, 500);
        }

        // Вспомогательные функции
        function simulateIPLocation(ip) {
            // В реальном приложении здесь должен быть запрос к Geolocation API
            // Для демонстрации возвращаем фиктивные данные
            const cities = ['Москва', 'Санкт-Петербург', 'Новосибирск', 'Екатеринбург', 'Казань'];
            const isps = ['Ростелеком', 'МТС', 'Билайн', 'МегаФон', 'Дом.ru'];
            const connectionTypes = ['Кабельный', 'Оптоволокно', 'DSL', 'Мобильный', 'Спутниковый'];
            
            return {
                ip: ip,
                city: cities[Math.floor(Math.random() * cities.length)],
                region: 'Центральный федеральный округ',
                country: 'Россия',
                timezone: 'GMT+3',
                isp: isps[Math.floor(Math.random() * isps.length)],
                connectionType: connectionTypes[Math.floor(Math.random() * connectionTypes.length)],
                type: 'public',
                proxy: Math.random() > 0.7,
                vpn: Math.random() > 0.8
            };
        }

        function getRandomThreatName() {
            const threats = [
                "Trojan.Win32.Generic",
                "RiskWare.Downloader",
                "Exploit.JS.Agent",
                "Adware.Elex",
                "Ransomware.Crypmod",
                "Backdoor.Agent",
                "PUP.Optional.OpenCandy"
            ];
            return threats[Math.floor(Math.random() * threats.length)];
        }

        // Автоматическая проверка IP при загрузке страницы
        window.addEventListener('DOMContentLoaded', () => {
            // Небольшая задержка для лучшего UX
            setTimeout(() => {
                checkIpBtn.click();
            }, 500);
        });
    </script>
</body>
</html>
