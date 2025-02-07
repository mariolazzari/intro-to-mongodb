# Intro to MongoDB

## Intro

### Mongo Atlas

[Atlas](https://www.mongodb.com/it-it/products/platform/atlas-database)

## Connecting fo Mongo


### Mongo shell

```sh
mongosh "mongodb+srv://<db_name>:<db_password>@cluster0.o4y8t.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"

const greetingArray = ["Hello", "World", "Welcome"];
const loopArray = (array) => array.forEach(el => console.log(el))
```

### Mongo Compass

```sh
mongodb+srv://<db_name>>:<db_password>@cluster0.o4y8t.mongodb.net/
```

### MongoDB driver for NodeJS

[Mongo dev center](https://www.mongodb.com/developer/)
[Mongo university](https://learn.mongodb.com/)

## CRUD operations insert & find

### Insert one

```js
db.grades.insertOne({ student:"Mario", grade: 30 });

// result
{
  acknowledged: true,
  insertedId: ObjectId('67a5351e1487e7144020f096')
}
```

### Insert many

```js
db.grades.insertMany([{ student:"Maria", grade: 30 }, { student:"Mariarosa", grade: 30 }]);

// result
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('67a5357f1487e7144020f097'),
    '1': ObjectId('67a5357f1487e7144020f098')
  }
}
```

### Find

```js
use sample_training
db.zips.find()

// eq
db.zips.find({ state: { $eq: "AZ" }});

// implicit eq
db.zips.find({ state: "AZ" });

// in
db.zips.find({ city: { $in: ["PHOENIX", "CHICACO"] }});
```

### Comparisons

```js
// gt gte lt lte
db.sales.find({ pop: { $gt: 30000 }});

// subfields
db.trips.find({"start station location.type": {$eq: "Point"}})
```

### Array element

```js
use sample_analytics
db.accounts.find({ products: "InvestmentStock"})

// $elemMatch
db.accounts.find({ products: { $elemMatch: { $eq: "InvestmentStock" }}})
```

### Logical operators

```js
use sample_training;

// $and
db.routes.find({ $and: [{ "airline.name": "Aerocondor"}, {src_airport: "KZN" }]})

// $or
db.routes.find({ $or: [{ "airline.name": "Aerocondor"}, {src_airport: "KZN" }]})

// $or (implicit)
db.routes.find({ "airline.name": "Aerocondor", src_airport: "KZN"})
```

