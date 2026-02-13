<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ТЗ | Интеграция системы отчётности в BotHelp</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #2563eb;
            --primary-dark: #1d4ed8;
            --secondary: #7c3aed;
            --accent: #0ea5e9;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --dark: #0f172a;
            --light: #f8fafc;
            --gray: #64748b;
            --light-gray: #e2e8f0;
            --gradient: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%);
            --gradient-accent: linear-gradient(135deg, #0ea5e9 0%, #2563eb 100%);
            --gradient-light: linear-gradient(135deg, rgba(37, 99, 235, 0.1) 0%, rgba(124, 58, 237, 0.1) 100%);
            --card-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            line-height: 1.6;
            color: var(--dark);
            background-color: var(--light);
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }
        
        /* Заголовок с градиентом и прозрачностью */
        .hero {
            padding: 100px 0 80px;
            background: var(--gradient-light);
            margin-bottom: 60px;
            position: relative;
            overflow: hidden;
        }
        
        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--gradient);
            opacity: 0.05;
            z-index: -1;
        }
        
        .hero-content {
            text-align: center;
            max-width: 900px;
            margin: 0 auto;
        }
        
        .hero-title {
            font-family: 'JetBrains Mono', monospace;
            font-size: 2.8rem;
            font-weight: 700;
            margin-bottom: 20px;
            background: var(--gradient);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: -1px;
            line-height: 1.2;
        }
        
        .hero-version {
            display: inline-block;
            background: var(--gradient);
            color: white;
            padding: 8px 24px;
            border-radius: 50px;
            font-weight: 600;
            margin-bottom: 30px;
            letter-spacing: 2px;
            box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
        }
        
        .hero-subtitle {
            font-size: 1.2rem;
            color: var(--gray);
            margin-bottom: 40px;
            max-width: 700px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .hero-tags {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .hero-tag {
            background: white;
            padding: 10px 22px;
            border-radius: 50px;
            font-weight: 600;
            color: var(--primary);
            border: 1px solid var(--light-gray);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            display: flex;
            align-items: center;
            gap: 10px;
            transition: var(--transition);
        }
        
        .hero-tag i {
            color: var(--primary);
        }
        
        .hero-tag:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(37, 99, 235, 0.15);
        }
        
        /* Карточки слайдов */
        .slides-section {
            margin-bottom: 80px;
        }
        
        .section-title {
            font-family: 'JetBrains Mono', monospace;
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 50px;
            color: var(--dark);
            text-align: center;
            position: relative;
        }
        
        .section-title::after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 5px;
            background: var(--gradient);
            border-radius: 3px;
        }
        
        .slides-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 30px;
        }
        
        @media (max-width: 768px) {
            .slides-container {
                grid-template-columns: 1fr;
            }
        }
        
        .slide-card {
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: var(--card-shadow);
            transition: var(--transition);
            border: 1px solid rgba(226, 232, 240, 0.8);
            position: relative;
            height: 100%;
            display: flex;
            flex-direction: column;
        }
        
        .slide-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
        }
        
        .slide-header {
            background: var(--gradient);
            color: white;
            padding: 25px;
            position: relative;
        }
        
        .slide-number {
            position: absolute;
            top: 25px;
            right: 25px;
            background: rgba(255, 255, 255, 0.2);
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 1.2rem;
            border: 2px solid rgba(255, 255, 255, 0.5);
        }
        
        .slide-icon {
            font-size: 2rem;
            margin-bottom: 15px;
        }
        
        .slide-title {
            font-family: 'JetBrains Mono', monospace;
            font-size: 1.3rem;
            font-weight: 700;
            margin-bottom: 5px;
            padding-right: 50px;
        }
        
        .slide-content {
            padding: 25px;
            flex: 1;
            display: flex;
            flex-direction: column;
        }
        
        .slide-content p {
            color: var(--gray);
            margin-bottom: 15px;
        }
        
        /* Текст в слайдах в выделенных блоках */
        .code-block {
            background: #0f172a;
            color: #e2e8f0;
            padding: 20px;
            border-radius: 12px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.85rem;
            line-height: 1.5;
            margin: 20px 0;
            overflow-x: auto;
            white-space: pre-wrap;
            word-wrap: break-word;
            border-left: 4px solid var(--accent);
        }
        
        .alert-block {
            background: var(--gradient-light);
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            border-left: 5px solid var(--danger);
        }
        
        .success-block {
            background: rgba(16, 185, 129, 0.1);
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            border-left: 5px solid var(--success);
        }
        
        .warning-block {
            background: rgba(245, 158, 11, 0.1);
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            border-left: 5px solid var(--warning);
        }
        
        .info-block {
            background: rgba(14, 165, 233, 0.1);
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            border-left: 5px solid var(--accent);
        }
        
        .message-preview {
            background: #e5e7eb;
            padding: 20px;
            border-radius: 12px;
            font-family: 'JetBrains Mono', monospace;
            margin: 20px 0;
            border: 1px solid #d1d5db;
        }
        
        .message-text {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .architecture-diagram {
            background: white;
            padding: 25px;
            border-radius: 12px;
            font-family: 'JetBrains Mono', monospace;
            margin: 20px 0;
            border: 1px solid var(--light-gray);
            text-align: center;
        }
        
        .architecture-diagram pre {
            background: #f1f5f9;
            padding: 20px;
            border-radius: 8px;
            overflow-x: auto;
            font-size: 0.85rem;
        }
        
        .endpoint-badge {
            display: inline-block;
            background: #0f172a;
            color: #e2e8f0;
            padding: 5px 12px;
            border-radius: 6px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.8rem;
            margin-right: 10px;
        }
        
        .method-post {
            background: #10b981;
            color: white;
            padding: 4px 10px;
            border-radius: 6px;
            font-weight: 600;
            font-size: 0.75rem;
            display: inline-block;
            margin-right: 8px;
        }
        
        .method-get {
            background: #3b82f6;
            color: white;
            padding: 4px 10px;
            border-radius: 6px;
            font-weight: 600;
            font-size: 0.75rem;
            display: inline-block;
            margin-right: 8px;
        }
        
        /* Сетка инструментов в виде тегов */
        .tools-section {
            margin-bottom: 80px;
        }
        
        .tools-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 30px;
        }
        
        .tool-tag {
            background: var(--gradient-light);
            color: var(--primary-dark);
            padding: 12px 24px;
            border-radius: 50px;
            font-size: 0.95rem;
            font-weight: 600;
            border: 2px solid rgba(37, 99, 235, 0.2);
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .tool-tag:hover {
            background: var(--gradient);
            color: white;
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(37, 99, 235, 0.2);
        }
        
        .tool-tag i {
            font-size: 1rem;
        }
        
        /* Таблицы ограничений */
        .constraints-table {
            width: 100%;
            border-collapse: collapse;
            margin: 25px 0;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
        }
        
        .constraints-table th {
            background: var(--gradient);
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: 600;
        }
        
        .constraints-table td {
            padding: 15px;
            border-bottom: 1px solid var(--light-gray);
            background: white;
        }
        
        .constraints-table tr:last-child td {
            border-bottom: none;
        }
        
        .risk-matrix {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin: 30px 0;
        }
        
        .risk-item {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: var(--card-shadow);
            border-top: 4px solid var(--danger);
        }
        
        .risk-probability {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            background: #fee2e2;
            color: #b91c1c;
            font-size: 0.8rem;
            font-weight: 600;
            margin-bottom: 10px;
        }
        
        /* Адаптивность для мобильных устройств */
        @media (max-width: 768px) {
            .hero {
                padding: 70px 0 50px;
            }
            
            .hero-title {
                font-size: 2rem;
            }
            
            .hero-subtitle {
                font-size: 1rem;
            }
            
            .section-title {
                font-size: 1.6rem;
            }
            
            .slide-header {
                padding: 20px;
            }
            
            .slide-number {
                top: 20px;
                right: 20px;
                width: 35px;
                height: 35px;
                font-size: 1rem;
            }
            
            .slide-title {
                font-size: 1.1rem;
            }
            
            .slide-content {
                padding: 20px;
            }
            
            .code-block {
                padding: 15px;
                font-size: 0.75rem;
            }
            
            .tools-grid {
                gap: 10px;
            }
            
            .tool-tag {
                padding: 10px 18px;
                font-size: 0.85rem;
            }
            
            .constraints-table td, .constraints-table th {
                padding: 12px 8px;
                font-size: 0.85rem;
            }
            
            .risk-matrix {
                grid-template-columns: 1fr;
            }
        }
        
        @media (max-width: 480px) {
            .hero-title {
                font-size: 1.6rem;
            }
            
            .hero-version {
                padding: 6px 18px;
                font-size: 0.9rem;
            }
            
            .section-title {
                font-size: 1.4rem;
            }
        }
        
        /* Анимации */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            opacity: 0;
            animation: fadeIn 0.8s ease forwards;
        }
        
        .delay-1 { animation-delay: 0.1s; }
        .delay-2 { animation-delay: 0.2s; }
        .delay-3 { animation-delay: 0.3s; }
        
        /* Кнопка наверх */
        .scroll-top {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            background: var(--gradient);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            box-shadow: 0 5px 15px rgba(37, 99, 235, 0.3);
            cursor: pointer;
            z-index: 100;
            transition: var(--transition);
            opacity: 0;
            visibility: hidden;
        }
        
        .scroll-top.visible {
            opacity: 1;
            visibility: visible;
        }
        
        .scroll-top:hover {
            transform: scale(1.1);
        }
        
        @media (max-width: 768px) {
            .scroll-top {
                bottom: 20px;
                right: 20px;
                width: 50px;
                height: 50px;
                font-size: 1.2rem;
            }
        }
        
        /* Моноширинный текст */
        .mono {
            font-family: 'JetBrains Mono', monospace;
        }
        
        /* Иконки ограничений */
        .constraint-icon {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 24px;
            height: 24px;
            background: var(--warning);
            color: white;
            border-radius: 6px;
            font-size: 0.8rem;
            margin-right: 8px;
        }
        
        /* Прогресс-бар для чек-листа */
        .progress-checklist {
            margin-top: 30px;
        }
        
        .checklist-item {
            display: flex;
            align-items: flex-start;
            gap: 15px;
            padding: 15px;
            background: white;
            border-radius: 12px;
            margin-bottom: 10px;
            border: 1px solid var(--light-gray);
        }
        
        .checklist-check {
            color: var(--success);
            font-size: 1.2rem;
        }
        
        .checklist-pending {
            color: var(--gray);
            font-size: 1.2rem;
        }
        
        /* Стили для БД */
        .sql-table {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.85rem;
            line-height: 1.5;
        }
        
        .footer {
            text-align: center;
            padding: 60px 0;
            border-top: 1px solid var(--light-gray);
            color: var(--gray);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Заголовок с градиентом -->
        <section class="hero fade-in">
            <div class="hero-content">
                <h1 class="hero-title">Техническое задание</h1>
                <div class="hero-version">Версия 2.0 — Реалистичная, с учётом ограничений платформы</div>
                <p class="hero-subtitle">Интеграция системы сбора и проверки отчётности в Telegram-бот (BotHelp)</p>
                
                <div class="hero-tags">
                    <span class="hero-tag"><i class="fas fa-robot"></i> BotHelp</span>
                    <span class="hero-tag"><i class="fas fa-code-branch"></i> FastAPI</span>
                    <span class="hero-tag"><i class="fas fa-database"></i> PostgreSQL</span>
                    <span class="hero-tag"><i class="fas fa-cloud"></i> S3</span>
                    <span class="hero-tag"><i class="fas fa-webhook"></i> Webhooks</span>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 1: Цель и архитектурный подход -->
        <section class="slides-section fade-in delay-1">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">1</div>
                    <div class="slide-icon">🎯</div>
                    <div class="slide-title">Цель и архитектурный подход</div>
                </div>
                <div class="slide-content">
                    <p><strong>🎯 Цель</strong></p>
                    <p>Внедрить в существующий Telegram-бот на BotHelp функционал:</p>
                    <ul style="margin-left: 20px; margin-bottom: 20px; color: var(--gray);">
                        <li>Мастер заполнения отчётных форм</li>
                        <li>Валидация и контрольные соотношения</li>
                        <li>Управление версиями</li>
                        <li>Аудит-лог</li>
                        <li>Экспорт с контролем качества</li>
                    </ul>
                    
                    <p><strong>🏗 Архитектурный подход</strong></p>
                    <div class="architecture-diagram">
                        <pre>
┌─────────────────┐     Webhooks     ┌─────────────────┐
│                 │ ───────────────> │                 │
│    BotHelp      │                  │    Backend      │
│   (UI layer)    │ <──────────────  │   (FastAPI)     │
│                 │   BotHelp API    │                 │
└─────────────────┘                  └─────────────────┘
      │                                       │
      │ Custom fields                        │ PostgreSQL
      │ Variables                            │   • packages
      │ Conditions                           │   • fields
      │                                      │   • audit
      ▼                                      ▼
   Пользователь                          Хранилище
   Telegram                           • данные • файлы</pre>
                    </div>
                    
                    <div class="success-block">
                        <strong>✅ КЛЮЧЕВОЕ РЕШЕНИЕ</strong>
                        <p style="margin-top: 10px;">BotHelp = интерфейс сбора данных + простая навигация<br>Backend = вся бизнес-логика, проверки, история, экспорт</p>
                    </div>
                    
                    <div class="alert-block">
                        <strong>❌ НЕ ПЫТАТЬСЯ:</strong>
                        <ul style="margin-left: 20px; margin-top: 10px;">
                            <li>Делать сложные проверки в BotHelp Condition</li>
                            <li>Хранить историю в Custom fields</li>
                            <li>Синхронно валидировать при вводе</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 2: Ограничения BotHelp -->
        <section class="slides-section fade-in delay-1">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">2</div>
                    <div class="slide-icon">⚠️</div>
                    <div class="slide-title">Ограничения BotHelp, которые определяют ТЗ</div>
                </div>
                <div class="slide-content">
                    <p><strong>⚠️ Технические ограничения платформы</strong></p>
                    
                    <table class="constraints-table">
                        <thead>
                            <tr>
                                <th>Ограничение</th>
                                <th>Влияние на ТЗ</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Webhook — одностороннее уведомление</td>
                                <td>Backend НЕ МОЖЕТ вернуть ответ и показать его пользователю в том же шаге</td>
                            </tr>
                            <tr>
                                <td>Нет синхронного вызова API из сценария</td>
                                <td>Нельзя "спросить" backend и сразу отреагировать</td>
                            </tr>
                            <tr>
                                <td>Custom fields — только строки, нет типов</td>
                                <td>Все данные хранятся как текст</td>
                            </tr>
                            <tr>
                                <td>Нет встроенной истории изменений полей</td>
                                <td>Аудит-лог только в backend</td>
                            </tr>
                            <tr>
                                <td>Telegram file_id — временный</td>
                                <td>Файлы нужно скачивать немедленно</td>
                            </tr>
                            <tr>
                                <td>Webhook может дублироваться</td>
                                <td>Нужна идемпотентность</td>
                            </tr>
                            <tr>
                                <td>Нет транзакций между webhook и API</td>
                                <td>Возможны race conditions</td>
                            </tr>
                        </tbody>
                    </table>
                    
                    <div class="info-block">
                        <strong>🎯 ПРИНЦИПЫ, ВЫТЕКАЮЩИЕ ИЗ ОГРАНИЧЕНИЙ</strong>
                        <ul style="margin-left: 20px; margin-top: 15px;">
                            <li><strong>НЕ ИСПОЛЬЗОВАТЬ</strong> webhook для мгновенной обратной связи</li>
                            <li><strong>ВСЕ ПРОВЕРКИ</strong> — либо по кнопке "Проверить", либо накопительные</li>
                            <li><strong>ОШИБКИ ПОКАЗЫВАТЬ</strong> списком, не отдельными сообщениями</li>
                            <li><strong>CRITICAL ОШИБКИ</strong> проверять в BotHelp Condition (простые правила)</li>
                            <li><strong>СЛОЖНЫЕ ПРОВЕРКИ</strong> — только через backend по запросу</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 3: Компоненты системы -->
        <section class="slides-section fade-in delay-2">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">3</div>
                    <div class="slide-icon">📦</div>
                    <div class="slide-title">Компоненты системы</div>
                </div>
                <div class="slide-content">
                    <p><strong>📦 BotHelp (существующий бот — доработка)</strong></p>
                    
                    <div class="code-block">
# Custom fields (обязательные):
package_id              // UUID пакета — СВЯТОЙ ГРААЛЬ
org_id                  // ID организации
org_name                // Название для отображения
period                  // Отчётный период (2025-Q4)
current_section         // Текущий раздел формы
current_field_code      // Текущее поле (для редактирования)
kb_version              // Версия методики
last_validation_at      // Время последней проверки
validation_error_count  // Количество ошибок
                    </div>
                    
                    <p style="margin-top: 20px;"><strong>🖥 Backend (новый компонент)</strong></p>
                    <p>Стек: <span class="tool-tag" style="display: inline-block; padding: 6px 16px; margin: 5px;">Python FastAPI</span> <span class="tool-tag" style="display: inline-block; padding: 6px 16px; margin: 5px;">Node.js Express</span> <span class="tool-tag" style="display: inline-block; padding: 6px 16px; margin: 5px;">PostgreSQL</span> <span class="tool-tag" style="display: inline-block; padding: 6px 16px; margin: 5px;">S3</span></p>
                    
                    <p style="margin-top: 20px;"><strong>Эндпоинты:</strong></p>
                    <div class="code-block">
POST  /webhook/bothelp          # Приём всех webhook'ов от BotHelp
GET   /health                   # Мониторинг
POST  /internal/reminders       # CRON-задача (напоминания)
                    </div>
                    
                    <p style="margin-top: 20px;"><strong>Модели данных (PostgreSQL):</strong></p>
                    <div class="code-block sql-table">
-- Пакет отчётности (главная сущность)
CREATE TABLE report_package (
  id UUID PRIMARY KEY,
  bothelp_user_id BIGINT NOT NULL,
  telegram_id BIGINT,
  org_id VARCHAR(100),
  org_name TEXT,
  period VARCHAR(20),
  status VARCHAR(50), -- draft, validation_failed, ready, exported
  kb_version VARCHAR(50),
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  exported_at TIMESTAMP,
  UNIQUE(bothelp_user_id, org_id, period)
);

-- Значения полей
CREATE TABLE field_value (
  id UUID PRIMARY KEY,
  package_id UUID REFERENCES report_package,
  section_code VARCHAR(100),
  field_code VARCHAR(100),
  value TEXT,
  raw_value TEXT,
  updated_by VARCHAR(50), -- user / system / import
  updated_at TIMESTAMP,
  UNIQUE(package_id, field_code)
);
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 4: Протокол взаимодействия -->
        <section class="slides-section fade-in delay-2">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">4</div>
                    <div class="slide-icon">📤</div>
                    <div class="slide-title">Протокол взаимодействия BotHelp ↔ Backend</div>
                </div>
                <div class="slide-content">
                    <p><strong>📤 Webhook: BotHelp → Backend</strong></p>
                    
                    <div class="code-block">
{
  "event_id": "evt_170820241234", // уникальный ID события
  "event_type": "field_input",    // field_input / section_check / package_check / export_request / history_request
  "timestamp": "2026-02-13T12:00:00Z",
  
  "subscriber": {
    "id": 123456789,              // bothelp_user_id
    "telegram_id": 1122334455,
    "external_id": null
  },
  
  "custom_fields": {
    "package_id": "550e8400-e29b-41d4-a716-446655440000",
    "org_id": "001",
    "org_name": "ООО Ромашка",
    "period": "2025-Q4",
    "current_field_code": "revenue_1100",
    "kb_version": "2025.1"
  },
  
  "data": {
    "field_code": "revenue_1100",
    "user_input": "1000000"
  }
}
                    </div>
                    
                    <p><strong>Требования:</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Все webhook'и идут на ОДИН URL → /webhook/bothelp</li>
                        <li>В каждом webhook обязательно передавать event_id</li>
                        <li>Backend реализует идемпотентность</li>
                        <li>Таймаут ответа ≤ 10 секунд</li>
                    </ul>
                    
                    <p><strong>📥 BotHelp API: Backend → BotHelp</strong></p>
                    <div class="code-block">
// 1. Отправка сообщения
POST https://api.bothelp.io/v1/messages/send
{
  userId: 123456789,
  text: "Текст сообщения",
  parseMode: "HTML",
  buttons: [{text: "Исправить", callbackData: "fix_field_revenue_1100"}]
}

// 2. Установка значения Custom field
POST https://api.bothelp.io/v1/subscribers/fields/set
{
  userId: 123456789,
  fieldKey: "validation_error_count",
  value: "3"
}
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 5: Сценарий 1 — Старт -->
        <section class="slides-section fade-in delay-2">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">5</div>
                    <div class="slide-icon">🟢</div>
                    <div class="slide-title">Сценарий 1 — Старт, выбор организации и периода</div>
                </div>
                <div class="slide-content">
                    <div class="success-block">
                        <strong>🟢 УЖЕ РЕАЛИЗОВАНО В СУЩЕСТВУЮЩЕМ БОТЕ</strong>
                        <p style="margin-top: 10px;">Доработки не требуются — существующие сценарии выбора сохраняются.</p>
                    </div>
                    
                    <p><strong>ЕДИНСТВЕННОЕ ИЗМЕНЕНИЕ:</strong></p>
                    <p>После выбора периода → отправить webhook с event_type: "package_init"</p>
                    
                    <div class="info-block">
                        <strong>Backend:</strong>
                        <ul style="margin-top: 10px;">
                            <li>Проверяет, существует ли пакет для (org_id, period, user_id)</li>
                            <li>Если нет — создаёт report_package в статусе draft</li>
                            <li>Если есть незавершённый — возвращает существующий package_id</li>
                        </ul>
                    </div>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>📨 Сообщение пользователю:</strong><br>
                            "Продолжаем заполнение отчёта за 2025-Q4. Ваш прогресс: 35%"
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 6: Сценарий 2 — Ввод значения поля -->
        <section class="slides-section fade-in delay-2">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">6</div>
                    <div class="slide-icon">🔧</div>
                    <div class="slide-title">Сценарий 2 — Ввод значения поля</div>
                </div>
                <div class="slide-content">
                    <div class="warning-block">
                        <strong>🔧 ДОРАБОТКА СУЩЕСТВУЮЩЕГО СЦЕНАРИЯ</strong>
                    </div>
                    
                    <p><strong>🔄 ПОТОК РАБОТЫ:</strong></p>
                    <ol style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Пользователь: вводит "1500000"</li>
                        <li>BotHelp: сохраняет в custom_field.revenue_1100 = "1500000"</li>
                        <li>BotHelp: отправляет webhook (event_type: field_input)</li>
                        <li>Backend: находит пакет по package_id, сохраняет field_value</li>
                        <li>НЕ ВЫПОЛНЯЕТ СЛОЖНЫЕ ПРОВЕРКИ</li>
                        <li>НЕ ОТПРАВЛЯЕТ СООБЩЕНИЕ</li>
                    </ol>
                    
                    <div class="alert-block">
                        <strong>⚠️ ИСКЛЮЧЕНИЕ — КРИТИЧЕСКАЯ ОШИБКА ФОРМАТА</strong>
                        <p style="margin-top: 10px;">Если поле обязательное, формат: число, дата, ИНН и т.п. — проверять в BotHelp:</p>
                        <div class="code-block">
Condition: 
  [revenue_1100] = "" ИЛИ [revenue_1100] НЕ число
→ 
  Отправить сообщение: "Ошибка: введите числовое значение"
  Вернуться на шаг ввода
                        </div>
                        <p style="margin-top: 10px;"><strong>❌ НЕ ИСПОЛЬЗОВАТЬ</strong> backend для простых проверок формата<br>
                        <strong>✅ Использовать</strong> только BotHelp Condition</p>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 7: Сценарий 3 — Проверка раздела / пакета -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">7</div>
                    <div class="slide-icon">🆕</div>
                    <div class="slide-title">Сценарий 3 — Проверка раздела / пакета</div>
                </div>
                <div class="slide-content">
                    <div class="success-block" style="border-left-color: var(--accent);">
                        <strong>🆕 НОВЫЙ СЦЕНАРИЙ</strong>
                    </div>
                    
                    <p><strong>Механика:</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Пользователь нажимает кнопку «Проверить раздел»</li>
                        <li>BotHelp отправляет webhook с event_type: "section_check"</li>
                        <li>Backend выполняет контрольные соотношения</li>
                        <li>Backend отправляет сообщение через BotHelp API</li>
                    </ul>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>📋 РЕЗУЛЬТАТ ПРОВЕРКИ: Раздел 1. Доходы</strong><br><br>
                            ✅ Выручка: 15 000 000<br>
                            ✅ Себестоимость: 8 200 000<br>
                            ✅ Валовая прибыль: 6 800 000<br><br>
                            ⚠️ ОБНАРУЖЕНЫ ОШИБКИ:<br><br>
                            1. [Критическая] Стр.1100 (Выручка) < Стр.1200 (Себестоимость)<br>
                               Соотношение: 15 000 000 < 8 200 000?<br>
                               Норма: Выручка должна быть больше Себестоимости<br><br>
                            2. [Рекомендация] Рентабельность < 5%<br>
                               Текущая: 4.2%<br>
                               Минимальная: 5%<br><br>
                            🔄 Что делать:<br>
                            - /fix_revenue_1100 — исправить Выручку<br>
                            - /fix_cost_1200 — исправить Себестоимость
                        </div>
                    </div>
                    
                    <div class="info-block">
                        <strong>✅ ПРЕИМУЩЕСТВА:</strong>
                        <ul style="margin-top: 10px;">
                            <li>Пользователь получает полную картину, а не спам из 10 сообщений</li>
                            <li>Можно группировать ошибки</li>
                            <li>Можно сортировать по критичности</li>
                            <li>Единый формат отчёта</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 8: Сценарий 4 — Просмотр ошибок -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">8</div>
                    <div class="slide-icon">🆕</div>
                    <div class="slide-title">Сценарий 4 — Просмотр ошибок</div>
                </div>
                <div class="slide-content">
                    <div class="success-block" style="border-left-color: var(--accent);">
                        <strong>🆕 НОВЫЙ СЦЕНАРИЙ</strong>
                    </div>
                    
                    <p><strong>Механика:</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Пользователь нажимает кнопку «Ошибки»</li>
                        <li>BotHelp отправляет webhook с event_type: "show_errors"</li>
                        <li>Backend загружает активные ошибки для пакета</li>
                        <li>Backend отправляет сообщение через BotHelp API</li>
                    </ul>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>❌ КРИТИЧЕСКИЕ ОШИБКИ (2):</strong><br>
                            • Выручка < Себестоимости<br>
                              Раздел 1, поле revenue_1100<br>
                              /fix_revenue_1100<br><br>
                            • Остаток на начало ≠ остатку на конец прошлого периода<br>
                              Раздел 4, поле balance_4100<br>
                              /fix_balance_4100<br><br>
                            ⚠️ ВАЖНЫЕ ОШИБКИ (3):<br>
                            • Не заполнено поле "Код ОКВЭД"<br>
                            • ...<br><br>
                            💡 РЕКОМЕНДАЦИИ (1):<br>
                            • ...
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 9: Сценарий 5 — Экспорт с контролем качества -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">9</div>
                    <div class="slide-icon">🆕</div>
                    <div class="slide-title">Сценарий 5 — Экспорт с контролем качества</div>
                </div>
                <div class="slide-content">
                    <div class="success-block" style="border-left-color: var(--accent);">
                        <strong>🆕 НОВЫЙ СЦЕНАРИЙ — ДВЕ СТАДИИ</strong>
                    </div>
                    
                    <p><strong>СТАДИЯ 1: Запрос экспорта</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Пользователь нажимает «Экспорт»</li>
                        <li>BotHelp отправляет webhook с event_type: "export_request"</li>
                        <li>Backend проверяет наличие критических ошибок</li>
                    </ul>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>⛔ ЭКСПОРТ НЕВОЗМОЖЕН</strong><br><br>
                            Обнаружены критические ошибки (2):<br>
                            • Выручка < Себестоимости<br>
                            • Остаток на начало ≠ остатку на конец<br><br>
                            ✅ Исправьте ошибки и повторите экспорт<br>
                            ⚠️ Или подтвердите, что принимаете риски: /accept_risks_and_export
                        </div>
                    </div>
                    
                    <p style="margin-top: 20px;"><strong>СТАДИЯ 2: Принятие риска</strong></p>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>📎 ВАШ ОТЧЁТ ГОТОВ</strong><br><br>
                            Период: 2025-Q4<br>
                            Организация: ООО Ромашка<br>
                            Статус: ЭКСПОРТ С ПРИНЯТИЕМ РИСКА (2 риска)<br><br>
                            📥 Скачать: https://storage.yandexcloud.net/reports/export_2025_Q4.xlsx<br>
                            🔗 Ссылка действительна 24 часа<br><br>
                            🗂 Вложение: (файл прикреплён к сообщению)
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 10: Сценарий 6 — История изменений -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">10</div>
                    <div class="slide-icon">🆕</div>
                    <div class="slide-title">Сценарий 6 — История изменений (аудит-лог)</div>
                </div>
                <div class="slide-content">
                    <div class="success-block" style="border-left-color: var(--accent);">
                        <strong>🆕 НОВЫЙ СЦЕНАРИЙ</strong>
                    </div>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>📜 ИСТОРИЯ ИЗМЕНЕНИЙ — Пакет #550e8400</strong><br><br>
                            🗓 13.02.2026<br>
                            14:23 — Поле "Выручка"<br>
                              &nbsp;&nbsp;1 000 000 → 950 000<br>
                              &nbsp;&nbsp;Причина: Исправление ошибки ввода<br>
                              &nbsp;&nbsp;Изменил: Иванов И.И.<br><br>
                            14:15 — Поле "Себестоимость"<br>
                              &nbsp;&nbsp;8 500 000 → 8 200 000<br>
                              &nbsp;&nbsp;Причина: Уточнение данных поставщика<br>
                              &nbsp;&nbsp;Изменил: Петров П.П.<br><br>
                            10:02 — Пакет создан<br><br>
                            📎 Полный аудит-лог: [ссылка на JSON]
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 11: Сценарий 7 — Работа с файлами -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">11</div>
                    <div class="slide-icon">🔧</div>
                    <div class="slide-title">Сценарий 7 — Работа с файлами</div>
                </div>
                <div class="slide-content">
                    <div class="warning-block">
                        <strong>🔧 ДОРАБОТКА СУЩЕСТВУЮЩЕГО СЦЕНАРИЯ</strong>
                    </div>
                    
                    <p><strong>ПРОБЛЕМА:</strong> Telegram file_id не вечен</p>
                    
                    <p><strong>РЕШЕНИЕ:</strong></p>
                    <ol style="margin-left: 20px; margin-bottom: 20px;">
                        <li>BotHelp: Пользователь загружает файл</li>
                        <li>Сохраняет file_id в custom field</li>
                        <li>НЕМЕДЛЕННО отправляет webhook с event_type: "file_upload"</li>
                        <li>Backend: Получает file_id</li>
                        <li>СРАЗУ вызывает Telegram API getFile и скачивает файл</li>
                        <li>Загружает в S3-совместимое хранилище</li>
                        <li>Сохраняет запись в attachment</li>
                    </ol>
                    
                    <div class="info-block">
                        <strong>📁 Формат экспорта:</strong>
                        <p style="margin-top: 10px;">Excel-файл, 3 листа:</p>
                        <ul>
                            <li><strong>Данные</strong> — все введённые поля</li>
                            <li><strong>Проверки</strong> — статус каждой проверки + версия правил</li>
                            <li><strong>Аудит</strong> — список принятых рисков</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 12: Сценарий 8 — Напоминания и дедлайны -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">12</div>
                    <div class="slide-icon">🆕</div>
                    <div class="slide-title">Сценарий 8 — Напоминания и дедлайны</div>
                </div>
                <div class="slide-content">
                    <div class="success-block" style="border-left-color: var(--accent);">
                        <strong>🆕 НОВЫЙ КОМПОНЕНТ — CRON-ЗАДАЧА</strong>
                    </div>
                    
                    <p><strong>Периодичность:</strong> Ежедневно в 10:00 и 16:00</p>
                    
                    <div class="code-block">
def check_deadlines():
    # 1. Найти все пакеты в статусе draft, validation_failed
    # 2. Для каждого пакета определить дедлайн
    # 3. Рассчитать дней до дедлайна
    # 4. Если (дней = 10, 5, 3, 1, 0, просрочено) И (уведомление не отправлялось)
    # 5. Отправить сообщение через BotHelp API
                    </div>
                    
                    <div class="message-preview">
                        <div class="message-text">
                            <strong>⏰ НАПОМИНАНИЕ: Отчётность за 2025-Q4</strong><br><br>
                            Дедлайн: 20.03.2026 (осталось 5 дней)<br><br>
                            Текущий статус:<br>
                            ✅ Заполнено: 65% (24 из 37 полей)<br>
                            ⚠️ Критические ошибки: 2<br>
                            ❌ Не экспортировано<br><br>
                            Что делать:<br>
                            /continue — продолжить заполнение<br>
                            /errors — посмотреть ошибки<br>
                            /export — экспорт (после исправления ошибок)
                        </div>
                    </div>
                    
                    <p><strong>Защита от спама:</strong></p>
                    <ul style="margin-left: 20px;">
                        <li>Не чаще 1 уведомления в день по одному пакету</li>
                        <li>После экспорта — уведомления прекращаются</li>
                    </ul>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 13: План работ — BotHelp -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">13</div>
                    <div class="slide-icon">📋</div>
                    <div class="slide-title">План работ — Что конкретно меняем в существующем боте</div>
                </div>
                <div class="slide-content">
                    <div class="success-block">
                        <strong>📋 ЧЕК-ЛИСТ ДОРАБОТОК BOTHELP</strong>
                    </div>
                    
                    <div class="progress-checklist">
                        <div class="checklist-item">
                            <span class="checklist-check"><i class="fas fa-check-circle"></i></span>
                            <div>
                                <strong>1. Custom fields (добавить)</strong>
                                <p style="color: var(--gray); margin-top: 5px;">package_id, current_section, current_field_code, kb_version, last_validation_at, validation_error_count</p>
                            </div>
                        </div>
                        
                        <div class="checklist-item">
                            <span class="checklist-check"><i class="fas fa-check-circle"></i></span>
                            <div>
                                <strong>2. Существующие сценарии (доработать)</strong>
                                <p style="color: var(--gray); margin-top: 5px;">После выбора периода → webhook (package_init)<br>После User Input → webhook (field_input)<br>После загрузки файла → webhook (file_upload)</p>
                            </div>
                        </div>
                        
                        <div class="checklist-item">
                            <span class="checklist-pending"><i class="fas fa-circle"></i></span>
                            <div>
                                <strong>3. Новые сценарии (создать)</strong>
                                <p style="color: var(--gray); margin-top: 5px;">Проверка раздела, Проверка пакета, Ошибки, Экспорт, История, Принять риск</p>
                            </div>
                        </div>
                        
                        <div class="checklist-item">
                            <span class="checklist-pending"><i class="fas fa-circle"></i></span>
                            <div>
                                <strong>4. Команды быстрого перехода (добавить)</strong>
                                <p style="color: var(--gray); margin-top: 5px;">/fix_&lt;field_code&gt;, /accept_risks_and_export, /continue</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 14: План работ — Backend -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">14</div>
                    <div class="slide-icon">🔧</div>
                    <div class="slide-title">План работ — Backend (новый компонент)</div>
                </div>
                <div class="slide-content">
                    <div class="success-block">
                        <strong>📋 ЧЕК-ЛИСТ РАЗРАБОТКИ BACKEND</strong>
                    </div>
                    
                    <p><strong>🔧 1. Инфраструктура</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 15px;">
                        <li>Выбрать хостинг (Railway / Render / VPS)</li>
                        <li>Настроить домен + SSL (обязательно!)</li>
                        <li>Создать БД PostgreSQL</li>
                        <li>Настроить S3-хранилище</li>
                        <li>Настроить мониторинг</li>
                    </ul>
                    
                    <p><strong>🔧 2. Разработка API</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 15px;">
                        <li>Эндпоинт POST /webhook/bothelp</li>
                        <li>Механизм идемпотентности</li>
                        <li>Интеграция с BotHelp API</li>
                        <li>Интеграция с Telegram API</li>
                    </ul>
                    
                    <p><strong>🔧 3. Бизнес-логика</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 15px;">
                        <li>CRUD для report_package, field_value + история</li>
                        <li>Система правил валидации</li>
                        <li>Движок проверок</li>
                        <li>Генератор Excel-отчётов</li>
                    </ul>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 15: Риски и митигация -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">15</div>
                    <div class="slide-icon">🔴</div>
                    <div class="slide-title">Риски и митигация</div>
                </div>
                <div class="slide-content">
                    <p><strong>🔴 ТОП-5 РИСКОВ ПРОЕКТА</strong></p>
                    
                    <table class="constraints-table">
                        <thead>
                            <tr>
                                <th>Риск</th>
                                <th>Вероятность</th>
                                <th>Влияние</th>
                                <th>Митигация</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Webhook не отправляется после поля</td>
                                <td><span style="color: var(--danger);">Высокая</span></td>
                                <td>Критическое</td>
                                <td>Регулярное логирование. Автотест</td>
                            </tr>
                            <tr>
                                <td>Двойные webhook'и</td>
                                <td><span style="color: var(--danger);">Высокая</span></td>
                                <td>Среднее</td>
                                <td>Идемпотентность по event_id</td>
                            </tr>
                            <tr>
                                <td>Таймаут BotHelp API</td>
                                <td><span style="color: var(--warning);">Средняя</span></td>
                                <td>Среднее</td>
                                <td>Асинхронные операции</td>
                            </tr>
                            <tr>
                                <td>Потеря package_id</td>
                                <td><span style="color: var(--warning);">Средняя</span></td>
                                <td>Критическое</td>
                                <td>Восстановление по org_id+period+user_id</td>
                            </tr>
                            <tr>
                                <td>Пользователь нажал "Назад"</td>
                                <td><span style="color: var(--danger);">Высокая</span></td>
                                <td>Среднее</td>
                                <td>Проверять наличие package_id</td>
                            </tr>
                        </tbody>
                    </table>
                    
                    <div class="warning-block" style="margin-top: 20px;">
                        <strong>🟡 ОРГАНИЗАЦИОННЫЕ РИСКИ</strong>
                        <ul style="margin-top: 10px;">
                            <li><strong>Нет доступа к настройкам BotHelp</strong> — запросить у администратора</li>
                            <li><strong>Нет возможности развернуть backend</strong> — использовать готовые шаблоны</li>
                            <li><strong>Сложность поддержки двух систем</strong> — детальная документация</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 16: Критерии приёмки -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">16</div>
                    <div class="slide-icon">✅</div>
                    <div class="slide-title">Критерии приёмки (Acceptance Criteria)</div>
                </div>
                <div class="slide-content">
                    <div class="success-block">
                        <strong>✅ МИНИМАЛЬНО РАБОТОСПОСОБНЫЙ ПРОДУКТ (MVP)</strong>
                    </div>
                    
                    <p><strong>Пользователь может:</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Выбрать организацию и период</li>
                        <li>Заполнить несколько полей формы</li>
                        <li>Нажать «Проверить» и увидеть список ошибок</li>
                        <li>Исправить ошибки по кнопке /fix_...</li>
                        <li>Экспортировать отчёт (если нет критических ошибок)</li>
                    </ul>
                    
                    <p><strong>Администратор может:</strong></p>
                    <ul style="margin-left: 20px; margin-bottom: 20px;">
                        <li>Посмотреть в БД все пакеты и значения полей</li>
                        <li>Посмотреть аудит-лог изменений</li>
                    </ul>
                    
                    <p><strong>Система:</strong></p>
                    <ul style="margin-left: 20px;">
                        <li>Не теряет данные при перезапуске</li>
                        <li>Не создаёт дубли пакетов</li>
                        <li>Отвечает на webhook за < 10 секунд</li>
                    </ul>
                    
                    <div class="info-block" style="margin-top: 20px;">
                        <strong>✅ ПОЛНОЦЕННЫЙ РЕЛИЗ</strong>
                        <p style="margin-top: 10px;">Всё из MVP + принятие рисков, история изменений, напоминания о дедлайнах, загрузка файлов в S3, полный аудит-лог с хешированием</p>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- СЛАЙД 17: Заключение -->
        <section class="slides-section fade-in delay-3">
            <div class="slide-card">
                <div class="slide-header">
                    <div class="slide-number">17</div>
                    <div class="slide-icon">🎯</div>
                    <div class="slide-title">Заключение</div>
                </div>
                <div class="slide-content">
                    <div class="info-block">
                        <strong>🎯 КЛЮЧЕВЫЕ ВЫВОДЫ</strong>
                        <ul style="margin-top: 15px;">
                            <li><strong>BotHelp</strong> — отличный конструктор интерфейсов, но плохая платформа для бизнес-логики</li>
                            <li><strong>Единственная рабочая архитектура:</strong> BotHelp отправляет события → Backend валидирует и хранит → Backend отправляет результаты через API</li>
                            <li><strong>Мгновенная валидация при вводе НЕВОЗМОЖНА</strong> технически. Принять это и использовать проверки по запросу</li>
                            <li>70% сложности — в настройке webhook'ов в правильных местах и обработке дублей</li>
                            <li>30% сложности — в генерации человекочитаемых отчётов об ошибках</li>
                        </ul>
                    </div>
                    
                    <div class="success-block" style="margin-top: 20px;">
                        <strong>📚 РЕКОМЕНДАЦИИ</strong>
                        <ul style="margin-top: 10px;">
                            <li>Начать с прототипа — один раздел, 5 полей, 3 правила проверки</li>
                            <li>Сразу настроить идемпотентность — потом будет больнее</li>
                            <li>Все сообщения пользователю шаблонизировать — единый стиль</li>
                            <li>Логировать всё — и в BotHelp, и в backend</li>
                            <li>Не пытаться объять необъятное — выпустить MVP за 2 недели, потом дорабатывать</li>
                        </ul>
                    </div>
                    
                    <p style="margin-top: 30px;"><strong>ПРИЛОЖЕНИЕ: Ссылки на документацию</strong></p>
                    <div class="tools-grid" style="margin-top: 15px;">
                        <span class="tool-tag"><i class="fas fa-link"></i> BotHelp: Исходящие webhooks</span>
                        <span class="tool-tag"><i class="fas fa-link"></i> BotHelp: API документация</span>
                        <span class="tool-tag"><i class="fas fa-link"></i> OAuth2 client_credentials</span>
                        <span class="tool-tag"><i class="fas fa-link"></i> Telegram API: getFile</span>
                        <span class="tool-tag"><i class="fas fa-link"></i> FastAPI + BackgroundTasks</span>
                    </div>
                    
                    <p style="margin-top: 30px; text-align: center; font-weight: 600; font-size: 1.2rem;">КОНЕЦ ТЗ</p>
                </div>
            </div>
        </section>
        
        <!-- Сетка инструментов в виде тегов -->
        <section class="tools-section fade-in delay-3">
            <h2 class="section-title">Технологический стек</h2>
            <div class="tools-grid">
                <span class="tool-tag"><i class="fab fa-python"></i> FastAPI</span>
                <span class="tool-tag"><i class="fab fa-node-js"></i> Node.js</span>
                <span class="tool-tag"><i class="fas fa-database"></i> PostgreSQL</span>
                <span class="tool-tag"><i class="fas fa-cloud"></i> S3</span>
                <span class="tool-tag"><i class="fas fa-robot"></i> BotHelp API</span>
                <span class="tool-tag"><i class="fab fa-telegram"></i> Telegram Bot API</span>
                <span class="tool-tag"><i class="fas fa-webhook"></i> Webhooks</span>
                <span class="tool-tag"><i class="fas fa-lock"></i> OAuth2</span>
                <span class="tool-tag"><i class="fas fa-clock"></i> Celery</span>
                <span class="tool-tag"><i class="fas fa-file-excel"></i> OpenPyXL</span>
                <span class="tool-tag"><i class="fas fa-shield-alt"></i> Idempotency</span>
                <span class="tool-tag"><i class="fas fa-chart-line"></i> Monitoring</span>
            </div>
        </section>
        
        <!-- Футер -->
        <footer class="footer fade-in">
            <p><strong>Техническое задание</strong> | Версия 2.0 — Реалистичная, с учётом ограничений платформы</p>
            <p style="margin-top: 15px; color: var(--gray);">Интеграция системы сбора и проверки отчётности в Telegram-бот (BotHelp)</p>
            <p style="margin-top: 30px; font-size: 0.8rem;">© 2026 | Документ подготовлен в рамках проектирования архитектуры системы</p>
        </footer>
    </div>
    
    <!-- Кнопка наверх -->
    <div class="scroll-top" id="scrollTop">
        <i class="fas fa-arrow-up"></i>
    </div>

    <script>
        // Анимация элементов при прокрутке
        document.addEventListener('DOMContentLoaded', function() {
            const fadeElements = document.querySelectorAll('.fade-in');
            
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.style.opacity = 1;
                        entry.target.style.transform = 'translateY(0)';
                    }
                });
            }, { threshold: 0.1 });
            
            fadeElements.forEach(el => {
                el.style.opacity = 0;
                el.style.transform = 'translateY(30px)';
                el.style.transition = 'opacity 0.8s ease, transform 0.8s ease';
                observer.observe(el);
            });
            
            // Анимация карточек слайдов при наведении
            const slideCards = document.querySelectorAll('.slide-card');
            slideCards.forEach(card => {
                card.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-8px)';
                });
                
                card.addEventListener('mouseleave', function() {
                    this.style.transform = 'translateY(0)';
                });
            });
            
            // Анимация тегов
            const toolTags = document.querySelectorAll('.tool-tag');
            toolTags.forEach(tag => {
                tag.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-3px)';
                });
                
                tag.addEventListener('mouseleave', function() {
                    this.style.transform = 'translateY(0)';
                });
            });
            
            // Кнопка наверх
            const scrollTopBtn = document.getElementById('scrollTop');
            
            window.addEventListener('scroll', function() {
                if (window.pageYOffset > 300) {
                    scrollTopBtn.classList.add('visible');
                } else {
                    scrollTopBtn.classList.remove('visible');
                }
            });
            
            scrollTopBtn.addEventListener('click', function() {
                window.scrollTo({
                    top: 0,
                    behavior: 'smooth'
                });
            });
        });
        
        // Адаптация для мобильных устройств
        function checkMobile() {
            if (window.innerWidth <= 768) {
                document.body.classList.add('mobile');
            } else {
                document.body.classList.remove('mobile');
            }
        }
        
        window.addEventListener('load', checkMobile);
        window.addEventListener('resize', checkMobile);
    </script>
</body>
</html>
