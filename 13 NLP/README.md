# Классификация комментариев
> **Определение токсичности комментариев.**

<br/>

### Описание:

Интернет-магазин запускает новый сервис. Теперь пользователи могут редактировать и дополнять описания товаров, как в вики-сообществах. То есть клиенты предлагают свои правки и комментируют изменения других. Требуется инструмент, который будет искать токсичные комментарии и отправлять их на модерацию.

### Задачи:

- Обучить модель классифицировать комментарии на позитивные и негативные.

### Данные:

Набор данных с разметкой о токсичности правок.

Столбец ` text ` в нём содержит текст комментария, а ` toxic ` – целевой признак.

---

## v1:

### Используемые библиотеки:
*lightgbm<br/>matplotlib<br/>nltk<br/>numpy<br/>pandas<br/>re<br/>sklearn<br/>spacy<br/>time*

### Ключевые аспекты:

*обработка естественного языка<br/>NLP<br/>nltk<br/>tf-idf<br/>классификация<br/>градиентный бустинг<br/>подбор гиперпараметров<br/>выбор модели МО<br/>кросс-валидация<br/>Pipeline<br/>продакшн*

### Результат работы:

**1. Загружен набор комментариев с разметкой о токсичности и подготовлен к работе с моделями**
* В среднем тексты комментариев содержат 400 символов, но есть и выбросы с количеством символов до 5000.
* Тексты комментариев леммаизированы с помощью билиотеки `spaCy`.
* Преобразование текстов техникой `мешка слов` имеет преимущество перед техникой `оценки важности слова величиной TF-IDF`

**2. Пободрана модель предсказания токсичности комментария:**

***LGBMRegressor(class_weight='balanced', learning_rate=0.3, max_depth=85, n_estimators=150, num_leaves=150, random_state=123)***
* Модель градиентного бустинга имеет лучшую точность предсказаний, но скорость работы ниже других рассматриваемых моделей
* Для модели градиентного бустинга подобраны гиперапараметры, создан рабочий код, проверена работоспособность на тестовой выборке
* Качество предсказаний, выраженное в метрике F1, составило порядка 0.775

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Функции" data-toc-modified-id="Функции-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Длина-текста" data-toc-modified-id="Длина-текста-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Длина текста</a></span></li><li><span><a href="#Оценка-дисбаланса" data-toc-modified-id="Оценка-дисбаланса-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Оценка дисбаланса</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Функция-подготовки-данных" data-toc-modified-id="Функция-подготовки-данных-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Функция подготовки данных</a></span></li><li><span><a href="#Отделим-тестовую-часть" data-toc-modified-id="Отделим-тестовую-часть-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Отделим тестовую часть</a></span></li></ul></li><li><span><a href="#Исследование-задачи" data-toc-modified-id="Исследование-задачи-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Исследование задачи</a></span><ul class="toc-item"><li><span><a href="#Подбор-базовой-модели" data-toc-modified-id="Подбор-базовой-модели-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Подбор базовой модели</a></span></li><li><span><a href="#Подбор-гиперпараметров" data-toc-modified-id="Подбор-гиперпараметров-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Подбор гиперпараметров</a></span><ul class="toc-item"><li><span><a href="#Логистическая-регрессия" data-toc-modified-id="Логистическая-регрессия-6.2.1"><span class="toc-item-num">6.2.1&nbsp;&nbsp;</span>Логистическая регрессия</a></span></li><li><span><a href="#Логистическая-регрессия-с-градиентным-спуском" data-toc-modified-id="Логистическая-регрессия-с-градиентным-спуском-6.2.2"><span class="toc-item-num">6.2.2&nbsp;&nbsp;</span>Логистическая регрессия с градиентным спуском</a></span></li><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-6.2.3"><span class="toc-item-num">6.2.3&nbsp;&nbsp;</span>Градиентный бустинг</a></span></li></ul></li><li><span><a href="#Выбор-лучшей-модели" data-toc-modified-id="Выбор-лучшей-модели-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Выбор лучшей модели</a></span></li></ul></li><li><span><a href="#Тестирование-модели" data-toc-modified-id="Тестирование-модели-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Тестирование модели</a></span><ul class="toc-item"><li><span><a href="#Тестовые-признаки" data-toc-modified-id="Тестовые-признаки-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Тестовые признаки</a></span></li><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Градиентный бустинг</a></span><ul class="toc-item"><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-7.2.1"><span class="toc-item-num">7.2.1&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-7.2.2"><span class="toc-item-num">7.2.2&nbsp;&nbsp;</span>Проверка модели</a></span></li></ul></li><li><span><a href="#Логистическая-регрессия" data-toc-modified-id="Логистическая-регрессия-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Логистическая регрессия</a></span><ul class="toc-item"><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-7.3.1"><span class="toc-item-num">7.3.1&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-7.3.2"><span class="toc-item-num">7.3.2&nbsp;&nbsp;</span>Проверка модели</a></span></li></ul></li><li><span><a href="#Рабочая-модель" data-toc-modified-id="Рабочая-модель-7.4"><span class="toc-item-num">7.4&nbsp;&nbsp;</span>Рабочая модель</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-7.5"><span class="toc-item-num">7.5&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li></ul></li><li><span><a href="#Рабочий-код" data-toc-modified-id="Рабочий-код-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Рабочий код</a></span></li><li><span><a href="#Общие-выводы" data-toc-modified-id="Общие-выводы-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Общие выводы</a></span></li></ul></div>
