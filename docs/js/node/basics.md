### Node Intro {docsify-ignore-all}
- A javascript runtime environment that executes the Javascript code outside of a browser. It can run on various platforms (Windows, Linux, Unix, Mac OS X, etc..)
- Acts as a Web Server, Server Language (Code), Tools (comes with NPM - Package Manager, Module Dependency Manager)
- Asynchronous operations without Threading (Single thread handles by using callback functions)
- Node JS is managed by Event Loop (Libuv Library), using Callback
  - When Open a file is fulfilled by OS, it triggers an event which is handled by Event Loop
  - An incoming HTTP request triggers an event which is handled by Event Loop
  - Timers trigger event which is handled by Event Loop

### Synchronus v/s Asynchronus
- Synchronus
```
function serveCustomer(customer) {
    let order = customer.placeOrder(menu);
    let food = cook.prepareFood(order);
    let tip = customer.eatAndPay(food);
    return tip;
}
```
- Asynchronus: Node Style
```
function serveCustomer(customer, done) {
    customer.placeOrder(menu, (error, order) => {
        cook.prepareFood(order, (error, food) => {
            customer.eatAndPay(food, done);
        });
    });
}
// also called as error first callback pattern - you can see the callback methods has error as the first parameter

// better way using promises
function serveCustomer() {
  return customer.placeOrder(menu)
    .then(order => cook.prepareFood(order))
    .then(food => customer.eatAndPay(food));
}
// better way using async & await
function serveCustomer = async(customer) => {
  let order = await customer.placeOrder(menu);
  let food = await cook.prepareFood(order);
  let tip = await customer.eatAndPay(food);
  return tip;
}
```
### Event Emitter : Built on Event Loop & asynchronus concepts
```
const EventEmitter = require('events');
const emitter = new EventEmitter();

// emitter.emit()
emitter.on('data', (msg) => {
  console.log(msg);
});

//emitter.on()
emitter.emit('data', 'Hello World!');
```
- For Example above
```
const serveCustomer = (customer, done) => {
  cusotmer.on('decided', order => {
    order.on('prepared', food => customer.eatAndPay(food))
    cook.prepareFood(order)
  })

  customer.on('leaving', tip => done(null, tip))
  customer.placeOrder(menu)
}
```
- Set Immediate
```
// In the below example the console log will not be printed. Since the subscription happened after emitting
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.emit('data', 'Hello World!');

emitter.on('data', (msg) => {
  console.log(msg);
});

// In the below example the console log will be printed. Since the setImmediate gets executed after the complete script 
// execution by when the subscription has happened
const EventEmitter = require('events');
const emitter = new EventEmitter();

setImmediate(() => {
  emitter.emit('data', 'Hello World!');
});

emitter.on('data', (msg) => {
  console.log(msg);
});

```

### Event Loop
- What node uses to process asynchronous actions and interface them for you so that you don't have to deal with threads

### Error v/s Exception
- Error is a problem
- Exception is a condition
```
const fs = require('fs');
const path = require('path');
try {
  const = filePath = path.resolve(provess.env.HOME, '.npmrc')
  const data = fs.readFileSync(filePath);   
  // const data = fs.readFileSync(filePath, 'utf-8');
  console.log('File data is - ', data);
} catch (err) {
  if (err.code === 'ENOENT') {
    console.log('File not found);
  } else {
    throw err;
  }
}
```

### As a Web Server
- Save the below code in a "firstNode.js" file
```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World!');
}).listen(8080);
```
- Same as above
```
var http = require('http');
const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer(function (req, res) {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/html');
  res.end('Hello World!');
});

server.listen(port, hostname, () => {
  console.log('server running at http://${hostname}:${port}');
});
```
- using Event Emitter: interally same
```
var http = require('http');

const requestListener = function (req, res) {
  res.write('Hello World!');
  // IMPORTANT :: If not the HTTP session will think that the server is still streaming data
  res.end();
};

const server = http.createServer();
server.on('request',requestListener);

server.listen(3000, () => {
  console.log('server is listening');
});
```
- run the below command and open http://localhost:8080 in browser
```
d:\> node firstNode.js
```
- nodemon - install nodemon and use it same as node. This will monitor file changes and restart the server if required. Of course only for development
- Web Frameworks - Is a wrapper around few of the node modules (ex: HTTP. HTTP/2, HTTPS...) and helps in faster development. Ex: Express JS, Koa JS, Sails JS, Meteor JS etc..
- Templating Framework : pug js, handlebars js, ejs etc..

### node & Operating System
- 'os' module : deals with OS of the server
- 'fs' module : deals with file system in the server
- 'child_process' module : for running commands (.bat & .cmd ) - runs in sub process (of node process) and gives data to main process (node process)

### Exports, Module, Global
- Exporting a module
```
// 1.js
exports.name = "kota"
module.exports.age = 25 // same as above ( exports is an alias to module.exports )

//2.js
var data = require(./1.js);
console.log(data.name);
```
```
// 1.js
exports = [0,1,2]

//2.js
var data = require(./1.js);
console.log(data);
```
```
// 1.js
exports.getSquare = function(number) {
  return number * number;
}

//2.js
var math = require(./1.js);
console.log(math.getSquare(2));
```
- global: internally used by Node | DO NOT USE
```
// 1.js
global.number = 3;

// 2.js
require(./1.js);
console.log(number);
```

## NPM - A package manager for Node.js packages (modules) & also a command line application
```
// command prompt
npm install upper-case
// installing from URL
npm install http://www.myserver.com/package1
npm install http://www.github.com/helo-fam-package

-- usage
var uc = require('upper-case');
```
- install gist
```
// instaling from github gist (ex: https://gist.github.com/sa-kota/e04c75a0d4005111e30a6aef1b4e0d07)
npm install gist:e04c75a0d4005111e30a6aef1b4e0d07

// package.json
"dependencies": {
  "hello-fam": "gist:e04c75a0d4005111e30a6aef1b4e0d07"
}
```
- installing from folder
```
npm i ../hello-fam
```
- Dependency tree & updates
```
> npm outdated 
// will list the dependency installed versions, requested versions (if any change because of semver) & latest versions of the library
```
- publishing to npm registry
```
> npm login
> npm publish
```

### Modularizing Node.js application
- By using exports in different JS files
```
// cook.js
const ingredients = 'stuff'
const prepareFood = (order, done) => {
  // prepare food
}
module.exports = { prepareFood }

// customer.js
class Customer {
  // methods and properties
}
module.exports = Customer

// waitress.js
const cook = require('./cook')
const Customer = require('./customer')
//cook.prepareFood()
// new Customer()
```

### Testing
- OPEN SOURCE TOOLS :: MochaJS (BDD), Chai (Assertion Library), Sinon (Spies, Stubs and Mocks), Istanbul (Code Coverage)
- nodeunit - Testing Module
``` 
// create files with Tests.js
var book = require('grades').book;

exports["setUp"] = function(callback) {
  book.reset();
  callback();
};

exports["Can add new Grade"] = function(test) {
  book.addGrade(80);
  var count = book.getCountOfGrades();
  test.equal(count, 1);
  test.done();
};

// run command 
> nodeunit tests 
```

### NPM Scripts
- Scripts (/commands) which helps in automating
```
// pacakge.json

"scripts:" {
  "start": "node server.js",
  "test": "jest",
  "check": "eslint server.js",
  "ready": "npm-run-all --parallel test start"
}

// in CMD
> npm run start OR npm start
> npx test (npx = npm execute -> Will find the binary under node_modules)
> npm run check

// for more events
> npm help npm-scripts
output => Ex: prepublish, prestest, posttest etc..
// custom scripts can also be added
```

### Debugging
- Consider a node file as below
```
// sample.js
const arr = [1,3,5]
console.log('Hello');
console.log(arr);
```
- To debug the above file 
  - run in cmd
  ``` node --inspect-brk sample.js ```
  - open chrome://inspect & you can see node process listed in it. Click on inspect.


### COMMENTS
- Node.js can be used to connect to various Databases like MySQL, MongoDB, MSSQL,  etc..
- Node.js can be used to connect to Raspberry Pi (mini computer)
- [6.10.3 Built in Modules](https://www.w3schools.com/nodejs/ref_modules.asp)
- Streams:: Helps is doing things in chunks 
- "process" object helps node to interact with OS
- Node Clusters : Ensure that each node process is running for each CPU cores. 
  - Even if it is single Core, run a cluster so that the cluster will monitor the process & start a new process if it crashes. Ex: PM2 tool helps in running cluster nodes
- Sourcemaps: Helps in debugging transpiled and bundled code
  - source maps are downloaded in browser only if you open developer tools

### Not Fit
- CPU intensive tasks: Designed to build scalable network (i/o) applications (node shouldn't be used where it spends too much time on its own)
- Javascript :: Not Type Based (Typescript solves it)

### Clients
- webpack, gulp, eslint, yeoman etc...
- Desktop Applications :: ElectronJS Framework - Ex: Skype, Slack, Github Desktop, Visual Studio Code, Hyper, Atom
