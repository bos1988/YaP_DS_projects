# Исследование объявлений о продаже квартир <br/>—  анализ рынка недвижимости
> **Определение рыночной стоимости объектов недвижимости и типичные параметры квартир по данным сервиса Яндекс.Недвижимость.**

<br/>

### Описание:

По данным архива объявлений сервиса Яндекс.Недвижимость нужно научиться определять рыночную стоимость объектов недвижимости. Задача — установить параметры. Это позволит построить автоматизированную систему: она отследит аномалии и мошенническую деятельность.

### Задачи:

- Изучить данные архива объявлений.
- Проанализировать время продажи квартиры с целью выявления средней продложительности процесса продажи.
- Выявить зависимости цены квартиры от различных параметров (площадь, число комнат, удаленность от центра, этажность, дата размещения).
- Определить населенные пункты с наибольшей активностью продаж квартир.
- Выявить сегмент квартир в центре Санкт-Петербурга и определить зависимости цены квартир в этом сегменте от тех же параметров.

### Данные:

Данные сервиса Яндекс.Недвижимость — архив объявлений о продаже квартир в Санкт-Петербурге и соседних населённых пунктах за несколько лет

- ` airports_nearest ` – расстояние до ближайшего аэропорта в метрах (м)
- ` balcony ` – число балконов
- ` ceiling_height ` – высота потолков (м)
- ` cityCenters_nearest ` – расстояние до центра города (м)
- ` days_exposition ` – сколько дней было размещено объявление (от публикации до снятия)
- ` first_day_exposition ` – дата публикации
- ` floor ` – этаж
- ` floors_total ` – всего этажей в доме
- ` is_apartment ` – апартаменты (булев тип)
- ` kitchen_area ` – площадь кухни в квадратных метрах (м2)
- ` last_price ` – цена на момент снятия с публикации
- ` living_area ` – жилая площадь в квадратных метрах (м2)
- ` locality_name ` – название населённого пункта
- ` open_plan ` – свободная планировка (булев тип)
- ` parks_around3000 ` – число парков в радиусе 3 км
- ` parks_nearest ` – расстояние до ближайшего парка (м)
- ` ponds_around3000 ` – число водоёмов в радиусе 3 км
- ` ponds_nearest ` – расстояние до ближайшего водоёма (м)
- ` rooms ` – число комнат
- ` studio ` – квартира-студия (булев тип)
- ` total_area ` – площадь квартиры в квадратных метрах (м2)
- ` total_images ` – число фотографий квартиры в объявлении

---

## v1:

### Используемые библиотеки:
*matplotlib<br/>pandas<br/>seaborn*

### Ключевые аспекты:

*обработка данных<br/>histogram<br/>boxplot<br/>barplot<br/>hetmap<br/>категоризация<br/>scatterplot<br/>фрод-мониторинг<br/>статистики*

### Результат работы:

В исходных данных весьма много пропущеных значений, есть блоки совсем остутствующих данных. В начале временной шкалы времени публикации имеются значаительные недостатки данных и в двух местах в середине временной шкалы недостаки поменьше. **На этих данных может быть повышенное число шумов!**

Присутствуют аномалии по высоте потолков, числу комнат, цене, площадям квартиры.

Имются неявные дубликаты по наименованиям населенных пунктов (путаница из-за типов).

В процессе предподготовки данных:
* Часть данных удалена. Это весь столбец `is_apartment`, пропущенные значения по столбцам `floors_total`, `locality_name`, аномалии по столбцам `ceiling_height`, `airports_nearest`, `last_price`, `total_area`, `living_area`, `kitchen_area`.
* Обработаны выбросы по всем данным
* Часть данных заменена на значения:
    * для столбца `balcony` пропуски заменены на нулевые значения
    * для столбца `living_area` и `living_area` на средние значения пропорционально общей площади
    * для столбца `ceiling_height` на средние значения
    * по столбцам `airports_nearest`, `city_centers_nearest`, `parks_around_3000`, `ponds_around_3000`, `parks_nearest`, `ponds_nearest` и `days_exposition` пропущенные значения оставлены
* Исправлены названия населенных пунктов
> Остаток от оригинала - 98,16%. Потеря данных менее 2%.

Произведена очистка данных от влияния недостающих данных. Очищенный набор данных меньше оригинала на 18%.

Изучены основные параметры квартиры и выявлены средние значения:
* Площадь = 51 м.кв
* Цена = 4 550 000 р
* Число комнат = 2
* Высота потолков = 2,7 м

Выявлены факторы, влияющие на цену квартиры:
* Площадь и число комнат (чем выше площадь или число комнат, тем выше цена)
* Удаленность от центра (в центральной зоне цены выше)
* Цена квартиры на первом этаже ниже средней, на последнем
* выше средней

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цели-проекта" data-toc-modified-id="Цели-проекта-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цели проекта</a></span></li><li><span><a href="#Изучение-данных-из-файла" data-toc-modified-id="Изучение-данных-из-файла-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Изучение данных из файла</a></span><ul class="toc-item"><li><span><a href="#Вывод" data-toc-modified-id="Вывод-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Обработка-явных-дубликатов" data-toc-modified-id="Обработка-явных-дубликатов-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Обработка явных дубликатов</a></span></li><li><span><a href="#Проверка-естественных-закономерностей" data-toc-modified-id="Проверка-естественных-закономерностей-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Проверка естественных закономерностей</a></span></li><li><span><a href="#Обработка-пропусков" data-toc-modified-id="Обработка-пропусков-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Обработка пропусков</a></span><ul class="toc-item"><li><span><a href="#Удалим-пропуски-с-долей-пропусков-менее-1%:" data-toc-modified-id="Удалим-пропуски-с-долей-пропусков-менее-1%:-3.4.1"><span class="toc-item-num">3.4.1&nbsp;&nbsp;</span>Удалим пропуски с долей пропусков менее 1%:</a></span></li><li><span><a href="#Заполним-пропущенные-площади" data-toc-modified-id="Заполним-пропущенные-площади-3.4.2"><span class="toc-item-num">3.4.2&nbsp;&nbsp;</span>Заполним пропущенные площади</a></span></li><li><span><a href="#Заполним-количественные-параметры-со-значением-0" data-toc-modified-id="Заполним-количественные-параметры-со-значением-0-3.4.3"><span class="toc-item-num">3.4.3&nbsp;&nbsp;</span>Заполним количественные параметры со значением <code>0</code></a></span></li><li><span><a href="#Зависимость-расстояний-от-населенного-пункта" data-toc-modified-id="Зависимость-расстояний-от-населенного-пункта-3.4.4"><span class="toc-item-num">3.4.4&nbsp;&nbsp;</span>Зависимость расстояний от населенного пункта</a></span></li><li><span><a href="#Пропуски-географических-признаков" data-toc-modified-id="Пропуски-географических-признаков-3.4.5"><span class="toc-item-num">3.4.5&nbsp;&nbsp;</span>Пропуски географических признаков</a></span></li><li><span><a href="#Заполним-расстояния-для-несуществующих-объектов" data-toc-modified-id="Заполним-расстояния-для-несуществующих-объектов-3.4.6"><span class="toc-item-num">3.4.6&nbsp;&nbsp;</span>Заполним расстояния для несуществующих объектов</a></span></li><li><span><a href="#Удаление-признака-is_apartment" data-toc-modified-id="Удаление-признака-is_apartment-3.4.7"><span class="toc-item-num">3.4.7&nbsp;&nbsp;</span>Удаление признака is_apartment</a></span></li><li><span><a href="#Замена-пропусков-для-высоты-потолков" data-toc-modified-id="Замена-пропусков-для-высоты-потолков-3.4.8"><span class="toc-item-num">3.4.8&nbsp;&nbsp;</span>Замена пропусков для высоты потолков</a></span></li><li><span><a href="#Пропуски-по-продолжительности-размещения-объявления" data-toc-modified-id="Пропуски-по-продолжительности-размещения-объявления-3.4.9"><span class="toc-item-num">3.4.9&nbsp;&nbsp;</span>Пропуски по продолжительности размещения объявления</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-3.4.10"><span class="toc-item-num">3.4.10&nbsp;&nbsp;</span>Проверка пропусков</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.4.11"><span class="toc-item-num">3.4.11&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Замена типов данных</a></span></li><li><span><a href="#Ошибки,-выбросы-и-дубликаты-значений" data-toc-modified-id="Ошибки,-выбросы-и-дубликаты-значений-3.6"><span class="toc-item-num">3.6&nbsp;&nbsp;</span>Ошибки, выбросы и дубликаты значений</a></span><ul class="toc-item"><li><span><a href="#Диапазоны-значений" data-toc-modified-id="Диапазоны-значений-3.6.1"><span class="toc-item-num">3.6.1&nbsp;&nbsp;</span>Диапазоны значений</a></span></li><li><span><a href="#Неявные-дубликаты" data-toc-modified-id="Неявные-дубликаты-3.6.2"><span class="toc-item-num">3.6.2&nbsp;&nbsp;</span>Неявные дубликаты</a></span></li><li><span><a href="#Выбросы-значений" data-toc-modified-id="Выбросы-значений-3.6.3"><span class="toc-item-num">3.6.3&nbsp;&nbsp;</span>Выбросы значений</a></span><ul class="toc-item"><li><span><a href="#Число-фотографий" data-toc-modified-id="Число-фотографий-3.6.3.1"><span class="toc-item-num">3.6.3.1&nbsp;&nbsp;</span>Число фотографий</a></span></li><li><span><a href="#Цена" data-toc-modified-id="Цена-3.6.3.2"><span class="toc-item-num">3.6.3.2&nbsp;&nbsp;</span>Цена</a></span></li><li><span><a href="#Площадь-квартиры" data-toc-modified-id="Площадь-квартиры-3.6.3.3"><span class="toc-item-num">3.6.3.3&nbsp;&nbsp;</span>Площадь квартиры</a></span></li><li><span><a href="#Жилая-площадь-и-площадь-кухни" data-toc-modified-id="Жилая-площадь-и-площадь-кухни-3.6.3.4"><span class="toc-item-num">3.6.3.4&nbsp;&nbsp;</span>Жилая площадь и площадь кухни</a></span></li><li><span><a href="#Число-комнат" data-toc-modified-id="Число-комнат-3.6.3.5"><span class="toc-item-num">3.6.3.5&nbsp;&nbsp;</span>Число комнат</a></span></li><li><span><a href="#Число-этажей-в-доме" data-toc-modified-id="Число-этажей-в-доме-3.6.3.6"><span class="toc-item-num">3.6.3.6&nbsp;&nbsp;</span>Число этажей в доме</a></span></li><li><span><a href="#Этаж" data-toc-modified-id="Этаж-3.6.3.7"><span class="toc-item-num">3.6.3.7&nbsp;&nbsp;</span>Этаж</a></span></li><li><span><a href="#Расстояние-до-эаропорта" data-toc-modified-id="Расстояние-до-эаропорта-3.6.3.8"><span class="toc-item-num">3.6.3.8&nbsp;&nbsp;</span>Расстояние до эаропорта</a></span></li><li><span><a href="#Расстояние-до-центра" data-toc-modified-id="Расстояние-до-центра-3.6.3.9"><span class="toc-item-num">3.6.3.9&nbsp;&nbsp;</span>Расстояние до центра</a></span></li><li><span><a href="#Расстояние-до-парка" data-toc-modified-id="Расстояние-до-парка-3.6.3.10"><span class="toc-item-num">3.6.3.10&nbsp;&nbsp;</span>Расстояние до парка</a></span></li><li><span><a href="#Расстояние-до-водоема" data-toc-modified-id="Расстояние-до-водоема-3.6.3.11"><span class="toc-item-num">3.6.3.11&nbsp;&nbsp;</span>Расстояние до водоема</a></span></li><li><span><a href="#Период-размещения-объявления" data-toc-modified-id="Период-размещения-объявления-3.6.3.12"><span class="toc-item-num">3.6.3.12&nbsp;&nbsp;</span>Период размещения объявления</a></span></li></ul></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-3.6.4"><span class="toc-item-num">3.6.4&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li></ul></li><li><span><a href="#Расчёты-и-добавление-результатов-в-таблицу" data-toc-modified-id="Расчёты-и-добавление-результатов-в-таблицу-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Расчёты и добавление результатов в таблицу</a></span></li><li><span><a href="#Исследовательский-анализ-данных" data-toc-modified-id="Исследовательский-анализ-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Исследовательский анализ данных</a></span><ul class="toc-item"><li><span><a href="#Параметры-квартиры" data-toc-modified-id="Параметры-квартиры-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Параметры квартиры</a></span><ul class="toc-item"><li><span><a href="#Площади" data-toc-modified-id="Площади-5.1.1"><span class="toc-item-num">5.1.1&nbsp;&nbsp;</span>Площади</a></span></li><li><span><a href="#Цена" data-toc-modified-id="Цена-5.1.2"><span class="toc-item-num">5.1.2&nbsp;&nbsp;</span>Цена</a></span></li><li><span><a href="#Число-комнат" data-toc-modified-id="Число-комнат-5.1.3"><span class="toc-item-num">5.1.3&nbsp;&nbsp;</span>Число комнат</a></span></li><li><span><a href="#Высота-потолков" data-toc-modified-id="Высота-потолков-5.1.4"><span class="toc-item-num">5.1.4&nbsp;&nbsp;</span>Высота потолков</a></span></li></ul></li><li><span><a href="#Время-продажи" data-toc-modified-id="Время-продажи-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Время продажи</a></span><ul class="toc-item"><li><span><a href="#Гипотеза-1" data-toc-modified-id="Гипотеза-1-5.2.1"><span class="toc-item-num">5.2.1&nbsp;&nbsp;</span>Гипотеза 1</a></span></li><li><span><a href="#Гипотеза-2" data-toc-modified-id="Гипотеза-2-5.2.2"><span class="toc-item-num">5.2.2&nbsp;&nbsp;</span>Гипотеза 2</a></span></li><li><span><a href="#Гипотеза-3" data-toc-modified-id="Гипотеза-3-5.2.3"><span class="toc-item-num">5.2.3&nbsp;&nbsp;</span>Гипотеза 3</a></span></li><li><span><a href="#Обработка-данных" data-toc-modified-id="Обработка-данных-5.2.4"><span class="toc-item-num">5.2.4&nbsp;&nbsp;</span>Обработка данных</a></span></li></ul></li><li><span><a href="#Факторы,-влияющие-на-цену-квартиры" data-toc-modified-id="Факторы,-влияющие-на-цену-квартиры-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Факторы, влияющие на цену квартиры</a></span></li><li><span><a href="#Населенные-пункты-с-наибольшим-числом-объявлений" data-toc-modified-id="Населенные-пункты-с-наибольшим-числом-объявлений-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Населенные пункты с наибольшим числом объявлений</a></span></li><li><span><a href="#Анализ-центра-Санкт-Петербурга" data-toc-modified-id="Анализ-центра-Санкт-Петербурга-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Анализ центра Санкт-Петербурга</a></span><ul class="toc-item"><li><span><a href="#Площади" data-toc-modified-id="Площади-5.5.1"><span class="toc-item-num">5.5.1&nbsp;&nbsp;</span>Площади</a></span></li><li><span><a href="#Цена" data-toc-modified-id="Цена-5.5.2"><span class="toc-item-num">5.5.2&nbsp;&nbsp;</span>Цена</a></span></li><li><span><a href="#Число-комнат" data-toc-modified-id="Число-комнат-5.5.3"><span class="toc-item-num">5.5.3&nbsp;&nbsp;</span>Число комнат</a></span></li><li><span><a href="#Высота-потолков" data-toc-modified-id="Высота-потолков-5.5.4"><span class="toc-item-num">5.5.4&nbsp;&nbsp;</span>Высота потолков</a></span></li><li><span><a href="#Факторы,-влияющие-на-цену-квартиры" data-toc-modified-id="Факторы,-влияющие-на-цену-квартиры-5.5.5"><span class="toc-item-num">5.5.5&nbsp;&nbsp;</span>Факторы, влияющие на цену квартиры</a></span></li></ul></li></ul></li><li><span><a href="#Общий-вывод" data-toc-modified-id="Общий-вывод-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Общий вывод</a></span></li></ul></div>
