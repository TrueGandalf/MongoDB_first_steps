# VerySecretRepository
JustForThirdHomework

## Academy'18 • 2nd stage • 3. MongoDB
### Задание:
---
#### 0 - Накатить бэкап базы
```javascript
cd D:\mongoDB\bin
d:
mongod
mongo
show dbs
exit
mongoimport --db binaryDB D:\mongodb\samples\users.json
mongo
show dbs
*****
  admin     0.000GB
  binaryDB  0.001GB
  config    0.000GB
  local     0.000GB
  sampleDB  0.001GB
*****
use binaryDB
show collections
*****
  users
*****
db.users.findOne()
```
---
#### 1 - Найти средний возраст людей в системе
```javascript
db.users.aggregate({ $group: {_id: "allUsers", averageAge: {$avg: '$age'} } })
*****
  { "_id" : "allUsers", "averageAge" : 30.38862559241706 }
*****
```
---
#### 2 - Найти средний возраст в штате Аляска
```javascript
db.users.aggregate([
  {$match:
    {"address": /.*Alaska.*/} },
  {$group:
     {_id: "usersFromAlaska",
     averageAge: { $avg: '$age' }}}
])
*****
  { "_id" : "usersFromAlaska", "averageAge" : 31.5 }
*****
```
#### 3 - Начиная от Math.ceil(avg + avg_alaska) (порядковый номер документа в БД ) найти первого человека с другом по имени Деннис
30,38862559241706+31,5 = 61,88862559241706 => Ceil => 62 => -1 => start after 61
```javascript
db.users.aggregate([
  { $skip : 61 },
  {$match:
    {"friends.name": /.*Dennis.*/ } },
  { $limit: 1 },
  { $project : {index: 1, name: 1, friends:1 } }
]).pretty()
*****
{
        "_id" : ObjectId("5adf3c1544abaca147cdd47c"),
        "index" : 306,
        "name" : "Esperanza Blevins",
        "friends" : [
                {
                        "id" : 0,
                        "name" : "Charles Dennis"
                },
                {
                        "id" : 1,
                        "name" : "Arlene Walton"
                },
                {
                        "id" : 2,
                        "name" : "Trisha Long"
                }
        ]
}
*****
```
---
#### 4 - Найти активных людей из того же штата, что и предыдущий человек и посмотреть какой фрукт любят больше всего в этом штате (аггрегация)
"address" : "659 Oceanic Avenue, Collins, **Utah**, 6277"
```javascript
db.users.aggregate([
    {$match:
      { $and: [ { "isActive" : true }, { "address" : /.*Utah.*/ } ] }
    },
  {
        $group : { _id : "$favoriteFruit", count: { $sum: 1 }}
  },
	{$sort: {count: -1}},
        { $limit: 1 }
])
*****
  { "_id" : "apple", "count" : 4 }
*****
```
---
#### 5 - Найти саммого раннего зарегистрировавшегося пользователя с таким любимым фруктом
{ "_id" : "apple", "count" : 4 }
```javascript
db.users.aggregate([
    {$match:
      {"favoriteFruit": "apple" }
    },
        {$sort: {"registered": 1}},
	{$limit: 1},
	{ $project : { _id: 1, index: 1, name: 1, registered: 1, "favoriteFruit": 1  } }
]).pretty()
****
	{
		"_id" : ObjectId("5adf3c1544abaca147cdd568"),
		"index" : 541,
		"name" : "Magdalena Compton",
		"registered" : "2014-01-02T10:16:56 -02:00",
		"favoriteFruit" : "apple"
	}
****
```
---

---

```javascript
```
