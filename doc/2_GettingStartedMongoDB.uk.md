
# Встановлення Mongodb на Windows

Перейдіть у [Центр завантажень MongoDB](https://www.mongodb.com/download-center/enterprise) -> виберіть у спадному меню **Windows x64** як свою операційну систему, а потім завантажте **файл .msi**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/donwload_center.png)

Після завантаження файлу **msi** на комп'ютер натисніть на завантажений файл.
Тепер ви можете побачити майстра встановлення Windows для встановлення MongoDB-сервера на комп'ютер.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard1.png)

Клацніть **Next**. У наступному вікні прийміть ліцензійну угоду клієнта та натисніть кнопку **Next**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard2.png)

Виберіть тип налаштування як **Complete**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard3.png)

У цьому вікні збережіть усі налаштування за замовчуванням та натисніть **Next**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard4.png)

У цьому вікні зніміть прапорець у полі **Install MongoDB Compass**. Інсталятор встановить **Community version** Compass, у якій бракує певної функціональності, яку ми будемо використовувати в цьому курсі.

Натисність **Install**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard6.png)

Натисніть кнопку **Finish** після того, як інсталятор успішно встановив **MongoDB Server** у вашій системі.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/Installation_wizard7.png)

Тепер перейдіть до цього каталогу, щоб побачити виконувані файли, завантажені на ваш комп’ютер із сервером MongoDB Enterprise.

```
C:\Program Files\MongoDB\Server\4.2\bin
```
![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/bin_directory.png)

Далі ми встановимо шлях для оболонки mongo у змінних оточення, щоб ви могли запускати ці виконувані файли у своєму командному рядку, не маючи необхідності вказувати повний шлях. Не закривайте це вікно ще, оскільки ми повернемось до нього для копіювання шляху до каталогу бін.

Введіть **Змінити змінні системного середовища** або **Edit the system environment variables** або **Изменение системных переменных среды**  на панелі пошуку Windows у нижньому лівому куті екрана.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/system_environment_variables.png)

Натисніть на **Змінні середовища** або **Переменные среды** або **Environment variables**

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/system_properties.png)

З'явиться нове вікно, де можна побачити **Користувацькі змінні** та **Системні змінні**.

Двічі натисніть на **Path** під **Системними змінними**.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/environment_variables.png)

У новому вікні ви можете побачити список шляхів, доданих до змінних оточення.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/list.png)

Поверніться до каталогу bin та скопіюйте адресу.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/copy_path.png)

У вікні **Редагування змінних середовищ** натисніть кнопку **Створити** та **вставте** повний шлях до каталогу бін, який ви щойно скопіювали у буфер обміну. Далі натисніть кнопку **ОК**, щоб зберегти зміни та закрити вікно.

![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/add_path.png)

Тепер ми перевіримо, чи вдало встановлено шлях для оболонки mongo.

Відкрийте командний рядок, ввівши cmd у панель пошуку Windows.

```
mongo --nodb
```
Якщо все пройшло добре, ви могли б бачити щось подібне на екрані.
```
MongoDB shell version v4.2.3
>
```
![](https://university-courses.s3.amazonaws.com/M001/Mongo+shell+installation+guide+windows/mongo_nodb.png)

# Встановлення Mongodb на Mac

MongoDB - це база даних документів, яка належить до сімейства баз даних під
назвою NoSQL - не тільки SQL. У MongoDB записи - це документи, які дуже схожі на
об'єкти JSON в JavaScript. Значення в документах можна шукати за допомогою ключа
їх поля. Документи можуть мати деякі поля / ключі, а не інші, що робить Mongo
надзвичайно гнучким.

**Tap the MongoDB Homebrew Tap**
```
brew install mongodb
```
```
brew install mongodb-community@4.2
```

**Після завантаження mongo**, створіть */data/db* directory в root
```
mkdir -p /data/db
```
В останньому Mac OS x ви стикаєтеся з такою проблемою, як:
```
mkdir: /data/db: Read-only file system
```

Тому ви більше не можете писати в корінь. Ось ще одне рішення (ми можемо
приймати dir де завгодно і під час виклику **mongo daemon** передати *- dbpath =
OUR_PATH / data / db*):
```
cd ~
mkdir -p data/db # preferably with sudo
```


**Запустіть Mongo daemon**, у терміналі:
```
# Absolute path in my case
mongod --dbpath=/Users/macair/data/db

# If you were able to make /data/db in root, then just
mongod
```
Це має запустити сервер Mongo.

**Запустіть Mongo shell**, при цьому Mongo daemon працює в одному терміналі,
введіть `mongo` в інше вікно терміналу. :
```
mongo
```
Це запустить оболонку Mongo, яка є додатком для доступу до даних у MongoDB.

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/mongo.png)


# Тестування mongo
Вставлення одна за одною команд:
```
# sets database with name 'use' for current usage, if it doesnt exist, it will be created
use test
# adds in collection 'users' of db 'test' object in json format
db.users.save( { name: "Tom" } )
# prints all objects from db 'test'
db.users.find()
```
![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/test1.png)

# Графічний Клієнт Compass
Натисніть сюди, щоб [завантажити](https://www.mongodb.com/download-center/compass)

Після установки за посиланням ми встановлюємо нове З'єднання, усі поля
встановлюються за замовчуванням.

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/compass1.png)

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/compass2.png)

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/compass3.png)

# Підключення до MongoDB за допомогою Compass

Завантажте Compass з [центру завантаження MongoDB](https://www.mongodb.com/download-center/compass). Якщо ви вже встановлини Compass, тоді переконайтеся, що ви використовуєте останню (стабільну) версію Compass та при необхідності оновіть. НЕ завантажуйте та не встановлюйте версію "Community Edition Stable". Встановіть його та запустіть.

Коли відкриється Компас, ви побачите сторінку під назвою "New Connection".
![](https://university-courses.s3.amazonaws.com/M001/Compass_start_screen.png)

Компас пропонує два способи підключення до розгортання:
- Використання рядка з'єднання
- Заповнення інформації про розгортання в окремих полях

**Використання рядка з'єднання**

Вставте цей рядок у поле рядка підключення та натисніть CONNECT.

```
mongodb+srv://student:mongodb-basics@cluster0-jxeqq.mongodb.net/test
```
Нагадуємо, що _student_ це логін _mongodb-basics_ це пароль до вашого облікового запису

Додавайте цей рядок з'єднання як улюблений. Це дозволить легко підключитися до розгортання нашого класу MongoDB після закриття та перезавантаження компаса. 

![](https://university-courses.s3.amazonaws.com/M001/Compass_add_favorite.png)

**Заповнення інформації про розгортання в окремих полях**

Якщо рядок підключення не працював для вас, ви можете вручну заповнити окремі поля та спробувати підключитись таким чином.

Клацніть на _Fill in connection fields individually_:

![](https://university-courses.s3.amazonaws.com/M001/Compass_fill_in_fields.png)

Використовуйте наступну інформацію, щоб заповнити цю форму, але не натискайте "Connect", поки ви не додасте це як своє улюблене з'єднання.

_Вкладка Hostname_

**Hostname**: cluster0-shard-00-00-jxeqq.mongodb.net

**Port**: 27017

**Authentication**: Username / Password

**Username**: student

**Password**: mongodb-basics

**Authentication Database**: admin


![](https://university-courses.s3.amazonaws.com/M001/Compass_hostname_connection.png)

Вкладка _More Options_

**Replica Set Name**: Cluster0-shard-0

**Read Preference**: Primary Preferred

![](https://university-courses.s3.amazonaws.com/M001/Compass_more_options_connection.png)

# Зверніть увагу, що mongod повинен працювати, інакше Компас не під'єднається

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/connection.png)

