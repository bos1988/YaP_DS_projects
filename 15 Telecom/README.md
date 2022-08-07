# Прогнозирование оттока клиентов телеком компании
> **Прогнозирование оттока клиентов на основе исторических данных оператора связи.**

<br/>

### Описание:

Оператор связи хочет научиться прогнозировать отток клиентов. Если выяснится, что пользователь планирует уйти, ему будут предложены промокоды и специальные условия. Команда оператора собрала персональные данные о некоторых клиентах, информацию об их тарифах и договорах.

### Задачи:

- Создать модель, определяющую потенциальных клиентов на уход от оператора связи.

### Данные:

Данные оператора связи о некоторых клиентах, информация об их тарифах и договорах

Таблица *contract* (информация о договоре):
- ` customerID ` – Код клиента
- ` BeginDate ` – Дата заключения договора
- ` EndDate ` – Дата окончания договора
- ` Type ` – Тип договора
- ` PaperlessBilling ` – Возможность получения электронного чека
- ` PaymentMethod ` – Метод оплаты
- ` MonthlyCharges ` – Ежемесячный платеж
- ` TotalCharges ` – Общие платежи

Таблица *personal* (персональные данные клиента):
- ` customerID ` – Код клиента
- ` gender ` – Пол клиента
- ` SeniorCitizen ` – Пожилой клиент
- ` Partner ` – Партнер
- ` Dependents ` – Иждивенцы

Таблица *internet* (информация об интернет-услугах):
- ` customerID ` – Код клиента
- ` InternetService ` – Тип подключения интернета
- ` OnlineSecurity ` – Подключени услуги "Интернет-безопасность: блокировка небезопасных сайтов"
- ` OnlineBackup ` – Подключение услуги "Облачное хранилище файлов для резервного копирования данных"
- ` DeviceProtection ` – Подключени услуги "Интернет-безопасность: антивирус"
- ` TechSupport ` – Подключение услуги "Выделенная линия технической поддержки"
- ` StreamingTV ` – Подключение услуги "Стриминговое телевидение"
- ` StreamingMovies ` – Подключение услуги "Каталог фильмов"

Таблица *phone* (информация об услугах телефонии):
- ` customerID ` – Код клиента
- ` MultipleLines ` – Наличие услуги подключения телефонного аппарата к нескольким линиям одновременно

---

## v1:

### Используемые библиотеки:
*lightgbm<br/>matplotlib<br/>numpy<br/>pandas<br/>seaborn<br/>sklearn<br/>time*

### Ключевые аспекты:

*EDA<br/>предобработка данных<br/>объединение таблиц<br/>утечка целевого признака<br/>классификация<br/>дисбаланс классов<br/>градиентный бустинг<br/>прямое кодирование<br/>порядковое кодирование<br/>масштабирование данных<br/>оценка важности признкаков<br/>регуляризация<br/>подбор гиперпараметров<br/>выбор модели МО<br/>кросс-валидация<br/>Pipeline<br/>продакшн*

### Результат работы:

**1. Проанализированы и обработаны исторические данные клиентской базы данных оператора связи:**

* В данных имеются пропуски общих платежей вновь пришедших клиентов, не производивших еще оплат (заманены нулями)
* Смсловых противоречий в данных не обнаружено
* Создан новый признак `n_pays` - ориентировочное количество ежемесячных платежей (на основании `TotalCharges` и `MonthlyCharges`)
* Проанализирована и исключена утечка целевого признака через другие признаки при создании модели
* Обнаружены выдающиеся отклонения от общей картины некоторых параметров в последние 4 месяца рассматриваемого периода данных:
    * Увеличение оттока клиентов
    * Увеличение нижней границы ежемесячных платежей
    * Увеличение притока клиентов

**2. Итоговая модель предсказания ухода клиентов впредставляет из себя ансамбль из трех отобранных моделей:**

* ***LGBMClassifier(learning_rate=0.24, max_depth=2, num_leaves=2, n_estimators=160, random_state=123)***<br/>
* ***RandomForestClassifier(max_depth=8, n_estimators=130, min_samples_leaf=3, min_samples_split=3, random_state=123)***-<br/>
* ***LogisticRegression(penalty=l2, C=150, random_state=123) с масштабированием признаков***

* Ансамбль более устойчив к переобучению и показал лучшие результаты на тесте метрик
* Для рабочей модели создан рабочий код, проверена работоспособность на тестовой выборке, подтверждена адекватность
* Качество предсказаний, выраженное в метрике **ROC_AUC**, составило порядка **`0.855`**
* Точность модели (accuracy) немногим выше константной модели
* Подбором порога предсказаний можно подстроить соотношение упущенных клиентов и ошибочных промо акций.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><ul class="toc-item"><li><span><a href="#Описание-услуг" data-toc-modified-id="Описание-услуг-0.1"><span class="toc-item-num">0.1&nbsp;&nbsp;</span>Описание услуг</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-0.2"><span class="toc-item-num">0.2&nbsp;&nbsp;</span>Описание данных</a></span></li></ul></li><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Настройка-окружения" data-toc-modified-id="Настройка-окружения-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Настройка окружения</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Функции" data-toc-modified-id="Функции-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Функции</a></span></li></ul></li><li><span><a href="#Исследование-первой-таблицы-(Информация-о-договоре)" data-toc-modified-id="Исследование-первой-таблицы-(Информация-о-договоре)-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Исследование первой таблицы (Информация о договоре)</a></span><ul class="toc-item"><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-3.1.1"><span class="toc-item-num">3.1.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-3.1.2"><span class="toc-item-num">3.1.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-3.1.3"><span class="toc-item-num">3.1.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.1.4"><span class="toc-item-num">3.1.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-3.2.1"><span class="toc-item-num">3.2.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-3.2.2"><span class="toc-item-num">3.2.2&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Дата-заключения-договора-(BeginDate)" data-toc-modified-id="Дата-заключения-договора-(BeginDate)-3.2.2.1"><span class="toc-item-num">3.2.2.1&nbsp;&nbsp;</span>Дата заключения договора (BeginDate)</a></span></li><li><span><a href="#Дата-окончания-договора-(EndDate)" data-toc-modified-id="Дата-окончания-договора-(EndDate)-3.2.2.2"><span class="toc-item-num">3.2.2.2&nbsp;&nbsp;</span>Дата окончания договора (EndDate)</a></span></li><li><span><a href="#Общие-платежи-(TotalCharges)" data-toc-modified-id="Общие-платежи-(TotalCharges)-3.2.2.3"><span class="toc-item-num">3.2.2.3&nbsp;&nbsp;</span>Общие платежи (TotalCharges)</a></span></li><li><span><a href="#Булевые-признаки(PaperlessBilling)" data-toc-modified-id="Булевые-признаки(PaperlessBilling)-3.2.2.4"><span class="toc-item-num">3.2.2.4&nbsp;&nbsp;</span>Булевые признаки(PaperlessBilling)</a></span></li><li><span><a href="#Перепроверим-типы-данных" data-toc-modified-id="Перепроверим-типы-данных-3.2.2.5"><span class="toc-item-num">3.2.2.5&nbsp;&nbsp;</span>Перепроверим типы данных</a></span></li></ul></li><li><span><a href="#Вывод-по-предобработке" data-toc-modified-id="Вывод-по-предобработке-3.2.3"><span class="toc-item-num">3.2.3&nbsp;&nbsp;</span>Вывод по предобработке</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-3.3.1"><span class="toc-item-num">3.3.1&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-3.3.2"><span class="toc-item-num">3.3.2&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Дата-время" data-toc-modified-id="Дата-время-3.3.2.1"><span class="toc-item-num">3.3.2.1&nbsp;&nbsp;</span>Дата-время</a></span></li><li><span><a href="#Категории" data-toc-modified-id="Категории-3.3.2.2"><span class="toc-item-num">3.3.2.2&nbsp;&nbsp;</span>Категории</a></span></li><li><span><a href="#Числовые" data-toc-modified-id="Числовые-3.3.2.3"><span class="toc-item-num">3.3.2.3&nbsp;&nbsp;</span>Числовые</a></span></li></ul></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-3.3.3"><span class="toc-item-num">3.3.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-3.3.4"><span class="toc-item-num">3.3.4&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li></ul></li><li><span><a href="#Исследование-второй-таблицы-(Персональные-данные-клиента)" data-toc-modified-id="Исследование-второй-таблицы-(Персональные-данные-клиента)-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Исследование второй таблицы (Персональные данные клиента)</a></span><ul class="toc-item"><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-4.1.1"><span class="toc-item-num">4.1.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-4.1.2"><span class="toc-item-num">4.1.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-4.1.3"><span class="toc-item-num">4.1.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-4.1.4"><span class="toc-item-num">4.1.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-4.2.1"><span class="toc-item-num">4.2.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-4.2.2"><span class="toc-item-num">4.2.2&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Булевые-признаки(Partner,-Dependents)" data-toc-modified-id="Булевые-признаки(Partner,-Dependents)-4.2.2.1"><span class="toc-item-num">4.2.2.1&nbsp;&nbsp;</span>Булевые признаки(Partner, Dependents)</a></span></li><li><span><a href="#Перепроверим-типы-данных" data-toc-modified-id="Перепроверим-типы-данных-4.2.2.2"><span class="toc-item-num">4.2.2.2&nbsp;&nbsp;</span>Перепроверим типы данных</a></span></li></ul></li><li><span><a href="#Вывод-по-предобработке" data-toc-modified-id="Вывод-по-предобработке-4.2.3"><span class="toc-item-num">4.2.3&nbsp;&nbsp;</span>Вывод по предобработке</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-4.3.1"><span class="toc-item-num">4.3.1&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-4.3.2"><span class="toc-item-num">4.3.2&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-4.3.3"><span class="toc-item-num">4.3.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-4.3.4"><span class="toc-item-num">4.3.4&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li></ul></li><li><span><a href="#Исследование-третьей-таблицы-(Информация-об-интернет-услугах)" data-toc-modified-id="Исследование-третьей-таблицы-(Информация-об-интернет-услугах)-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Исследование третьей таблицы (Информация об интернет-услугах)</a></span><ul class="toc-item"><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-5.1.1"><span class="toc-item-num">5.1.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-5.1.2"><span class="toc-item-num">5.1.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-5.1.3"><span class="toc-item-num">5.1.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-5.1.4"><span class="toc-item-num">5.1.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-5.2.1"><span class="toc-item-num">5.2.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-5.2.2"><span class="toc-item-num">5.2.2&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Булевые-признаки(OnlineSecurity,-OnlineBackup,-DeviceProtection,-TechSupport,-StreamingTV,-StreamingMovies)" data-toc-modified-id="Булевые-признаки(OnlineSecurity,-OnlineBackup,-DeviceProtection,-TechSupport,-StreamingTV,-StreamingMovies)-5.2.2.1"><span class="toc-item-num">5.2.2.1&nbsp;&nbsp;</span>Булевые признаки(OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies)</a></span></li><li><span><a href="#Перепроверим-типы-данных" data-toc-modified-id="Перепроверим-типы-данных-5.2.2.2"><span class="toc-item-num">5.2.2.2&nbsp;&nbsp;</span>Перепроверим типы данных</a></span></li></ul></li><li><span><a href="#Вывод-по-предобработке" data-toc-modified-id="Вывод-по-предобработке-5.2.3"><span class="toc-item-num">5.2.3&nbsp;&nbsp;</span>Вывод по предобработке</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-5.3.1"><span class="toc-item-num">5.3.1&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-5.3.2"><span class="toc-item-num">5.3.2&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Категории" data-toc-modified-id="Категории-5.3.2.1"><span class="toc-item-num">5.3.2.1&nbsp;&nbsp;</span>Категории</a></span></li></ul></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-5.3.3"><span class="toc-item-num">5.3.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-5.3.4"><span class="toc-item-num">5.3.4&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li></ul></li><li><span><a href="#Исследование-Четвертой-таблицы-(Информация-об-услугах-телефонии)" data-toc-modified-id="Исследование-Четвертой-таблицы-(Информация-об-услугах-телефонии)-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Исследование Четвертой таблицы (Информация об услугах телефонии)</a></span><ul class="toc-item"><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-6.1.1"><span class="toc-item-num">6.1.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-6.1.2"><span class="toc-item-num">6.1.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-6.1.3"><span class="toc-item-num">6.1.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-6.1.4"><span class="toc-item-num">6.1.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-6.2.1"><span class="toc-item-num">6.2.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-6.2.2"><span class="toc-item-num">6.2.2&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Булевые-признаки(MultipleLines)" data-toc-modified-id="Булевые-признаки(MultipleLines)-6.2.2.1"><span class="toc-item-num">6.2.2.1&nbsp;&nbsp;</span>Булевые признаки(MultipleLines)</a></span></li><li><span><a href="#Перепроверим-типы-данных" data-toc-modified-id="Перепроверим-типы-данных-6.2.2.2"><span class="toc-item-num">6.2.2.2&nbsp;&nbsp;</span>Перепроверим типы данных</a></span></li></ul></li><li><span><a href="#Вывод-по-предобработке" data-toc-modified-id="Вывод-по-предобработке-6.2.3"><span class="toc-item-num">6.2.3&nbsp;&nbsp;</span>Вывод по предобработке</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-6.3.1"><span class="toc-item-num">6.3.1&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-6.3.2"><span class="toc-item-num">6.3.2&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-6.3.3"><span class="toc-item-num">6.3.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-6.3.4"><span class="toc-item-num">6.3.4&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li></ul></li><li><span><a href="#Общ
ая-таблица-данных" data-toc-modified-id="Общая-таблица-данных-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Общая таблица данных</a></span><ul class="toc-item"><li><span><a href="#Объединение-таблиц" data-toc-modified-id="Объединение-таблиц-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Объединение таблиц</a></span><ul class="toc-item"><li><span><a href="#Дополнение-данных:" data-toc-modified-id="Дополнение-данных:-7.1.1"><span class="toc-item-num">7.1.1&nbsp;&nbsp;</span>Дополнение данных:</a></span><ul class="toc-item"><li><span><a href="#Информация-об-интернет-услугах" data-toc-modified-id="Информация-об-интернет-услугах-7.1.1.1"><span class="toc-item-num">7.1.1.1&nbsp;&nbsp;</span>Информация об интернет-услугах</a></span></li><li><span><a href="#Информация-об-услугах-телефонии" data-toc-modified-id="Информация-об-услугах-телефонии-7.1.1.2"><span class="toc-item-num">7.1.1.2&nbsp;&nbsp;</span>Информация об услугах телефонии</a></span></li></ul></li></ul></li><li><span><a href="#Общая-таблица" data-toc-modified-id="Общая-таблица-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Общая таблица</a></span></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-услуг-(целевой-признак)" data-toc-modified-id="Проверка-услуг-(целевой-признак)-7.3.1"><span class="toc-item-num">7.3.1&nbsp;&nbsp;</span>Проверка услуг (целевой признак)</a></span></li><li><span><a href="#Анализ-временных-промежутков" data-toc-modified-id="Анализ-временных-промежутков-7.3.2"><span class="toc-item-num">7.3.2&nbsp;&nbsp;</span>Анализ временных промежутков</a></span></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-7.3.3"><span class="toc-item-num">7.3.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Оценка-дисбаланса" data-toc-modified-id="Оценка-дисбаланса-7.3.4"><span class="toc-item-num">7.3.4&nbsp;&nbsp;</span>Оценка дисбаланса</a></span></li><li><span><a href="#Отбор-признаков" data-toc-modified-id="Отбор-признаков-7.3.5"><span class="toc-item-num">7.3.5&nbsp;&nbsp;</span>Отбор признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-7.3.6"><span class="toc-item-num">7.3.6&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Функция-подготовки-данных" data-toc-modified-id="Функция-подготовки-данных-8.1"><span class="toc-item-num">8.1&nbsp;&nbsp;</span>Функция подготовки данных</a></span></li><li><span><a href="#Отделим-тестовую-часть" data-toc-modified-id="Отделим-тестовую-часть-8.2"><span class="toc-item-num">8.2&nbsp;&nbsp;</span>Отделим тестовую часть</a></span><ul class="toc-item"><li><span><a href="#Без-кодирования" data-toc-modified-id="Без-кодирования-8.2.1"><span class="toc-item-num">8.2.1&nbsp;&nbsp;</span>Без кодирования</a></span></li><li><span><a href="#Прямое-кодирование" data-toc-modified-id="Прямое-кодирование-8.2.2"><span class="toc-item-num">8.2.2&nbsp;&nbsp;</span>Прямое кодирование</a></span></li><li><span><a href="#Порядковое-кодирование" data-toc-modified-id="Порядковое-кодирование-8.2.3"><span class="toc-item-num">8.2.3&nbsp;&nbsp;</span>Порядковое кодирование</a></span></li></ul></li></ul></li><li><span><a href="#Разработка-модели" data-toc-modified-id="Разработка-модели-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Разработка модели</a></span><ul class="toc-item"><li><span><a href="#Подбор-базовой-модели" data-toc-modified-id="Подбор-базовой-модели-9.1"><span class="toc-item-num">9.1&nbsp;&nbsp;</span>Подбор базовой модели</a></span></li><li><span><a href="#Оценка-признаков-на-базовой-модели" data-toc-modified-id="Оценка-признаков-на-базовой-модели-9.2"><span class="toc-item-num">9.2&nbsp;&nbsp;</span>Оценка признаков на базовой модели</a></span><ul class="toc-item"><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-9.2.1"><span class="toc-item-num">9.2.1&nbsp;&nbsp;</span>Градиентный бустинг</a></span></li><li><span><a href="#Случайный-лес" data-toc-modified-id="Случайный-лес-9.2.2"><span class="toc-item-num">9.2.2&nbsp;&nbsp;</span>Случайный лес</a></span></li><li><span><a href="#Логистическая-регрессия" data-toc-modified-id="Логистическая-регрессия-9.2.3"><span class="toc-item-num">9.2.3&nbsp;&nbsp;</span>Логистическая регрессия</a></span></li><li><span><a href="#Логистическая-регрессия-с-градиентным-спуском" data-toc-modified-id="Логистическая-регрессия-с-градиентным-спуском-9.2.4"><span class="toc-item-num">9.2.4&nbsp;&nbsp;</span>Логистическая регрессия с градиентным спуском</a></span></li></ul></li><li><span><a href="#Подбор-гиперпараметров" data-toc-modified-id="Подбор-гиперпараметров-9.3"><span class="toc-item-num">9.3&nbsp;&nbsp;</span>Подбор гиперпараметров</a></span><ul class="toc-item"><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-9.3.1"><span class="toc-item-num">9.3.1&nbsp;&nbsp;</span>Градиентный бустинг</a></span></li><li><span><a href="#Случайный-лес" data-toc-modified-id="Случайный-лес-9.3.2"><span class="toc-item-num">9.3.2&nbsp;&nbsp;</span>Случайный лес</a></span></li><li><span><a href="#Логистическая-регрессия" data-toc-modified-id="Логистическая-регрессия-9.3.3"><span class="toc-item-num">9.3.3&nbsp;&nbsp;</span>Логистическая регрессия</a></span></li><li><span><a href="#Логистическая-регрессия-с-градиентым-спуском" data-toc-modified-id="Логистическая-регрессия-с-градиентым-спуском-9.3.4"><span class="toc-item-num">9.3.4&nbsp;&nbsp;</span>Логистическая регрессия с градиентым спуском</a></span></li></ul></li><li><span><a href="#Выбор-лучшей-модели" data-toc-modified-id="Выбор-лучшей-модели-9.4"><span class="toc-item-num">9.4&nbsp;&nbsp;</span>Выбор лучшей модели</a></span></li></ul></li><li><span><a href="#Тестирование-модели" data-toc-modified-id="Тестирование-модели-10"><span class="toc-item-num">10&nbsp;&nbsp;</span>Тестирование модели</a></span><ul class="toc-item"><li><span><a href="#Тестовые-признаки" data-toc-modified-id="Тестовые-признаки-10.1"><span class="toc-item-num">10.1&nbsp;&nbsp;</span>Тестовые признаки</a></span></li><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-10.2"><span class="toc-item-num">10.2&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-10.3"><span class="toc-item-num">10.3&nbsp;&nbsp;</span>Проверка модели</a></span><ul class="toc-item"><li><span><a href="#Простые-модели" data-toc-modified-id="Простые-модели-10.3.1"><span class="toc-item-num">10.3.1&nbsp;&nbsp;</span>Простые модели</a></span></li><li><span><a href="#Ансамбли" data-toc-modified-id="Ансамбли-10.3.2"><span class="toc-item-num">10.3.2&nbsp;&nbsp;</span>Ансамбли</a></span></li></ul></li><li><span><a href="#ROC-кривая" data-toc-modified-id="ROC-кривая-10.4"><span class="toc-item-num">10.4&nbsp;&nbsp;</span>ROC-кривая</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-10.5"><span class="toc-item-num">10.5&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li></ul></li><li><span><a href="#Рабочий-код" data-toc-modified-id="Рабочий-код-11"><span class="toc-item-num">11&nbsp;&nbsp;</span>Рабочий код</a></span></li><li><span><a href="#Общие-выводы" data-toc-modified-id="Общие-выводы-12"><span class="toc-item-num">12&nbsp;&nbsp;</span>Общие выводы</a></span></li></ul></div>