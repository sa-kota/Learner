- promise, async & await
```
const https = require('https');

function fetch(url) {
  return new Promise( (resolve, reject) => {
    https.get(url, (res) =>{
      let data = '';
      res.on('data', (rd) => data = data + rd);
      res.on('end', () => resolve(data));
      res.on('error', reject);
    });
  });
}

fetch('https://www.javascript.com/')
  .then( data => {
    console.log(data);
  });

(async function read(){
  const data = await fetch('https://www.javascript.com/');
  console.log(data);
})();

```

- Class
```
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hello ${this.name}`);
  }
}

class Student extends Person {
  constructor(name, level) {
    super(name);
    this.level = level;
  }
  greet() {
    console.log(`Hello ${this.name} from ${this.level}`);
  }
}

const o1 = new Person("Ramesh");
const o2 = new Student("Sandesh", "10th");
const o3 = new Student("Suresh", "13th");

o3.greet = () => console.log('I am Special!!!');

o1.greet();
o2.greet();
o3.greet();
```

- Templates (the char => ` )
```
// evaluating
const html = `<div>${Math.random()}</div>`;
// multiple lines
const html = `
  <div>
    ${Math.random()}
  </div>`;
```

- Arrow functions
```
// sample
const square = (a) => {
  return a * a;
};
// shot hand
const square = (a) => a * a;
const square = a => a * a;   // if only one parameter
```
- Arrow Functions :: Output ?
```
this.id = 'exports';

const testerObj = {
  func1: function() {
    console.log('func1', this);
  },
  func2: () => {
    console.log('func2', this);
  },
  func3: () => console.log('func3', this)
}

testerObj.func1();  // shows testerObj info
testerObj.func2();  // shows exports/window info
testerObj.func3();  // shows exports/window info
```

- Dynamic property
```
const mystery = 'answer';
const PI = Math.PI;

const obj = {
  p1: 10,
  PI,
  [mystery]: 42
}

console.log(obj.PI);
console.log(obj.mystery);
console.log(obj.answer);
```
- Dynamic Property from class : Destructuring
```
const PI = Math.PI;
const E = Math.E;
const SQRT2 = Math.SQRT2;
// short code
const { PI, E, SQRT2 } = Math;

// another example
// basically takes readFile property/method from 'fs' module and assigns it to locally created variable named readFile
const { readFile } = require('fs');

// another example - destructuring and taking what exactly is required
const circleArea = ({ radius }, { precision = 2 } = {}) => {
  //var result = (radius * radius).ToFixed(2);
  var result = (radius * radius).toFixed(precision);
  console.log(result);
}
const circle = { radius: 5, diameter: 10, thickness: 1 };
circleArea(circle, { precision: 0 });
circleArea({ radius:15, height:100 });
circleArea({ radius:3 }, { precision: 1});
// { precision = 2 }     => Default value
// { precision = 2 }     => Optional

// skip if not required
const [first, , , fourth] = [10, 20, 30, 40];
console.log(first);
console.log(fourth);

// rest of items
var[first, ...restOfItems] = [10, 20, 30];
console.log(first);
console.log(restOfItems);

// shallow copies
var newA = [...restOfItems];
console.log(newA);
// how is it different ???
var newB = restOfItems;
console.log(newB);
```

### [Questions](https://github.com/ILearny/Standards/wiki/IV-Javascript)
