
# Підключення Atlas

**Що таке Atlas?**

- База даних як послуга
- Зберігає дані у хмарі
- Обробляє реплікацію: підтримання зайвих копій даних для збільшення доступності даних

**Як працює Atlas?**

Користувачі Atlas можуть розгорнути кластери, що представляють собою групи серверів, які зберігають ваші дані.
Ці сервери налаштовані в тому, що ми називаємо набір реплік, який є кластером, де кожен сервер зберігає однакові дані.

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/cluster.jpg)

Це означає, що щоразу, коли ви вставляєте або оновлюєте документ, надлишки копій зберігаються у наборі.
Використання набору реплік гарантує, що якщо ви втратите доступ до одного сервера у своєму кластері, ви не збираєтеся втрачати свої дані.
Усі сервери вашого кластеру існують у хмарі на обраного вами постачальника хмарних послуг.

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/cluster-lose.jpg)

**Чому ми повинні використовувати Atlas?**

Однією з головних причин є простота налаштування.
Atlas керує деталями створення кластерів для вас, що спрощує оперативні витрати на виконання розгортання.
Інтерфейс Atlas полегшує управління та розгортання MongoDB через хмарні постачальники та регіони.
Atlas - це також чудовий спосіб експериментувати з новими інструментами та можливостями MongoDB.

**Створення свого кластера-пісочниці**

Якщо у вас немає облікового запису Atlas, [створіть](https://www.mongodb.com/university-signup)
Після створення нового облікового запису вам буде запропоновано створити ваш перший кластер:

![](https://university-courses.s3.amazonaws.com/M001/cluster_create.png)

Тепер ви повинні обрати хмарного постачальника (провайдера) для свого кластера. 
Виберіть AWS як постачальника хмарних ситуацій, а для регіону _Region_ виберіть місце, найближче до вас, яке має мітку _Free Tier Available_. Зауважте, що місцем розташування за замовчуванням є _N.Virginia (us-east-1). Free Tier Available_

![](https://s3.amazonaws.com/university-courses/m220/cluster_provider.png)

Виберіть кластер рівня M0 Sandbox:

![](https://university-courses.s3.amazonaws.com/M001/cluster_tier.png)

Клацніть на Cluster Name, введіть "Sandbox" у відповідне текстове поле та натисніть "Create Cluster":

![](https://university-courses.s3.amazonaws.com/M001/m001_cluster_name.png)

Після створення кластера вас буде переспрямовано на інформаційну панель облікового запису:

![](https://university-courses.s3.amazonaws.com/M001/account_dashboard.png)

Перейдіть до Settings та змініть назву проекту на "M001":

![](https://university-courses.s3.amazonaws.com/M001/m001_project_rename.png)

Потім налаштуйте параметри безпеки цього кластеру, включивши Білий список IP Whitelist:

Оновіть свій білий список IP-адрес, щоб ваша програма могла спілкуватися з кластером. Перейдіть на вкладку "Network Access", а потім натисніть "Add IP Address".

![](https://university-courses.s3.amazonaws.com/M001/m001_ip_whitelisting0.png)

На екрані з’явиться нове запит із запитом "Add Whitelist Entry". Клацніть на "Allow Access from Anywhere", а потім натисніть "Confirm".

![](https://university-courses.s3.amazonaws.com/M001/m001_ip_whitelisting.png)

_Зауважте, що ми зазвичай не рекомендуємо відкривати кластер Atlas, щоб дозволити доступ з будь-якого місця. Ми робимо це для цього класу, щоб мінімізувати проблеми з мережею, з якими ви можете зіткнутися._

Потім створіть додаток MongoDB користувача, необхідного для цього курсу.

Перейдіть на вкладку "Database Access", а потім натисніть "ADD NEW USER".

![](https://university-courses.s3.amazonaws.com/M001/m001_user0.png)

Запам'ятайте свої логін та пароль та нікому не кажіть. Однак для спроби, можете створити користувача з такими обліковими даними:

- ім'я користувача: student
- пароль: mongodb-basics

Дайте цьому користувачеві привілей Read and write to any database:

![](https://s3.amazonaws.com/university-courses/M001/m001_user.png)

Вітаємо вас зі створенням першого Atlas застосунку! Тепер у вас є база даних MongoDB, до якої ви можете отримати доступ з будь-якої точки світу.

