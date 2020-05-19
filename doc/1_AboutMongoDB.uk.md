# MongoDB 

MongoDB — документо-орієнтована система керування базами даних (СКБД) з відкритим вихідним кодом, яка не потребує опису схеми таблиць. MongoDB займає нішу між швидкими і масштабованими системами, що оперують даними у форматі ключ/значення, і реляційними СКБД, функціональними і зручними у формуванні запитів.

# Основні можливості MongoDB:

* Документо-орієнтоване сховище (проста та потужна JSON-подібна схема даних)
* Досить гнучка мова для формування запитів
* Динамічні запити
* Повна підтримка індексів
* Профілювання запитів
* Швидкі оновлення «на місці»
* Ефективне зберігання бінарних даних великих обсягів, наприклад, фото та відео
* Журналювання операцій, що модифікують дані в БД
* Підтримка відмовостійкості і масштабованості: асинхронна реплікація, набір реплік і шардінг
* Може працювати відповідно до парадигми MapReduce

# MongoDB vs. RDBMS

![](https://github.com/BeefMILF/MongoDB-tutorial/raw/master/doc/photos/difference.png)

На відміну від баз даних SQL, Mongodb не використовує пристрій таблиці з чітко
визначеною кількістю стовпців і типів даних.

БД складається з колекцій. Кожна колекція має унікальну назву - * ідентифікатор
*, що називається `_id '. Документ являє собою набір * пар-ключ-значення * пар.

# Типи даних
*   **String** - utf-8
*   **Array**
*   **Binary data**
*   **Boolean**
*   **Date**
*   **Double**
*   **Integer**
*   **JavaScript**
*   **Min key/Max key**
*   **Object** - string type
*   **ObjectID** - id document
*   **Regular expression** -
*   **Symbol** - identical to string, used for spec symbols
*   **Timestamp**



# Приклад документу
```
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview',
   description: 'MongoDB is no sql database',
   by: 'tutorials point',
   url: 'http://www.tutorialspoint.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100,
   comments: [
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2011,1,20,2,15),
         like: 0
      },
      {
         user:'user2',
         message: 'My second comments',
         dateCreated: new Date(2011,1,25,7,45),
         like: 5
      }
   ]
}
```

