# Базові команди для роботи з MongoDB

**db.collection.insertOne()**

Призначення: Вставляє документ у колекцію.

Синтаксис:
```js
db.collection.insertOne(
   <document>
)
```

**db.collection.insertMany()**

Призначення: Вставляє декілька документів у колекцію.

Синтаксис:
```js
db.collection.insertOne(
   [ <document 1> , <document 2>, ... ]
)
```

**db.collection.updateOne()**

Призначення: Оновлення одного документа в колекції на основі фільтра.

Синтаксис:
```js
db.collection.updateOne(
   <filter>,
   <update>
)
```


**db.collection.updateMany()**

Призначення: Оновлення декількох документів в колекції на основі фільтра.

Синтаксис:
```js
db.collection.updateMany(
   <filter>,
   <update>
)
```

**db.collection.replaceOne()**

Призначення: Замінює один документ у колекції на основі фільтра.

Синтаксис:
```js
db.collection.replaceOne(
   <filter>,
   <replacement>
)
```


**db.collection.deleteOne()**

Призначення: Вилучає один документ із колекції.

Синтаксис:
```js
db.collection.deleteOne(
   <filter>
)
```


**db.collection.deleteMany()**

Призначення: Вилучає декілька документів із колекції.

Синтаксис:
```js
db.collection.deleteOne(
   <filter>
)
```

**db.collection.find()**

Призначення: Вибирає документи у колекції чи перегляді та повертає курсор до вибраних документів.

Синтаксис: 
```js
db.collection.find(
   { field1: <value>, field2: <value> ... }
)
```


# Оператори запитів(query) та проекцій(projection)

_для кожного селектору є посилання з прикладами_

## Селектори запитів

| Name   | Description                                                         |
|--------|:-------------------------------------------------------------------:|
| [$eq](https://www.bookstack.cn/read/mongodb-4.2-manual/6340320dc3dfc823.md#op._S_eq)	   | Відповідає значенням, рівним заданому значенню.                     | 
| [$gt](https://www.bookstack.cn/read/mongodb-4.2-manual/b18f99cdd71e3948.md#op._S_gt)	   | Відповідає значенням, що перевищують задане значення.               |
| [$gte](https://www.bookstack.cn/read/mongodb-4.2-manual/69028f025e729a31.md#op._S_gte)	| Відповідає значенням, що більше або дорівнює заданому значенню.     |
| [$in](https://www.bookstack.cn/read/mongodb-4.2-manual/17161a7651d20b5a.md#op._S_in)	   | Відповідає будь-якому зі значень, вказаних у масиві.                |
| [$lt](https://www.bookstack.cn/read/mongodb-4.2-manual/750260d4249e8c35.md#op._S_lt)	   | Відповідає значенням, меншим за вказане значення.                   |
| [$lte](https://www.bookstack.cn/read/mongodb-4.2-manual/eea81d13c92ecd5f.md#op._S_lte)	| Відповідає значенням, меншим або рівним заданому значенню.          |
| [$ne](https://www.bookstack.cn/read/mongodb-4.2-manual/bd164579d80de3c9.md#op._S_ne)	   | Відповідає всім значенням, не рівним заданому значенню.             |
| [$nin](https://www.bookstack.cn/read/mongodb-4.2-manual/a2c67159e3c89845.md#op._S_nin)	| Не відповідає жодному зі значень, вказаних у масиві.                |

## Логічні

| Name	| Description                                                                                                     |
|--------|:---------------------------------------------------------------------------------------------------------------:|
| [$and](https://www.bookstack.cn/read/mongodb-4.2-manual/0ac9f896539e32b6.md#op._S_and)	| Приєднується до запитів із логічним ```AND``` повертає всі документи, які відповідають умовам обох пунктів.     |
| [$not](https://www.bookstack.cn/read/mongodb-4.2-manual/3d1841137078acd3.md#op._S_not)	| Інвертує вираження запиту та повертає документи, які не відповідають виразу запиту.                             |
| [$nor](https://www.bookstack.cn/read/mongodb-4.2-manual/df28b9509c0a357d.md#op._S_nor)	| Приєднується до запитів із логічним ```NOR``` повертає всі документи, які не відповідають обом пунктам.         |
| [$or](https://www.bookstack.cn/read/mongodb-4.2-manual/0c1c9c15f2142aa5.md#op._S_or)	   | Приєднується до запитів із логічним ```OR``` повертає всі документи, які відповідають умовам будь-якого пункту. |

## Елемент

| Name	   | Description                                   | 
|-----------|:---------------------------------------------:|
| [$exists](https://www.bookstack.cn/read/mongodb-4.2-manual/4add7c495fb97712.md#op._S_exists)	| Відповідає документам, що мають вказане поле. |
| [$type](https://www.bookstack.cn/read/mongodb-4.2-manual/2c4a8fe9a41bc1ac.md#op._S_type)	   | Вибирає документи, якщо поле вказаного типу.  |

## Оцінення

| Name	      | Description                                                                                        |
|--------------|:--------------------------------------------------------------------------------------------------:|
| [$expr](https://www.bookstack.cn/read/mongodb-4.2-manual/b901afb7f2d4a064.md#op._S_expr)	      | Дозволяє використовувати вирази агрегації в мові запиту.                                           |
| [$jsonSchema](https://www.bookstack.cn/read/mongodb-4.2-manual/1bd456d35e6d6b5e.md#op._S_jsonSchema)	| Обгрунтуйте документи щодо даної схеми JSON.                                                       |
| [$mod](https://www.bookstack.cn/read/mongodb-4.2-manual/fccf3a5c1e8c9ba5.md#op._S_mod)	      | Виконує модульну операцію над значенням поля та вибирає документи із заданим результатом.          |
| [$regex](https://www.bookstack.cn/read/mongodb-4.2-manual/83747e50d5c91415.md#op._S_regex)	      | Вибирає документи, у яких значення відповідають заданому регулярному виразу.                       |
| [$text](https://www.bookstack.cn/read/mongodb-4.2-manual/6b560c502d5a7e0e.md#op._S_text)	      | Здійснює пошук тексту.                                                                             |
| [$where](https://www.bookstack.cn/read/mongodb-4.2-manual/4781820d20f6b9c9.md#op._S_where)	      | Відповідає документам, що задовольняють вираз JavaScript.                                          |

## Масив

| Name	      | Description                                                                               |
|--------------|:-----------------------------------------------------------------------------------------:|
| [$all](https://www.bookstack.cn/read/mongodb-4.2-manual/5591935e963488aa.md#op._S_all)	      | Збігає масиви, які містять усі елементи, вказані в запиті.                                |
| [$elemMatch](https://www.bookstack.cn/read/mongodb-4.2-manual/df0d30f33b468592.md#op._S_elemMatch)	| Вибирає документи, якщо елемент у полі масиву відповідає всім заданим умовам $ elemMatch. |
| [$size](https://www.bookstack.cn/read/mongodb-4.2-manual/1f2e158b67d9160d.md#op._S_size)	      | Вибирає документи, якщо поле масиву заданого розміру.                                     |

## За приклад візьмемо: 

### $gte

_**Syntax: {field: {$gte: value} }**_

Цей запит вибере всі документи в ```inventory```, де значення кількості поля більше або дорівнює ```20```.

```js
db.inventory.find( { qty: { $gte: 20 } } )
```

Ця операція update () встановлює значення ```price``` поля, яке містить вбудований носій документа ```carrier```, значення поля ```fee``` перевищує або дорівнює ```2```.

```js
db.inventory.update( { "carrier.fee": { $gte: 2 } }, { $set: { price: 9.99 } } )
```

### $elemMatch (query)

_**Syntax: { <field>: { $elemMatch: {<query1>,<query2>, ... } } }**_
 
Враховуючи такі документи в колекції: 

```js
{ _id: 1, results: [ 82, 85, 88 ] }
{ _id: 2, results: [ 75, 88, 89 ] }
```

Наступний запит відповідає лише тим документам, де масив ```results``` містить щонайменше один елемент, який є більшим або рівним ```80``` та менше ```85```.

```js
db.scores.find(
   { results: { $elemMatch: { $gte: 80, $lt: 85 } } }
)
```

Запит поверне 

```js
{ "_id" : 1, "results" : [ 82, 85, 88 ] }
```


# Інші приклади

Припустимо, ми маємо моделювати структуру, яка містить списки розсилки та дані
про людей. Приклад [тут](https://habr.com/ru/post/144798/)

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/schema.png)

Наступні вимоги:
* У людини може бути одна або кілька адрес електронної пошти;
* Людина може перебувати в будь-якій кількості списків розсилки;
* Людина може вибрати будь-яке ім’я для будь-якого списку розсилки, в якому вона
перебуває.

Давайте подивимось, як буде виглядати наша модель даних, якщо ніде дані не
вбудовуються.

**People** з ім'ям та паролем

```js
{
    _id: PERSON_ID,
    name: "Name Surname",
    pw: "Hashed password"
}
```

**Adresses** де кожен email прив'язується до конкретного абонента
```js
{
    _id: ADDRESS_ID,
    person: PERSON_ID,
    address: "vpupkin@gmail.com"
}
```

**Groups** ми можемо визначити деякі додаткові поля
```js
{
    _id: GROUP_ID
}
```

**Memberships** об’єднує людей у ​​групи

```js
{
    _id: MEMBERSHIP_ID,
    person: PERSON_ID,
    group: GROUP_ID,
    address: ADDRESS_ID,
    group_name: "Семья"
}
```

Ця модель даних чітка, проста у розробці та проста у обслуговуванні. Ми створили
модель, яку зручно використовувати в db SQL. У той же час ми не врахували
документоорієнтований підхід MongoDB.

Давайте подивимось, що ми зробимо, щоб, наприклад, отримати електронні адреси
всіх членів однієї групи, якщо ми маємо одну відому адресу електронної пошти та
назву цієї групи:

1. У колекції Adresses за відомою електронною поштою ми знаходимо PERSON_ID;
2. У колекції Memberships за отриманою PERSON_ID та відомою назвою Групи
знаходимо GROUP_ID;
3. Знову в колекції Memberships за отриманим GROUP_ID знаходимо список підписок
цієї групи;
4. І нарешті, із колекції адрес ADDRESS_ID, переглядаючи кожну підписку зі
списку отриманих, ми отримуємо список адрес електронної пошти.


## Модель з частково вбудованими даними
**People**
```js
{
    _id: PERSON_ID,
    name: "Name Surname",
    pw: "Hashed password",
    addresses: ["random@gmail.com", "random@mail.ru", ...],
    memberships: [{
        address: "random@gmail.com",
        group_name: "mongodb tutorial",
        group: GROUP_ID
    }, ...]
}
```

**Groups** ми можемо визначити деякі додаткові поля
```js
{
    _id: GROUP_ID
}
```
Запит, про який ми говорили вище, тепер виглядатиме так:

1. У колекції People ми знаходимо абонента з потрібною адресою електронної
пошти, серед підписок якої є группа з потрібним іменем;
2. Використовуючи GROUP_ID знайденої підписки, ми знайдемо інших людей цієї
групи в колекції People і отримаємо їхні електронні адреси безпосередньо з
підписки.

```js
people = [
               {
                 "_id": 1,
                 "name": "Name1 Surname1",
                 "pw": "1111",
                 "addresses": ["beefmilf@gmail.com", "beefmilf@mail.ru"],
                 "memberships": [
                                 {"address": "beefmilf@gmail.com", "group_name": "group1", "group": 1},
                                 {"address": "beefmilf@gmail.com", "group_name": "group2", "group": 2}
                                 ]
               },
               {
                 "_id": 2,
                 "name": "Name2 Surname2",
                 "pw": "2222",
                 "addresses": ["convmonk@gmail.com", "convmonk@mail.ru"],
                 "memberships": [
                                 {"address": "convmonk@gmail.com", "group_name": "mongo tutorial", "group": 1},
                                 {"address": "convmonk@mail.ru", "group_name": "mongoose", "group": 2},
                                 {"address": "convmonk@gmail.com", "group_name": "something", "group": 3}
                                 ]
               },
               {
                 "_id": 3,
                 "name": "Name3 Surname3",
                 "pw": "3333",
                 "addresses": ["random@gmail.com", "random@mail.ru"],
                 "memberships": [
                                 {"address": "random@gmail.com", "group_name": "rand name 1", "group": 1},
                                 {"address": "random@gmail.com", "group_name": "rand name 1", "group": 2},
                                 {"address": "random@mail.ru", "group_name": "rand name 1", "group": 3}
                                 ]
               },
               {
                 "_id": 4,
                 "name": "Name4 Surname4",
                 "pw": "3333",
                 "addresses": ["bot2001@gmail.com", "bot2001@mail.ru"],
                 "memberships": [
                                 {"address": "bot2001gmail.ru", "group_name": "mongo tutorial", "group": 1},
                                 {"address": "bot2001@gmail.com", "group_name": "mongoose", "group": 2},
                                 {"address": "bot2001@gmail.com", "group_name": "something", "group": 3}
                                 ]
               }

]

groups = [
          {"_id": 1}, 
          {"_id": 2}, 
          {"_id": 3}
]
```

Створіть колекцію people та додайте 4 документи

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/people1.png)

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/people2.png)

