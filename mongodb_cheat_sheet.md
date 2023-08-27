To show all Database
--> show dbs

create a new or to swicth Database
--> use dbName

View current databse
--> db

Delete current databse
--> db.dropDatabase() (if deleted returns Ok)

**\*** Commands for Collections **\***
View all Collections of databse
--> show collections

Create new Collection named as "mycol"
--> db.createCollection("mycol")

Delete Collection named as "mycol"
--> db.mycol.drop()

**_ Commands for Rows _**
Inserting rows in a collection named as "mycol"
--> db.mycol.insert({
"name": "umer",
"lang": "JS",
"role": "Web Dev",
})

Inserting Many rows in a collection named as "mycol"
--> db.mycol.insertMany([
{
"name": "umer",
"lang": "JS",
"role": "Web Dev",
"age": 44,
},
{
"name": "Abbas1",
"lang": "Python",
"role": "Data Scientist",
"age": 20,
},
{
"name": "Abdullah1",
"lang": "PHP, Python",
"role": "WordPress Dev",
"age": 32,
},
])

View all the rows in a collection named as "mycol"
--> db.mycol.find()

View a single row in a collection named as "mycol"
--> db.mycol.findOne({"field": "value"})

View row in a collection by any field named as "mycol"
--> db.mycol.findById({"field": "value"})

View row in a collection by Id named as "mycol"
--> db.mycol.findById({id: "id"})

Limit the no of rows in the output of a collection named as "mycol"
--> db.mycol.find().limit(4)

count the no of rows of a collection named as "mycol"
--> db.mycol.countDocuments()

count the match rows of a collection named as "mycol"
--> db.mycol.find({"field": "value"}).countDocuments()

Update a row in a collection by Id named as "mycol"
--> db.mycol.update({age: 24}, {
$set:
{"name": "Umer Memon",
"lang": "Python",
"role": "Web Dev",
"age": 45,
}})

Delete a row in a collection named as "mycol"
--> db.mycol.deleteOne({agee: 44})

Delete those rows where age less than 12 in MongoDB
--> db.mycol.deleteMany({agee: {$lte: 12}})

Delete the field in MongoDB
--> db.mycol.remove({name: 'umer'})

Increment Operator in MongoDB
--> db.mycol.update({age: 45}, {
$inc:
{"age": 10}
})

Rename Operator in MongoDB
--> db.mycol.updateMany({}, {
$rename:
{"age": "agee"}
})

less than or equal to Operator in MongoDB
--> db.mycol.find(
{agee: {$lte: 20}}
)

greater than or equal to Operator in MongoDB
--> db.mycol.find(
{agee: {$gte: 25}}
)

in Between 25 to 50 in MongoDB
--> db.mycol.find(
{agee: {$gte: 25, $lte: 50}}
)

# Aggregation (pipeline operations)

Group values from multiple documents together. · Perform operations on the grouped data to return a single result.

Aggregation process has several steps that transform data in some way. The output of first step is act as input for next step, the same process goes on until the final result produced

syntax:
db.collection.aggregate(pipeline as [], options)

## Databases

Example: Finds the teachers whose gender is male

```shell
db.t_info.aggregate([
  {$match: {gender: "Male"}}
])
```

Example: Group teachers by age

```shell
db.t_info.aggregate([
  {$group: {_id: "$age"}}
])
```

Example: Group teachers by age and show all teachers name per age group

```shell
db.t_info.aggregate([
  {$group: {_id: "$age", names: {$push: "$name"} }}
])
```

since the names field use $push operator to add name field from each document to an array

Example: Group teachers by age and show all teachers documents per age group

```shell
db.t_info.aggregate([
  {$group: {_id: "$age", poraDoc: {$push: "$$ROOT"} }}
])
```

$$ROOT is a reference to complete current document ,since the names field use $push operator to add poraDoc from each document to an array.

Example: Group teachers by age and show all teachers documents per age group and count

```shell
db.t_info.aggregate([
  {$group: {_id: "$age", poraDoc: {$push: "$$ROOT"}, count: {$sum: 1} }}
])
```

Example: Give a count per age male teacher

```shell
db.t_info.aggregate([
  {$match: {gender: "Male"}},
  {$group: {_id: "$age", count: {$sum: 1} }}
])
```

The value of $sum, for each document in a group, the value count is increment by 1 in a group document

Example: Give a count per age of female teacher and sort them in desc order

```shell
db.t_info.aggregate([
  {$match: {gender: "Female"}},
  {$group: {_id: "$age", count: {$sum: 1}}},
  {$sort: {count: -1}}
])
```

    Example: Give a count per age of female teacher, sort them in desc order and find max number of female teacher having same age and return that group

```shell
db.t_info.aggregate([
  {$match: {gender: "Female"}},
  {$group: {_id: "$age", count: {$sum: 1}}},
  {$sort: {count: -1}},
  {$group: {_id: null, maxCount:{$max: "$count"}}}
])
```

## $unwind operator is used to deconstruct an array field in a document and create separate output documents for each item in the array.

    Example: Group teachers by age and show hobbies of a teacher within a group as an single array

```shell
db.t_info.aggregate([
  {$unwind: "$hobbies"},
  {$group: {_id: "$age", hobbiesArray: {$addToSet: "$hobbies"}}},
  {$sort: {_id: 1}}
])
```

## Example: No of teachers per each hobbies

```shell
db.t_info.aggregate([
  {$unwind: "$hobbies"},
  {$group: {_id: "$hobbies", count: {$sum: 1}}}
])
```

## Example: Avg age of teachers in a single document

```shell
db.t_info.aggregate([
    {$unwind: "$hobbies"},
  {$group: {_id: null, averageAge: {$avg: "$age"}}}
])
```

\_id: null in $group operator means all the document will be grouped together in a single collection

## Example: Find total number of hobbies for all teachers in a single document

```shell
db.t_info.aggregate([
  { $unwind: "$hobbies" },
  {
    $group: {
      _id: null,
      uniqueHobbies: { $addToSet: "$hobbies" }
    }
  },
  {
    $project: {
      _id: 0,
      count: { $size: "$uniqueHobbies" }
      }
  }
])
```

# lookup in Aggregation (pipeline operations)

$lookup is an aggregation pipeline stage that allow you to perform a left outer join between two collections

syntax:
db.collection.aggregate( [
{
$lookup: {
from: "",
localField: "",
foreignField: "",
as: ""
}
}
], options)

```shell
Customer Collection:
{
  "_id": 1,
  "name": "John Doe"
},

{
  "_id": 2,
  "name": "Jane Smith"
}
Order Collection:
{
  "_id": 101,
  "product": "Smartphone",
  "customerid": 1
}

{
  "_id": 102,
  "product": "Laptop",
  "customerid": 2
}

{
  "_id": 103,
  "product": "Tablet",
  "customerid": 3  // This customer ID doesn't match any existing customer
}
```

```shell
db.customer.aggregate([
  {
    $lookup:{
      from: "order",
      localField: "_id",
      foreignField: "customerid",
      as: "customerOrders"

    }
  }
])
```
