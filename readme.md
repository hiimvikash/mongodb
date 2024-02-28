# 1. Mongo DB Terminologies

![image](https://github.com/hiimvikash/mongodb/assets/71629248/8f93110f-0fd9-488f-aa20-760d6d63329b)

# 2. Managing Databases and Collections

- `show dbs` : shows you the name of your databases. **This will only show output, when the db will have atleast one collection**
- `use <db name>` : creates DB or switch DB
  - `use college`
- `db.dropDatabase()` : deletes the particular database
- `db.createCollection('students')` : creates students collection in college DB, here `db.` means `students.`
- `db.<collection name>.insertOne({})` : inserting one row (document)
  - `db.students.insertOne({name : "vikash", age : 21, profession : "btech"})` : inserting one row (document) in **students collection.**
- `db.<collection name>.insertMany([ {}, {}, {} ])` : insert more than one row at once.
  - `db.students.insertMany([ {name : "ashish", age : 43, profession : "finance"}, {name : "suman", age : 26, profession: "House wife"} ])`

```js
db.books.insertOne({
    title: "The Way of Kings", author: "Brandon Sanderson", rating: 9, pages: 400, genres: ["fantasy", "adventure"], reviews: [
        {name: "Yoshi", body: "Great book!!"},
        {name: "mario", body: "so so"}
    ]
})
```

- **Ordered Insertion** [video](https://youtu.be/v3rKeOpqKV4?si=7T9WF-SRgVDgJUa4)
  - Ordered insertion means that MongoDB processes each document one by one.
  - If one document fails to insert due to an error (like a duplicate ID), MongoDB stops inserting further documents but keeps the ones that succeeded.
  - if there's a duplicate ID, MongoDB throws an error and stops inserting further documents.
  - we can change this behavior by setting "ordered" to false, allowing MongoDB to continue inserting documents after encountering an error.
  - `db.<collection name>.insertMany([ {}, {}, {} ], {ordered : false})`
  - Even if some documents fail to insert, the ones that succeeded remain in the collection.
  - They emphasize that once a document is successfully inserted, it won't roll back even if later documents encounter errors.

# 3. Search in DB

- `db.students.find().pretty()` : shows all the row of students collection in prettified format.
- `db.students.find({name : "ashish", age : 43}).pretty()` : find documents in `students` collection whose name : "ashish" and age : "43"
- `db.students.find({age : {$gte : 50}}).count()` : returns the count.
- `db.students.find({age : {$gte : 50}}).limit(3)` : this limits the number of document we get for particular filtered query.
- `db.students.find({name : "ashish", age : {$lt : 50}}).pretty()`
- `db.students.find({name : "ashish", age : {$lte : 50}}).pretty()`
- `db.students.find({name : "ashish", age : {$gt : 50}}).pretty()`
- `db.students.find({name : "ashish", age : {$gte : 50}}).pretty()`
- `db.students.find({age : {$in : [12, 300, 9, 200]}})` : show all document which has age **includes** 12, 300, 9, 200
- `db.students.find({age : {$nin : [12, 300, 9, 200]}})` : show all document which has age **not includes** 12, 300, 9, 200
- `db.products.find({price: {$exists: true}}).count()` : This will give us the documents having/not-having particular field.
- `db.books.find({reviews : {$size : 2}})` : this will give me the books having 2 reviews
- `db.students.find({math_score : {$type : 'int'}}).count()` : This will give us the documents count having field of particular type. [BSON types doc](https://www.mongodb.com/docs/manual/reference/bson-types/)

Shows document fields of your choice [either give what **to-show** orelse what **not-to-show**, dont give both (_id is exception)]
- `db.books.find({search query}, {fields to show / fields not-to show})`
- `db.books.find({ 'reviews': {$size:2}}, {title:1, author:1}` : shows title and author only 
- `db.books.find({ 'reviews': {$size:2}}, {title:0, author:0}` : shows other-field except title and author.

```js
{
    _id: ObjectId("622b746cd28eddc86c808d41"),
    title: '1984',
    author: 'George Orwell',
    pages: 300,
    rating: 7,
    genres: [ 'sci-fi', 'dystopian', 'fantasy' ],
    reviews: [
        { name: 'luigi', body: 'not my cup of tea' },
        { name: 'mario', body: 'meh' }
    ],
    metadata: { views: 1500, likes: 60 }
}
```

- `db.books.find().sort({ rating: 1 })` : sort ascending order of rating
- `db.books.find().sort({ rating: -1 })`: sort descending order of rating
- `db.books.find({$or: [{rating: 7}, {rating: 9}]})` : we give array of filters, if anyone of the filters matches it print that document.
- `db.books.find({$or: [{pages: {$lt: 300 }}, {pages: {$gt: 400}}]})`
- `db.books.find({$and: [{pages: {$gt: 500}}, {pages: {$lt: 1300 }}]})` : both condition should be true.
- `db.books.find({genres: {$all: [fantasy", "sci-fi"]}})`
  - we are finding books which has genres[] containing both(fantasy and sci-fi) atleast.
- `db.books.find({genres: "fantasy"})` : this will find book which has genres as fantasy atleast.
- `db.books.find({"reviews.name": "luigi"})`
- `db.books.find({"metadata.views": {$gt : 1200}})`
- `db.books.find({"reviews.name": "luigi", "metadata.views": {$gt : 1200}})`
- `db.books.find({'reviews.name': {$all: ['luigi', 'mario'] }`

Itteration of document
```js
db.<collection_name>.find().forEach(function(doc) {
    print("Key: " + doc.<key> + " Value: " + doc.<value>);
})
```

# 4. Update document in DB

- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d40")}, {$set: {rating: 8, pages: 360}})`
- `db.books.updateMany({author: "Terry Pratchett"}, {$set: {author: "Terry Pratchet"}})`
- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d41")}, {$inc: {pages: 2}})` : increment the pages by 2.
- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d41")}, {$inc: {pages: -2}})` : decrement the pages by 2.
- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d41")}, {$pull: {genres: "fantasy"}})` : this will remove the "fantasy" from genres[]
- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d41")}, {$push: {genres: "fantasy"}})` : this will add the "fantasy" in genres[]
- `db.books.updateOne({_id: ObjectId("622b746cd28eddc86c808d41")}{$push:{genres: {$each: ["1","2"]}}})` : this will add more than one value in genres[]

# 5. delete documents in DB

- `db.students.deleteMany({id : {$gt:2}})`
- `db.students.deleteMany({})` : delete all documents in students collection

# 6. Import/Export in mongoDB

- `mongoimport <filePath\filename.json> -d <db name> -c <collection name>` {}, {}, {}
- `mongoimport products.json -d shop -c products --jsonArray` if .json is [{} {} {}]
