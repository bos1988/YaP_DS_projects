# Выбор локации для скважины
> **Определение наиболее выгодного (прибыльного) региона для разработки скважин на основе данных геологоразведки.**

<br/>

### Описание:

Нужно решить, где бурить новую скважину. Характеристики для каждой скважины в регионе уже известны. Построить модель для определения региона, где добыча принесёт наибольшую прибыль. 

### Задачи:

- Построить модель для определения региона, где добыча принесёт наибольшую прибыль.
- Проанализировать возможную прибыль и риски техникой Bootstrap.

### Данные:

Пробы нефти в трёх регионах

Данные синтетические: детали контрактов и характеристики месторождений не разглашаются.

- ` id ` – уникальный идентификатор скважины;
- ` f0, f1, f2 ` – три признака точек (неважно, что они означают, но сами признаки значимы);
- ` product ` – объём запасов в скважине (тыс. баррелей).

---

## v1:

### Используемые библиотеки:
*matplotlib<br/>numpy<br/>pandas<br/>scipy<br/>seaborn<br/>sklearn*

### Ключевые аспекты:

*EDA<br/>предобработка данных<br/>регрессия<br/>разработка бизнес-модели<br/>бутстреп*

### Результат работы:

1. Для разработки скважин с веротностью убытков менее 2.5% подходит только **Регион 2** (`geo_data_1.csv`).

1. Средняя выручка по Региону 2 составит **523 млн.руб.**.

1. С вероятностью 95% выручка по региону 2 будет от 111.7 млн.руб до 930.7 млн.руб.

Обоснования выбора региона 2:
* Веростность убытоков по Району 1 составляет 2.7%
* Веростность убытоков по Району 2 составляет 0,7%
* Веростность убытоков по Району 3 составляет 9,5%
* Поскольку нужно отобрать районы с вероятностью убытков меньше 2.5%, то выбираем Район 2 (`geo_data_1.csv`).

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Условия-задачи" data-toc-modified-id="Условия-задачи-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Условия задачи</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Общие-функции" data-toc-modified-id="Общие-функции-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Общие функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Общая-информацция" data-toc-modified-id="Общая-информацция-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Общая информацция</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Проверка пропусков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Замена типов данных</a></span></li><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-5.6"><span class="toc-item-num">5.6&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-5.7"><span class="toc-item-num">5.7&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Корреляция" data-toc-modified-id="Корреляция-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Корреляция</a></span></li><li><span><a href="#X,-Y-параметры" data-toc-modified-id="X,-Y-параметры-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>X, Y параметры</a></span></li><li><span><a href="#Набор-данных" data-toc-modified-id="Набор-данных-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Набор данных</a></span></li><li><span><a href="#Разбивка-данных" data-toc-modified-id="Разбивка-данных-6.4"><span class="toc-item-num">6.4&nbsp;&nbsp;</span>Разбивка данных</a></span></li><li><span><a href="#Масштабирование-данных" data-toc-modified-id="Масштабирование-данных-6.5"><span class="toc-item-num">6.5&nbsp;&nbsp;</span>Масштабирование данных</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-6.6"><span class="toc-item-num">6.6&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Обучение-и-проверка-модели" data-toc-modified-id="Обучение-и-проверка-модели-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Обучение и проверка модели</a></span><ul class="toc-item"><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Проверка модели</a></span></li><li><span><a href="#Результаты-обучения" data-toc-modified-id="Результаты-обучения-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Результаты обучения</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-7.4"><span class="toc-item-num">7.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Подготовка-к-расчёту-прибыли" data-toc-modified-id="Подготовка-к-расчёту-прибыли-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Подготовка к расчёту прибыли</a></span><ul class="toc-item"><li><span><a href="#Ключевые-параметры-рассчета" data-toc-modified-id="Ключевые-параметры-рассчета-8.1"><span class="toc-item-num">8.1&nbsp;&nbsp;</span>Ключевые параметры рассчета</a></span></li><li><span><a href="#Достаточный-объем-скважины" data-toc-modified-id="Достаточный-объем-скважины-8.2"><span class="toc-item-num">8.2&nbsp;&nbsp;</span>Достаточный объем скважины</a></span></li></ul></li><li><span><a href="#Расчёт-прибыли-и-рисков" data-toc-modified-id="Расчёт-прибыли-и-рисков-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Расчёт прибыли и рисков</a></span><ul class="toc-item"><li><span><a href="#Функция-расчета-прибыли" data-toc-modified-id="Функция-расчета-прибыли-9.1"><span class="toc-item-num">9.1&nbsp;&nbsp;</span>Функция расчета прибыли</a></span></li><li><span><a href="#Расчет-прибыли-по-регионам" data-toc-modified-id="Расчет-прибыли-по-регионам-9.2"><span class="toc-item-num">9.2&nbsp;&nbsp;</span>Расчет прибыли по регионам</a></span><ul class="toc-item"><li><span><a href="#Регион-1" data-toc-modified-id="Регион-1-9.2.1"><span class="toc-item-num">9.2.1&nbsp;&nbsp;</span>Регион 1</a></span></li><li><span><a href="#Регион-2" data-toc-modified-id="Регион-2-9.2.2"><span class="toc-item-num">9.2.2&nbsp;&nbsp;</span>Регион 2</a></span></li><li><span><a href="#Регион-3" data-toc-modified-id="Регион-3-9.2.3"><span class="toc-item-num">9.2.3&nbsp;&nbsp;</span>Регион 3</a></span></li></ul></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-9.3"><span class="toc-item-num">9.3&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li></ul></div>
