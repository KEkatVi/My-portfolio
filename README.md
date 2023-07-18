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

with    first_payments as
        (select   user_id
                , min(transaction_datetime) ::date as first_payment_date ---первая дата транзакции без времени
        from skyeng_db.payments
        where status_name='success'
        group by user_id
        order by user_id
        ),
-------------------конец 1 шага
        all_dates as
        (select distinct(class_start_datetime ::date) as dt ---уникальные даты уроков без времени
        from skyeng_db.classes
        where date_trunc('year',class_start_datetime)='2016-01-01'
        group by dt
        order by dt
        ),
-------------------конец 2 шага
        all_dates_by_user as
        (select   user_id
                , dt
        from first_payments a
            join all_dates b
                 on a.first_payment_date <= b.dt ---даты жизни студента после первой транзакции
        ),
-------------------конец 3 шага
        payments_by_dates as
        (select   user_id
                , transaction_datetime ::date as payment_date ---дата транзакции без времени
                , sum(classes) as transaction_balance_change ---сколько уроков списано в этот день
        from skyeng_db.payments
        where status_name='success'
        group by user_id, payment_date
        order by user_id
        ),
-------------------конец 4 шага
        payments_by_dates_cumsum as
        (select   c.user_id
                , dt
                , transaction_balance_change
                , sum(coalesce(transaction_balance_change, 0)) over (partition by c.user_id order by dt rows between unbounded preceding and current row) as transaction_balance_change_cs ---кумулятивная сумма по полю transaction_balance_change для всех строк до текущ вкл, с разбивкой по user_id и сортировкой по dt
        from all_dates_by_user c 
            left join payments_by_dates d
                     on c.user_id=d.user_id
                     and c.dt=d.payment_date
-------------------конец 5 шага
        ),
        classes_by_dates as
        (select   user_id
                , class_start_datetime ::date as class_date ---день урока
                , count(id_class)*(-1) as classes ---количество пройденных в этот день уроков(минус урок, списание)
        from skyeng_db.classes
        where   class_status in ('success', 'failed_by_student')
                and class_type !='trial'
        group by user_id, class_date
        ),
-------------------конец 6 шага
        classes_by_dates_cumsum as
        (select   f.user_id
                , dt
                , classes
                , sum(coalesce(classes, 0)) over (partition by f.user_id order by dt rows between unbounded preceding and current row) as classes_cs ---кумулятивная сумма по полю classes для всех строк до текущ вкл, с разбивкой по user_id и сортировкой по dt
        from all_dates_by_user f
            left join classes_by_dates g
                     on f.user_id=g.user_id
                     and f.dt=g.class_date
        ),
-------------------конец 7 шага
        balances as
        (select   h.user_id
                , h.dt
                , transaction_balance_change
                , transaction_balance_change_cs
                , classes
                , classes_cs
                , classes_cs + transaction_balance_change_cs as balance
        from payments_by_dates_cumsum h
            left join classes_by_dates_cumsum j
                     on h.user_id=j.user_id
                     and h.dt=j.dt
        )
-------------------конец 8 шага

-- select *
-- from balances
-- order by user_id, dt
-- limit 1000
-- -----------------конец запроса для Задания 1

select    dt
        , sum(transaction_balance_change) as sum_transaction_balance_change
        , sum(transaction_balance_change_cs) as sum_transaction_balance_change_cs
        , sum(classes) as sum_classes
        , sum(classes_cs) as sum_classes_cs
        , sum(balance) as sum_balance
from balances
group by dt
order by dt
-------------------конец 9 шага

