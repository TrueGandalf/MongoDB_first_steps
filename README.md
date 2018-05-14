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
  admin     0.000GB
  binaryDB  0.001GB
  config    0.000GB
  local     0.000GB
  sampleDB  0.001GB
use binaryDB
show collections
  users

```
