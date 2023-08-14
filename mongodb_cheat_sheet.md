To show all Database 
--> show dbs

create a new or to swicth Database
--> use dbName

View current databse
--> db

Delete current databse
--> db.dropDatabase() (if deleted returns Ok)

***** Commands for Collections *****
View all Collections  of databse
--> show collections

Create new Collection named as "mycol"
--> db.createCollection("mycol")

Delete Collection named as "mycol"
--> db.mycol.drop()

***** Commands for Rows *****
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
## Databases

- Show all databases:
  ```shell
  show dbs
  ```

