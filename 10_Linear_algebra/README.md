# Защита персональных данных клиентов страховой компании
> **Разработка модели анонимизации персональных данных клиентов страховой компании.**

<br/>

### Описание:

Необходимо защитить данные клиентов страховой компании таким образом, чтобы при преобразовании качество моделей машинного обучения не ухудшилось. 

### Задачи:

- Разработайте такой метод преобразования данных, чтобы по ним было сложно восстановить персональную информацию. Обоснуйте корректность его работы. Подбирать наилучшую модель не требуется.

### Данные:

Персональная информация о клиентах страховой компании

- Признаки: ` пол, возраст и зарплата застрахованного, количество членов его семьи. `
- Целевой признак: ` количество страховых выплат клиенту за последние 5 лет. `

---

## v1:

### Используемые библиотеки:
*matplotlib<br/>numpy<br/>pandas<br/>seaborn<br/>sklearn*

### Ключевые аспекты:

*линейная алгебра<br/>EDA<br/>предобработка данных<br/>регрессия*

### Результат работы:

Данные закодированы.

От преобразования признаков качество модели не изменилось.

### Table of contents:

<div class="toc"><ul class="toc-item"><li><span><a href="#Исходные-данные" data-toc-modified-id="Исходные-данные-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Исходные данные</a></span><ul class="toc-item"><li><span><a href="#Подключаемые-модули" data-toc-modified-id="Подключаемые-модули-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>Подключаемые модули</a></span></li><li><span><a href="#Настройка-окружения" data-toc-modified-id="Настройка-окружения-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>Настройка окружения</a></span></li><li><span><a href="#Общие-параметры" data-toc-modified-id="Общие-параметры-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>Общие параметры</a></span></li><li><span><a href="#Общие-функции" data-toc-modified-id="Общие-функции-1.4"><span class="toc-item-num">1.4&nbsp;&nbsp;</span>Общие функции</a></span></li></ul></li><li><span><a href="#Получение-данных" data-toc-modified-id="Получение-данных-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Получение данных</a></span><ul class="toc-item"><li><span><a href="#Чтение-данных-из-файла" data-toc-modified-id="Чтение-данных-из-файла-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Чтение данных из файла</a></span></li><li><span><a href="#Описание-данных" data-toc-modified-id="Описание-данных-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Описание данных</a></span></li><li><span><a href="#Общая-информация" data-toc-modified-id="Общая-информация-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Общая информация</a></span></li><li><span><a href="#Выводы" data-toc-modified-id="Выводы-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Выводы</a></span></li></ul></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Исправление-заголовков" data-toc-modified-id="Исправление-заголовков-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Исправление заголовков</a></span></li><li><span><a href="#Проверка-на-явные-дубликаты" data-toc-modified-id="Проверка-на-явные-дубликаты-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Проверка на явные дубликаты</a></span></li><li><span><a href="#Проверка-пропусков" data-toc-modified-id="Проверка-пропусков-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Проверка пропусков</a></span></li><li><span><a href="#Замена-типов-данных" data-toc-modified-id="Замена-типов-данных-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Замена типов данных</a></span></li><li><span><a href="#Проверка-смысловых-зависимостей" data-toc-modified-id="Проверка-смысловых-зависимостей-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Проверка смысловых зависимостей</a></span></li><li><span><a href="#Проверка-значений:-ошибки,-выбросы,-дубликаты" data-toc-modified-id="Проверка-значений:-ошибки,-выбросы,-дубликаты-3.6"><span class="toc-item-num">3.6&nbsp;&nbsp;</span>Проверка значений: ошибки, выбросы, дубликаты</a></span><ul class="toc-item"><li><span><a href="#Пол" data-toc-modified-id="Пол-3.6.1"><span class="toc-item-num">3.6.1&nbsp;&nbsp;</span>Пол</a></span></li><li><span><a href="#Возраст" data-toc-modified-id="Возраст-3.6.2"><span class="toc-item-num">3.6.2&nbsp;&nbsp;</span>Возраст</a></span></li><li><span><a href="#Зарплата" data-toc-modified-id="Зарплата-3.6.3"><span class="toc-item-num">3.6.3&nbsp;&nbsp;</span>Зарплата</a></span></li><li><span><a href="#Количество-членов-семьи" data-toc-modified-id="Количество-членов-семьи-3.6.4"><span class="toc-item-num">3.6.4&nbsp;&nbsp;</span>Количество членов семьи</a></span></li><li><span><a href="#Страховые-выплаты" data-toc-modified-id="Страховые-выплаты-3.6.5"><span class="toc-item-num">3.6.5&nbsp;&nbsp;</span>Страховые выплаты</a></span></li></ul></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-3.7"><span class="toc-item-num">3.7&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li><li><span><a href="#Умножение-матриц" data-toc-modified-id="Умножение-матриц-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Умножение матриц</a></span></li><li><span><a href="#Алгоритм-преобразования" data-toc-modified-id="Алгоритм-преобразования-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Алгоритм преобразования</a></span></li><li><span><a href="#Подготовка-данных" data-toc-modified-id="Подготовка-данных-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Признаки-и-целевой-признак" data-toc-modified-id="Признаки-и-целевой-признак-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Признаки и целевой признак</a></span></li><li><span><a href="#Обучающая-и-тестовая-выборки" data-toc-modified-id="Обучающая-и-тестовая-выборки-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Обучающая и тестовая выборки</a></span></li></ul></li><li><span><a href="#Проверка-алгоритма" data-toc-modified-id="Проверка-алгоритма-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Проверка алгоритма</a></span><ul class="toc-item"><li><span><a href="#Модель-на-исходных-(не-преобразованных)-данных:" data-toc-modified-id="Модель-на-исходных-(не-преобразованных)-данных:-7.1"><span class="toc-item-num">7.1&nbsp;&nbsp;</span>Модель на исходных (не преобразованных) данных:</a></span></li><li><span><a href="#Модель-на-преобразованных-данных-(рабочий-алгоритм):" data-toc-modified-id="Модель-на-преобразованных-данных-(рабочий-алгоритм):-7.2"><span class="toc-item-num">7.2&nbsp;&nbsp;</span>Модель на преобразованных данных (рабочий алгоритм):</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-7.3"><span class="toc-item-num">7.3&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li></ul></div>
