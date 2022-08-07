# Исследование рынка компьютерных игр
> **Выявление закономерностей, определяющих успешность игры на основе исторических данных о продажах игр из открытых источников**

<br/>

### Описание:

Для интернет-магазина, который продаёт по всему миру компьютерные игры нужно выявить определяющие успешность игры закономерности. Это позволит сделать ставку на потенциально популярный продукт и спланировать рекламные кампании.<br/>
Перед вами данные до 2016 года. Представим, что сейчас декабрь 2016 г., и вы планируете кампанию на 2017-й. Нужно отработать принцип работы с данными. Неважно, прогнозируете ли вы продажи на 2017 год по данным 2016-го или же 2027-й — по данным 2026 года.

### Задачи:

- Подготовить данные.
- Провести исследовательский анализ данных.
- Составить портрет пользователя каждого региона.
- Проверить гипотезы:
    - Средние пользовательские рейтинги платформ Xbox One и PC одинаковые;
    - Средние пользовательские рейтинги жанров Action и Sports разные.


### Данные:

Исторические данные о продажах игр из открытых источников

- ` Name ` – название игры
- ` Platform ` – платформа
- ` Year_of_Release ` – год выпуска
- ` Genre ` – жанр игры
- ` NA_sales ` – продажи в Северной Америке (миллионы проданных копий)
- ` EU_sales ` – продажи в Европе (миллионы проданных копий)
- ` JP_sales ` – продажи в Японии (миллионы проданных копий)
- ` Other_sales ` – продажи в других странах (миллионы проданных копий)
- ` Critic_Score ` – оценка критиков (максимум 100)
- ` User_Score ` – оценка пользователей (максимум 10)
- ` Rating ` – рейтинг от организации ESRB (англ. Entertainment Software Rating Board). Эта ассоциация определяет рейтинг компьютерных игр и присваивает им подходящую возрастную категорию.

---

## v1:

### Используемые библиотеки:
*collections<br/>matplotlib<br/>numpy<br/>pandas<br/>pymystem3<br/>random<br/>scipy<br/>seaborn*

### Ключевые аспекты:

*обработка данных<br/>дубликаты<br/>пропуски<br/>логическая индексация<br/>группировка<br/>сортировка<br/>визуализация данных<br/>статистики<br/>статистический тест*

### Результат работы:

1. За последние 5 лет продажи и выпуск игр несколько снизились в сравнении с периодом 5-10 летней давности.

2. Старые платформы теряют популярность (период расцвета и заката примерно 6-10 лет). Потенциалтно прибыльные платформы - это новые **PlayStation4** и **XboxOne**

3. Оценки критиков могут повлиять на продажи: чем выше оценка критиков тем больший объем продаж обеспечен игре.

4. Большее предпочтение игроки отдают "динамичным" жанрам игр (**Action**, **Shooter**, **Sports**) и **Role-Playing**.

 Частные картины по регонам:

***Северная Америка:***

- Играют одинаково на ***Xbox*** и ***Playstation*** в жанры ***Action***, ***Shooter***, ***Sports*** с рейтингом ESBR `M`, `E`(`E10+`)

***Япония:***

- Играют одинаково на портативных консолях (***Nintendo, PSVita, WiiU***) в жанры ***Role-Playing*** и ***Action*** без рейтинга ESBR

***Европа и другие страны:***

- Играют преимущественно на ***Playstation*** в жанры ***Action***, ***Shooter***, ***Sports*** с рейтингом ESBR `M`, `E`(`E10+`)

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Цели-проекта" data-toc-modified-id="Цели-проекта-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Цели проекта</a></span></li><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули,-общие-функции:-" data-toc-modified-id="Подключаемые-модули,-общие-функции:--2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Подключаемые модули, общие функции: <a id="functions" rel="nofollow"></a></a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Проверка пропусков</a></span><ul class="toc-item"><li><span><a href="#Названия-игр,-жанры" data-toc-modified-id="Названия-игр,-жанры-3.3.1"><span class="toc-item-num">3.3.1&nbsp;&nbsp;</span>Названия игр, жанры</a></span></li><li><span><a href="#Год-выпуска" data-toc-modified-id="Год-выпуска-3.3.2"><span class="toc-item-num">3.3.2&nbsp;&nbsp;</span>Год выпуска</a></span></li><li><span><a href="#Рейтинг" data-toc-modified-id="Рейтинг-3.3.3"><span class="toc-item-num">3.3.3&nbsp;&nbsp;</span>Рейтинг</a></span></li><li><span><a href="#Итоги-обработки-пропусков" data-toc-modified-id="Итоги-обработки-пропусков-3.3.4"><span class="toc-item-num">3.3.4&nbsp;&nbsp;</span>Итоги обработки пропусков</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.3.5"><span class="toc-item-num">3.3.5&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Замена типов данных</a></span><ul class="toc-item"><li><span><a href="#Год-выпуска" data-toc-modified-id="Год-выпуска-3.4.1"><span class="toc-item-num">3.4.1&nbsp;&nbsp;</span>Год выпуска</a></span></li><li><span><a href="#Оценки-пользователей" data-toc-modified-id="Оценки-пользователей-3.4.2"><span class="toc-item-num">3.4.2&nbsp;&nbsp;</span>Оценки пользователей</a></span></li><li><span><a href="#Шкала-оценок" data-toc-modified-id="Шкала-оценок-3.4.3"><span class="toc-item-num">3.4.3&nbsp;&nbsp;</span>Шкала оценок</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.4.4"><span class="toc-item-num">3.4.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-3.6"><span class="toc-item-num">3.6&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Название-игры" data-toc-modified-id="Название-игры-3.6.1"><span class="toc-item-num">3.6.1&nbsp;&nbsp;</span>Название игры</a></span></li><li><span><a href="#Платформа" data-toc-modified-id="Платформа-3.6.2"><span class="toc-item-num">3.6.2&nbsp;&nbsp;</span>Платформа</a></span></li><li><span><a href="#Жанры" data-toc-modified-id="Жанры-3.6.3"><span class="toc-item-num">3.6.3&nbsp;&nbsp;</span>Жанры</a></span></li><li><span><a href="#Рейтинг-ESRB" data-toc-modified-id="Рейтинг-ESRB-3.6.4"><span class="toc-item-num">3.6.4&nbsp;&nbsp;</span>Рейтинг ESRB</a></span></li><li><span><a href="#Год-выпуска" data-toc-modified-id="Год-выпуска-3.6.5"><span class="toc-item-num">3.6.5&nbsp;&nbsp;</span>Год выпуска</a></span></li><li><span><a href="#Продажи-в-Северной-Америке" data-toc-modified-id="Продажи-в-Северной-Америке-3.6.6"><span class="toc-item-num">3.6.6&nbsp;&nbsp;</span>Продажи в Северной Америке</a></span></li><li><span><a href="#Продажи-в-Европе" data-toc-modified-id="Продажи-в-Европе-3.6.7"><span class="toc-item-num">3.6.7&nbsp;&nbsp;</span>Продажи в Европе</a></span></li><li><span><a href="#Продажи-в-Японии" data-toc-modified-id="Продажи-в-Японии-3.6.8"><span class="toc-item-num">3.6.8&nbsp;&nbsp;</span>Продажи в Японии</a></span></li><li><span><a href="#Продажи-в-других-странах" data-toc-modified-id="Продажи-в-других-странах-3.6.9"><span class="toc-item-num">3.6.9&nbsp;&nbsp;</span>Продажи в других странах</a></span></li><li><span><a href="#Оценка-критиков" data-toc-modified-id="Оценка-критиков-3.6.10"><span class="toc-item-num">3.6.10&nbsp;&nbsp;</span>Оценка критиков</a></span></li><li><span><a href="#Оценка-пользователей" data-toc-modified-id="Оценка-пользователей-3.6.11"><span class="toc-item-num">3.6.11&nbsp;&nbsp;</span>Оценка пользователей</a></span></li></ul></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-3.7"><span class="toc-item-num">3.7&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Дополнение-данных" data-toc-modified-id="Дополнение-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Дополнение данных</a></span><ul class="toc-item"><li><span><a href="#Общий-объем-продаж" data-toc-modified-id="Общий-объем-продаж-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Общий объем продаж</a></span></li><li><span><a href="#Категория-оценки" data-toc-modified-id="Категория-оценки-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Категория оценки</a></span></li></ul></li><li><span><a href="#Анализ-данных" data-toc-modified-id="Анализ-данных-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Анализ данных</a></span><ul class="toc-item"><li><span><a href="#Платформы,-актуальный-период" data-toc-modified-id="Платформы,-актуальный-период-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>Платформы, актуальный период</a></span><ul class="toc-item"><li><span><a href="#Временная-шкала" data-toc-modified-id="Временная-шкала-5.1.1"><span class="toc-item-num">5.1.1&nbsp;&nbsp;</span>Временная шкала</a></span></li><li><span><a href="#Игры-по-платформам" data-toc-modified-id="Игры-по-платформам-5.1.2"><span class="toc-item-num">5.1.2&nbsp;&nbsp;</span>Игры по платформам</a></span></li></ul></li><li><span><a href="#Оценки-игры" data-toc-modified-id="Оценки-игры-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>Оценки игры</a></span></li><li><span><a href="#Жанры" data-toc-modified-id="Жанры-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>Жанры</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Портрет-пользователя-по-регионам" data-toc-modified-id="Портрет-пользователя-по-регионам-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Портрет пользователя по регионам</a></span><ul class="toc-item"><li><span><a href="#Популярные-платформы" data-toc-modified-id="Популярные-платформы-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Популярные платформы</a></span></li><li><span><a href="#Популярные-жанры" data-toc-modified-id="Популярные-жанры-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Популярные жанры</a></span></li><li><span><a href="#Влияние-ретинга-ESBR" data-toc-modified-id="Влияние-ретинга-ESBR-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Влияние ретинга ESBR</a></span></li></ul></li><li><span><a href="#Проверка-гипотез" data-toc-modified-id="Проверка-гипотез-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Проверка гипотез</a></span><ul class="toc-item"><li><span><a href="#Гипотеза-1" data-toc-modified-id="Гипотеза-1-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Гипотеза 1</a></span></li><li><span><a href="#Гипотеза-2" data-toc-modified-id="Гипотеза-2-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Гипотеза 2</a></span></li></ul></li><li><span><a href="#Общие-выводы" data-toc-modified-id="Общие-выводы-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Общие выводы</a></span></li></ul></div>
