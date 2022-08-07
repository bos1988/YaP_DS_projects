# Исследование тарифных планов <br/>—  анализ телеком компании
> **Анализ поведения клиентов и поиск оптимального тарифа на основе данных клиентов оператора сотовой связи.**

<br/>

### Описание:

Клиентам федерального оператора сотовой связи предлагают два тарифных плана: «Смарт» и «Ультра». Чтобы скорректировать рекламный бюджет, коммерческий департамент хочет понять, какой тариф приносит больше денег.<br/>
Нужно сделать предварительный анализ тарифов на небольшой выборке клиентов, проанализировать поведение клиентов и сделать вывод — какой тариф лучше.

### Задачи:

- Подготовить данные.
- Провести предварительный анализ тарифов на небольшой выборке клиентов.
- Проанализировать поведение клиентов и сделать вывод — какой тариф лучше.
- Проверить гипотезы:
    - средняя выручка пользователей тарифов «Ультра» и «Смарт» различаются;
    - средняя выручка пользователи из Москвы отличается от выручки пользователей из других регионов.

### Данные:

Данные федерального оператора сотовой связи.

Таблица *users* (информация о пользователях):
- ` user_id ` – уникальный идентификатор пользователя
- ` first_name ` – имя пользователя
- ` last_name ` – фамилия пользователя
- ` age ` – возраст пользователя (годы)
- ` reg_date ` – дата подключения тарифа (день, месяц, год)
- ` churn_date ` – дата прекращения пользования тарифом (если значение пропущено, то тариф ещё действовал на момент выгрузки данных)
- ` city ` – город проживания пользователя
- ` tarif ` – название тарифного плана

Таблица *calls* (информация о звонках):
- ` id ` – уникальный номер звонка
- ` call_date ` – дата звонка
- ` duration ` – длительность звонка в минутах
- ` user_id ` – идентификатор пользователя, сделавшего звонок

Таблица *messages* (информация о сообщениях):
- ` id ` – уникальный номер звонка
- ` message_date ` – дата сообщения
- ` user_id ` – идентификатор пользователя, отправившего сообщение

Таблица *internet* (информация об интернет-сессиях):
- ` id ` – уникальный номер сессии
- ` mb_used ` – объём потраченного за сессию интернет-трафика (в мегабайтах)
- ` session_date ` – дата интернет-сессии
- ` user_id ` – идентификатор пользователя

Таблица *tariffs* (информация о тарифах):
- ` tariff_name ` – название тарифа
- ` rub_monthly_fee ` – ежемесячная абонентская плата в рублях
- ` minutes_included ` – количество минут разговора в месяц, включённых в абонентскую плату
- ` messages_included ` – количество сообщений в месяц, включённых в абонентскую плату
- ` mb_per_month_included ` – объём интернет-трафика, включённого в абонентскую плату (в мегабайтах)
- ` rub_per_minute ` – стоимость минуты разговора сверх тарифного пакета (например, если в тарифе 100 минут разговора в месяц, то со 101 минуты будет взиматься плата)
- ` rub_per_message ` – стоимость отправки сообщения сверх тарифного пакета
- ` rub_per_gb ` – стоимость дополнительного гигабайта интернет-трафика сверх тарифного пакета (1 гигабайт = 1024 мегабайта)

---

## v1:

### Используемые библиотеки:
*collections<br/>matplotlib<br/>numpy<br/>pandas<br/>pymystem3<br/>scipy*

### Ключевые аспекты:

*обработка данных<br/>histogram<br/>boxplot<br/>объединение таблиц<br/>статистический тест<br/>критерий Стьюдента*

### Результат работы:

**Тариф "Смарт" выгоднее:** - более низкий порог вхождения (больший охват клиентов) - параметры подходят под запросы большей аудитории (больший охват клиентов) - доход более чем в 2,5 раза потенциально заявленного - суммарная выручка за год выше

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цель-проекта" data-toc-modified-id="Цель-проекта-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цель проекта</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули,-общие-функции:-" data-toc-modified-id="Подключаемые-модули,-общие-функции:--2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули, общие функции: <a id="functions" rel="nofollow"></a></a></span></li><li><span><a href="#Общая-информацция" data-toc-modified-id="Общая-информацция-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Общая информацция</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Выводы:" data-toc-modified-id="Выводы:-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Выводы:</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Проверка пропусков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Замена типов данных</a></span></li><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Ошибки,-выбросы,-дубликаты" data-toc-modified-id="Ошибки,-выбросы,-дубликаты-3.6"><span class="toc-item-num">3.6&nbsp;&nbsp;</span>Ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Неявные-дубликаты" data-toc-modified-id="Неявные-дубликаты-3.6.1"><span class="toc-item-num">3.6.1&nbsp;&nbsp;</span>Неявные дубликаты</a></span></li><li><span><a href="#Диапазоны,-выбросы" data-toc-modified-id="Диапазоны,-выбросы-3.6.2"><span class="toc-item-num">3.6.2&nbsp;&nbsp;</span>Диапазоны, выбросы</a></span><ul class="toc-item"><li><span><a href="#Даты-подключения" data-toc-modified-id="Даты-подключения-3.6.2.1"><span class="toc-item-num">3.6.2.1&nbsp;&nbsp;</span>Даты подключения</a></span></li><li><span><a href="#Даты-отключения" data-toc-modified-id="Даты-отключения-3.6.2.2"><span class="toc-item-num">3.6.2.2&nbsp;&nbsp;</span>Даты отключения</a></span></li><li><span><a href="#Возраст-пользователей" data-toc-modified-id="Возраст-пользователей-3.6.2.3"><span class="toc-item-num">3.6.2.3&nbsp;&nbsp;</span>Возраст пользователей</a></span></li><li><span><a href="#Дительность-звонков" data-toc-modified-id="Дительность-звонков-3.6.2.4"><span class="toc-item-num">3.6.2.4&nbsp;&nbsp;</span>Дительность звонков</a></span></li><li><span><a href="#Объем-интернет-трафика" data-toc-modified-id="Объем-интернет-трафика-3.6.2.5"><span class="toc-item-num">3.6.2.5&nbsp;&nbsp;</span>Объем интернет-трафика</a></span></li><li><span><a href="#Число-сообщений" data-toc-modified-id="Число-сообщений-3.6.2.6"><span class="toc-item-num">3.6.2.6&nbsp;&nbsp;</span>Число сообщений</a></span></li></ul></li></ul></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.7"><span class="toc-item-num">3.7&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Дополнение-данных" data-toc-modified-id="Дополнение-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Дополнение данных</a></span><ul class="toc-item"><li><span><a href="#Добавленние-расчетных-дат" data-toc-modified-id="Добавленние-расчетных-дат-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Добавленние расчетных дат</a></span></li><li><span><a href="#Добавление-порядкового-номера-месяца-пользования" data-toc-modified-id="Добавление-порядкового-номера-месяца-пользования-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Добавление порядкового номера месяца пользования</a></span></li><li><span><a href="#Месячные-объемы-потребления" data-toc-modified-id="Месячные-объемы-потребления-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Месячные объемы потребления</a></span></li><li><span><a href="#Помесячная-выручка-с-каждого-пользователя" data-toc-modified-id="Помесячная-выручка-с-каждого-пользователя-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Помесячная выручка с каждого пользователя</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Таблицы" data-toc-modified-id="Таблицы-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Таблицы</a></span></li><li><span><a href="#Анализ-минут-разговора" data-toc-modified-id="Анализ-минут-разговора-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Анализ минут разговора</a></span></li><li><span><a href="#Анализ-сообщений" data-toc-modified-id="Анализ-сообщений-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Анализ сообщений</a></span></li><li><span><a href="#Анализ-интернет-трафика" data-toc-modified-id="Анализ-интернет-трафика-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Анализ интернет-трафика</a></span></li><li><span><a href="#Анализ-стоимости-услуг" data-toc-modified-id="Анализ-стоимости-услуг-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Анализ стоимости услуг</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-5.6"><span class="toc-item-num">5.6&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Проверка-гипотез" data-toc-modified-id="Проверка-гипотез-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Проверка гипотез</a></span><ul class="toc-item"><li><span><a href="#Гипотеза-1" data-toc-modified-id="Гипотеза-1-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Гипотеза 1</a></span></li><li><span><a href="#Гипотеза-2" data-toc-modified-id="Гипотеза-2-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Гипотеза 2</a></span></li><li><span><a href="#Гипотеза-3" data-toc-modified-id="Гипотеза-3-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Гипотеза 3</a></span></li><li><span><a href="#Гипотеза-4" data-toc-modified-id="Гипотеза-4-6.4"><span class="toc-item-num">6.4&nbsp;&nbsp;</span>Гипотеза 4</a></span></li><li><span><a href="#Гипотеза-5" data-toc-modified-id="Гипотеза-5-6.5"><span class="toc-item-num">6.5&nbsp;&nbsp;</span>Гипотеза 5</a></span></li></ul></li><li><span><a href="#Общий-вывод" data-toc-modified-id="Общий-вывод-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Общий вывод</a></span></li></ul></div>
