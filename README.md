# Different ways of creating an Object in javascript

**Using the Object() constructor:**
```javascript
var d = new Object();
This is the simplest way to create an empty object. I believe it is now discouraged.
```
**Using Object.create() method:**

```javascript
var a = Object.create(null);
This method creates a new object extending the prototype object passed as a parameter.
```
** Using the bracket's syntactig sugar - Object Literal: **
```javascript
var b = {};
This is equivalent to Object.create(null) method, using a null prototype as an argument.
```
**Using a function constructor**
```javascript
var Obj = function(name) {
  this.name = name
}
var c = new Obj("hello"); 

//What the new operator does is call a function and setting this of the function to a fresh new Object.
//and binding the prototype of that new Object to the function's prototype.

function f {};
new f(a, b, c);

//Would be equivalent to: 

// Create a new instance using f's prototype.
var newInstance = Object.create(f.prototype)
var result;

// Call the function
result = f.call(newInstance, a, b, c),

// If the result is a non-null object, use it, otherwise use the new instance.
result && typeof result === 'object' ? result : newInstance
```
**Using the function constructor + prototype:**
```javascript
function myObj(){};
myObj.prototype.name = "hello";
var k = new myObj();
```
**Using ES6 class syntax:**
```javascript
class myObject  {
  constructor(name) {
    this.name = name;
  }
}
var e = new myObject("hello");
```
**Singleton pattern:**
```javascript
var l = new function(){
  this.name = "hello";
}
```

# Prototypal inheritance
```javascript
// Consider it as Base Class
function VehicleMaster(regNumber, taxPaid, state){
    this.regNumber = regNumber;
    this.taxPaid = taxPaid; 
    this.state = state; 
}

// Add methods that will be shared 
VehicleMaster.prototype.printLegalDetails = function(){
    console.log("Reg. Number : " + this.regNumber);
    console.log("Tax Paid : " + this.taxPaid);
    console.log("State : " + this.state);        
}

VehicleMaster.prototype.startEngine = function(){
    console.log("Engine Started")    
}

VehicleMaster.prototype.stopEngine = function(){
   console.log("Engine Stopped")    
}

VehicleMaster.prototype.honk = function(){
   console.log("Bi Bi Bi....")    
}


// Consider it as Child Class
function Vehicle(model,year,color, regNumber, taxPaid, state){   

    // Chain constructor with call, reuse properties from Base
    VehicleMaster.call(this, regNumber, taxPaid, state);

    this.model = model;
    this.year = year;
    this.color = color;    
   

    this.getDetails =function(){
        console.log("Name : " + this.model);
        console.log("Year : " + this.year);
        console.log("Color : " + this.color);
        // get form parent object
        this.printLegalDetails();
    }
    
}

console.log('Method 1 : Object.create() & Constructor re-assignment')
Vehicle.prototype = Object.create(VehicleMaster.prototype);
Vehicle.prototype.constructor = Vehicle;

let vehicle = new Vehicle('BMW',2019,'red','789987','valid','XYZ');


console.log(vehicle);
console.log('Constructor : ' + vehicle.constructor.name); // vehicle
console.log(vehicle.getDetails()); 
console.log(vehicle.honk()); 
````

```javascript
*Create Vehicle object from existing object which has same attributes*
let carData  = {
    model:'Audi',
    year:2018,
    color:'red',
    regNumber:99999,
    taxPaid:'valid',
    state:'CD'
}

console.log('Method 2 : Object.assign(new Vehicle(), car )')

let car = Object.assign(new Vehicle(), carData )
console.log(car);
console.log('Constructor : ' + car.constructor.name); // object
console.log(car.getDetails()); 
console.log(car.startEngine()); 


console.log('Method 3 : Object.setPrototypeOf(car, Vehicle.prototype)')
// this will simply assign the prototype and other attributes will be skipped
let car2 = Object.setPrototypeOf(carData, Vehicle.prototype)
console.log(car2);
console.log('Constructor : ' + car2.constructor.name); // Vehicle
console.log(car2.getDetails()); //Error
```
# Promise
A “producing code” that does something and takes time. For instance, the code loads data over a network.
A “consuming code” that wants the result of the “producing code” once it’s ready. Many functions may need that result. 
A promise is a special JavaScript object that links the “producing code” and the “consuming code” together. 
The “producing code” takes whatever time it needs to produce the promised result, and the “promise” makes that result available to all of the subscribed code when it’s ready.
```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve runs the first function in .then
promise.then(
  result => alert(result), // shows "done!" after 1 second
  error => alert(error) // doesn't run
);

// Let’s say we want to run many promises to execute in parallel, and wait till all of them are ready.
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://api.github.com/users/jeresig'
];

// map every url to the promise fetch(github url)
let requests = urls.map(url => fetch(url));

// Promise.all waits until all jobs are resolved
Promise.all(requests)
  .then(responses => responses.forEach(
    response => alert(`${response.url}: ${response.status}`)
  ));

```
When the executor finishes the job, it should call one of the functions that it gets as arguments:
`resolve(value)` — to indicate that the job finished successfully:
sets `state` to `fulfilled`,
sets `result` to `value`.
`reject(error)` — to indicate that an error occurred:
sets `state` to `rejected`,
sets `result` to `error`.

# Event loop
In-browser JavaScript execution flow, as well as Node.js, is based on an event loop.
`Event loop` is a process when the engine sleeps and waits for events. When they occur – handles them and sleeps again.
Events may come either comes from external sources, like user actions, or just as the end signal of an internal task.
Examples of events:
mousemove, a user moved their mouse.
setTimeout handler is to be called.
an external <script src="..."> is loaded, ready to be executed.
a network operation, e.g. fetch is complete.
Things happen – the engine handles them – and waits for more to happen (while sleeping and consuming close to zero CPU).
  
**Stack** 
Function calls form a stack of frames.  
**Heap** 
Objects are allocated in a heap which is just a name to denote a large mostly unstructured region of memory.
**Queue** 
A JavaScript runtime uses a message queue, which is a list of messages to be processed. Each message has an associated function which gets called in order to handle the message.
**Browser or Web APIs**  
They are built into your web browser, and are able to expose data from the browser and surrounding computer environment and do useful complex things with it.

![Diagram](https://miro.medium.com/max/400/1*RuCaP1t09YaF7wfernuLWA.png)
  
```javascript
function main(){
  console.log('A');
  setTimeout(
    function display(){ console.log('B'); }
  ,0);
	console.log('C');
}
main();
//	Output
//	A
//	C
//  B
``` 
Here we have the main function which has 2 console.log commands logging ‘A’ and ‘C’ to the console. Sandwiched between them is a setTimeout call which logs ‘B’ to the console with 0ms wait time.

![Diagram](https://miro.medium.com/max/1000/1*64BQlpR00yfDKsXVv9lnIg.png)

The call to the main function is first pushed into the stack (as a frame). Then the browser pushes the first statement in the main function into the stack which is console.log(‘A’). This statement is executed and upon completion that frame is popped out. Alphabet A is displayed in the console.

The next statement (setTimeout() with callback exec() and 0ms wait time) is pushed into the call stack and execution starts. setTimeout function uses a Browser API to delay a callback to the provided function. The frame (with setTimeout) is then popped out once the handover to browser is complete (for the timer).

console.log(‘C’) is pushed to the stack while the timer runs in the browser for the callback to the exec() function. In this particular case, as the delay provided was 0ms, the callback will be added to the message queue as soon as the browser receives it (ideally).
After the execution of the last statement in the main function, the main() frame is popped out of the call stack, thereby making it empty. For the browser to push any message from the queue to the call stack, the call stack has to be empty first. That is why even though the delay provided in the setTimeout() was 0 seconds, the callback to exec() has to wait till the execution of all the frames in the call stack is complete.

Now the callback exec() is pushed into the call stack and executed. The alphabet C is display on the console. This is the event loop of javascript.

>So the delay parameter in setTimeout(function, delayTime) does not stand for the precise time delay after which the function is executed. It stands for the minimum wait time after which at some point in time the function will be executed.

**Links -** 
https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4
https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop


# x^n using recursion 
A function pow(x, n) that raises x to a natural power of n. In other words, multiplies x by itself n times.
```javascript
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}
alert( pow(2, 3) ); // 8
```
# Factorial using recursion
```javascript
function factorial(x){ 
 if (x === 0){
    return 1;
 }
 return x * factorial(x-1);   
}
console.log(factorial(5)); //120
```

# Fibonacci Series using for loop
```javascript
var var1 = 0;
var var2 = 1;
var var3;
var count = 8
console.log(var1);
console.log(var2);
for(var i=3; i <= count;i++){
    var3 = var1 + var2;
    var1 = var2;
    var2 = var3;
    console.log(var3); // 0 1 1 2 3 5 8 13
}
```
