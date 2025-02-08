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

## CRUD operations replace & delete

```js
// replace document
db.collection.replaceOne(
   { name: "Alice" },  // Filter to find the document
   { name: "Alice", age: 30, city: "New York" }  // New document replacing the old one
)

// output
{
  "acknowledged" : true,
  "matchedCount" : 0,
  "modifiedCount" : 0
}

// update one
db.users.updateOne(
   { "name": "Alice" },  
   { $set: { "city": "Los Angeles" } }
)

// create if not exists
db.users.updateOne(
   { "name": "Bob" },
   { $set: { "age": 30, "city": "Chicago" } },
   { upsert: true }
)

// result
{
  "acknowledged": true,
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedId": null
}

// Push operator
{
  "_id": 1,
  "name": "Alice",
  "hobbies": ["reading", "traveling"]
}

db.users.updateOne(
   { "name": "Alice" },
   { $push: { "hobbies": "cycling" } }
)

// find And Modify
db.collection.findAndModify({
   query: { <filter> },         // Find the document
   update: { <update> },        // Modify the document
   new: true | false,           // Return the updated document (true) or the original (false)
   sort: { <field>: 1 | -1 },   // Optional: Sort if multiple docs match
   upsert: true | false         // Optional: Insert if no document is found
})

db.users.findAndModify({
   query: { "name": "Alice" },
   update: { $set: { "age": 27 } },
   new: false  // Return the original document before update
})

// update many
db.users.updateMany(
   { "age": { $gte: 25 } },  // Update users with age 25 or older
   { $inc: { "age": 1 } }    // Increase age by 1
)

// delete one
db.users.deleteOne({ "age": { $gt: 30 } })
// results
{
  "acknowledged": true,
  "deletedCount": 1
}

// delete many 
db.users.deleteMany({ "age": 25 })
// results
{
  "acknowledged": true,
  "deletedCount": 2
}
```

## Modifing results

```js
use sample_training;
// sorting and projections
db.companies
  .find(
    { category_code: "music" }, 
    { name: 1 }
  )
  .sort({ name: 1 });

// limiting
db.companies
  .find({category_code: "music"}, {name: 1, number_of_employees: 1})
  .sort({number_of_employees: -1})
  .limit(3);

// projections
use sample_training;
db.inspections.find({ sector: "Fuel Oil Dealer - 814" }, { business_name: 1, result: 1, _id: 0})

// in operator
db.inspections.find({ result: { $in: ["Pass", "Warnung"] }}, { business_name: 1, result: 1, _id: 0})

// dot notation
db.inspections.find({ result: { $in: ["Pass", "Warnung"] }}, { business_name: 1, result: 1, "address.city": 1, _id: 0})

// count documents
db.trips.countDocuments()
db.trips.countDocuments({ tripduration: { $gt: 120 }})

```