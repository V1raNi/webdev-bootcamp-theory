# JS knowledge doc

#### Table of Contents

- [JS knowledge doc](#js-knowledge-doc)
      - [Table of Contents](#table-of-contents)
  - [DRY concept](#dry-concept)
  - [Semicolons](#semicolons)
  - [Data types](#data-types)
  - [Numerical operators](#numerical-operators)
  - [Strings](#strings)
  - [Variables](#variables)
  - [Methods for exercises](#methods-for-exercises)
  - [Comparison operators](#comparison-operators)
  - [Logical operators](#logical-operators)
    - ['Truthy' or 'Falsy' statements](#truthy-or-falsy-statements)
  - [Conditionals](#conditionals)
    - [Ternary operator](#ternary-operator)
  - [Loops](#loops)
    - [While loop](#while-loop)
    - [For loop](#for-loop)
  - [Functions](#functions)
    - [Arguments](#arguments)
    - [Return keyword](#return-keyword)
    - [Function Declaration vs Function Expression](#function-declaration-vs-function-expression)
    - [Pure Functions](#pure-functions)
    - [Scope](#scope)
    - [Higher Order Functions](#higher-order-functions)
  - [Arrays](#arrays)
    - [Some array methods](#some-array-methods)
    - [Array iteration](#array-iteration)
    - [Comparing arrays and objects](#comparing-arrays-and-objects)
      - [Array](#array)
      - [Object](#object)
    - [Nested objects and arrays](#nested-objects-and-arrays)
    - [Adding methods to an object](#adding-methods-to-an-object)
    - [The Keyword This](#the-keyword-this)

## DRY concept

Don't Repeat Yourself\
Code should be as DRY as possible, it's not good to repeat your code, i.e. you should simplify and optimize everything

---

## Semicolons

Some people omit semicolons, and some not\
Google format is to include semicolons everywhere except for function and class declaration after {} (see https://google.github.io/styleguide/jsguide.html#formatting-semicolons-are-required or https://google.github.io/styleguide/javascriptguide.xml?showone=Semicolons#Semicolons (older version))

## Data types

1. Number 
    - 4
    - 9.8
    - -10
2. String 
    - "hello world"
    - '32'
3. Boolean 
    - true
    - false
4. Null
   - Basically means 'nothing'
5. Undefined
   - Variable that is declared but not initialized to a value

## Numerical operators

1. Regular (+, -, /, *)
2. Modulo (mod)
    - 10 % 3 // 1
    - 24 % 2 // 0

## Strings

1. Single of double quotes are ok
    - Although "I can't stop eating" is valid, but 'I can't stop' is not (see 'Escape characters')
2. Concatenation
    - `'charlie' + 'brown' // 'charliebrown'`
3. Escape characters begin with \
    - `'I can\'t stop'`
4. Length property
    - `'hello world!'.length // 12`
5. Access individual chars using [] and index
    - `'hello'[4] // 'o'`

## Variables

1. Container that stores values
    - `var yourVariableName = yourValue;`
2. Store all the values
3. Recalled by the name
4. We can update the value
    ```javascript
    var name = 'Bob';
    name = 'Dima';
    ```
5. camelCase in JS (there is also snake_case and kebab-case or dash-case)

##  Methods for exercises

1. alert()
2. prompt()
    - Popup where user writes something
    - `prompt('What is your name');`
    - `var userName = prompt('What is your name?');`
3. console.log()
4. string.indexOf('X') 
    - returns position of X in a string
    - X can be a word, but it will return position where X starts in the string, i.e. 
        ```javascript
        var str = 'Hello world';
        str.indexOf('World'); // 6
        ```
    - returns -1 if X is not in the string
5. Number()
    - turns string into a number
6. String()
    - turns number into a string
7. Math.sqrt()
    - returns a square root of a number
8. str.includes('X')
    - checks if str includes X
9. Math.random()
    - returns a random number between 0 and 1 (exclusively)
    - to mimic a dice roll use Math.random() * 6 + 1
        - multiplying by 6 gives us the 'dice' and adding one is to get number about 5.999...
10. Math.floor()
    - 'chops' off the decimal part of a number

---

## Comparison operators

They are: >, >=, <, <=, == (equal), != (not equal), === (equal value and type), !== (not equal value or equal type)

x = 5

**Examples**

- `x == "5" // true`\
Uses type coercion: it takes 2 variables and turns them into similar type\
When the operands of an operator are different types, one of them will be converted to an "equivalent" value of the other operand's type
    ```javascript
    boolean == integer // Boolean is converted to an integer, false = 0, true = 1
    var x = 99;
    X == "99" // true;
    x === "99" // false;
    var y = null;
    y == undefined // true;
    y === undefined // false;
    ```
- `x != "b" // true`
- `x === "5" // false`
- `x !== "5" // true`

 **Weird examples**

- `true == "1" // true (only for 1)`
- `0 == false // true`
- `null == undefined // true`
- `NaN == NaN // false (Not a Number)`

## Logical operators

x = 5, y = 9

1. && - And (not sure if it is used in English, but it is multiplication)
    - `(x < 10 && x !==5 // false)`
2. || - Or (addition)
    - `(y > 9 || x === 5 // true)`
3. ! - Not
    - `(!(x === y) // true)`

### 'Truthy' or 'Falsy' statements

Values that aren't actually true or false, are still inherintly 'truthy' or 'falsy' when evaluated in a boolean statement (try checking with !)

- `!'hello' // false`
- `!!'hello' // true`
- `!!'' // false`
- `!!-1 // true`

**Falsy values (everything else is truthy):**
1. false
2. 0
3. ""
4. null
5. undefined
6. NaN

## Conditionals

Keywords: if, else if (it is one word for JS), else

Syntax:

```javascript
if (age < 18) {
    console.log("Sorry, you are not allowed to enter");
}
else if (age < 21) { // Equals to (age > 18 && age < 21)
    console.log("You can enter, but cannot drink");
}
else {
    console.log("Come on in. You can drink");
}
```

### Ternary operator

The conditional (ternary) operator is the only JavaScript operator that takes three operands; it is frequently used as a shortcut for the `if` statement

Syntax:

```javascript
var age = 26;
var beverage = (age >= 21) ? "Beer" : "Juice";
console.log(beverage); // "Beer"
```

With null:

```javascript
function greeting(person) {
    var name = person ? person.name : "stranger";
    return "Howdy, " + name;
}

console.log(greeting({name: 'Alice'}));  // "Howdy, Alice"
console.log(greeting(null));             // "Howdy, stranger"​​​​​
```

Conditional chain:

```javascript
function example(…) {
    return condition1 ? value1
         : condition2 ? value2
         : condition3 ? value3
         : value4;
}

// Equivalent to:

function example(…) {
    if (condition1) { return value1; }
    else if (condition2) { return value2; }
    else if (condition3) { return value3; }
    else { return value4; }
}
```

---

## Loops

### While loop

Repeats a code while a condition is true

```javascript
while (condition) {
    //code
}
// Printing numbers from 1 to 5
var count = 1;
while (count < 6) {
    console.log('Count is ' + count);
    count++; // count += 2 if you want to add 2
}
```

Printing each character in a string example

```javascript
// string we are looping over:
var str = 'Hello';
// first char is at index 0
var count = 0;

while (count < str.length) {
    console.log(str[count]);
    count++;
}
```

**Avoid infinite loops (a loop where condition can't be false)**

### For loop

Syntax

```javascript
for (init; condition; step) {
    // code
}

for (var count = 1; count < 6; count++) { // variable can be initialized here, exists only inside of the loop; we can use count += 5 or anything
    console.log(count);
}
```

Printing each character in a string with a for loop

```javascript
var str = 'hello';

for (var i = 0; i < str.length; i++) {
    console.log(str[i]);
})
```
---

## Functions

Functions let wrap bits of code up into reusable packages, sort of a building block

Declare a function first

```javascript
function doSomething() {
    console.log('Doing something');
    console.log('Yes, I am doing something');
}
```

Then call it (parentheses are important)
```javascript
doSomething();
// Doing something
// Yes, I am doing something
```

### Arguments

Input for the function

```javascript
function square(num) {
    console.log(num*num);
}

square(10); // prints 100
```

Functions can have many arguments

```javascript
function volume(length, width, height) {
    console.log(length * width * height);
}
```

If an argument is missing, there will be no error message

```javascript
function greet(person1, person2, person3) {
    console.log('Hi ' + person1);
    console.log('Hi ' + person2);
    console.log('Hi ' + person3);
}

greet('Harry', 'Hermione');
// Hi Harry
// Hi Hermione
// Hi undefined
```

### Return keyword
Function does something with inputs and returns, sends back an output value\
Returns only one thing per function (if there are many returns, only the first one will work)

It is possible to store this value in a variable

```javascript
function square(x) {
    return x * x;
}

var result = square(4);
// result = 16
```

The return keyword stops execution of a function

```javascript
function capitalize(str) {
    if (typeof str === 'number') {
        return 'that\'s not a string!'
    }
    return str.charAt(0).toUpperCase() + str.slice(1);
}

var city = 'paris'; // 'paris'
var capital = capitalize(city); // 'Paris'

var num = 378;
var capital = capitalize(num); // 'that's not a string!
```

### Function Declaration vs Function Expression

Function declaration:

```javascript
function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}

// function functionName (argument) {}
```

Function expression:

```javascript
var capitalize = function(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}

// var variableName = function(argument) {} 
// doesn't require a name
```

In case of expression: if we decide to change 'capitalize' to be equal to something else, the function is lost

```javascript
var sayHi = function() {
    console.log('Hi');
}

// sayHi()
// Hi

sayHi = 35

// sayHi()
// sayHi is not a function
```

### Pure Functions

It is a functional programming concept

It is a function with no side effects
- It does not modify its inputs
- It does not modify any global state
- It is repeatable (same inputs -> same outputs)

```javascript
// not a pure function (the result of this function will keep changing our array)
function doubleVals(arr) {
    for (var i = 0; i < arr.length; i++) {
        arr[i] = arr[i] * 2;
    }
    return arr;
}

// pure function, we're not modifying an input since array.map returns a new array
function doubleVals(arr) {
  return arr.map(v => v * 2);
}

// not a pure function (it is modifying a global state, adds a property to the object)
var person = {id: 53, name: "Tim"};
function addJob(job){
    person.job = job;
}
addJob("Instructor");

// pure function
// we're taking that person object as a parameter, since that data is involved in what we're doing in the function, it should be a parameter to our function
// also, we're not modifying that person object directly, we're using Object.assign to create a new object which is a combination of the original object, and our new value that we want to add (job)
var person = {id: 53, name: "Tim"};
function addJob(personObj, job){
    return Object.assign({},
                        personObj,
                        {job});
}
addJob(person, "Instructor");

// another way to write that same function as a pure function (using object spread operator)
// spread operator takes all the keys and the values from that object, and puts it in this new object that we're creating
// any key to the right of that spread operator overwrites any key that may have been an object on the left
// if Person object in this case had a key called job, job would be overwritten, and if it didn't have a key called job then we would also have the new job
var person = {id: 53, name: "Tim"};
function addJob(personObj, job){
    return {...personObj, job}; // job is shorthand for job: job
}
addJob(person, "Instructor");
```

### Scope

Scope in JS refers to the context that some code is being executed in\
Scope determines which variables and properties are visible in the function

```javascript
function doMath() {
    var x = 40;
    console.log(x);
}

// doMath()
// 40
// x
// x is not defined
```

This example shows that there are two scopes: inside of the function, and outside (global scope, not inside of any function)\
X is visible only inside of the function doMath

If we define x and access it when we're outside of the function (in the global scope), we will get the defined value

```javascript
var x = 'hello';

// x
// 'hello'
```

But if we run function again, we will get the value returned by the function

```javascript
// doMath()
// 40
```

When we create a function, it has its own scope, its own set of variables\
But it doesn't mean that we can't use variables defined outside of the function inside of it

```javascript
var y = 99;

// y
// 99

function doMoreMath() {
    console.log(y);
}

// doMoreMath()
// 99
```

What this means is that 'child' scopes have access to variables defined in the 'parent' scope

Functions use previously declared, available variables, they do not declare another one in the child scope

```javascript
var y = 99; // y is declared in the global scope

// y
// 99

function doMoreMath() {
    y = 100; // y is changed inside of the function
    console.log(y);
}

// doMoreMath()
// 100
// y
// 100
```

This does not happen if we specifically declare a new variable inside of the function (even with the same name; it exists only in the function scope)

```javascript
var phrase = 'hi there';

// phrase
// 'hi there'

function doSomething() {
    var phrase = 'goodbye';
    console.log(phrase);
}

// doSomething()
// 'goodbye'
// phrase
// 'hi there'
```

### Higher Order Functions

Higher order functions are functions that either take a function as an argument or they return another function

```javascript
function sing() {
    console.log('Twinkle twinkle...');
    console.log('How I wonder...');
}

setInterval(sing, 1000); // (function (no parentheses since we are not the ones calling the function), interval in ms)
 
 // 2 (a (random?) number which is used to stop setInterval)
 // 'Twinkle twinkle...'
 // 'How I wonder...'
 // 'Twinkle twinkle...'
 // 'How I wonder...'

clearInterval(2); // number which was given by setInterval
```

Sometimes we want to run some code every second, but we don't want to define a separate function ahead of time

```javascript
setInterval(function() {
    console.log('Example of an anonymous function');
}, 2000);
```
---

## Arrays

Array is a data structure, a way of storing data, holding information using JS

Arrays can be initialized in 2 ways:

```javascript
var friends = []; // no friends
var friends = new Array() // uncommon way (a function which creates a new array)

var friend1 = 'Charlie';
var friend2 = 'Liz';
var friend3 = 'David';
var friend3 = 'Matt';

var friends = ['Charlie', 'Liz', 'David', 'Matt'];
```

Arrays can hold any type of data and have a length property

```javascript
var collection = [45, true, 'Cake', null];
collection.length // 4

var friendGroups = [
    ['Harry', 'Ron', 'Hermione'],
    ['Malfoy', 'Crabbe', 'Goyle'],
    ['Mooney', 'Wormtail', 'Prongs']
]

```

Arrays are indexed starting at 0, and every slot has a correspondent number

We can retreive or change data by index, or even add new data:

```javascript
console.log(friends[1]); // Liz
console.log(friendGroups[2][0]); // Mooney

friends[0] = 'Chuck'; // changes Charlie to Chuck

friends[4] = 'Amelia'; // adds Amelia to the array

friends[10] = 'Danny'; // items from 5 to 9 will be undefined
```

### Some array methods

Push is used to add to the end of an array\
Pop is used to remove the last item in an array

```javascript
var colors = ['red', 'green', 'blue'];
colors.push('black'); // 'red', 'green', 'blue', 'black'; and returns number of elements in the array
colors.pop(); // 'red', 'green', 'blue'; returns the removed element

var col = colors.pop(); // blue
```

Unshift is used to add to the front of an array\
Shift is used to remove the first item in an array

```javascript
var colors = ['red', 'green', 'blue'];
colors.unshift('infrared'); // 'infrared', 'red', 'green', 'blue'; returns number of elements in the array
colors.shift(); // 'red', 'green', 'blue'; returns the removed element

var col = colors.shift(); // red
```

IndexOf is used to find the index of an item in an array

```javascript
var friends = ['Charlie', 'Liz', 'David', 'Matt', 'Liz'];

// returns the first index at which a given element can be found
friends.indexOf('David'); // 2
friends.indexOf('Liz'); // 1, not 4
friends.indexOf('Hagrid'); // -1 (element is not present)
```

Slice is used to copy parts of an array

```javascript
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];

// use slice to copy the 2nd and 3rd fruits
// specify index where the new array starts (1) and ends (3);
var citrus = fruits.slice(1, 3); // citrus contains 'Orange', 'Lemon'

// this doesn't change the original fruits array

// you can also use slice() to copy an entire array
var nums = [1, 2, 3];
var moreNums = nums.slice();
// both arrays are [1, 2, 3]
```

Splice is used to remove specific number of elements out of an array

```javascript
var nums = [12, 24, 48, 56, 72];
nums.splice(3, 1); // (index, number of items to delete); returns the removed element(s)
// nums = [12, 24, 48, 72]
```

### Array iteration

Using a for loop to iterate over an array (dealing with number which is used to access the array)

```javascript
var colors = ['red', 'orange', 'blue', 'green'];

for (var i = 0; i < colors.length; i++) {
    console.log(colors[i]);
}
```

Using forEach to iterate over an array (more abstracted way)\
.forEach takes a callback function, that callback function is expected to have at least 1, but up to 3, arguments

The arguments are in a specific order:
- The first one represents each element in the array (per loop iteration) that .forEach was called on.
- The second represents the index of said element.
- The third represents the array that .forEach was called on (it will be the same for every iteration of the loop).

```javascript
arr.forEach(someFunction) // general syntax


var colors = ['red', 'orange', 'blue', 'green'];

colors.forEach(function() {
    console.log('Something')
}
// Something x4

// using an anonymous function
colors.forEach(function(iLoveColor) { // iLoveColor is a placeholder, it holds a value of an item in an array
    console.log('I love ' + iLoveColor);
});

// using pre-written, named function
function printColor(colorWithAsterisks) {
    console.log('*****');
    console.log(colorWithAsterisks)
    console.log('*****');
};

colors.forEach(printColor); // note that there are no () since every time there are () with function, it is called
// will print all the colors from an array with ***** between them
```

While loop can also be used, although we need a new variable count = 0 first\
 ``while (count < arr.length){};``

---

 ## Objects

 Another data structure

```javascript
// modelling a person
var person = ['Dan', 23, 'Moscow'];

// retrieving age
person[2];

// but what if the order is reversed or unknown?
// it is best to use an object

var person = {
    name: 'Dan',
    age: 23,
    city: 'Moscow'
};
```

There are two ways to retrieve a data: bracket and dot notation

```javascript
var person = {
    name: 'Dan',
    age: 23,
    city: 'Moscow'
};

// bracket notation (similar to arrays), but there is a string instead of a number
console.log(person['name']);

// dot notation (shorter and preferable, mostly)
console.log(person.name);
```

You cannot use dot notation in some cases:

```javascript
// you cannot use dot notation if the property starts with a number
someObject.1blah // INVALID
someObject['1blah'] // VALID

// you cannot use dot notation for property names with spaces
someObject.fav color // INVALID
someObject['fav color'] // VALID

// you cannot lookup using a variable with dot notation
var str = 'name';
someObject.str // doesn't look for 'name'
someObject[str] // does evalute str and looks for 'name'
```

We can update data by accessing a property and reassigning it

```javascript
var person = {
    name: 'Dan',
    age: 23,
    city: 'Moscow'
};

// updating age
person['age'] += 1;
// updating city
person.city = 'London';
```

Like arrays, there are a few methods of initializing objects

```javascript
// make an empty object and then add data to it
var person = {};

person.name: 'Dan';
person.age: 23;
person.city: 'Moscow';

// all at once (object literal notation)
var person = {
    name: 'Dan',
    age: 23,
    city: 'Moscow'
};

// another way of initializing an object
var person = new Object(); // function creating empty object
person.name: 'Dan';
person.age: 23;
person.city: 'Moscow';
```

Object can hold all sorts of data

```javascript
var someObject = {
    age: 52,
    color: 'purple',
    isHungry: true,
    friends: ['Harry', 'Ron'],
    pet: {
        name: 'Aragog',
        species: 'Spider',
        age: 36
    }
};
```

---

### Comparing arrays and objects

#### Array 
- It is a list of data with a specific order
- Syntax:
    - ``var colors = ['red', 'green', 'blue'];``
- Retrieving data
    - Need to know the exact index of an item
    - ``colors[2];``
    - We can think of an array as a special type of object where key is always a number
- Adding new data
    - ``colors.push('black');``
    - Many methods (push, unshift) exist only because the order of items is important
- Updating data
    - We need to find an item first and then change it
    - ``colors[1] = 'yellow';``

#### Object
- There is no particular order in objects
    - Key: value pairs
    - In some languages are called dictionaries (word - definition)
- Syntax:
    - ``var dog = {name: 'Bubba', breed: 'Lab'};``
    - Rarely written in a one line
- Retrieving data
    - Need to know the property
    - ``dog['name'];``
    - ``dog.breed;``
- Adding new data
    - ``dog.age = 6;``
    - Just assign any key and value that you want
- Updating data
    - Look for the property you need to change, and change it 
    - ``dog.breed = 'Black Lab';``

### Nested objects and arrays

```javascript
// example of blog posts
var posts = [
    {
        title: 'Cats are mediocre',
        author: 'Colt',
        comments: ['Amazing post', 'terrible post']
    },
    {
        title: 'Cats are awesome',
        author: 'Cat',
        comments: ['<3', 'I hate cats']
    }
]

posts.length // 2
posts[0].title // Cats are mediocre
posts[1].comments[1] // I hate cats
posts[1]['comments'][0] // <3
```

---

### Adding methods to an object

We can add functions as properties, and in that case they are called methods

```javascript
var obj = {
    name: 'Chuck',
    age: 45,
    isCool: false,
    friends: ['Bob', 'Tina'],
    add: function(x, y) {
        return x + y;
    }
}

add(); // won't work
obj.add(10, 5); // 15
```

The reasons for this are:
- keeping the code organized, to group things logically together
- to avoid namespace collision
    ```javascript
    function speak() {
        return 'Woof';
    }
    speak(); // Woof

    function speak() {
        return 'Meow';
    }
    speak(); // Meow

    // the first function is lost, there was a namespace collision, i.e. two things have the same name

    var dogSpace = {};
    dogSpace.speak = function speak() {
        return 'Woof';
    }
    var catSpace = {};
    catSpace.speak = function speak() {
        return 'Meow';
    }
    ```

### The Keyword This

'This' is the way to share the data inside of an object\
Used in different contexts to do different things

For example, 'this' when used in methods refers to an object that the method is defined in

```javascript
var comments = {};

comments.data = ['Good job', 'Not bad', 'Lame'];

// to print comments we could use something like this:
function print(arr) {
    arr.forEach(function(el) {
        console.log(el);
    });
}

print(comments.data); // prints comments from the 'data'

// instead we can write
comments.print = function () { // we remove argument from the function
    this.data.forEach(function(el) { // 'this' refers to the object 'comments'
        console.log(el);
    });
}

comments.print(); // prints comments from the 'data'
```

---