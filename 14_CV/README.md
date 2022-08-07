# Определение возраста покупателей
> **Определение возраста по фотографии.**

<br/>

### Описание:

Сетевой супермаркет внедряет систему компьютерного зрения для обработки фотографий покупателей. Фотофиксация в прикассовой зоне поможет определять возраст клиентов, чтобы анализировать покупки и предлагать товары, которые могут заинтересовать покупателей этой возрастной группы и контролировать добросовестность кассиров при продаже алкоголя.
Нужно построить модель, которая по фотографии определит приблизительный возраст человека.

### Задачи:

- Провести исследовательский анализ набора фотографий.
- Подготовить данные к обучению.
- Обучить нейронную сеть и рассчитать её качество.

### Данные:

Набор фотографий людей с указанием возраста - APPA-REAL (real and apparent age)

Данные взяты с сайта ChaLearn Looking at People.

В нашем распоряжении одна папка со всеми изображениями (/final_files) и csv-файл *labels.csv* с двумя колонками: ` file_name ` и ` real_age `.

---

## v1:

### Используемые библиотеки:
*matplotlib<br/>numpy<br/>pandas<br/>PIL<br/>seaborn<br/>tensorflow<br/>time*

### Ключевые аспекты:

*EDA<br/>обработка изображений<br/>нейронные сети<br/>подбор гиперпараметров<br/>выбор модели МО*

### Результат работы:

1. Получена модель предсказания реального возраста человека по изображению

1. Метрика **MSE = 5.9** (лучше константной модели)

1. Для построения модели были получены уже обработанные данные (лица сориентированы на изображениях одинаковым образом)

1. Для обучения принята архитектура сверточных сетей `ResNet50`

1. Лучшие значения предсказаний дает предобученная модель **ResNet('imagenet')** с оптимизатором **Adam(learning_rate=0.0002)** и загрузчиком данных батчами по **64**, **без агументаций**.

1. Получанная модель может быть использована для анализа покупок и предложения товаров, которые могут заинтересовать покупателей определенной возрастной группы

1. Для контроля добросовестности кассиров при продаже алкоголя точность данной модели не подойдет и применяться не может.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Исследовательский-анализ-данных" data-toc-modified-id="Исследовательский-анализ-данных-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исследовательский анализ данных</a></span><ul class="toc-item"><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Путь-к-базе-данных" data-toc-modified-id="Путь-к-базе-данных-2.1.1"><span class="toc-item-num">2.1.1&nbsp;&nbsp;</span>Путь к базе данных</a></span></li><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-2.1.2"><span class="toc-item-num">2.1.2&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-2.1.3"><span class="toc-item-num">2.1.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Функции" data-toc-modified-id="Функции-2.1.4"><span class="toc-item-num">2.1.4&nbsp;&nbsp;</span>Функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-2.2.1"><span class="toc-item-num">2.2.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-2.2.2"><span class="toc-item-num">2.2.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-2.2.3"><span class="toc-item-num">2.2.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-2.2.4"><span class="toc-item-num">2.2.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Признаки-изображений" data-toc-modified-id="Признаки-изображений-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Признаки изображений</a></span></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-2.4.1"><span class="toc-item-num">2.4.1&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Числовые" data-toc-modified-id="Числовые-2.4.1.1"><span class="toc-item-num">2.4.1.1&nbsp;&nbsp;</span>Числовые</a></span></li><li><span><a href="#Изображения" data-toc-modified-id="Изображения-2.4.1.2"><span class="toc-item-num">2.4.1.2&nbsp;&nbsp;</span>Изображения</a></span></li></ul></li><li><span><a href="#Выводы-по-результатам-исследования" data-toc-modified-id="Выводы-по-результатам-исследования-2.4.2"><span class="toc-item-num">2.4.2&nbsp;&nbsp;</span>Выводы по результатам исследования</a></span></li></ul></li><li><span><a href="#Подбор-моделей" data-toc-modified-id="Подбор-моделей-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>Подбор моделей</a></span><ul class="toc-item"><li><span><a href="#Базовые-изменения-модели" data-toc-modified-id="Базовые-изменения-модели-2.5.1"><span class="toc-item-num">2.5.1&nbsp;&nbsp;</span>Базовые изменения модели</a></span><ul class="toc-item"><li><span><a href="#Результаты-пробных-обучений" data-toc-modified-id="Результаты-пробных-обучений-2.5.1.1"><span class="toc-item-num">2.5.1.1&nbsp;&nbsp;</span>Результаты пробных обучений</a></span></li><li><span><a href="#Подбор-характеристик-моделирования" data-toc-modified-id="Подбор-характеристик-моделирования-2.5.1.2"><span class="toc-item-num">2.5.1.2&nbsp;&nbsp;</span>Подбор характеристик моделирования</a></span></li></ul></li><li><span><a href="#Комбинированные-изменения-модели" data-toc-modified-id="Комбинированные-изменения-модели-2.5.2"><span class="toc-item-num">2.5.2&nbsp;&nbsp;</span>Комбинированные изменения модели</a></span><ul class="toc-item"><li><span><a href="#Результаты-пробных-обучений" data-toc-modified-id="Результаты-пробных-обучений-2.5.2.1"><span class="toc-item-num">2.5.2.1&nbsp;&nbsp;</span>Результаты пробных обучений</a></span></li><li><span><a href="#Подбор-характеристик-моделирования" data-toc-modified-id="Подбор-характеристик-моделирования-2.5.2.2"><span class="toc-item-num">2.5.2.2&nbsp;&nbsp;</span>Подбор характеристик моделирования</a></span></li></ul></li><li><span><a href="#Лучшие-комбинированные-изменения-модели" data-toc-modified-id="Лучшие-комбинированные-изменения-модели-2.5.3"><span class="toc-item-num">2.5.3&nbsp;&nbsp;</span>Лучшие комбинированные изменения модели</a></span><ul class="toc-item"><li><span><a href="#Результаты-пробных-обучений" data-toc-modified-id="Результаты-пробных-обучений-2.5.3.1"><span class="toc-item-num">2.5.3.1&nbsp;&nbsp;</span>Результаты пробных обучений</a></span></li><li><span><a href="#Подбор-характеристик-моделирования" data-toc-modified-id="Подбор-характеристик-моделирования-2.5.3.2"><span class="toc-item-num">2.5.3.2&nbsp;&nbsp;</span>Подбор характеристик моделирования</a></span></li></ul></li></ul></li></ul></li><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li><li><span><a href="#Анализ-обученной-модели" data-toc-modified-id="Анализ-обученной-модели-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Анализ обученной модели</a></span></li></ul></div>
