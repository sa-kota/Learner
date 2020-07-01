- To connect to MongoDB Database through Node.js we need to have MongoDB driver. First we need to ensure MongoDB is installed and running. Then we need to isntall the driver as below
```npm install mongodb```

- CREATING DB
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/mydb";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  console.log("Database created!");
  db.close();
});
```
- CREATING COLLECTION
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.createCollection("customers", function(err, res) {
    if (err) throw err;
    console.log("Collection created!");
    db.close();
  });
});
```
-- INSERT INTO COLLECTION
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myobj = { name: "Company Inc", address: "Highway 37" };
  dbo.collection("customers").insertOne(myobj, function(err, res) {
    if (err) throw err;
    console.log("1 document inserted");
    db.close();
  });

  var myobj = [
    { _id: 154, name: 'John', address: 'Highway 71'}, -- if you do not assign _id, mongodb will create a unique id
    { name: 'Peter', address: 'Lowstreet 4'},
  ];
  dbo.collection("customers").insertMany(myobj, function(err, res) {
    if (err) throw err;
    console.log("Number of documents inserted: " + res.insertedCount);
    db.close();
  });
});
```
- FIND
```
-- first
dbo.collection("customers").findOne({}, function(err, result) {
  if (err) throw err;
  console.log(result.name);
  db.close();
});

-- all
dbo.collection("customers").find({}).toArray(function(err, result) {
  if (err) throw err;
  console.log(result);
  db.close();
});
-- include / exclude columns can be done using projection
dbo.collection("customers").find({}, { projection: { _id: 0, name: 1, address: 1 } }) -- loads only name & address
dbo.collection("customers").find({}, { projection: { address: 0 } })  -- loads all except address
dbo.collection("customers").find({}, { projection: { name: 1, address: 0 } }) -- ERROR
```
- QUERY
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var query = { address: "Park Lane 38" };
  dbo.collection("customers").find(query).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });

  -- var query = { address: /^S/ }; -- finds all address which starts with letter "S"
})
```
- SORT
```
var dbo = db.db("mydb");
var mysort = { name: 1 };
-- var mysort = { name: -1 }; -- for descending
dbo.collection("customers").find().sort(mysort).ToArray(.....);
```
- DELETE
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: 'Mountain 21' };
  dbo.collection("customers").deleteOne(myquery, function(err, obj) {
    if (err) throw err;
    console.log("1 document deleted");
    db.close();
  });

  -- DELETE MANY
  var myquery = { address: /^O/ };
  dbo.collection("customers").deleteMany(myquery, function(err, obj) {
    if (err) throw err;
    console.log(obj.result.n + " document(s) deleted");
    db.close();
  });
});
```
- DROP COLLECTION
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").drop(function(err, delOK) {
    if (err) throw err;
    if (delOK) console.log("Collection deleted");
    db.close();
  });

  -- DROP COLLECTION CAN ALSO BE USED
  dbo.dropCollection("customers", function(err, delOK) {
    if (err) throw err;
    if (delOK) console.log("Collection deleted");
    db.close();
  });
});
```
- UPDATE DOCUMENT
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://127.0.0.1:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: "Valley 345" };
  var newvalues = { $set: {name: "Mickey", address: "Canyon 123" } };
  dbo.collection("customers").updateOne(myquery, newvalues, function(err, res) {
    if (err) throw err;
    console.log("1 document updated");
    db.close();
  });

  -- UPDATE MANY
  var myquery = { address: /^S/ };
  var newvalues = {$set: {name: "Minnie"} };
  dbo.collection("customers").updateMany(myquery, newvalues, function(err, res) {...});
});
```
- LIMIT
```
dbo.collection("customers").find().limit(5)
```
- JOIN
```
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://127.0.0.1:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection('orders').aggregate([
    { $lookup:
       {
         from: 'products',
         localField: 'product_id',
         foreignField: '_id',
         as: 'orderdetails'
       }
     }
    ]).toArray(function(err, res) {
    if (err) throw err;
    console.log(JSON.stringify(res));
    db.close();
  });
});
```
