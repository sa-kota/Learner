- To connect to MySQL Database through Node.js we need to have MySQL driver. First we need to ensure MySQL is installed and running. Then we need to isntall the driver as below
```
npm install mysql
```

- Connecting to MySQL server
```
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");

  -- Creating DB
  con.query("CREATE DATABASE mydb", function (err, result) {
    if (err) throw err;
    console.log("Database created");
  });

  -- Creating Table
  var sql = "CREATE TABLE customers (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255), address VARCHAR(255))";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Table created");
  });
  -- var sql = "ALTER TABLE customers ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY";
});
```

### Insert
- sql above can vary based on the intent
```
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "INSERT INTO customers (name, address) VALUES ?";
  var values = [     ['John', 'Highway 71'],    ['Peter', 'Lowstreet 4'],    ['Amy', 'Apple st 652']  ];
  -- [values] is optional
  con.query(sql, [values], function (err, result, fields) {
    if (err) throw err;
    -- result contains all the data related to the operation status
    console.log("Number of records inserted: " + result.affectedRows);
  });

  -- SINGLE INSERT :: var sql = "INSERT INTO customers (name, address) VALUES ('Company Inc', 'Highway 37')";
  -- SELECT :: var sql = "SELECT * FROM Customers WHERE name like '%Jo%' ORDER BY name DESC";
  -- Escaping Query Values :: When query values are provided by the user, we should escape the values
    --  var adr = 'Mountain 21';
    -- var sql = 'SELECT * FROM customers WHERE address = ' + mysql.escape(adr);
  -- DELETE :: var sql = "DELETE FROM customers WHERE address = 'Mountain 21'";
  -- DROP TABLE :: var sql = "DROP TABLE IF EXISTS customers";
  -- UPDATE TABLE ::  var sql = "UPDATE customers SET address = 'Canyon 123' WHERE address = 'Valley 345'";
  -- LIMIT :: var sql = "SELECT * FROM customers LIMIT 5";
  -- LIMIT BY OFFSET :: var sql = "SELECT * FROM customers LIMIT 5 OFFSET 2"; -- (same as LIMIT 2,5)
  -- JOINS :: var sql = "SELECT users.name AS user, products.name AS favorite FROM users JOIN products ON users.favorite_product = products.id"; -- (LEFT, RIGHT)
});

var name = 'Amy';
var adr = 'Mountain 21';
var sql = 'SELECT * FROM customers WHERE name = ? OR address = ?';
con.query(sql, [name, adr], function (err, result) {
  if (err) throw err;
  console.log(result);
});
```
