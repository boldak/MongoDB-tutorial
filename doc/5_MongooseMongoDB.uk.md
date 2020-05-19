
# Mongoose ODM

Mongoose - це ODM (* Object Document Mapper - об'єктно-документний відображувач). Це означає, що Mongoose дозволяє вам визначати об'єкти зі строго-типізованою схемою, яка відповідає документу MongoDB.

Mongoose надає величезний набір функціональних можливостей для створення і роботи зі схемами. На даний момент Mongoose містить вісім SchemaTypes (* типи даних схеми), які можуть мати властивість, яка зберігається в MongoDB. Ці типи наступні:

* String
* Number
* Date
* Buffer
* Boolean
* Mixed
* ObjectId (* унікальний ідентифікатор об'єкта, первинний ключ, _id)
* Array

## Встановлення

```
npm install mongoose
```


Types:


```js
var schema = new Schema(
{
  name: String,
  binary: Buffer,
  living: Boolean,
  updated: { type: Date, default: Date.now },
  age: { type: Number, min: 18, max: 65, required: true },
  mixed: Schema.Types.Mixed,
  _someId: Schema.Types.ObjectId,
  array: [],
  ofString: [String], // You can also have an array of each of the other types
too.
  nested: { stuff: { type: String, lowercase: true, trim: true } }
})
```

```js
%%node

// import mongoose lib
var mongoose = require('mongoose');

// connection
var url = 'mongodb://localhost:27017/mail';
// first two keys for ignoring some warnings of deprecation
mongoose.connect(url, {useUnifiedTopology: true, useNewUrlParser: true,  autoIndex: false});

// define schema
var Schema = mongoose.Schema;
var peopleSchema = new Schema({
    _id: {
        type: Number,
        unique: true,
        required: true,
        auto: true
    },
    pw: {
        type: String,
        minlength: 4,
        maxlength: 32,
        required: true,
    },
    name: {
        type: String,
        minlength: 4,
        maxlength: 32,
        required: true
    },
    addresses: {
        type: Array,
        required: true,
        validate: function (array) {
            if (array.length === 0) return false;
            return array.every((v) => typeof v === 'string');
        }
    },
    memberships: {
        type: Array
    }
});
// compiling model from schema 
var Person = mongoose.model('Person', peopleSchema);
```

Якщо ми не вкажемо * {autoIndex: false} *, ми можемо спостерігати:
```
mongoose: Cannot specify a custom index on `_id` for model name "Person",
MongoDB does not allow overwriting the default `_id` index.   See
http://bit.ly/mongodb-id-index
```
> Here is [explanation](https://mongoosejs.com/docs/guide.html), in the section
**Index**

# Приклад роботи з колекціями
Код для створення двух колекцій:
```js
const mongoose = require('mongoose');
const Article = require("../models/tests/articlesTest/Article");
const Comment = require("../models/tests/articlesTest/Comment");

const article = new Article({
  title: "COVID News",
  author: "amaterasu",
  body: "Here lies the body of the article",
})

const comment = new Comment({
  author: "amaterasu",
  body: "A very deep overview of the nowadays situation.",
  article,
})

const comment2 = new Comment({
  author: "amaterasu",
  body: "COVID is a lie.",
  article,
})

await comment.save();
await comment2.save();

article.comments = article.comments.concat(comment, comment2);
await article.save();
```

Отримаємо:

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/test-collection1.png)

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/test-collection2.png)

# Back-end userAPI:
```js
const { Router } = require("express");
const Test = require("../models/Test");
const User = require("../models/User");
const auth = require("../middleware/auth.middleware");
const router = Router();

router.post("/create", auth, async (req, resp) => {
  try {
    const newTest = new Test({
      name: req.body.testName,
      task: req.body.taskQuery,
      defaultInput: req.body.defaultInput,
      correctQuery: req.body.output,
    });

    const formed = await eval("(async () => {" + newTest.task + "})()");
    const res = await eval("(async () => {" + newTest.correctQuery + "})()");
    newTest.expectedOutput = res.toString();
    const queryResult = await newTest.save();

    resp.json({
      message: "Correct query",
      queryResult,
      created: formed,
      expected: res,
    });
  } catch (e) {
    console.log(e);
    resp.status(500).json({ message: "Something went wrong..." });
  }
});

router.get("/", auth, async (req, resp) => {
  try {
    const tests = await Test.find();
    resp.json({ tests });
  } catch (e) {
    resp.status(500).json({ message: "Something went wrong..." });
  }
});

router.post("/query", auth, async (req, resp) => {
  try {
    if (
      req.body.query.search("remove") < 0 &&
      req.body.query.search("update") < 0
    ) {
      const result = await eval("(async () => {" + req.body.query + "})()");
      const expectedOutput = req.body.activeTest.expectedOutput;
      if (result.includes(expectedOutput) || expectedOutput.includes(result)) {
        const completed = await Test.findOne({
          name: req.body.activeTest.name,
        });
        const user = await User.findById(req.user.userId);
        user.completedTests.push(completed);
        console.log(user);
        await user.save();
        resp.json({
          user,
          result,
          message: "success",
        });
      } else {
        resp.json({
          result,
          message: "failed",
        });
      }
    } else {
      resp.json({ message: "Error processing your request" });
    }
  } catch (e) {
    console.log(e);
    resp.status(500).json({ message: "Something went wrong..." });
  }
});

module.exports = router;
```


# Валідація
Mongoose забезпечує вбудовані валідатори, валідатори користувачів, синхронні та
асинхронні валідатори. У всіх випадках ви можете вказати дійсні діапазони або
значення, а також повідомлення про помилки, якщо порушено умови перевірки.

Вбудовані валідатори включають:
* Усі схеми SchemaType мають вбудований необхідний валідатор, який визначає, чи
слід встановлювати поле перед збереженням документа.
* Числа мають валідатори min та max.
* У рядках:
* * enum (перерахування): вказати набір допустимих значень для поля.
* * match (match)): задає регулярний вираз, що рядок повинен відповідати.
* * maxlength, minlength - максимальна і мінімальна довжина струни.

> here is more [info](https://mongoosejs.com/docs/validation.html)

Простий приклад перевірки:

```js
%%node
// create new doc for testing validation
var item = new Person({
    _id: 2,
    pw: "0000",
    name: "bhsdsb",
    addresses: ['email@gmail.com']
});

item.validate(function(err) {
    if (err)
        console.log(err.message);
    else
        console.log('pass validate');
    }
);
```

Знову, знайдіть усі електронні листи людей, які мають членство в групі з
"group_name" особи з "addresses"

```js
var query_fields = {'addresses': 'convmonk@gmail.com', 'memberships.group_name': 'something'};
var select = {'memberships.group_name': 1, 'memberships.group': 1, '_id': 0};

var group_id;

// select is used as 'projection' param in mongodb 
var query = Person.findOne(query_fields).select(select);

// same code as in mongodb example
query.exec(function (err, res) {
    if (err) return err;
    console.log(res.memberships);
    if (res) {
        res.memberships.forEach(function (res) {
            if (res.group_name === 'something') {
                group_id = res.group;
            }
        });
    }

    var query_fields = {'memberships.group': group_id};
    var select = {'addresses': 1};
    Person.find(query_fields).select(select).exec(function (err, res) {
        if (err) throw err;
        res.forEach(function (res) {
            console.log(res);
        });
    });
});
```

# Promise
Уявіть, що ви відомий співак, якого шанувальники постійно домагають питаннями
щодо майбутнього синглу.

Щоб отримати перерву, ви обіцяєте надіслати їм сингл, коли він вийде. Ви надаєте
шанувальникам список, на який вони можуть підписатися. Вони можуть залишити свою
електронну пошту там, щоб отримати пісню, як тільки вона вийде. І навіть більше:
якщо щось піде не так, наприклад, в студії буде пожежа, і пісня не може бути
випущена, вони також отримають повідомлення про це.

1. Існує код "створення", який робить щось, на що потрібно час. Наприклад, він
завантажує дані по мережі. За нашою аналогією, це "співак".
2. Існує "споживчий" код, який хоче отримати результат коду "створення", коли
він буде готовий. Для цього може знадобитися для декількох функцій. Це "фани".
3. `Promise` ([`згода`](https://learn.javascript.ru/promise-basics), ми будемо
називати такий об’єкт «Promis») - це спеціальний об’єкт у JavaScript, який
посилається на "створення" та "споживання" кодів разом. З точки зору нашої
аналогії, це "список підписки". Код "створення" може бути виконаний стільки,
скільки потрібно для отримання результату, а обіцянка робить результат доступним
для коду, який підписаний до нього, коли результат буде готовий.

Синтаксис створення Promise:

```js
let promise = new Promise(function(resolve, reject) {
  // function (executor)
  // "singer"
});
```

Функція, передана в конструкції `new Promise`, називається виконавцем. Коли
створено Обіцяння, воно запускається автоматично. Він повинен містити
"створюючий" код, який колись дасть результат. З точки зору нашої аналогії:
виконавець - «співак».

Його аргументи `resolve` та` reject` - це зворотні виклики(callbacks), які надає
сам JavaScript. Наш код лише всередині виконавця.

Коли він отримує результат, зараз чи пізніше - це не має значення, він повинен
викликати в один з таких зворотних дзвінків:

* `resolve` (value) - якщо операція завершена успішно, з результатом value.
* `reject` (error) - якщо сталася помилка, error - object of the error.

Отже, виконавець запускається автоматично, він повинен виконати роботу, а потім
викликати `resolve` or `reject`.

Об'єкт `promise`, повернений конструктором` new Promise`, має внутрішні
властивості:
* `state` - спочатку очікує на розгляд(pending), потім змінюється на
виконання(fulfilled), коли викликається рішення(resolve), або відхиляється, коли
викликається відхилення(reject).
* `result` - спочатку не визначено(undefined), потім змінюється на значення при
виклику рішення (value) або на помилку при відхиленні виклику (error)


![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/promise.png)

```js
function logger(doc) {
    console.log(doc);
}

// promise findOne
// null is projection argument(but then we use select)
// {emptyError: true} just not to check if res is null   
var prom = Person.findOne(query_fields, null, {lean: true, emptyError: true}) 
    .select(select)
    .then(doc => {
        doc.memberships.forEach(function (doc) {
            if (doc.group_name === 'something') {
                group_id = doc.group;
            }
        });
    })
    .then(() => {
        var query_fields = {'memberships.group': group_id};
        var select = {'addresses': 1};
        Person.find(query_fields).select(select).exec(function (err, res) {
            if (err) throw err;
            res.forEach(function (res) {
                logger(res);
            });
        });
    })
    .catch(err => {
        console.log(err);
    });
```

Update/delete promise

```js
var query_fields = {'addresses': 'convmonk@gmail.com'};
var select = {'addresses': 1, 'memberships.address': 1};


function update(doc, addrs) {
    if (!addrs.length){
        Person.deleteOne({'_id': doc._id});
    }
    else {
        var addr = addrs[0];
        Person.updateOne(
            query_fields,
            { $set: {'memberships.$[elem].address': addr, 'addresses': addrs} },
            {
                multi: true, // default false, applied just to the first match
                arrayFilters: [{'elem.address': query_fields.addresses}] // means if elem.address == query.addresses
            },
            {new: true},
            function (err, res) {
                if (err) throw err;

                // res of operation
                console.log(res);
            }
        );
    }
}

// promise update/delete
var prom = Person.findOne(query_fields, null, {emptyError: true})
    .select(select)
    .then(doc => {
        var addrs = doc.addresses.filter(function (e) {return e !== query_fields.addresses});
        update(doc, addrs);
    })
    .catch(err => {
    console.log(err.message);
});
```

