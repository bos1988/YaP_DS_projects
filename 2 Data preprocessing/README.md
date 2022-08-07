# Исследование надёжности заёмщиков <br/>—  анализ банковских данных
> **По данным статистики о платежеспособности клиентов банка определение зависимости факта погашения кредита в срок от таких факторов: наличие детей, семейное положение, уровень дохода, цель кредита.**

<br/>

### Описание:

Заказчик — кредитный отдел банка. Нужно разобраться, влияет ли семейное положение и количество детей клиента на факт погашения кредита в срок. Входные данные от банка — статистика о платёжеспособности клиентов.<br/>
Результаты исследования будут учтены при построении модели кредитного скоринга — специальной системы, которая оценивает способность потенциального заёмщика вернуть кредит банку.

### Задачи:

- Провести предобработку данных.
- Определить зависимость факта погашения кредита в срок от таких факторов: наличие детей, семейное положение, уровень дохода, цель кредита.

### Данные:

Данные от банка — статистика о платёжеспособности клиентов. 

- ` children ` – количество детей в семье
- ` days_employed ` – трудовой стаж в днях
- ` dob_days ` – возраст клиента в годах
- ` education ` – образова ни е клиента
- ` education_id ` – идентификатор образования
- ` family_status ` – семейное положение
- ` family_status_id ` – идентификатор семейного положения
- ` gender ` – пол клиента
- ` income_type ` – тип занятости
- ` debt ` – имел ли задолженность по возврату кредитов
- ` total_income ` – доход в месяц
- ` purpose ` – цель получения кредита

---

## v1:

### Используемые библиотеки:
*collections<br/>json<br/>numpy<br/>pandas<br/>pymystem3<br/>seaborn*

### Ключевые аспекты:

*обработка данных<br/>дубликаты<br/>пропуски<br/>типы данных<br/>категоризация<br/>декомпозиция<br/>лемматизация*

### Результат работы:

Был проведен анализ и построены зависимости факта погашения кредита в срок от таких факторов: наличие детей, семейное положение, уровень дохода, цель кредита.

Общий вывод по выявленным зависимостям можно сдеалать такой:

Более ответственные и осторожные люди (которые ответственнее подходят к браку, осторожнее заводят детей, обдумывают крупные покупки, берут кредит при соответсвующем уровне дохода) чаще возвращают кредит вовремя.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Шаг-1.-Откроем-файл-с-данными-и-изучим-общую-информацию" data-toc-modified-id="Шаг-1.-Откроем-файл-с-данными-и-изучим-общую-информацию-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Шаг 1. Откроем файл с данными и изучим общую информацию</a></span></li><li><span><a href="#Шаг-2.-Предобработка-данных" data-toc-modified-id="Шаг-2.-Предобработка-данных-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Шаг 2. Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Обработка-наименований-колонок" data-toc-modified-id="Обработка-наименований-колонок-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Обработка наименований колонок</a></span></li><li><span><a href="#Обработка-пропусков-" data-toc-modified-id="Обработка-пропусков--2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Обработка пропусков <a id="empty" rel="nofollow"></a></a></span></li><li><span><a href="#Замена-типа-данных-" data-toc-modified-id="Замена-типа-данных--2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Замена типа данных <a id="error_type" rel="nofollow"></a></a></span></li><li><span><a href="#Обработка-ошибок-в-значениях-" data-toc-modified-id="Обработка-ошибок-в-значениях--2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Обработка ошибок в значениях <a id="error_value" rel="nofollow"></a></a></span><ul class="toc-item"><li><span><a href="#Столбец-'children'" data-toc-modified-id="Столбец-'children'-2.4.1"><span class="toc-item-num">2.4.1&nbsp;&nbsp;</span>Столбец 'children'</a></span></li><li><span><a href="#Столбец-'dob_years'" data-toc-modified-id="Столбец-'dob_years'-2.4.2"><span class="toc-item-num">2.4.2&nbsp;&nbsp;</span>Столбец 'dob_years'</a></span></li><li><span><a href="#Столбец-'education'" data-toc-modified-id="Столбец-'education'-2.4.3"><span class="toc-item-num">2.4.3&nbsp;&nbsp;</span>Столбец 'education'</a></span></li><li><span><a href="#Столбец-'gender'" data-toc-modified-id="Столбец-'gender'-2.4.4"><span class="toc-item-num">2.4.4&nbsp;&nbsp;</span>Столбец 'gender'</a></span></li><li><span><a href="#Столбец-'income_type'" data-toc-modified-id="Столбец-'income_type'-2.4.5"><span class="toc-item-num">2.4.5&nbsp;&nbsp;</span>Столбец 'income_type'</a></span></li></ul></li><li><span><a href="#Обработка-дубликатов-" data-toc-modified-id="Обработка-дубликатов--2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>Обработка дубликатов <a id="duplicate" rel="nofollow"></a></a></span></li><li><span><a href="#Лемматизация-" data-toc-modified-id="Лемматизация--2.6"><span class="toc-item-num">2.6&nbsp;&nbsp;</span>Лемматизация <a id="lemmas" rel="nofollow"></a></a></span></li><li><span><a href="#Категоризация-данных-" data-toc-modified-id="Категоризация-данных--2.7"><span class="toc-item-num">2.7&nbsp;&nbsp;</span>Категоризация данных <a id="categories" rel="nofollow"></a></a></span><ul class="toc-item"><li><span><a href="#Добавление-категорий" data-toc-modified-id="Добавление-категорий-2.7.1"><span class="toc-item-num">2.7.1&nbsp;&nbsp;</span>Добавление категорий</a></span></li><li><span><a href="#СЛОВАРИ-КАТЕГОРИЙ:" data-toc-modified-id="СЛОВАРИ-КАТЕГОРИЙ:-2.7.2"><span class="toc-item-num">2.7.2&nbsp;&nbsp;</span>СЛОВАРИ КАТЕГОРИЙ:</a></span></li></ul></li></ul></li><li><span><a href="#Шаг-3.-Ответы-на-вопросы" data-toc-modified-id="Шаг-3.-Ответы-на-вопросы-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Шаг 3. Ответы на вопросы</a></span></li><li><span><a href="#Шаг-4.-Общий-вывод" data-toc-modified-id="Шаг-4.-Общий-вывод-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Шаг 4. Общий вывод</a></span></li></ul></div>
