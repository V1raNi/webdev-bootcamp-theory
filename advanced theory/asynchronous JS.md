# Asynchronous JS

Asynchronous events are those occurring independently of the main program flow (Don't call us, we'll call you)

#### Table of Contents

- [Asynchronous JS](#asynchronous-js)
      - [Table of Contents](#table-of-contents)
  - [Callback Functions](#callback-functions)
    - [forEach](#foreach)
    - [findIndex](#findindex)
  - [The Stack and the Heap](#the-stack-and-the-heap)
    - [Call Stack](#call-stack)
    - [Heap](#heap)
    - [Stack Example](#stack-example)
  - [setTimeout and setInterval](#settimeout-and-setinterval)
    - [setTimeout](#settimeout)
    - [setInterval](#setinterval)
  - [The Event Loop and the Queue](#the-event-loop-and-the-queue)
      - [The Queue](#the-queue)
      - [The Event Loop](#the-event-loop)
      - [Queue Example](#queue-example)
      - [Queue Example: Waiting for the Stack to Empty](#queue-example-waiting-for-the-stack-to-empty)
      - [JavaScript Is Single Threaded](#javascript-is-single-threaded)
  - [Promises](#promises)
      - [Creating a Promise](#creating-a-promise)
      - [.then and .catch](#then-and-catch)
      - [Handling Errors](#handling-errors)
      - [Randomly Occuring Errors](#randomly-occuring-errors)
      - [setTimeout and Promises](#settimeout-and-promises)
      - [Asynchronous Handlers of .then and .catch](#asynchronous-handlers-of-then-and-catch)
    - [Promise chaining](#promise-chaining)
      - [Nested Async Callbacks](#nested-async-callbacks)
      - [Returing a Promise: Promise Chaining](#returing-a-promise-promise-chaining)
      - [Promise Chaining: Returning Data](#promise-chaining-returning-data)
      - [Nested Callbacks: Refactored](#nested-callbacks-refactored)
      - [Promise.all](#promiseall)
      - [Promises in Practice](#promises-in-practice)

## Callback Functions

It is a function that is passed into another function as a parameter then invoked by that other function
- Higher order function is a function that accepts a callback as a parameter

```javascript
function callback() {
  console.log('coming from callback');
}

function higherOrder(fn) {
  console.log('about to call callback');
  fn(); // callback function is invoked
  console.log('callback has been invoked');
}

higherOrder(callback);
```

Callbacks are used for:
- Advanced array methods
- Browser events
- AJAX requests
- React development

```javascript
function sendMessageConsole(message) {
  console.log(message);
}

function sendMessageAlert(message) {
  alert(message);
}

function sendMessageConfirm(message) {
  return confirm(message);
}

sendMessageAlert("Lots of duplication");

// lots of duplication, instead: 
function sendMessage(message, callback) {
  return callback(message);
}

sendMessage("Message for console", console.log);

sendMessage("Message for alert", alert);

var answer = sendMessage("Are you sure??", confirm);

// another example
function greet(name, formatter) {
  return "Hello, " + formatter(name);
}

function upperCaseName(name) {
  return name.toUpperCase();
}

greet("Tim", upperCaseName);
```

The most often scenario is to use anonymous functions:

```javascript
function greet(name, formatter) {
  return "Hello, " + formatter(name);
}

greet("Tim", function(name) {
  return name.toUpperCase();
});
```

### forEach

This function is used to iterate over items in an array

```javascript
var arr = [1,2,3,4,5,6];
function double(arr) {
  for(var i = 0; i < arr.length; i++) {
    console.log(arr[i] * 2);
  }
}
double(arr);

// with forEach
var arr = [1,2,3,4,5,6];
forEach(arr, function(number) {
    console.log(number * 2);
});
```

Syntax:

```javascript
function forEach(array, callback) {
  // To be implemented
}

// Callback signature
function callback(curElement, currentIndex, array) {
  // Implemented by the caller of forEach
}

// Example with all parameters:
var strings = ["my", "forEach", "example"];
var result = "";

forEach(strings, function(str, index, array) {  
  if (array.length - 1 !== index){
    result += str + " ";
  } else {
    result += str + "!!!";
  }
});

// result = "my forEach example!!!"
```

### findIndex

Returns the index of the first element in the array for which the callback returns a truthy value
- -1 is returned if the callback never returns a truthy value

Syntax:

```javascript
function findIndex(array, callback) {
  // findIndex code to be implemented
}

function callback(curElement, curIndex, array) {
  // callback implemented by caller of function
}
```

Examples:

```javascript
// Find A Number
var arr = [3,4,6,2,1];
findIndex(arr, function(num, index, array) {
  return num === 6;
});

// result: 2

// Find First Even
var arr = [5,11,13,8,6,7];
findIndex(arr, function(num, index, array) {
  return num % 2 === 0;
});

// result: 3

// Could Not Find
var langs = ["Java", "C++", "Python", "Ruby"];
findIndex(langs, function(lang, index, arr) {
  return lang === "JavaScript";
});

// result: -1

// Bad Callback (no return)
var langs = ["Java", "C++", "JavaScript"];
findIndex(langs, function(lang, index, arr) {
  lang === "JavaScript";
});

// result: -1
```

---

## The Stack and the Heap

### Call Stack

Stack is an ordered data structire which keeps track of function invocations; it's a part of JS runtime (we don't access it directly)
- Every time we invoke a function, the stack gets modified to keep track of that function; when the function is finished running, it'll be popped off the stack, and JS runtime will take it off for us


Process:
- Whenever we invoke a function, the details of the invocation are saved to the top of the stack (pushed to the top)
- Whenever a function returns, the information about the invocation is taken off the top of the stack (popped off of the top)

Example:

```javascript
1 function multiply(x, y) {
2   return x * y;
3 }
4
5 var res = multiply(3, 5);

// before anything is executed JS runtime will place the main program on the stack

// stack: 


// 5 function: main

// multiply is being invoked, JS runtime places multiply on top

// stack:

// 2 function: multiply(3, 5) (stack frame)
// 5 function: main (stack frame)

// after it's done multiplying, JS runtimes pops it off

// stack: 


// 5 function: main

// our main function is still on the stack, once we assigned the result to 15, our code is all done, JS runtime takes main off the stack as well

// stack empty
```

Stack frame contents:
- The function that was invoked
- The parameters that were passed to the function
- Current line number

So, stack is:
- An ordered set of stack frames
- Most recently invoked function is on the top of the stack
- The bottom of the stack is the first function invoked
- The stack is processed from top to bottom

### Heap

Heap is an area in memory where our data is stored

```javascript
// The object is created in the heap. 
// obj is a reference to the object.
var obj = {firstName: "Tim", lastName: "Garcia"};

// New data is not created, it is only a copy of the reference (same area in memory, if value in referenceCopy is changed, it is chanjed in obj as well)
var referenceCopy = obj;
```

### Stack Example

```javascript
1  function upperCaseFirst(word) {
2    return word[0].toUpperCase() + word.slice(1);
3  }
4 
5  function upperCaseWords(sentence) {
6    var words = sentence.split(" ");
7    for (var i = 0; i < words.length; i++) {
8      words[i] = upperCaseFirst(words[i]);
9    }
10   return words.join(" ");
11 }
12
13 upperCaseWords("lowercase words");
```

Stack:
- First, we get a main function to the stack
  - 13 function: main (waiting on line 13 for something to do)
- When upperCaseWords is invoked, we are placing it on the stack (now we are inside of this function, and have passed in 'lowercase words', moving to line 6)
  - 6 function: upperCaseWords
  - 13 function: main
- On line 6 we're invoking 'split' function (it'll take our sentence and break into an array of words separating them by ' ')
  - ? function: split
  - 6 function: upperCaseWords
  - 13 function: main
- Split is done, it comes off the stack, 'words' is assigned that new array, and we move on to the next line of code
  - 8 function: upperCaseWords
  - 13 function: main
- On line 8, we're going to invoke upperCaseFirst with the current element that we're on here
  - 2 function: upperCaseFirst
  - 8 function: upperCaseWords
  - 13 function: main
- Now upperCaseFirst is on the stack, we're on line 2, we have the word 'lowercase'; the first letter of our word is going to be uppercased, so 'l' -> 'L', and that causes a function invokation, putting toUpperCase on top of the stack:
  - ? function: toUpperCase
  - 2 function: upperCaseFirst
  - 8 function: upperCaseWords
  - 13 function: main
- toUpperCase does its thing, returns 'L', and gets taken off the stack
  - 2 function: upperCaseFirst
  - 8 function: upperCaseWords
  - 13 function: main
- Next, we have to invoke slice which returns the string minus the first character
  - ? function: slice
  - 2 function: upperCaseFirst
  - 8 function: upperCaseWords
  - 13 function: main
- Slice is done, it has returned the new string, and gets popped off the stack
  - 2 function: upperCaseFirst
  - 8 function: upperCaseWords
  - 13 function: main
- Now we have the first letter and the rest of the string, we're gonna concatenate them together, and upperCaseFirst is now finished (we're back on line 8, the previous stack frame, waiting for the result)
  - 8 function: upperCaseWords
  - 13 function: main
- words[i] is equal to the result of invoking that function 'Lowercase', and then the process is repeated
  - 8 function: upperCaseWords
  - 13 function: main
- We move on to line 10, where we're taking the 'words' array of capitalized words and then join them together with ' ' (takes an array, converts back into string with spaces in between them)
  - ? function: join
  - 10 function: upperCaseWords
  - 13 function: main
- Then join gets popped off the stack, now on line 10 we have our result
  - 10 function: upperCaseWords
  - 13 function: main
- We return the result, and upperCaseWords is done
  - 13 function: main
- Our main function finishes up
  - empty

---

## setTimeout and setInterval

These functions facilitate asynchronous code

### setTimeout

A function that asynchronously invokes a callback after a delay in milliseconds

```javascript
// setTimeout usage
function callback() {
  console.log("callback function");
}
var delay = 1000;  // Delay is in ms
setTimeout(callback, delay);

// with anonymous function
setTimeout(function() {
  console.log("Runs in approx. 2000ms");
}, 2000);

// cancelling
// every time we invoke setTimeout, we get back an id for that timeout

var timerId = setTimeout(function() {
  console.log("This function runs in 30 seconds");
}, 30000);
// this will never actually run because the second setTimeout will be invoked first, and it will clear the first setTimeout

setTimeout(function() {
  console.log("Canceling the first setTimeout", timerId);
  clearTimeout(timerId);
}, 2000);

// Canceling the first setTimeout 42 (some integer number that references the first setTimeout)
```

### setInterval

A function that continually invokes a callback after every X milliseconds, where X is provided to setInterval

```javascript
// setInterval usage
function callback() {
  console.log("callback is called continuously");
}
var repeat = 3000;
setInterval(callback, repeat);

// example
var num = 0;
setInterval(function() {
  num++;
  console.log("num:", num);
}, 1000);

// num: 1
// num: 2
// num: 3

// cancelling
var num = 0;
var intervalId = setInterval(function() {
  num++;
  console.log("num:", num);
  if (num === 2) {
    clearInterval(intervalId);
  }
}, 1000);

// num: 1
// num: 2
```

## The Event Loop and the Queue

#### The Queue

An ordered list of functions waiting to be placed on the stack

Functions in the queue are processed on a first in, first out basis (FIFO)

#### The Event Loop

Functionality in the JavaScript runtime that checks the queue when the stack is empty

If the stack is empty, the front of the queue is placed in the stack

#### Queue Example

```javascript
1 setTimeout(function() {
2   console.log('Hello World');
3 }, 0);
```

Console.log will NOT happen immediately, it runs after the stack is empty:
- We have our main function
  - Stack:
    - 1 function: main
  - Queue:
    - | --- | | --- | | --- | | --- |
- It invokes our setTimeout (it takes our callback function that we want to run, and places it in the queue (here it places it after 0 sec))
  - Stack:
    - ? function: setTimeout
    - 1 function: main
  - Queue:
    - |function()| | --- | | --- | | --- |
- setTimeout is popped off
  - Stack:
    - 1 function: main
  - Queue:
    - |function()| | --- | | --- | | --- |
- Main function is finished
  - Stack:
    - empty
  - Queue:
    - |function()| | --- | | --- | | --- |
- Event loop checks if there is anything in the queue, if there is, it takes it out of the queue and places in the stack
  - Stack:
    - 2 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- console.log is placed on the stack as another function that gets invoked
  - Stack:
    - ? function: console.log
    - 2 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- console.log comes off the stack
  - Stack:
    - 2 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- Callback function finishes up
  - Stack:
    - empty
  - Queue:
    - | --- | | --- | | --- | | --- |

#### Queue Example: Waiting for the Stack to Empty

```javascript
1 function square(n) {
2   return n * n;
3 }
4 setTimeout(function() {
5   console.log("Callback is placed", "on the queue");
6 }, 0);
7 console.log(square(2));
```

- Main function is on the stack
  - Stack:
    - 4 function: main
  - Queue:
    - | --- | | --- | | --- | | --- |
- Invoking setTimeout, it's placed on the stack
  - Stack:
    - ? function: setTimeout
    - 4 function: main
  - Queue:
    - | --- | | --- | | --- | | --- |
- setTimeout places that function on the queue
  - Stack:
    - ? function: setTimeout
    - 4 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- setTimeout finished and comes off of the stack
  - Stack:
    - 4 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- We move on to line 7, the first thing we have to do is square number, so, we're invoking square
  - Stack:
    - 2 function: square
    - 7 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- We calculate 2*2, return 4, and square comes off of the stack (we're back on line 7)
  - Stack:
    - 7 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- We put console.log on the stack
  - Stack:
    - ? function: console.log
    - 7 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- It adds 4 to the console, and comes off of the stack
  - Stack:
    - 7 function: main
  - Queue:
    - | function() | | --- | | --- | | --- |
- Main function comes off of the stack
  - Stack:
    - empty
  - Queue:
    - | function() | | --- | | --- | | --- |
- Event loop takes the function() and places it on the stack
  - Stack:
    - 5 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- console.log from callback is placed on the stack
  - Stack:
    - ? function: console.log
    - 5 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- 'Callback was placed on the queue' is printed, and console.log is taken off the stack
  - Stack:
    - 5 function: function
  - Queue:
    - | --- | | --- | | --- | | --- |
- Callback function is taken off the stack
  - Stack:
    - empty
  - Queue:
    - | --- | | --- | | --- | | --- |

#### JavaScript Is Single Threaded

Single Threaded: Code execution is linear; code that is running cannot be interrupted by something else going on in the program

```javascript
setTimeout(function() {
  console.log("Hello from the timeout");
}, 0);
for (var i = 0; i < 1000000000; i++) {
  var x = i * 2;
}
console.log("Done with loop");

// Done with loop
// Hello from the timeout
```

## Promises

A promise is an object that represents a task that will be completed in the future

Analogy: taking a number at a government office before you can get helped
- The piece of paper you get is like your promise
- The help you get at the counter is like the invocation of your callback

Another analogy: you are a singer, and your fans are waiting for your new song; you offer them to subscribe for updates so when the song is available or is cancelled, they receive notifications

There are 3 parts:
- A “producing code” that does something and takes time. For instance, the code loads a remote script. That’s a “singer”.
- A “consuming code” that wants the result of the “producing code” once it’s ready. Many functions may need that result. These are the “fans”.
- A promise is a special JavaScript object that links the “producing code” and the “consuming code” together.
- In terms of our analogy: this is the “subscription list”. The “producing code” takes whatever time it needs to produce the promised result, and the “promise” makes that result available to all of the subscribed code when it’s ready.

#### Creating a Promise

```javascript
var p1 = new Promise(function(resolve, reject) {
  resolve([1,2,3,4]);
});
// this function inside is called executor
// resolve and reject — these functions are pre-defined 

// When the promise is created, this executor function runs automatically. It contains the producing code, that should eventually produce a result

p1.then(function(arr) {
  console.log("Promise p1 resolved with data:", arr);
});

// Promise p1 resolved with data: [1,2,3,4]
```

The resulting promise object has internal properties:
- state — initially "pending", then changes to either "fulfilled" or "rejected"
- result — an arbitrary value of your choosing, initially undefined

When the executor finishes the job, it should call one of the functions that it gets as arguments:
- resolve(value) — to indicate that the job finished successfully:
  - sets state to "fulfilled"
  - sets result to value
- reject(error) — to indicate that an error occurred:
  - sets state to "rejected"
  - sets result to error
- The properties state and result of the Promise object are internal. We can’t directly access them from our "consuming code"


#### .then and .catch

Consuming functions can be registered (subscribed) using the methods .then and .catch which are functions to be executed when the promise is resolved or rejected
- Each .then returns another promise with .then method
  - it is called 'thenable'
- The first argument of .then is a function that:
  - runs when the Promise is resolved 
  - receives the result
  - i.e. it accepts a callback function with the value passed to the resolve function
- The second argument of .then is a function that:
  - runs when the Promise is rejected
  - receives the error
  - i.e. it accepts a callback function with the value passed to the reject function
- If we’re interested only in successful completions, then we can provide only one function argument to .then
- If we’re interested only in errors, then we can use null as the first argument: .then(null, errorHandlingFunction)
  - Or we can use .catch(errorHandlingFunction), which is exactly the same
- If a promise is pending, .then/catch handlers wait for the result
  - Otherwise, if a promise has already settled, they execute immediately
- Handlers of .then/.catch are always asynchronous
  - Even when the Promise is immediately resolved, code which occurs on lines below your .then/.catch may still execute first

#### Handling Errors

```javascript
var p1 = new Promise(function(resolve, reject) {
  reject("ERROR");
});

p1.then(function(data) {
  console.log("Promise p1 resolved with data:", data);
}).catch(function(data) {
  console.log("Promise p1 was rejected with data:", data);
});

// Promise p1 was rejected with data: ERROR

// If a promise is pending, .then/catch handlers wait for the result. Otherwise, if a promise has already settled, they execute immediately
```

#### Randomly Occuring Errors

```javascript
var p1 = new Promise(function(resolve, reject) {
  var num = Math.random();
  if (num < 0.5) {
    resolve(num);
  } else {
    reject(num);
  }
});

p1.then(function(result) {
  console.log("Success:", result);
}).catch(function(error) {
  console.log("Error:", error);
});

// There can be only a single result or an error
// The executor should call only one resolve or reject; the promise’s state change is final

// All further calls of resolve and reject are ignored
```

#### setTimeout and Promises

setTimeout
- an asynchronous function that runs a callback function after a specified amount of time
Promises 
- a tool to manage asynchronous code (as opposed to callbacks); so to manage one or many asynchronous functions (like setTimeout or AJAX calls), we can use promises

Wrap setTimeout with promise:

```javascript
var promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    var randomInt = Math.floor(Math.random() * 10);
    resolve(randomInt);
  }, 4000);
});
// The executor is called automatically and immediately (by the new Promise)
// Although 'resolve' code won't have finished running, we will have an object already to attach our callback to (promise will be created before it resolves)

promise.then(function(data) {
  console.log("Random int passed to resolve:", data);
});

// This promise object will immediately be returned even though the setTimeout is not done, and resolve has not been called
// We can decide what callback functions we want to add to .then
```

#### Asynchronous Handlers of .then and .catch

Even when the Promise is immediately resolved, code which occurs on lines below your .then/.catch may still execute first
- The JavaScript engine has an internal execution queue which gets all .then/catch handlers
- But it only looks into that queue when the current execution is finished
- In other words, .then/catch handlers are pending execution until the engine is done with the current code

```javascript
// an "immediately" resolved Promise
const executor = resolve => resolve("done!");
const promise = new Promise(executor);

promise.then(alert); // this alert shows last (*)

alert("code finished"); // this alert shows first
```

The promise becomes settled immediately, but the engine first finishes the current code, calls alert, and only afterwards looks into the queue to run .then handler

So the code after .then ends up always running before the Promise’s subscribers, even in the case of an immediately-resolved Promise

Usually that’s unimportant, but in some scenarios the order may matter a great deal

### Promise chaining

Since a promise always returns something that has a .then (thenable) - we can chain promises together and return values from one promise to another

#### Nested Async Callbacks

```javascript
// Nested Async Callbacks
var counter = 0;
setTimeout(function() {
  counter++;
  console.log("Counter:", counter);
  setTimeout(function() {
    counter++;
    console.log("Counter:", counter);
    setTimeout(function() {
      counter++;
      console.log("Counter:", counter);
    }, 3000);
  }, 2000);
}, 1000);

// Counter: 1
// Counter: 2
// Counter: 3
```

Disadvantages of nested callbacks:
- The code is hard to read
- Logic is difficult to reason about
- The code is not modular

#### Returing a Promise: Promise Chaining

Promise chaining allows multiple .then functions to be chained to a promise, and if a previous callback inside .then returns a promise, the next .then will be waiting for that promise to be resolved

```javascript
var promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    randomInt = Math.floor(Math.random() * 10);
    resolve(randomInt);
  }, 500);
});

// first .then will be waiting for that promise above to finish
promise.then(function(data) {
  console.log("Random int passed to resolve:", data);
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve(Math.floor(Math.random() * 10));
    }, 3000);
  });
}).then(function(data) {
  // this .then will be waiting for the returned promise above to finish
  console.log("Second random int passed to resolve:", data);
});
```

#### Promise Chaining: Returning Data

```javascript
var promise = new Promise(function(resolve, reject) {
  resolve(5);
});

promise.then(function(data) {
  return data * 2;
}).then(function(data) {
  return data + 20;
}).then(function(data) {
  console.log(data);
});

// 30 (5 * 2 and then 10 + 20)
```

#### Nested Callbacks: Refactored

```javascript
// create a function declaration (for a counter)
var counter = 0;
function incCounter() {
  counter++;
  console.log("Counter:", counter);
}

// create a runLater function which wraps setTimeout in a promise
function runLater(callback, timeInMs) {
  var p = new Promise(function(resolve, reject) {
    setTimeout(function() {
      var res = callback();
      resolve(res);
    }, timeInMs);
  });
  return p;
}

// chain promises
runLater(incCounter, 1000).then(function() {
  return runLater(incCounter, 2000);
}).then(function() {
  return runLater(incCounter, 3000);
}).then(function() {
  // final .then not necessary
});
```

#### Promise.all

It is a method on a promise constructor

- Accepts an array of promises and resolves all of them or rejects once a single one of the promises has been first rejected (fail fast behavior)
- If all of the passed-in promises fullfill, Promise.all is resolved with an array of the values from the passed-in promises, in the same order as the promises passed in to Promise.all
- Promise.all doesn't guarantee that these promises will resolve sequentially, that it will return an array of resolved promises in the same order that you pass them in to the function
  - Something like this happens with $.when in jQuery or Q
- The promises don't resolve sequentially, but Promise.all waits for them to resolve

```javascript
function getMovie('title'){
  return $.getJSON(`https://omdbapi.com?t=${title}&apikey=thewdb`);
}

// value of these variables is a pending promise, it has not been rejected or resolved, there is simply a promise for some future value
var titanicPromise = getMovie('titanic');
var shrekPromise = getMovie('shrek');
var braveheartPromise = getMovie('braveheart');

// instead of chaining all of these promises together with .then and another .then and another .then, we can simply place them all inside of a Promise.all invocation
Promise.all([titanicPromise, shrekPromise, braveheartPromise]).then(function(movies) {
  return movies.forEach(function(value) {
    console.log(value.Year);
  });
});

// 1997
// 2001
// 1995
```

#### Promises in Practice

It is useful to understand how promises work (resolve, reject), but in practice we will often use promises that are returned to you (instead of creating ones)