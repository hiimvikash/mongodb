# 1. Mongo DB Terminologies
![image](https://github.com/hiimvikash/mongodb/assets/71629248/8f93110f-0fd9-488f-aa20-760d6d63329b)

# 2. Managing Databases and Collections
- ```show dbs``` : shows you the name of your databases. **This will only show output, when the db will have atleast one collection**
- ```use <db name>``` : creates  DB or switch DB
    - ```use college```
- ```db.dropDatabase()``` : deletes the particular database
- ```db.createCollection('students')``` : creates students collection in college DB, here ```db.``` means ```students.```
- ```db.<collection name>.insertOne({})``` : inserting one row (document)
    -  ```db.students.insertOne({name : "vikash", age : 21, profession : "btech"})``` : inserting one row (document) in **students collection.**
-  ```db.<collection name>.insertMany([ {}, {}, {} ])``` : insert more than one row at once.
    - ```db.students.insertMany([ {name : "ashish", age : 43, profession : "finance"}, {name : "suman", age : 26, profession: "House wife"} ])```
- **Ordered Insertion** [video](https://youtu.be/v3rKeOpqKV4?si=7T9WF-SRgVDgJUa4)
    - Ordered insertion means that MongoDB processes each document one by one.
    - If one document fails to insert due to an error (like a duplicate ID), MongoDB stops inserting further documents but keeps the ones that succeeded.
    -  if there's a duplicate ID, MongoDB throws an error and stops inserting further documents.
    - we can change this behavior by setting "ordered" to false, allowing MongoDB to continue inserting documents after encountering an error.
    - ```db.<collection name>.insertMany([ {}, {}, {} ], {ordered : false})```
    - Even if some documents fail to insert, the ones that succeeded remain in the collection.
    - They emphasize that once a document is successfully inserted, it won't roll back even if later documents encounter errors.
# 3. Search in DB 
- ```db.students.find().pretty()``` : shows all the row of students collection in prettified format.
- ```db.students.find({name : "ashish", age : 43}).pretty()``` : find a document in ```students``` collection whose name : "ashish" and age : "43"
- ```db.students.find({name : "ashish", age : {$lt : 50}}).pretty()```
- ```db.students.find({name : "ashish", age : {$lte : 50}}).pretty()```
- ```db.students.find({name : "ashish", age : {$gt : 50}}).pretty()```
- ```db.students.find({name : "ashish", age : {$gte : 50}}).pretty()```
- ```db.students.find({age : {$in : [12, 300, 9, 200]}})``` : show all document which has age **includes** 12, 300, 9, 200
- ```db.students.find({age : {$nin : [12, 300, 9, 200]}})``` : show all document which has age **not includes** 12, 300, 9, 200
# 4. delete documents in DB
- ```db.students.deleteMany({id : {$gt:2}})```
# 4. Import/Export in mongoDB
- ```mongoimport <filePath\filename.json> -d <db name> -c <collection name>``` {}, {}, {}
- ```mongoimport products.json -d shop -c products --jsonArray``` if .json is  [{} {} {}]
