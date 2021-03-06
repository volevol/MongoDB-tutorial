```python
# loading our google drive in order to use photos 
from google.colab import drive
drive.mount('/content/drive')
```

```python
# class to display images 
from IPython.display import Image
```

[**Девід Трейб**](https://medium.com/u/23afa685a28e?source=post_page-----
aa140bea21ba----------------------)  створив
[pixiedust_node](https://github.com/ibm-watson-data-lab/pixiedust_node)
надбудову для Jupiter notebooks, яка дозволяє Node.js / JavaScript запускати
всередині комірок ноутбука. Він побудований на популярній бібліотеці помічників
PixieDust

# Installing

```python
!pip install pixiedust
!pip install pixiedust_node
```

Цей код може написати, що версія застаріла, і вам потрібно перезапустити
**Runtime**

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/mongo.png')
```

```python
import pixiedust_node
```

Виконайте команди js за допомогою **_%%node_**, Приклад:

```python
%%node
var date = new Date();
print(date);
```

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

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/mongo.png')
```

# Тестування mongo
Вставлення одна за одною команд:
```
# sets database with name 'use' for current usage, if it doesnt exist, it will
be created
use test
# adds in collection 'users' of db 'test' object in json format
db.users.save( { name: "Tom" } )
# prints all objects from db 'test'
db.users.find()
```


```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/test1.png')
```

# Графічний Клієнт Compass
click here to [download](https://www.mongodb.com/download-center/compass)

Після установки за посиланням ми встановлюємо нове З'єднання, усі поля
встановлюються за замовчуванням.

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/compass1.png')
```

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/compass2.png')
```

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/compass3.png')
```

# Зверніть увагу, що mongod повинен працювати, інакше Компас не під'єднається

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/connection.png')
```


# Перш ніж працювати з db & schemas, давайте розглянемо відмінності від інших
типів db

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/difference.png')
```

На відміну від баз даних SQL, Mongodb не використовує пристрій таблиці з чітко
визначеною кількістю стовпців і типів даних.

БД складається з колекцій. Кожна колекція має унікальну назву - * ідентифікатор
*, що називається `_id '. Документ являє собою набір * пар-ключ-значення * пар.
# Data types
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



# Sample Document
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


# Трохи практики

Припустимо, ми маємо моделювати структуру, яка містить списки розсилки та дані
про людей. Приклад [тут](https://habr.com/ru/post/144798/)

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/model.png', width=400, height=200)
```

Наступні вимоги:
* У людини може бути одна або кілька адрес електронної пошти;
* Людина може перебувати в будь-якій кількості списків розсилки;
* Людина може вибрати будь-яке ім’я для будь-якого списку розсилки, в якому вона
перебуває.

Давайте подивимось, як буде виглядати наша модель даних, якщо ніде дані не
вбудовуються.

**People** з ім'ям та паролем

```
{
    _id: PERSON_ID,
    name: "Name Surname",
    pw: "Hashed password"
}
```

**Adresses** де кожен email прив'язується до конкретного абонента
```
{
    _id: ADDRESS_ID,
    person: PERSON_ID,
    address: "vpupkin@gmail.com"
}
```

**Groups** ми можемо визначити деякі додаткові поля
```
{
    _id: GROUP_ID
}
```

**Memberships** об’єднує людей у ​​групи

```
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
```
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
```
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

```python
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

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/people1.png')
```

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/people2.png')
```

## Installation mongodb for node.js

```
npm install mongodb
```

```python
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
```
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


```python
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
*
[$[\<identifier\>]](https://docs.mongodb.com/master/reference/operator/update/positional-
filtered/)
*
[db.collection.updateOne](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/)

**Результат:** ми видалили `"convmonk@gmail.com"` і змінили `"address"` поля

```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/remove1.png')
```

# Mongoose ODM
## Installation

```
npm install mongoose
```


Types:


```
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

```python
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


# Валідація
Mongoose забезпечує вбудовані валідатори, валідатори користувачів, синхронні та
асинхронні валідатори. У всіх випадках ви можете вказати дійсні діапазони або
значення, а також повідомлення про помилки, якщо порушено умови перевірки.

Вбудовані валідатори включають:
* Усі схеми SchemaType мають вбудований необхідний валідатор, який визначає, чи
слід встановлювати поле перед збереженням документа.
* Числа мають валідатори min та max.
* У рядках:
** enum (перерахування): вказати набір допустимих значень для поля.
** match (match)): задає регулярний вираз, що рядок повинен відповідати.
** maxlength, minlength - максимальна і мінімальна довжина струни.

> here is more [info](https://mongoosejs.com/docs/validation.html)

Простий приклад перевірки:

```python
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
```

Знову, знайдіть усі електронні листи людей, які мають членство в групі з
"group_name" особи з "addresses"

```python
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

```
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


```python
Image('/content/drive/My Drive/Colab Notebooks/Mongodb/photos/promise.png')
```

```python
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

```python
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
