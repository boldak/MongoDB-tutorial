
## Встановлення mongodb для node.js

```
npm install mongodb
```

```js
%%node
// create MongoClient object
var MongoClient = require('mongodb').MongoClient, format = require('util').format;
// create a db in mongodb, specify a connection URL with the correct ip address
// and the name of the database you want to create,
var url = 'mongodb://localhost:27017/';

// group_id for finding all group's emails
var group_id;

MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    // connecting to db 'mail', collection 'people'
    const db_ = db.db('mail');
    var collection = db_.collection('people');

    // for example, we have person's addres and group_name, due to them we get then id in a field 'group'
    var query =  {'addresses': 'convmonk@gmail.com', 'memberships.group_name': 'something'};
    // which fields to get from query
    var field = {'memberships.group_name': 1, 'memberships.group': 1, '_id': 0};

    // this block finds to doc with query params and assigns 'group' value to var 'group_id'
    // findOne takes: query, fields to get, function
    collection.findOne(query, {'projection': field}, function (err, res) {
        if (err) throw err;
        if (res) {
            // when we use forEach, all records are loaded into memory
            res.memberships.forEach(function (res) {
                if (res.group_name == 'something') {
                    group_id = res.group;
                }
            });
        }


        var query = {'memberships.group': group_id};
        var field = {'addresses': 1};
        // this block finds all emails of people who have membership in group with group_id
        // just logs result to console
        collection.find(query, {'projection': field}, function (err, res) {
            if (err) throw err;
            res.forEach(function (res) {
                console.log(res);
            });
        });
        db.close();
    });
});
```


**Result:**
the first `id` is excluded as long as it has no membership in group with
`group_id`
```js
{ _id: 2,
  addresses: [
    'convmonk@gmail.com',
    'convmonk@mail.ru'
    ]
}
{ _id: 3,
  addresses: [
    'random@gmail.com',
    'random@mail.ru'
    ]
}
{ _id: 4,
  addresses: [
    'bot2001@gmail.com',
    'bot2001@mail.ru'
    ]
}
  ```


```js
%%node
// create MongoClient object
var MongoClient = require('mongodb').MongoClient, format = require('util').format;
// create a db in mongodb, specify a connection URL with the correct ip address
// and the name of the database you want to create,
var url = 'mongodb://localhost:27017/';


MongoClient.connect(url, function(err, db) {
    if (err) throw err;

    // connecting to db 'mail', collection 'people'
    const db_ = db.db('mail');
    var collection = db_.collection('people');

    // addresses to delete, it is also used in 'memberships.address'
    var query = {'addresses': 'convmonk@gmail.com'};
    var field = {'addresses': 1, 'memberships.address': 1};
    
    // firsty lets find res 
    collection.findOne(query, {'projection': field}, function (err, res) {
        if (err) throw err;
        
        // get rid of 'addresses' in query 
        var addrs = res.addresses.filter(function (e) {return e != query.addresses});
        
        // if addrs empty, we just delete document 
        if (!addrs.length){
            collection.deleteOne({'_id': res._id});
        }
        // if addrs exists, we delete 'addresses' in query and change all 'memberships.address' == query.addresses 
        // to the first accessible address in addresses 
        else {
            // get first accessible address for change 
            var addr = addrs[0];
            
            collection.updateOne(
                query, // filter 
                {
                    // first param means set to each element's field 'address' and in arrayFilter we define condition
                    // which elem exactly to change for addr
                    // second param just filtered array of addresses
                    $set: {'memberships.$[elem].address': addr, 'addresses': addrs}
                },
                {
                    multi: true, // default false, applied just to the first match
                    arrayFilters: [{'elem.address': query.addresses}] // means if elem.address == query.addresses
                },
                function (err, res) {
                    if (err) throw err;
                    
                    // res of operation 
                    console.log(res);
                }
                );
        }
        db.close();
    });

});
```

Джерела, які були використані в останньому коді:
*[$[\<identifier\>]](https://docs.mongodb.com/master/reference/operator/update/positional-
filtered/)*

[db.collection.updateOne](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/)

**Результат:** ми видалили `"convmonk@gmail.com"` і змінили `"address"` поля

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/remove1.png)
# Тестування jest-mongodb

[Jest MongoDB](https://github.com/shelfio/jest-mongodb) забезпечує всю необхідну конфігурацію для запуску тестів за допомогою MongoDB.

1. Спочатку встановіть @shelf/jest-mongodb
```js
yarn add @shelf/jest-mongodb --dev
```

2. Вкажіть попередньо встановлене налаштування в Jest-конфігурації:
```js
{
  "preset": "@shelf/jest-mongodb"
}

```

3. Напишіть свій тест:
```js
const {MongoClient} = require('mongodb');

describe('insert', () => {
  let connection;
  let db;

  beforeAll(async () => {
    connection = await MongoClient.connect(global.__MONGO_URI__, {
      useNewUrlParser: true,
    });
    db = await connection.db(global.__MONGO_DB_NAME__);
  });

  afterAll(async () => {
    await connection.close();
    await db.close();
  });

  it('should insert a doc into collection', async () => {
    const users = db.collection('users');

    const mockUser = {_id: 'some-user-id', name: 'John'};
    await users.insertOne(mockUser);

    const insertedUser = await users.findOne({_id: 'some-user-id'});
    expect(insertedUser).toEqual(mockUser);
  });
});
```

Немає необхідності завантажувати залежності.
