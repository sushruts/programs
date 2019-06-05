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
