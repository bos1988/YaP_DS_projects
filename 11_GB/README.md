# Определение стоимости автомобилей
> **Разработка системы рекомендации стоимости автомобиля на основе его описания.**

<br/>

### Описание:

Сервис по продаже автомобилей с пробегом разрабатывает приложение для привлечения новых клиентов. В нём можно быстро узнать рыночную стоимость своего автомобиля. Нужно построить модель для определения стоимости автомобиля.

Заказчику важны:
-  качество предсказания;
-  скорость предсказания;
-  время обучения.

### Задачи:

- Построить модель для определения стоимости автомобиля.
- Оптимизировать время и качество предсказаний модели.

### Данные:

Исторические данные: технические характеристики, комплектации и цены автомобилей. 

- ` DateCrawled ` – дата скачивания анкеты из базы
- ` VehicleType ` – тип автомобильного кузова
- ` RegistrationYear ` – год регистрации автомобиля
- ` Gearbox ` – тип коробки передач
- ` Power ` – мощность (л. с.)
- ` Model ` – модель автомобиля
- ` Kilometer ` – пробег (км)
- ` RegistrationMonth ` – месяц регистрации автомобиля
- ` FuelType ` – тип топлива
- ` Brand ` – марка автомобиля
- ` NotRepaired ` – была машина в ремонте или нет
- ` DateCreated ` – дата создания анкеты
- ` NumberOfPictures ` – количество фотографий автомобиля
- ` PostalCode ` – почтовый индекс владельца анкеты (пользователя)
- ` LastSeen ` – дата последней активности пользователя

---

## v1:

### Используемые библиотеки:
*itertools<br/>lightgbm<br/>matplotlib<br/>numpy<br/>pandas<br/>seaborn<br/>sklearn<br/>time*

### Ключевые аспекты:

*EDA<br/>предобработка данных<br/>градиентный бустинг<br/>регрессия<br/>порядковое кодирование<br/>масштабирование данных<br/>оценка важности признкаков<br/>регуляризация<br/>подбор гиперпараметров<br/>выбор модели МО<br/>кросс-валидация<br/>Pipeline<br/>продакшн*

### Результат работы:

**1. Проанализированы и обработаны исторические данные по продаже автомобилей:**
* В данных имеются пропуски категриальных значений, которые заменяются значением `unknown` в обучающей выборке
* В данных выявлены противоречия по части объектов:
    * дата регистрации автомобиля старше даты создания анкеты (порядка 6% даных)
* В данных имеются значения низких цен (далеких от реальности) и значения цен = 0
    * нулевые значнеия цен удаляются из обучающей выборки
    * *возможная более тонкая настройка диапазона цен*
* В данных имеются низкие и высокие значения мощности двигателя (далекие от реальности)
    * выбросы значнеий мощности двигателя удаляются из обучающей выборки
* В данных имеются невероятные года регистрации автомобиля (начиная с 1000 года)
    * выбросы значнеий года регистрации автомобиля удаляются из обучающей выборки
* В наибольшей степени цена на автомобиль зависит от мощности двигателя и года регистрации автомобиля
* Для обучения модели отобрано 10 наиболее важных признаков (не учитывая целевой):
    * 'vehicle_type', 'registration_year', 'gearbox', 'power', 'model', 'kilometer', 'fuel_type', 'brand', 'not_repaired', 'postal_code'

**2. Пободрана модель предсказания цены автомобиля:**

***LGBMRegressor(learning_rate=0.1, max_depth=45, num_leaves=180, random_state=123)***
* Модель градиентного бустинга имеет лучшие оптимальное соотношение скорости вычислений и точности предсказаний
* Для модели градиентного бустинга подобраны гиперапараметры, создан рабочий код, проверена работоспособность на тестовой выборке
* Качество предсказаний, выраженное в метрике MRSE, составило порядка 1500 евро

**3. На реальных данных метрики численно могут оказаться хуже, чем в исследовательской части данного проекта (но на уровне метрики из "рабочего кода").**<br/>

Это нормально, поскольку модель настраивается на предсказание более *адекватной* цены и может чаще ошибаться в предсказаниях некорректных и аномальных анкет.<br/> Можно дополнительно подстроить выборку *адекватного* дипазона цен для обучения модели и таким образом скорректировать *адекватный* диапазон предсказаний.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-исследования" data-toc-modified-id="Цель-исследования-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель исследования</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Настройка-окружения" data-toc-modified-id="Настройка-окружения-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Настройка окружения</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Функции" data-toc-modified-id="Функции-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Проверка пропусков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Была-машина-в-ремонте-или-нет-(NotRepaired)" data-toc-modified-id="Была-машина-в-ремонте-или-нет-(NotRepaired)-4.4.1"><span class="toc-item-num">4.4.1&nbsp;&nbsp;</span>Была машина в ремонте или нет (NotRepaired)</a></span></li><li><span><a href="#Признаки-даты-в-фомате-даты-(DateCrawled,-DateCreated,-LastSeen)" data-toc-modified-id="Признаки-даты-в-фомате-даты-(DateCrawled,-DateCreated,-LastSeen)-4.4.2"><span class="toc-item-num">4.4.2&nbsp;&nbsp;</span>Признаки даты в фомате даты (DateCrawled, DateCreated, LastSeen)</a></span></li><li><span><a href="#Почтовый-индекс-владельца-анкеты-(PostalCode)" data-toc-modified-id="Почтовый-индекс-владельца-анкеты-(PostalCode)-4.4.3"><span class="toc-item-num">4.4.3&nbsp;&nbsp;</span>Почтовый индекс владельца анкеты (PostalCode)</a></span></li><li><span><a href="#Перепроверим-типы-данных" data-toc-modified-id="Перепроверим-типы-данных-4.4.4"><span class="toc-item-num">4.4.4&nbsp;&nbsp;</span>Перепроверим типы данных</a></span></li></ul></li><li><span><a href="#Вывод-по-предобработке" data-toc-modified-id="Вывод-по-предобработке-4.5"><span class="toc-item-num">4.5&nbsp;&nbsp;</span>Вывод по предобработке</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span><ul class="toc-item"><li><span><a href="#Условие-1:" data-toc-modified-id="Условие-1:-5.1.1"><span class="toc-item-num">5.1.1&nbsp;&nbsp;</span>Условие 1:</a></span></li><li><span><a href="#Условие-2:" data-toc-modified-id="Условие-2:-5.1.2"><span class="toc-item-num">5.1.2&nbsp;&nbsp;</span>Условие 2:</a></span></li><li><span><a href="#Условие-3:" data-toc-modified-id="Условие-3:-5.1.3"><span class="toc-item-num">5.1.3&nbsp;&nbsp;</span>Условие 3:</a></span></li></ul></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Дата-время" data-toc-modified-id="Дата-время-5.2.1"><span class="toc-item-num">5.2.1&nbsp;&nbsp;</span>Дата-время</a></span><ul class="toc-item"><li><span><a href="#Дата-скачивания-анкеты-из-базы-[DateCrawled]" data-toc-modified-id="Дата-скачивания-анкеты-из-базы-[DateCrawled]-5.2.1.1"><span class="toc-item-num">5.2.1.1&nbsp;&nbsp;</span>Дата скачивания анкеты из базы [DateCrawled]</a></span></li><li><span><a href="#Дата-создания-анкеты-[DateCreated]" data-toc-modified-id="Дата-создания-анкеты-[DateCreated]-5.2.1.2"><span class="toc-item-num">5.2.1.2&nbsp;&nbsp;</span>Дата создания анкеты [DateCreated]</a></span></li><li><span><a href="#Дата-последней-активности-пользователя-[LastSeen]" data-toc-modified-id="Дата-последней-активности-пользователя-[LastSeen]-5.2.1.3"><span class="toc-item-num">5.2.1.3&nbsp;&nbsp;</span>Дата последней активности пользователя [LastSeen]</a></span></li></ul></li><li><span><a href="#Категории" data-toc-modified-id="Категории-5.2.2"><span class="toc-item-num">5.2.2&nbsp;&nbsp;</span>Категории</a></span><ul class="toc-item"><li><span><a href="#Модель-автомобиля-[Model]" data-toc-modified-id="Модель-автомобиля-[Model]-5.2.2.1"><span class="toc-item-num">5.2.2.1&nbsp;&nbsp;</span>Модель автомобиля [Model]</a></span></li><li><span><a href="#Марка-автомобиля-[Brand]" data-toc-modified-id="Марка-автомобиля-[Brand]-5.2.2.2"><span class="toc-item-num">5.2.2.2&nbsp;&nbsp;</span>Марка автомобиля [Brand]</a></span></li><li><span><a href="#Тип-автомобильного-кузова-[VehicleType]" data-toc-modified-id="Тип-автомобильного-кузова-[VehicleType]-5.2.2.3"><span class="toc-item-num">5.2.2.3&nbsp;&nbsp;</span>Тип автомобильного кузова [VehicleType]</a></span></li><li><span><a href="#Тип-коробки-передач-[Gearbox]" data-toc-modified-id="Тип-коробки-передач-[Gearbox]-5.2.2.4"><span class="toc-item-num">5.2.2.4&nbsp;&nbsp;</span>Тип коробки передач [Gearbox]</a></span></li><li><span><a href="#Тип-топлива-[FuelType]" data-toc-modified-id="Тип-топлива-[FuelType]-5.2.2.5"><span class="toc-item-num">5.2.2.5&nbsp;&nbsp;</span>Тип топлива [FuelType]</a></span></li><li><span><a href="#Была-машина-в-ремонте-или-нет-[NotRepaired]" data-toc-modified-id="Была-машина-в-ремонте-или-нет-[NotRepaired]-5.2.2.6"><span class="toc-item-num">5.2.2.6&nbsp;&nbsp;</span>Была машина в ремонте или нет [NotRepaired]</a></span></li><li><span><a href="#Почтовый-индекс-владельца-анкеты-(пользователя)-[PostalCode]" data-toc-modified-id="Почтовый-индекс-владельца-анкеты-(пользователя)-[PostalCode]-5.2.2.7"><span class="toc-item-num">5.2.2.7&nbsp;&nbsp;</span>Почтовый индекс владельца анкеты (пользователя) [PostalCode]</a></span></li></ul></li><li><span><a href="#Числовые" data-toc-modified-id="Числовые-5.2.3"><span class="toc-item-num">5.2.3&nbsp;&nbsp;</span>Числовые</a></span><ul class="toc-item"><li><span><a href="#Цена-(евро)-[Price]" data-toc-modified-id="Цена-(евро)-[Price]-5.2.3.1"><span class="toc-item-num">5.2.3.1&nbsp;&nbsp;</span>Цена (евро) [Price]</a></span></li><li><span><a href="#Мощность-(л.-с.)-[Power]" data-toc-modified-id="Мощность-(л.-с.)-[Power]-5.2.3.2"><span class="toc-item-num">5.2.3.2&nbsp;&nbsp;</span>Мощность (л. с.) [Power]</a></span></li><li><span><a href="#Пробег-(км)-[Kilometer]" data-toc-modified-id="Пробег-(км)-[Kilometer]-5.2.3.3"><span class="toc-item-num">5.2.3.3&nbsp;&nbsp;</span>Пробег (км) [Kilometer]</a></span></li><li><span><a href="#Год-регистрации-автомобиля-[RegistrationYear]" data-toc-modified-id="Год-регистрации-автомобиля-[RegistrationYear]-5.2.3.4"><span class="toc-item-num">5.2.3.4&nbsp;&nbsp;</span>Год регистрации автомобиля [RegistrationYear]</a></span></li><li><span><a href="#Месяц-регистрации-автомобиля-[RegistrationMonth]" data-toc-modified-id="Месяц-регистрации-автомобиля-[RegistrationMonth]-5.2.3.5"><span class="toc-item-num">5.2.3.5&nbsp;&nbsp;</span>Месяц регистрации автомобиля [RegistrationMonth]</a></span></li></ul></li></ul></li><li><span><a href="#Корреляция-признаков" data-toc-modified-id="Корреляция-признаков-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Корреляция признаков</a></span></li><li><span><a href="#Отбор-признаков" data-toc-modified-id="Отбор-признаков-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Отбор признаков</a></span></li><li><span><a href="#Вывод-по-анализу-данных" data-toc-modified-id="Вывод-по-анализу-данных-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Вывод по анализу данных</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Функция-подготовки-данных" data-toc-modified-id="Функция-подготовки-данных-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Функция подготовки данных</a></span></li><li><span><a href="#Отделим-тестовую-часть" data-toc-modified-id="Отделим-тестовую-часть-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Отделим тестовую часть</a></span><ul class="toc-item"><li><span><a href="#Прямое-кодирование" data-toc-modified-id="Прямое-кодирование-6.2.1"><span class="toc-item-num">6.2.1&nbsp;&nbsp;</span>Прямое кодирование</a></span></li><li><span><a href="#Порядковое-кодирование" data-toc-modified-id="Порядковое-кодирование-6.2.2"><span class="toc-item-num">6.2.2&nbsp;&nbsp;</span>Порядковое кодирование</a></span></li></ul></li></ul></li><li><span><a href="#Исследование-задачи" data-toc-modified-id="Исследование-задачи-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Исследование задачи</a></span><ul class="toc-item"><li><span><a href="#Подбор-базовой-модели" data-toc-modified-id="Подбор-базовой-модели-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Подбор базовой модели</a></span></li><li><span><a href="#Оценка-признаков-на-базовой-модели" data-toc-modified-id="Оценка-признаков-на-базовой-модели-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Оценка признаков на базовой модели</a></span><ul class="toc-item"><li><span><a href="#Случайный-лес" data-toc-modified-id="Случайный-лес-7.2.1"><span class="toc-item-num">7.2.1&nbsp;&nbsp;</span>Случайный лес</a></span></li><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-7.2.2"><span class="toc-item-num">7.2.2&nbsp;&nbsp;</span>Градиентный бустинг</a></span></li></ul></li><li><span><a href="#Переподготовка-данных-(переоценка-признаков)" data-toc-modified-id="Переподготовка-данных-(переоценка-признаков)-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Переподготовка данных (переоценка признаков)</a></span></li><li><span><a href="#Подбор-гиперпараметров" data-toc-modified-id="Подбор-гиперпараметров-7.4"><span class="toc-item-num">7.4&nbsp;&nbsp;</span>Подбор гиперпараметров</a></span><ul class="toc-item"><li><span><a href="#Случайный-лес" data-toc-modified-id="Случайный-лес-7.4.1"><span class="toc-item-num">7.4.1&nbsp;&nbsp;</span>Случайный лес</a></span></li><li><span><a href="#Градиентный-бустинг" data-toc-modified-id="Градиентный-бустинг-7.4.2"><span class="toc-item-num">7.4.2&nbsp;&nbsp;</span>Градиентный бустинг</a></span></li></ul></li><li><span><a href="#Выбор-лучшей-модели" data-toc-modified-id="Выбор-лучшей-модели-7.5"><span class="toc-item-num">7.5&nbsp;&nbsp;</span>Выбор лучшей модели</a></span></li></ul></li><li><span><a href="#Тестирование-модели" data-toc-modified-id="Тестирование-модели-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Тестирование модели</a></span><ul class="toc-item"><li><span><a href="#Тестовые-признаки" data-toc-modified-id="Тестовые-признаки-8.1"><span class="toc-item-num">8.1&nbsp;&nbsp;</span>Тестовые признаки</a></span></li><li><span><a href="#Обучение-модели" data-toc-modified-id="Обучение-модели-8.2"><span class="toc-item-num">8.2&nbsp;&nbsp;</span>Обучение модели</a></span></li><li><span><a href="#Проверка-модели" data-toc-modified-id="Проверка-модели-8.3"><span class="toc-item-num">8.3&nbsp;&nbsp;</span>Проверка модели</a></span></li><li><span><a href="#Проверка-модели-на-адекватность" data-toc-modified-id="Проверка-модели-на-адекватность-8.4"><span class="toc-item-num">8.4&nbsp;&nbsp;</span>Проверка модели на адекватность</a></span></li></ul></li><li><span><a href="#Рабочий-код" data-toc-modified-id="Рабочий-код-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Рабочий код</a></span></li><li><span><a href="#Общие-выводы" data-toc-modified-id="Общие-выводы-10"><span class="toc-item-num">10&nbsp;&nbsp;</span>Общие выводы</a></span></li></ul></div>