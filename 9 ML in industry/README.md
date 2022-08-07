# Исследование технологического процесса очистки золота
> **Прогнозирование концентрации золота при проведении процесса очистки золота.**

<br/>

### Описание:

Нужно подготовить прототип модели машинного обучения для компании, разрабатывающей решения для эффективной работы промышленных предприятий.<br/>
Модель должна предсказать коэффициент восстановления золота из золотосодержащей руды.<br/>
Модель поможет оптимизировать производство, чтобы не запускать предприятие с убыточными характеристиками.<br/>

### Задачи:

- Подготовить данные.
- Провести исследовательский анализ данных.
- Построить и обучить модель.

### Данные:

Сырые данные с параметрами технологического процесса добычи и очистки.

Технологический процесс
- ` Rougher feed ` – исходное сырье
- ` Rougher additions (или reagent additions) ` – флотационные реагенты: Xanthate, Sulphate, Depressant
- ` Rougher process (англ. «грубый процесс») ` – флотация
- ` Rougher tails ` – отвальные хвосты
- ` Float banks ` – флотационная установка
- ` Cleaner process ` – очистка
- ` Rougher Au ` – черновой концентрат золота
- ` Final Au ` – финальный концентрат золота

Параметры этапов
- ` air amount ` – объём воздуха
- ` fluid levels ` – уровень жидкости
- ` feed size ` – размер гранул сырья
- ` feed rate ` – скорость подачи

Наименование признаков
- ` rougher ` – флотация
- ` primary_cleaner ` – первичная очистка
- ` secondary_cleaner ` – вторичная очистка
- ` final ` – финальные характеристики
- ` input ` – параметры сырья
- ` output ` – параметры продукта
- ` state ` – параметры, характеризующие текущее состояние этапа
- ` calculation ` – расчётные характеристики

---

## v1:

### Используемые библиотеки:
*matplotlib<br/>numpy<br/>pandas<br/>scipy<br/>seaborn<br/>sklearn*

### Ключевые аспекты:

*EDA<br/>предобработка данных<br/>регрессия<br/>кастомные метрики<br/>масштабирование данных<br/>подбор гиперпараметров<br/>выбор модели МО<br/>кросс-валидация*

### Результат работы:

1. Была проведена работа по разработке прототип модели машинного,<br/> определяющей коэффициент восстановления золота из золотосодержащей руды.

1. В исходных данных имеются множественные пропуски значений
    - Исходные данные (таблица `full`) оставлена без изменений пропусков
    - В тестовой выборке (таблица `test`) удалены все пропуски
    - В обучающей выборке (таблица `train`) пропуски с большой долей заменены на медианные значения, а пропуски с малой долей удалены

1. Признак `date` приведен к типу даты, все осталные данные - численные с соответствующим типом. 1. Исправлены аномальные концентрации по стадиям процесса.

1. В данных имеются значительные выбросы по размерам гранул на вход во влотационную установку (оставлены без изменений).

1. Распределение размеров гранул сырья на тестовой и обучающей выборках разные,<br/> соовтетсвенно значения получаемых метрик имеют низкую точность.

1. Если нужна скорость работы модели, то можно не расссмативать признаки уровня жидкости.<br/> Без данных признаков размер данных значительно меньше, а точность модели практически не страдает.

1. Без обработки данных моделирование не имеет смысла, поскольку в таком случае метрики хуже, чем на констатной модели средних величин.

1. Подобрана модель с лучшими показателями `sMAPE`, это:
    - **Линейная регрессия** для для нахождения эффективности обогащения чернового концентрата
    - **Случайный лес** с гиперпараметрами: `max_depth =`**`5`**, `n_estimators =`**`15`**, `min_samples_leaf =`**`3`**, `min_samples_split =`**`2`**<br/> для нахождения эффективности обогащения финального концентрата.

1. По средним величинам эффективности обогащения значение итогового `sMAPE` = **0.0930 (9.30%)**.

1. Тест модели показал значение итогового `sMAPE` = **0.0904 (9.04%)**.

1. Полученная модель имеет слабую предсказательную силу.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Условия-задачи" data-toc-modified-id="Условия-задачи-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Условия задачи</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Настройка-окружения" data-toc-modified-id="Настройка-окружения-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Настройка окружения</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Общие-функции" data-toc-modified-id="Общие-функции-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Общие функции</a></span></li><li><span><a href="#Функции-машинного-обучения" data-toc-modified-id="Функции-машинного-обучения-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Функции машинного обучения</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Проверка пропусков</a></span><ul class="toc-item"><li><span><a href="#Удалим-пропуски-из-тестовой-выборки:" data-toc-modified-id="Удалим-пропуски-из-тестовой-выборки:-5.3.1"><span class="toc-item-num">5.3.1&nbsp;&nbsp;</span>Удалим пропуски из тестовой выборки:</a></span></li><li><span><a href="#Обработаем-пропуски-из-обучающей-выборки:" data-toc-modified-id="Обработаем-пропуски-из-обучающей-выборки:-5.3.2"><span class="toc-item-num">5.3.2&nbsp;&nbsp;</span>Обработаем пропуски из обучающей выборки:</a></span></li></ul></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Замена типов данных</a></span></li><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-5.6"><span class="toc-item-num">5.6&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Объемы-воздуха-и-Уровень-жидкости:" data-toc-modified-id="Объемы-воздуха-и-Уровень-жидкости:-5.6.1"><span class="toc-item-num">5.6.1&nbsp;&nbsp;</span>Объемы воздуха и Уровень жидкости:</a></span></li><li><span><a href="#Размер-гранул-сырья:" data-toc-modified-id="Размер-гранул-сырья:-5.6.2"><span class="toc-item-num">5.6.2&nbsp;&nbsp;</span>Размер гранул сырья:</a></span></li><li><span><a href="#Скорость-подачи:" data-toc-modified-id="Скорость-подачи:-5.6.3"><span class="toc-item-num">5.6.3&nbsp;&nbsp;</span>Скорость подачи:</a></span></li><li><span><a href="#Исходное-сырье:" data-toc-modified-id="Исходное-сырье:-5.6.4"><span class="toc-item-num">5.6.4&nbsp;&nbsp;</span>Исходное сырье:</a></span></li><li><span><a href="#Депрессант-(силикат-натрия).:" data-toc-modified-id="Депрессант-(силикат-натрия).:-5.6.5"><span class="toc-item-num">5.6.5&nbsp;&nbsp;</span>Депрессант (силикат натрия).:</a></span></li><li><span><a href="#Ксантогенат-(промотер,-или-активатор-флотации):" data-toc-modified-id="Ксантогенат-(промотер,-или-активатор-флотации):-5.6.6"><span class="toc-item-num">5.6.6&nbsp;&nbsp;</span>Ксантогенат (промотер, или активатор флотации):</a></span></li><li><span><a href="#Сульфат-(сульфид-натрия):" data-toc-modified-id="Сульфат-(сульфид-натрия):-5.6.7"><span class="toc-item-num">5.6.7&nbsp;&nbsp;</span>Сульфат (сульфид натрия):</a></span></li><li><span><a href="#Целевые-признаки-(эффективность-обогащения):" data-toc-modified-id="Целевые-признаки-(эффективность-обогащения):-5.6.8"><span class="toc-item-num">5.6.8&nbsp;&nbsp;</span>Целевые признаки (эффективность обогащения):</a></span></li><li><span><a href="#Выводы:" data-toc-modified-id="Выводы:-5.6.9"><span class="toc-item-num">5.6.9&nbsp;&nbsp;</span>Выводы:</a></span></li><li><span><a href="#Проверка-исправление:" data-toc-modified-id="Проверка-исправление:-5.6.10"><span class="toc-item-num">5.6.10&nbsp;&nbsp;</span>Проверка-исправление:</a></span></li></ul></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Концентрация-металлов-(Au,-Ag,-Pb)-на-различных-этапах-очистки." data-toc-modified-id="Концентрация-металлов-(Au,-Ag,-Pb)-на-различных-этапах-очистки.-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Концентрация металлов (Au, Ag, Pb) на различных этапах очистки.</a></span><ul class="toc-item"><li><span><a href="#Золото" data-toc-modified-id="Золото-6.1.1"><span class="toc-item-num">6.1.1&nbsp;&nbsp;</span>Золото</a></span></li><li><span><a href="#Серебро" data-toc-modified-id="Серебро-6.1.2"><span class="toc-item-num">6.1.2&nbsp;&nbsp;</span>Серебро</a></span></li><li><span><a href="#Свинец" data-toc-modified-id="Свинец-6.1.3"><span class="toc-item-num">6.1.3&nbsp;&nbsp;</span>Свинец</a></span></li></ul></li><li><span><a href="#Сравнение-распределения-размеров-гранул-сырья-на-обучающей-и-тестовой-выборках." data-toc-modified-id="Сравнение-распределения-размеров-гранул-сырья-на-обучающей-и-тестовой-выборках.-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Сравнение распределения размеров гранул сырья на обучающей и тестовой выборках.</a></span></li><li><span><a href="#Суммарная-концентрацию-всех-веществ-на-разных-стадиях:-в-сырье,-в-черновом-и-финальном-концентратах." data-toc-modified-id="Суммарная-концентрацию-всех-веществ-на-разных-стадиях:-в-сырье,-в-черновом-и-финальном-концентратах.-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Суммарная концентрацию всех веществ на разных стадиях: в сырье, в черновом и финальном концентратах.</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Новые-индексы" data-toc-modified-id="Новые-индексы-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Новые индексы</a></span></li><li><span><a href="#Упрощенная-выборка" data-toc-modified-id="Упрощенная-выборка-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Упрощенная выборка</a></span></li><li><span><a href="#Признаки" data-toc-modified-id="Признаки-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Признаки</a></span><ul class="toc-item"><li><span><a href="#Исходные-признаки" data-toc-modified-id="Исходные-признаки-7.3.1"><span class="toc-item-num">7.3.1&nbsp;&nbsp;</span>Исходные признаки</a></span></li><li><span><a href="#Предобработанные-признаки" data-toc-modified-id="Предобработанные-признаки-7.3.2"><span class="toc-item-num">7.3.2&nbsp;&nbsp;</span>Предобработанные признаки</a></span></li><li><span><a href="#Предобработанные-упрощенные-признаки" data-toc-modified-id="Предобработанные-упрощенные-признаки-7.3.3"><span class="toc-item-num">7.3.3&nbsp;&nbsp;</span>Предобработанные упрощенные признаки</a></span></li></ul></li><li><span><a href="#Целевой-признак" data-toc-modified-id="Целевой-признак-7.4"><span class="toc-item-num">7.4&nbsp;&nbsp;</span>Целевой признак</a></span><ul class="toc-item"><li><span><a href="#Исходный-целевой-признак" data-toc-modified-id="Исходный-целевой-признак-7.4.1"><span class="toc-item-num">7.4.1&nbsp;&nbsp;</span>Исходный целевой признак</a></span></li><li><span><a href="#Предобработанный-целевой-признак" data-toc-modified-id="Предобработанный-целевой-признак-7.4.2"><span class="toc-item-num">7.4.2&nbsp;&nbsp;</span>Предобработанный целевой признак</a></span></li></ul></li></ul></li><li><span><a href="#Модель" data-toc-modified-id="Модель-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Модель</a></span><ul class="toc-item"><li><span><a href="#Функции-вычисления-метрик" data-toc-modified-id="Функции-вычисления-метрик-8.1"><span class="toc-item-num">8.1&nbsp;&nbsp;</span>Функции вычисления метрик</a></span></li><li><span><a href="#Подбор-базовой-модели" data-toc-modified-id="Подбор-базовой-модели-8.2"><span class="toc-item-num">8.2&nbsp;&nbsp;</span>Подбор базовой модели</a></span><ul class="toc-item"><li><span><a href="#ЛИНЕЙНАЯ-РЕГРЕССИЯ" data-toc-modified-id="ЛИНЕЙНАЯ-РЕГРЕССИЯ-8.2.1"><span class="toc-item-num">8.2.1&nbsp;&nbsp;</span>ЛИНЕЙНАЯ РЕГРЕССИЯ</a></span><ul class="toc-item"><li><span><a href="#Оригинальный-датасет" data-toc-modified-id="Оригинальный-датасет-8.2.1.1"><span class="toc-item-num">8.2.1.1&nbsp;&nbsp;</span>Оригинальный датасет</a></span></li><li><span><a href="#Предобработанный-датасет" data-toc-modified-id="Предобработанный-датасет-8.2.1.2"><span class="toc-item-num">8.2.1.2&nbsp;&nbsp;</span>Предобработанный датасет</a></span></li><li><span><a href="#Предобработанный-упрощенный-датасет" data-toc-modified-id="Предобработанный-упрощенный-датасет-8.2.1.3"><span class="toc-item-num">8.2.1.3&nbsp;&nbsp;</span>Предобработанный упрощенный датасет</a></span></li></ul></li><li><span><a href="#ДЕРЕВО-РЕШЕНИЙ" data-toc-modified-id="ДЕРЕВО-РЕШЕНИЙ-8.2.2"><span class="toc-item-num">8.2.2&nbsp;&nbsp;</span>ДЕРЕВО РЕШЕНИЙ</a></span><ul class="toc-item"><li><span><a href="#Оригинальный-датасет" data-toc-modified-id="Оригинальный-датасет-8.2.2.1"><span class="toc-item-num">8.2.2.1&nbsp;&nbsp;</span>Оригинальный датасет</a></span></li><li><span><a href="#Предобработанный-датасет" data-toc-modified-id="Предобработанный-датасет-8.2.2.2"><span class="toc-item-num">8.2.2.2&nbsp;&nbsp;</span>Предобработанный датасет</a></span></li><li><span><a href="#Предобработанный-упрощенный-датасет" data-toc-modified-id="Предобработанный-упрощенный-датасет-8.2.2.3"><span class="toc-item-num">8.2.2.3&nbsp;&nbsp;</span>Предобработанный упрощенный датасет</a></span></li></ul></li><li><span><a href="#СЛУЧАНЫЙ-ЛЕС" data-toc-modified-id="СЛУЧАНЫЙ-ЛЕС-8.2.3"><span class="toc-item-num">8.2.3&nbsp;&nbsp;</span>СЛУЧАНЫЙ ЛЕС</a></span><ul class="toc-item"><li><span><a href="#Оригинальный-датасет" data-toc-modified-id="Оригинальный-датасет-8.2.3.1"><span class="toc-item-num">8.2.3.1&nbsp;&nbsp;</span>Оригинальный датасет</a></span></li><li><span><a href="#Предобработанный-датасет" data-toc-modified-id="Предобработанный-датасет-8.2.3.2"><span class="toc-item-num">8.2.3.2&nbsp;&nbsp;</span>Предобработанный датасет</a></span></li><li><span><a href="#Предобработанный-упрощенный-датасет" data-toc-modified-id="Предобработанный-упрощенный-датасет-8.2.3.3"><span class="toc-item-num">8.2.3.3&nbsp;&nbsp;</span>Предобработанный упрощенный датасет</a></span></li></ul></li><li><span><a href="#Вывод:" data-toc-modified-id="Вывод:-8.2.4"><span class="toc-item-num">8.2.4&nbsp;&nbsp;</span>Вывод:</a></span></li></ul></li><li><span><a href="#Подбор-гиперпараметров" data-toc-modified-id="Подбор-гиперпараметров-8.3"><span class="toc-item-num">8.3&nbsp;&nbsp;</span>Подбор гиперпараметров</a></span><ul class="toc-item"><li><span><a href="#СЛУЧАНЫЙ-ЛЕС" data-toc-modified-id="СЛУЧАНЫЙ-ЛЕС-8.3.1"><span class="toc-item-num">8.3.1&nbsp;&nbsp;</span>СЛУЧАНЫЙ ЛЕС</a></span><ul class="toc-item"><li><span><a href="#Предобработанный-датасет" data-toc-modified-id="Предобработанный-датасет-8.3.1.1"><span class="toc-item-num">8.3.1.1&nbsp;&nbsp;</span>Предобработанный датасет</a></span></li><li><span><a href="#Предобработанный-упрощенный-датасет" data-toc-modified-id="Предобработанный-упрощенный-датасет-8.3.1.2"><span class="toc-item-num">8.3.1.2&nbsp;&nbsp;</span>Предобработанный упрощенный датасет</a></span></li></ul></li><li><span><a href="#Вывод:" data-toc-modified-id="Вывод:-8.3.2"><span class="toc-item-num">8.3.2&nbsp;&nbsp;</span>Вывод:</a></span></li></ul></li><li><span><a href="#Тестирование-модели" data-toc-modified-id="Тестирование-модели-8.4"><span class="toc-item-num">8.4&nbsp;&nbsp;</span>Тестирование модели</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-8.5"><span class="toc-item-num">8.5&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li><li><span><a href="#Итоговое-sMAPE" data-toc-modified-id="Итоговое-sMAPE-8.6"><span class="toc-item-num">8.6&nbsp;&nbsp;</span>Итоговое sMAPE</a></span></li></ul></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Выводы</a></span></li></ul></div>
