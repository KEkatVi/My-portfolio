Обо мне
Привет! Меня зовут Екатерина Кузнецова, я начинающий аналитик данных. В этом репозитории вы можете найти некоторые из моих проектов, выполненных во время обучения и опытов.

Навыки и технологии
Инструменты анализа данных: SQL, Excel:
Системы управления базами данных: PostgreSQL

    Проекты
  
  Проект 1: Калькулятор юнит-экономики онлайн-школы (Excel)

Задачи по проекту:
1. Определить, что является юнитом в экономике онлайн-школы.
2. Посчитать юнит-экономику продукта и предложить сценарий по настройке параметров для выхода на 25%-ную маржинальность.
3. Выбрать оптимальный вариант расчета Retention. 
4. Собрать визуализации основных бизнес-показателей.
5. Исследовать данные о пользователях и их поведении.

Исходные данные

- Подписчики: ID пользователя, часовой пояс, подписка_дата
- Просмотры: идентификатор просмотра, время просмотра (UTC), ID пользователя, movie_id
- Финансы: Месяц, Оплата всего, Базовая цена, Объём скидок, Выручка, Затраты на маркетинг, Постоянные расходы

Для понимания картины, были рассчитаны следующие метрики:

1. Кол-во подписок в каждый месяц       
2. Кол-во просмотров в каждый месяц  
3. Кол-во уникальных просматривающих пользователей в каждый месяц
4. Дата первого просмотра для каждого юзера
5. Кол-во первых просмотров для пользователя в каждый месяц
6. Среднее кол-во просмотров на одного юзера в каждом месяце

Для создания калькулятора юнит-экономики нашего продукта, нам необходимо рассчитать следующие метрики:

1. Количество повторных оплат в каждом месяце
2. Retention для каждого месяца
3. Среднее геометрическое Retention    
4. Лайфтайм       
5. LTR 
6. CAC    
7. Маржинальность

В результате была получена юнит-экономика AS-IS и TO-BE, произведена визуализация результатов в виде презентации с итогами в виде таблицы и графиков, сделаны были выводы и даны рекомендациями по развитию.
  
[Юнит экономика.pptx](https://github.com/KEkatVi/My-portfolio/files/12076146/default.pptx)

  Проект 2: Моделирование изменения балансов студентов (SQL)

Основная задача по проекту: смоделировать изменение балансов студентов. 

Подзадачи:
1. Получить таблицу с датами за каждый календарный день 2016 года для каждого студента.
2. Вывести изменения балансов студентов, найти аномалии в данных, составить список вопросов дата-инженерам и владельцам таблиц.
3. Создать визуализацию (линейную диаграмму) итогового результата. Сделать выводы.

Выводы по проекту:
1. В таблице balances имеются минусовые балансы. В таблице classes имеются студенты, по которым нет успешных оплат в таблице payments в 2016 году, при этом для них в 2016 году успешно проводились регулярные уроки. Есть проблемы с данными? Т.е. данные неполные? или не успевает подгружать? Или был сбой? Также в таблице classes много уроков, где время окончания уроков меньше, чем время его начала. Почему уроки имеют отрицательную продолжительность? Данные о времени начала и окончания некорректные? Поскольку это действительно не стандартная ситуация и нужно выяснить как так произошло. Возможно часть данных была потерена, а возможно на платформе действительно была пробелма и ученики занимались в минус.
2. Общая сумма уроков по балансу учащихся постепенно растет в течение года. Имеется сезонность уроков по дням недели. Максимальная активность с понедельника по четверг, в пятницу спадает, в выходные - примерно в 2 раза ниже, чем в пиковые дни. Среднее количество уроков практически все время росло в течение года от недели к неделе и от месяца к месяцу, было меньше во время майских праздников, летом и во второй половине декабря. Имеется сезонность бесплатных уроков по дням недели. Пиковый день - понедельник, меньше оплат в выходные. Среднее количество оплаченных уроков довольно стабильно росло в течение года. Также положительный баланс, в паре с ростом количества прохождений и начислений уроков может свидетельствовать о развитие школы. Поскольку количество студентов растет и баланс соответственно тоже растет.
![визуал sql](https://github.com/KEkatVi/My-portfolio/assets/139838497/03a519a8-bf43-4dd0-a135-671a38bd5205)

Код запроса

[Код запроса sql.docx](https://github.com/KEkatVi/My-portfolio/files/12076603/sql.docx)

Контактная информация
Электронная почта: ekvi26@yandex.ru
Telegram: ekater1na_kv
