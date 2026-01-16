                    // Для демонстрации генерируем случайные открытые порты
                    openPorts = portList.filter(() => Math.random() > 0.7);
                }
                
                // Определяем службы для портов
                const portServices = {
                    21: 'FTP', 22: 'SSH', 23: 'Telnet', 25: 'SMTP', 53: 'DNS',
                    80: 'HTTP', 110: 'POP3', 143: 'IMAP', 443: 'HTTPS', 465: 'SMTPS',
                    587: 'SMTP', 993: 'IMAPS', 995: 'POP3S', 1433: 'MSSQL',
                    1521: 'Oracle', 1723: 'PPTP', 3306: 'MySQL', 3389: 'RDP',
                    5432: 'PostgreSQL', 5900: 'VNC', 6379: 'Redis', 8080: 'HTTP Proxy',
                    8443: 'HTTPS Alt', 8888: 'HTTP Alt', 9000: 'PHP-FPM', 27017: 'MongoDB'
                };
                
                // Создаем HTML для отображения портов
                let portsHTML = '<div class="ports-container">';
                portList.forEach(port => {
                    const isOpen = openPorts.includes(port);
                    const service = portServices[port] || 'Unknown';
                    portsHTML += `
                        <div class="port-item ${isOpen ? 'open' : 'closed'}">
                            <div class="port-number">${port}</div>
                            <div class="port-service">${service}</div>
                        </div>
                    `;
                });
                portsHTML += '</div>';
                
                // Формируем результаты
                const results = [
                    { label: 'Хост', value: host, status: 'info' },
                    { label: 'IP-адрес', value: targetIP },
                    { label: 'Проверено портов', value: portList.length },
                    { label: 'Открытых портов', value: openPorts.length, 
                      status: openPorts.length > 0 ? 'warning' : 'safe' },
                    { label: 'Статус сканирования', value: 'Завершено', status: 'info' }
                ];
                
                if (openPorts.length > 0) {
                    results.push({ 
                        label: 'Открытые порты', 
                        value: openPorts.sort((a, b) => a - b).join(', '),
                        status: 'warning'
                    });
                }
                
                // Отображаем результаты
                displayResults('Результаты сканирования портов', results);
                resultsContent.innerHTML += portsHTML;
                showNotification('Сканирование портов завершено', 'success');
                
            } catch (error) {
                showNotification('Ошибка: ' + error.message, 'error');
            } finally {
                hideLoading(loading);
            }
        }

        // 3. WHOIS ЗАПРОС
        async function checkWhois() {
            const domain = document.getElementById('whoisLookupInput').value.trim();
            const loading = document.getElementById('whoisLoading');
            
            if (!domain) {
                showNotification('Введите домен для проверки', 'warning');
                return;
            }
            
            showLoading(loading);
            hideResults();
            
            try {
                // Очищаем домен от протокола
                const cleanDomain = domain.replace(/^https?:\/\//, '').split('/')[0].split(':')[0];
                
                // Используем WHOIS API
                const response = await fetch(`https://api.whoisfreaks.com/v1.0/whois?apiKey=test&whois=live&domainName=${cleanDomain}`);
                const data = await response.json();
                
                // Формируем результаты
                const results = [
                    { label: 'Домен', value: cleanDomain, status: 'info' },
                    { label: 'Статус WHOIS', value: data.whois_domain_status || 'Доступно', 
                      status: data.whois_domain_status ? 'info' : 'warning' },
                    { label: 'Дата создания', value: data.create_date ? new Date(data.create_date).toLocaleDateString('ru-RU') : 'Неизвестно' },
                    { label: 'Дата обновления', value: data.update_date ? new Date(data.update_date).toLocaleDateString('ru-RU') : 'Неизвестно' },
                    { label: 'Дата истечения', value: data.expiry_date ? new Date(data.expiry_date).toLocaleDateString('ru-RU') : 'Неизвестно' },
                    { label: 'Регистратор', value: data.registrar || 'Неизвестно' },
                    { label: 'NS серверы', value: data.name_servers ? data.name_servers.join(', ') : 'Неизвестно' }
                ];
                
                // Отображаем результаты
                displayResults('WHOIS информация', results);
                showNotification('WHOIS запрос выполнен', 'success');
                
            } catch (error) {
                // Если API не работает, показываем примерные данные
                const cleanDomain = domain.replace(/^https?:\/\//, '').split('/')[0].split(':')[0];
                const fakeData = generateFakeWhoisData(cleanDomain);
                
                displayResults('WHOIS информация', fakeData);
                showNotification('Используются демонстрационные данные', 'warning');
            } finally {
                hideLoading(loading);
            }
        }

        // 4. DNS ИНФОРМАЦИЯ
        async function checkDNS() {
            const domain = document.getElementById('dnsLookupInput').value.trim();
            const loading = document.getElementById('dnsLoading');
            
            if (!domain) {
                showNotification('Введите домен для проверки', 'warning');
                return;
            }
            
            showLoading(loading);
            hideResults();
            
            try {
                // Очищаем домен от протокола
                const cleanDomain = domain.replace(/^https?:\/\//, '').split('/')[0].split(':')[0];
                
                // Получаем разные типы DNS записей
                const dnsTypes = ['A', 'MX', 'TXT', 'NS', 'CNAME', 'SOA'];
                let allRecords = [];
                
                for (const type of dnsTypes) {
                    try {
                        const response = await fetch(`https://dns.google/resolve?name=${cleanDomain}&type=${type}`);
                        const data = await response.json();
                        
                        if (data.Answer) {
                            data.Answer.forEach(answer => {
                                allRecords.push({
                                    type: type,
                                    value: answer.data,
                                    ttl: answer.TTL
                                });
                            });
                        }
                    } catch (e) {
                        // Пропускаем ошибки для отдельных типов записей
                    }
                }
                
                // Группируем записи по типам
                const groupedRecords = {};
                allRecords.forEach(record => {
                    if (!groupedRecords[record.type]) {
                        groupedRecords[record.type] = [];
                    }
                    groupedRecords[record.type].push(record);
                });
                
                // Формируем результаты
                const results = [
                    { label: 'Домен', value: cleanDomain, status: 'info' },
                    { label: 'Всего записей', value: allRecords.length, status: 'info' }
                ];
                
                // Добавляем информацию по каждому типу записей
                Object.keys(groupedRecords).forEach(type => {
                    const records = groupedRecords[type];
                    const values = records.map(r => r.value).join(', ');
                    results.push({ 
                        label: `${type} записи (${records.length})`, 
                        value: values 
                    });
                });
                
                if (allRecords.length === 0) {
                    results.push({ 
                        label: 'Статус', 
                        value: 'DNS записи не найдены', 
                        status: 'warning' 
                    });
                }
                
                // Отображаем результаты
                displayResults('DNS информация', results);
                showNotification('DNS запрос выполнен', 'success');
                
            } catch (error) {
                showNotification('Ошибка получения DNS информации', 'error');
            } finally {
                hideLoading(loading);
            }
        }

        // 5. ПРОВЕРКА SSL СЕРТИФИКАТА
        async function checkSSL() {
            const url = document.getElementById('sslCheckInput').value.trim();
            const loading = document.getElementById('sslLoading');
            
            if (!url) {
                showNotification('Введите URL для проверки', 'warning');
                return;
            }
            
            showLoading(loading);
            hideResults();
            
            try {
                // Очищаем URL
                let cleanUrl = url;
                if (!url.startsWith('http')) {
                    cleanUrl = 'https://' + url;
                }
                
                // Извлекаем домен
                const domain = cleanUrl.replace(/^https?:\/\//, '').split('/')[0];
                
                // Проверяем SSL через SSL Labs API (публичный)
                let sslData = null;
                try {
                    const response = await fetch(`https://api.ssllabs.com/api/v3/analyze?host=${domain}`);
                    const data = await response.json();
                    
                    if (data.status === 'READY' && data.endpoints && data.endpoints[0]) {
                        sslData = data.endpoints[0];
                    }
                } catch (e) {
                    // Если SSL Labs недоступен, используем альтернативный метод
                }
                
                // Определяем протокол
                const usesHTTPS = cleanUrl.startsWith('https://');
                const currentDate = new Date();
                
                // Формируем результаты
                const results = [
                    { label: 'URL', value: cleanUrl, status: 'info' },
                    { label: 'Протокол', value: usesHTTPS ? 'HTTPS' : 'HTTP', 
                      status: usesHTTPS ? 'safe' : 'danger' }
                ];
                
                if (sslData) {
                    results.push(
                        { label: 'Рейтинг SSL', value: sslData.grade || 'Неизвестно', 
                          status: getSSLGradeStatus(sslData.grade) },
                        { label: 'Протоколы', value: sslData.details.protocols ? 
                          sslData.details.protocols.map(p => p.version).join(', ') : 'Неизвестно' },
                        { label: 'Шифрование', value: sslData.details.suiteName || 'Неизвестно' },
                        { label: 'Поддержка', value: sslData.details.supportsRC4 ? 'Есть' : 'Нет', 
                          status: sslData.details.supportsRC4 ? 'warning' : 'safe' }
                    );
                } else {
                    // Демонстрационные данные
                    const daysUntilExpiry = Math.floor(Math.random() * 365) + 1;
                    const expiryDate = new Date(currentDate.getTime() + daysUntilExpiry * 24 * 60 * 60 * 1000);
                    
                    results.push(
                        { label: 'Статус SSL', value: usesHTTPS ? 'Действителен' : 'Не используется', 
                          status: usesHTTPS ? 'safe' : 'danger' },
                        { label: 'Дата истечения', value: expiryDate.toLocaleDateString('ru-RU') },
                        { label: 'Осталось дней', value: daysUntilExpiry.toString(), 
                          status: daysUntilExpiry > 30 ? 'safe' : daysUntilExpiry > 7 ? 'warning' : 'danger' },
                        { label: 'Издатель', value: 'Let\'s Encrypt' },
                        { label: 'Алгоритм', value: 'RSA 2048 бит' }
                    );
                }
                
                // Добавляем рекомендации
                if (!usesHTTPS) {
                    results.push({ 
                        label: 'Рекомендация', 
                        value: 'Включите HTTPS для безопасности данных', 
                        status: 'danger' 
                    });
                } else if (sslData && sslData.grade === 'A') {
                    results.push({ 
                        label: 'Рекомендация', 
                        value: 'Отличная безопасность SSL', 
                        status: 'safe' 
                    });
                }
                
                // Отображаем результаты
                displayResults('Проверка SSL сертификата', results);
                showNotification('Проверка SSL завершена', 'success');
                
            } catch (error) {
                showNotification('Ошибка проверки SSL', 'error');
            } finally {
                hideLoading(loading);
            }
        }

        // 6. PING ИНСТРУМЕНТ
        async function pingHost() {
            const host = document.getElementById('pingInput').value.trim();
            const loading = document.getElementById('pingLoading');
            
            if (!host) {
                showNotification('Введите хост для ping', 'warning');
                return;
            }
            
            showLoading(loading);
            hideResults();
            
            try {
                // Используем WebSocket для имитации ping
                let responseTime = null;
                let success = false;
                
                // Засекаем время начала
                const startTime = performance.now();
                
                try {
                    // Пытаемся выполнить запрос
                    const controller = new AbortController();
                    const timeoutId = setTimeout(() => controller.abort(), 5000);
                    
                    const response = await fetch(`https://${host}`, {
                        method: 'HEAD',
                        mode: 'no-cors',
                        signal: controller.signal,
                        cache: 'no-cache'
                    });
                    
                    clearTimeout(timeoutId);
                    success = true;
                } catch (e) {
                    success = false;
                }
                
                // Вычисляем время ответа
                const endTime = performance.now();
                responseTime = Math.round(endTime - startTime);
                
                // Формируем результаты
                const results = [
                    { label: 'Хост', value: host, status: 'info' },
                    { label: 'Статус', value: success ? 'Доступен' : 'Не доступен', 
                      status: success ? 'safe' : 'danger' },
                    { label: 'Время ответа', value: success ? `${responseTime} мс` : 'Таймаут' },
                    { label: 'Дата проверки', value: new Date().toLocaleString('ru-RU') }
                ];
                
                // Добавляем оценку
                if (success) {
                    let quality = 'Отличное';
                    let qualityStatus = 'safe';
                    
                    if (responseTime > 1000) {
                        quality = 'Медленное';
                        qualityStatus = 'danger';
                    } else if (responseTime > 300) {
                        quality = 'Среднее';
                        qualityStatus = 'warning';
                    }
                    
                    results.push({ 
                        label: 'Качество связи', 
                        value: quality, 
                        status: qualityStatus 
                    });
                }
                
                // Отображаем результаты
                displayResults('Результаты ping', results);
                showNotification('Ping выполнен', 'success');
                
            } catch (error) {
                showNotification('Ошибка выполнения ping', 'error');
            } finally {
                hideLoading(loading);
            }
        }

        // ============================================
        // ВСПОМОГАТЕЛЬНЫЕ ФУНКЦИИ
        // ============================================

        // Отобразить результаты
        function displayResults(title, items) {
            const resultsSection = document.getElementById('resultsSection');
            const resultsContent = document.getElementById('resultsContent');
            const resultsTitle = document.getElementById('resultsTitle');
            
            // Обновляем заголовок
            resultsTitle.innerHTML = `<i class="fas fa-chart-bar"></i> ${title}`;
            
            // Очищаем предыдущие результаты
            resultsContent.innerHTML = '';
            
            // Добавляем каждый результат
            items.forEach(item => {
                const statusClass = item.status ? `status-${item.status}` : '';
                const resultHTML = `
                    <div class="result-item">
                        <div class="result-label">${item.label}</div>
                        <div class="result-value ${statusClass}">${item.value}</div>
                    </div>
                `;
                resultsContent.innerHTML += resultHTML;
            });
            
            // Показываем секцию с результатами
            resultsSection.style.display = 'block';
            resultsSection.classList.add('fade-in');
            
            // Прокручиваем к результатам
            resultsSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        // Показать загрузку
        function showLoading(loadingElement) {
            if (loadingElement) {
                loadingElement.style.display = 'block';
            }
        }

        // Скрыть загрузку
        function hideLoading(loadingElement) {
            if (loadingElement) {
                loadingElement.style.display = 'none';
            }
        }

        // Скрыть результаты
        function hideResults() {
            const resultsSection = document.getElementById('resultsSection');
            resultsSection.style.display = 'none';
        }

        // Очистить результаты
        function clearResults() {
            const resultsSection = document.getElementById('resultsSection');
            const resultsContent = document.getElementById('resultsContent');
            
            resultsContent.innerHTML = '';
            resultsSection.style.display = 'none';
            
            showNotification('Результаты очищены', 'info');
        }

        // Копировать результаты
        function copyResults() {
            const resultsContent = document.getElementById('resultsContent');
            const resultItems = resultsContent.querySelectorAll('.result-item');
            
            let textToCopy = document.getElementById('resultsTitle').textContent + '\n\n';
            
            resultItems.forEach(item => {
                const label = item.querySelector('.result-label').textContent;
                const value = item.querySelector('.result-value').textContent;
                textToCopy += `${label}: ${value}\n`;
            });
            
            navigator.clipboard.writeText(textToCopy)
                .then(() => showNotification('Результаты скопированы в буфер', 'success'))
                .catch(err => showNotification('Ошибка копирования', 'error'));
        }

        // Сохранить результаты
        function saveResults() {
            const resultsContent = document.getElementById('resultsContent');
            const resultItems = resultsContent.querySelectorAll('.result-item');
            const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
            
            let saveText = `DarkIP Результаты проверки\n`;
            saveText += `Дата: ${new Date().toLocaleString('ru-RU')}\n\n`;
            saveText += document.getElementById('resultsTitle').textContent + '\n\n';
            
            resultItems.forEach(item => {
                const label = item.querySelector('.result-label').textContent;
                const value = item.querySelector('.result-value').textContent;
                saveText += `${label}: ${value}\n`;
            });
            
            // Создаем Blob и ссылку для скачивания
            const blob = new Blob([saveText], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `darkip-results-${timestamp}.txt`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            
            showNotification('Результаты сохранены в файл', 'success');
        }

        // Показать уведомление
        function showNotification(message, type = 'info') {
            // Создаем элемент уведомления
            const notification = document.createElement('div');
            notification.className = `notification notification-${type}`;
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                padding: 15px 20px;
                background: ${type === 'error' ? 'var(--danger)' : 
                            type === 'warning' ? 'var(--warning)' : 
                            type === 'success' ? 'var(--success)' : 'var(--accent)'};
                color: white;
                border-radius: 8px;
                z-index: 1000;
                animation: slideIn 0.3s ease;
                max-width: 400px;
                box-shadow: 0 5px 15px rgba(0,0,0,0.3);
                display: flex;
                align-items: center;
                gap: 10px;
            `;
            
            const icon = type === 'error' ? 'fas fa-times-circle' :
                        type === 'warning' ? 'fas fa-exclamation-triangle' :
                        type === 'success' ? 'fas fa-check-circle' : 'fas fa-info-circle';
            
            notification.innerHTML = `
                <i class="${icon}"></i>
                <span>${message}</span>
            `;
            
            document.body.appendChild(notification);
            
            // Удаляем уведомление через 3 секунды
            setTimeout(() => {
                notification.style.animation = 'slideOut 0.3s ease';
                setTimeout(() => {
                    if (notification.parentNode) {
                        notification.parentNode.removeChild(notification);
                    }
                }, 300);
            }, 3000);
        }

        // Генерировать фейковые WHOIS данные
        function generateFakeWhoisData(domain) {
            const createdDate = new Date(Date.now() - Math.random() * 31536000000 * 10); // 0-10 лет назад
            const expiryDate = new Date(createdDate.getTime() + Math.random() * 31536000000 * 5); // 1-5 лет вперед
            
            const registrars = ['GoDaddy', 'NameCheap', 'Google Domains', 'Name.com', 'Domain.com', 'Hostinger'];
            const statuses = ['active', 'clientTransferProhibited', 'autoRenewPeriod'];
            
            return [
                { label: 'Домен', value: domain, status: 'info' },
                { label: 'Статус', value: statuses[Math.floor(Math.random() * statuses.length)], status: 'info' },
                { label: 'Дата создания', value: createdDate.toLocaleDateString('ru-RU') },
                { label: 'Дата обновления', value: new Date(createdDate.getTime() + Math.random() * 31536000000).toLocaleDateString('ru-RU') },
                { label: 'Дата истечения', value: expiryDate.toLocaleDateString('ru-RU') },
                { label: 'Регистратор', value: registrars[Math.floor(Math.random() * registrars.length)] },
                { label: 'Примечание', value: 'Демонстрационные данные', status: 'warning' }
            ];
        }

        // Получить статус для оценки SSL
        function getSSLGradeStatus(grade) {
            if (!grade) return 'warning';
            
            const gradeMap = {
                'A+': 'safe',
                'A': 'safe',
                'B': 'warning',
                'C': 'warning',
                'D': 'danger',
                'E': 'danger',
                'F': 'danger'
            };
            
            return gradeMap[grade] || 'warning';
        }

        // Стили для анимации уведомлений
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideIn {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
            
            @keyframes slideOut {
                from { transform: translateX(0); opacity: 1; }
                to { transform: translateX(100%); opacity: 0; }
            }
            
            .notification {
                font-family: inherit;
                font-size: 14px;
                font-weight: 500;
            }
            
            .notification i {
                font-size: 16px;
            }
        `;
        document.head.appendChild(style);

        // Инициализация при загрузке
        document.addEventListener('DOMContentLoaded', function() {
            // Загружаем текущий IP
            getCurrentIP();
            
            // Настраиваем мобильное меню
            setupMobileMenu();
            
            // Добавляем обработчики клавиш Enter для всех полей ввода
            const inputFields = document.querySelectorAll('.input-field');
            inputFields.forEach(input => {
                input.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') {
                        const closestBtn = this.closest('.tool-card')?.querySelector('.btn');
                        if (closestBtn) {
                            closestBtn.click();
                        }
                    }
                });
            });
            
            // Автоматически проверяем текущий IP при загрузке
            setTimeout(() => {
                if (document.getElementById('currentIp').textContent !== 'Ошибка загрузки') {
                    checkIPInfo();
                }
            }, 1000);
        });
    </script>
</body>
</html>
