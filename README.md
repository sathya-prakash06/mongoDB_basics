# mongoDB_basics

## MongoDB

- Collection - an organized store of documents in MongoDB usually with common fields between documents. There can be many collections per database and many documents per collection.

- Document - a way to organize and store data as a set of field-value pairs.

- Field - a unique identifier for a datapoint.

- Value - data related to a given identifier.

- Collection - an organized store of documents in MongoDB, usually with common fields between documents. There can be many collections per database and many documents per collection.

## MongoDB Atlas

- Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.

- Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

- Cluster - group of servers that store your data.

## What is JSON?

JavaScript Object Notation, more commonly known as JSON, was defined as part of the JavaScript language in the early 2000s by JavaScript creator Douglas Crockford, though it wasn’t until 2013 that the format was officially specified.

JavaScript objects are simple associative containers, wherein a string key is mapped to a value (which can be a number, string, function, or even another object). This simple language trait allowed JavaScript objects to be represented remarkably simply in text:

```
{
  "_id": 1,
  "name" : { "first" : "John", "last" : "Backus" },
  "contribs" : [ "Fortran", "ALGOL", "Backus-Naur Form", "FP" ],
  "awards" : [
    {
      "award" : "W.W. McDowell Award",
      "year" : 1967,
      "by" : "IEEE Computer Society"
    }, {
      "award" : "Draper Prize",
      "year" : 1993,
      "by" : "National Academy of Engineering"
    }
  ]
}

```

## What is BSON?

BSON simply stands for “Binary JSON,” and that’s exactly what it was invented to be. BSON’s binary structure encodes type and length information, which allows it to be parsed much more quickly.

Since its initial formulation, BSON has been extended to add some optional non-JSON-native data types, like dates and binary data, without which MongoDB would have been missing some valuable support.

Languages that support any kind of complex mathematics typically have different sized integers (ints vs longs) or various levels of decimal precision (float, double, decimal128, etc.).

Not only is it helpful to be able to represent those distinctions in data stored in MongoDB, it also allows for comparisons and calculations to happen directly on data in ways that simplify consuming application code.

```
{"hello": "world"} →
\x16\x00\x00\x00           // total document size
\x02                       // 0x02 = type String
hello\x00                  // field name
\x06\x00\x00\x00world\x00  // field value
\x00                       // 0x00 = type EOO ('end of object')

{"BSON": ["awesome", 5.05, 1986]} →
 \x31\x00\x00\x00
 \x04BSON\x00
 \x26\x00\x00\x00
 \x02\x30\x00\x08\x00\x00\x00awesome\x00
 \x01\x31\x00\x33\x33\x33\x33\x33\x33\x14\x40
 \x10\x32\x00\xc2\x07\x00\x00
 \x00
 \x00

```

Unlike systems that simply store JSON as string-encoded values, or binary-encoded blobs, MongoDB uses BSON to offer the industry’s most powerful indexing and querying features on top of the web’s most usable data format.

For example, MongoDB allows developers to query and manipulate objects by specific keys inside the JSON/BSON document, even in nested documents many layers deep into a record, and create high performance indexes on those same keys and values.

When using a MongoDB driver in your language of choice, it’s still important to know that you’re accessing BSON data through the abstractions available in that language.

Firstly, BSON objects may contain Date or Binary objects that are not natively representable in pure JSON. Second, each programming language has its own object semantics. JSON objects have ordered keys, for instance, while Python dictionaries (the closest native data structure that’s analogous to JavaScript Objects) are unordered, while differences in numeric and string data types can also come into play. Third, BSON supports a variety of numeric types that are not native to JSON, and each language will represent these differently.

## JSON

```
mongoimport
mongoexport
```

## BSON

```
mongorestore
mongodump
```

## commands

```
show dbs

use sample_training

show collections

db.zips.find({"state": "NY"})

db.zips.find({"state": "NY"}).count()

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()

db.inspections.findOne();

db.inspections.insert({
      "_id" : ObjectId("56d61033a378eccde8a8354f"),
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.insert({
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.find({"id" : "10021-2015-ENFO", "certificate_number" : 9278806}).pretty()

db.inspections.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])

```

## Update

- update operators : $inc, $set, $unset, $push, $pull, $rename, $bit

- updateOne(), updateMany()

```
db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })

db.zips.updateOne({ "zip": "12534" }, { "$set": { "pop": 17630 } })

db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })

db.grades.find({ "student_id": 250, "class_id": 339 }).pretty()

db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { "$push": { "scores": { "type": "extra credit",
                                             "score": 100 }
                                }
                     })

```

## Delete

```
Delete all the documents that have test field equal to 1.
db.inspections.deleteMany({ "test": 1 })

Delete one document that has test field equal to 3.
db.inspections.deleteOne({ "test": 3 })

Drop the inspection collection.
db.inspection.drop()


```

## Comparision operator

1. $eq - Equal to
2. $ne - Not equal to
3. $gt - Greater than
4. $gte - Greater than or equal to
5. $lt - Less than
6. $lte - Less than or equal to

```

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was not Subscriber:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using a redundant equality operator:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using the implicit equality operator:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": "Customer" }).pretty()


```

## Logical operator

1. $and -match all the specified query clauses
2. $or - al least one of the query clauses is matched
3. $not - negates the query requirement
4. $nor - fail to match both given clauses

```

- Find all documents where airplanes CR2 or A81 left or landed in the KZN airport:
db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()

- How many businesses in the sample_training.inspections dataset have the inspection result "Out of Business" and belong to the "Home Improvement Contractor - 100" sector?

db.inspections.find({"result": "Out of Business", "sector": "Home Improvement Contractor - 100"}).count()

- How many zips in the sample_training.zips dataset are neither over-populated nor under-populated?

db.zips.find({"pop": { "$not": { "$gt": 1000000 } },
              "pop": { "$not": { "$lt": 5000 } }
             }).count()
or

db.zips.find({ "pop": { "$gte": 5000, "$lte": 1000000 }}).count()


- How many companies in the sample_training.companies dataset were either founded in 2004
[and] either have the social category_code [or] web category_code,
[or] were founded in the month of October
[and] also either have the social category_code [or] web category_code?

db.companies.find({ "$and": [ { "founded_year": 2004 },
                             { "$or": [ { "category_code": "social" },
                                        { "category_code": "web" } ] },

                                { "$nor": [ { "category_code": "social" },
                                            { "category_code": "web" } ] }
                           ]
                  }).pretty()
```

## Expressive $expr operator

- expressions allows the use of aggregation expressions within the query language.
- { $expr: { expression }}

```
- Find all documents where the trip started and ended at the same station:
db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }
              }).count()

- Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station:
db.trips.find({ "$expr": { "$and": [ { "$gt": [ "$tripduration", 1200 ]},
                         { "$eq": [ "$end station id", "$start station id" ]}
                       ]}}).count(

- How many companies in the sample_training.companies collection have the same permalink as their twitter_username?
db.companies.find({ "$expr": { "$eq": [ "$permalink", "$twitter_username" ]}}).count()

```

## Array operators

- $push - allow us to add an element to an array
- $pop - remove an element from an array
- $unset - remove an element from an array
- $slice - return a slice of an array
- $position - return the position of an element in an array
- $elemMatch - return the first element in an array that matches the specified criteria
- $size - return the size of an array
- $all - return true if all elements in the specified array match the specified criteria
- $in - return true if the specified element is in the specified array
- $nin - return true if the specified element is not in the specified array
- $exists - return true if the specified element exists
- $type - return the type of the specified element
- $mod - return true if the specified element is a modulus of the specified divisor
- $regex - return true if the specified element matches the specified regular expression
- $text - return true if the specified element matches the specified text search query
- $where - return true if the specified JavaScript expression returns true

```

- What is the name of the listing in the sample_airbnb.listingsAndReviews dataset that accommodates more than 6 people and has exactly 50 reviews?
db.listingsAndReviews.find({ "$and": [ { "$gt": [ "$accommodates", 6 ]},
                                      { "$eq": [ "$review_scores_rating", 50 ]}
                                    ]}).pretty()

```

## Array operations and Projections

- 1 : included in the result
- 0 : excluded from the result

```

- Find all documents with exactly 20 amenities which include all the amenities listed in the query array, and display their price and address:
db.listingsAndReviews.find({ "amenities":
        { "$size": 20, "$all": [ "Internet", "Wifi",  "Kitchen", "Heating",
                                 "Family/kid friendly", "Washer", "Dryer",
                                 "Essentials", "Shampoo", "Hangers",
                                 "Hair dryer", "Iron",
                                 "Laptop friendly workspace" ] } },
                            {"price": 1, "address": 1}).pretty()

- Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor:
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()

- Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor, also exclude ``"maximum_nights"``. **This will be an error:*
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1,
                             "_id": 0, "maximum_nights":0 }).pretty()

- Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment:
db.grades.find({ "class_id": 431 },
               { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
             }).pretty()

- Find all documents where the student had an extra credit score:
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
               }).pretty()

- How many companies in the sample_training.companies collection have offices in the city of Seattle?
db.companies.find({ "offices": { "$elemMatch": { "city": "Seattle" } }
                  }).count()



```

## Querying Arrays and sub-documents

```

use sample_training

db.trips.findOne({ "start station location.type": "Point" })

db.companies.find({ "relationships.0.person.last_name": "Zuckerberg" },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships.0.person.first_name": "Mark",
                    "relationships.0.title": { "$regex": "CEO" } },
                  { "name": 1 }).count()


db.companies.find({ "relationships.0.person.first_name": "Mark",
                    "relationships.0.title": {"$regex": "CEO" } },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships":
                      { "$elemMatch": { "is_past": true,
                                        "person.first_name": "Mark" } } },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships":
                      { "$elemMatch": { "is_past": true,
                                        "person.first_name": "Mark" } } },
                  { "name": 1 }).count()


- How many trips in the sample_training.trips collection started at stations that are to the west of the -74 longitude coordinate?
db.trips.find({ "start station longitude": { "$lt": -74 }
               }).count()

- How many inspections from the sample_training.inspections collection were conducted in the city of NEW YORK?
db.inspections.find({ "address.city": "NEW YORK" }).count()

```

## Aggregation Framework

- In its simplest form, another way to query data in MongoDB

- $group - allows us to group data by a specified field
- $match - allows us to filter data based on a specified criteria
- $sort - allows us to sort data based on a specified criteria
- $skip - allows us to skip a specified number of documents
- $limit - allows us to limit the number of documents returned
- $unwind - allows us to unwind an array
- $project - allows us to project a specified field
- $sample - allows us to randomly sample a specified number of documents
- $geoNear - allows us to find the closest documents to a specified point
- $lookup - allows us to perform a join operation
- $graphLookup - allows us to perform a join operation
- $out - allows us to output the results of an aggregation pipeline to a specified collection
- $merge - allows us to merge the results of two or more aggregation pipelines
- $redact - allows us to remove data from a document based on a specified criteria
- $replaceRoot - allows us to replace the root of a document with the result of an aggregation pipeline
- $unset - allows us to remove a specified field from a document
- $addFields - allows us to add a specified field to a document
- $bucket - allows us to group documents into buckets based on a specified criteria
- $bucketAuto - allows us to group documents into buckets based on a specified criteria
- $replaceWith - allows us to replace a specified field with the result of an aggregation pipeline
- $sortByCount - allows us to sort documents based on the number of documents that match a specified criteria.

- Find all documents that have Wifi as one of the amenities. Only include price and address in the resulting cursor.

```
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()
```

- Using the aggregation framework find all documents that have Wifi as one of the amenities``\*. Only include price and address in the resulting cursor

  ```
  db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()
  ```

- Find one document in the collection and only include the address field in the resulting cursor.

  ```
  db.listingsAndReviews.findOne({ },{ "address": 1, "_id": 0 })
  ```

  - Project only the address field value for each document, then group all documents into one document per address.country value.

  ```
  db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])

  ```

  - Project only the address field value for each document, then group all documents into one document per address.country value, and count one for each document in each group.

  ```
  db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])

  ```

  ### sort and limit

  ```
  use sample_training

  db.zips.find().sort({ "pop": 1 }).limit(1)

  db.zips.find({ "pop": 0 }).count()

  db.zips.find().sort({ "pop": -1 }).limit(1)

  db.zips.find().sort({ "pop": -1 }).limit(10)

  db.zips.find().sort({ "pop": 1, "city": -1 })
  ```

## Indexes

- In a database - special data structure taht stores a small portion of the collection's data set in an easzy to traverse form.

- Indexes are a way to speed up queries by allowing MongoDB to use a specific query plan for a query.

```
use sample_training

db.trips.find({ "birth year": 1989 })

db.trips.find({ "start station id": 476 }).sort( { "birth year": 1 } )

db.trips.createIndex({ "birth year": 1 })

db.trips.createIndex({ "start station id": 1, "birth year": 1 })

```

## Data Modeling

- Data modeling - a way to organize fields in a document to support your application performance and querying capabilities.
- A data model is a way to represent the data in a collection.

## Upserts

- Upserts - a way to update a document if it exists or insert a new document if it does not exist.

```

db.iot.updateOne({ "sensor": r.sensor, "date": r.date,
                   "valcount": { "$lt": 48 } },
                         { "$push": { "readings": { "v": r.value, "t": r.time } },
                        "$inc": { "valcount": 1, "total": r.value } },
                 { "upsert": true })


```
