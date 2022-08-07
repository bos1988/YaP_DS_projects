# Прогнозирование количества заказов такси
> **Чтобы привлекать больше водителей в период пиковой нагрузки, нужно спрогнозировать количество заказов такси на следующий час.**

<br/>

### Описание:

Компания такси собрала исторические данные о заказах такси в аэропортах. Чтобы привлекать больше водителей в период пиковой нагрузки, нужно спрогнозировать количество заказов такси на следующий час.

### Задачи:

- Загрузить данные и выполнить их ресемплирование по одному часу.
- Проанализировать данные.
- Обучить разные модели с различными гиперпараметрами.
- Проверить данные на тестовой выборке и сделать выводы.

### Данные:

Исторические данные о заказах такси в аэропортах.

Количество заказов находится в столбце ` num_orders `.

---

## v1:

### Используемые библиотеки:
*itertools<br/>lightgbm<br/>matplotlib<br/>numpy<br/>pandas<br/>scipy<br/>seaborn<br/>sklearn<br/>statsmodels<br/>time*

### Ключевые аспекты:

*EDA<br/>предобработка данных<br/>временные ряды<br/>регрессия<br/>градиентный бустинг<br/>масштабирование данных<br/>оценка важности признаков<br/>регуляризация<br/>подбор гиперпараметров<br/>выбор модели МО<br/>кросс-валидация<br/>Pipeline<br/>продакшн*

### Результат работы:

**1. Загружены и проанализированы исторические данные по количеству заказов такси:**
* В данных содержатся записи с **1 марта 2018 года** по **31 августа 2018** года включительно
* Аномалий и ошибок в данных нет
* Общий тренд на имеющихся данных - растущий
* Имеется недельная сезонность по дням недели
* Имеется суточная сезонность по часам

**2. Пободрана модель предсказания цены автомобиля:**

***Линейная регрессия с L1 регуляризацией без масштабирования признаков (Lasso)***
* Определен актуальный период для обучания модели
* 1464 часа (2 месяца)
* Созданы и отобраны лучшие признаки для обучанеия модели:
    * `hour` и `week`
    * смещения на `168`, `48` и `24` часа
    * скользящие `минимальные`, `максимальные` и `стандартные отклонения`
* Гиперпараметр модели `aplpha` подбирается автоматически при создании модели
* Качество предсказаний, выраженное в метрике **RMSE**, на тестовой выборке составляет порядка **35 заказов**

**2. Дополнительные замечания:**
* В загруженных данных представлен период менее полугода и наблюдаемый тренд может меняться в течение года. Поэтому, чтобы не терять в качестве модели нужно ее периодически переучивать.
* Хороший вариант - переобучивать модель при кажном предсказании. В силу простоты модели предсказание будет происходить по-прежнему быстро*, а актуальность данных используемых при обучении даст более точные результаты.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Настройка-окружения" data-toc-modified-id="Настройка-окружения-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Настройка окружения</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Функции" data-toc-modified-id="Функции-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Заменим-индексацию-данных-на-дату-время" data-toc-modified-id="Заменим-индексацию-данных-на-дату-время-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Заменим индексацию данных на дату-время</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Дата-время" data-toc-modified-id="Дата-время-5.1.1"><span class="toc-item-num">5.1.1&nbsp;&nbsp;</span>Дата-время</a></span><ul class="toc-item"><li><span><a href="#Дата-и-время" data-toc-modified-id="Дата-и-время-5.1.1.1"><span class="toc-item-num">5.1.1.1&nbsp;&nbsp;</span>Дата и время</a></span></li></ul></li><li><span><a href="#Числовые" data-toc-modified-id="Числовые-5.1.2"><span class="toc-item-num">5.1.2&nbsp;&nbsp;</span>Числовые</a></span><ul class="toc-item"><li><span><a href="#Количество-заказов" data-toc-modified-id="Количество-заказов-5.1.2.1"><span class="toc-item-num">5.1.2.1&nbsp;&nbsp;</span>Количество заказов</a></span></li></ul></li></ul></li><li><span><a href="#Тренды-и-сезонность" data-toc-modified-id="Тренды-и-сезонность-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Тренды и сезонность</a></span><ul class="toc-item"><li><span><a href="#Дневной" data-toc-modified-id="Дневной-5.2.1"><span class="toc-item-num">5.2.1&nbsp;&nbsp;</span>Дневной</a></span></li><li><span><a href="#Дневной" data-toc-modified-id="Дневной-5.2.2"><span class="toc-item-num">5.2.2&nbsp;&nbsp;</span>Дневной</a></span></li><li><span><a href="#Часовой" data-toc-modified-id="Часовой-5.2.3"><span class="toc-item-num">5.2.3&nbsp;&nbsp;</span>Часовой</a></span></li><li><span><a href="#Изменение-суммы-заказов-с-различными-периодами" data-toc-modified-id="Изменение-суммы-заказов-с-различными-периодами-5.2.4"><span class="toc-item-num">5.2.4&nbsp;&nbsp;</span>Изменение суммы заказов с различными периодами</a></span></li></ul></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Функция-подготовки-данных" data-toc-modified-id="Функция-подготовки-данных-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Функция подготовки данных</a></span></li><li><span><a href="#Отделим-тестовую-часть" data-toc-modified-id="Отделим-тестовую-часть-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Отделим тестовую часть</a></span></li></ul></li><li><span><a href="#Исследование-задачи" data-toc-modified-id="Исследование-задачи-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Исследование задачи</a></span><ul class="toc-item"><li><span><a href="#Подбор-базовой-модели" data-toc-modified-id="Подбор-базовой-модели-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Подбор базовой модели</a></span></li><li><span><a href="#Оценка-признаков-на-базовой-модели" data-toc-modified-id="Оценка-признаков-на-базовой-модели-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Оценка признаков на базовой модели</a></span><ul class="toc-item"><li><span><a href="#Линейная-регрессия-(Lasso)" data-toc-modified-id="Линейная-регрессия-(Lasso)-7.2.1"><span class="toc-item-num">7.2.1&nbsp;&nbsp;</span>Линейная регрессия (Lasso)</a></span></li></ul></li><li><span><a href="#Переподготовка-данных-(переоценка-признаков)" data-toc-modified-id="Переподготовка-данных-(переоценка-признаков)-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Переподготовка данных (переоценка признаков)</a></span></li><li><span><a href="#Подбор-гиперпараметров" data-toc-modified-id="Подбор-гиперпараметров-7.4"><span class="toc-item-num">7.4&nbsp;&nbsp;</span>Подбор гиперпараметров</a></span><ul class="toc-item"><li><span><a href="#Линейная-регрессия" data-toc-modified-id="Линейная-регрессия-7.4.1"><span class="toc-item-num">7.4.1&nbsp;&nbsp;</span>Линейная регрессия</a></span></li></ul></li></ul></li><li><span><a href="#Тестирование-модели" data-toc-modified-id="Тестирование-модели-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Тестирование модели</a></span><ul class="toc-item"><li><span><a href="#Тестовые-признаки" data-toc-modified-id="Тестовые-признаки-8.1"><span class="toc-item-num">8.1&nbsp;&nbsp;</span>Тестовые признаки</a></span></li><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-8.2"><span class="toc-item-num">8.2&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-8.3"><span class="toc-item-num">8.3&nbsp;&nbsp;</span>Проверка модели</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-8.4"><span class="toc-item-num">8.4&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li></ul></li><li><span><a href="#Рабочий-код" data-toc-modified-id="Рабочий-код-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Рабочий код</a></span><ul class="toc-item"><li><span><a href="#Основной-рабочий-код" data-toc-modified-id="Основной-рабочий-код-9.1"><span class="toc-item-num">9.1&nbsp;&nbsp;</span>Основной рабочий код</a></span><ul class="toc-item"><li><span><a href="#Рабочие-функции:" data-toc-modified-id="Рабочие-функции:-9.1.1"><span class="toc-item-num">9.1.1&nbsp;&nbsp;</span>Рабочие функции:</a></span></li><li><span><a href="#Отработка-кода:" data-toc-modified-id="Отработка-кода:-9.1.2"><span class="toc-item-num">9.1.2&nbsp;&nbsp;</span>Отработка кода:</a></span></li><li><span><a href="#Анализ-результатов:" data-toc-modified-id="Анализ-результатов:-9.1.3"><span class="toc-item-num">9.1.3&nbsp;&nbsp;</span>Анализ результатов:</a></span></li><li><span><a href="#Анализ-остатков" data-toc-modified-id="Анализ-остатков-9.1.4"><span class="toc-item-num">9.1.4&nbsp;&nbsp;</span>Анализ остатков</a></span></li></ul></li><li><span><a href="#Пробный-рабочий-код" data-toc-modified-id="Пробный-рабочий-код-9.2"><span class="toc-item-num">9.2&nbsp;&nbsp;</span>Пробный рабочий код</a></span></li></ul></li><li><span><a href="#Общие-выводы" data-toc-modified-id="Общие-выводы-10"><span class="toc-item-num">10&nbsp;&nbsp;</span>Общие выводы</a></span></li></ul></div>
