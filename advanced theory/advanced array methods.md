# Advanced Array Methods

#### Table of Contents

- [Advanced Array Methods](#advanced-array-methods)
      - [Table of Contents](#table-of-contents)
  - [forEach](#foreach)
  - [map](#map)
  - [filter](#filter)
  - [some](#some)
  - [every](#every)
  - [reduce](#reduce)

## forEach

- Iterates through an array
- Runs a callback function on each value in the array
- Returns 'undefined' ALWAYS when the loop ends (i.e. storing in a var or returning in a function won't work)

```javascript
// array  method  callback each value&index entire
//                            in the arr   array
[1, 2, 3].forEach(function(value, index, array) {
  // the callback will be executed 3 times since there are 3 values in the array
  // each time value and index are different
  // we can call parameters whatever we want
  // we don't always need all three parameters, but their order is important
});

// implementation of forEach
function forEach(array, callback) {
  for (var i = 0; i < array.length; i++) {
    callback(array[i], i, array);
  }
}

// using forEach
function halfValues(arr) {
  var newArr = [];
  arr.forEach(function(val) {
    newArr.push(val / 2);
  });
  return newArr;
}

halfValues[2, 4, 6]; // [1, 2, 3]
```

## map

Basically it is forEach which transforms an array into another array with different values but of the same length

- creates a new array
- iterates through an array
- runs a callback function for each value in the array
- adds the result of that callback function to the new array
- returns the new array

```javascript
var arr = [1, 2, 3];

arr.map(function(value, index, array) {
  return value * 2; // if no return, there is an array of undefined's
});

// [2, 4, 6]

// implementation
function map(arr, callback) {
  var newArr = [];
  for (var i = 0; i < arr.length; i++) {
    newArr.push(callback(arr[i], i, arr));
  }
  return newArr;
}

// usage
// we could use forEach here, but map is a bit more friendly since it already returns a new array to us
// forEach can be used if we want to overwrite values in an array or change something externally
// when we would like a new array to be returned, especially one of the same length, map should always be used
function tripleValues(arr) {
  return arr.map(function(value) {
    return value * 3;
  });
}

tripleValues([1, 2, 3]); // [3, 6, 9]

function onlyFirstName(arr) {
  return arr.map(function(val) {
    return val.first;
  });
}

onlyFirstName([{first: 'Tim', last: 'Garcia'}, {first: 'Matt', last: 'Lane'}]);

// ['Tim', 'Matt']
```

## filter

.filter helps to return arrays of the same or smaller size

- creates a new array
- iterates through an array
- runs a callback function on each value in the array
- if the callback function returns trues, that value will be added to the new array
- if the callback function returns false, that value will ve ignored from the new array
- the result of the callback will ALWAYS be a boolean

```javascript
var arr = [1, 2, 3];

arr.filter(function(value, index, array) {
  // no need for an if statement, just return an expression that evaluates to true or false
  return value > 2;
});

// [3]

var instructors = [
  {name: 'Elie'},
  {name: 'Tim'},
  {name: 'Colt'},
  {name: 'Matt'}
];

instructors.filter(function(value, index, array) {
  return value.name.length > 3;
});

// [{name: 'Elie'}, {name: 'Colt'}, {name: 'Matt'}];

// implementation
function filter(array, callback) {
  var newArr = [];
  for (var i = 0; i < array.length; i++) {
    if (callback(array[i], i, array)) {
      newArr.push(array[i])l
    }
  }
  return newArr;
}

// usage
function onlyFourLetters(arr) {
  return arr.filter(function(value) {
    return value.length === 4;
  });
}

onlyFourLetters(['Rusty', 'Matt', 'Moxie', 'Colt']); // ['Matt', 'Colt']

function divisibleByThree(arr) {
  return arr.filter(function(value) {
    return value % 3 === 0;
  });
}

divisibleByThree([1, 2, 3, 4, 5, 6, 7, 8, 9]); // [3, 6, 9]
```

## some

- iterates through an array
- runs a callback on each value in the array
- if the callback returns true for at least one single value, return true
- otherwise, return false
- the result of the callback will ALWAYS be a boolean

```javascript
var arr = [1, 2, 3];

arr.some(function(value, index, array) {
  return value < 2;
});

// true

arr.some(function(value, index, array) {
  return value > 4;
});

// false

// implementation
function some(array, callback) {
  for (var i = 0; i < array.length; i++) {
    if (callback(array[i], i, array)) {
      return true;
    }
  }
  return false;
}

// usage
function hasEvenNumber(arr) {
  return arr.some(function(value) {
    return value % 2 === 0;
  });
}

hasEvenNumber([1, 2, 3, 4]); // true
hasEvenNumber([1, 3, 5]); // false

function hasComme(str) {
  return str.split('').some(function(value) {
    return value === ',';
  });
}

hasComma('This is wonderful'); // false
hasComma('This, is wonderful'); // true
```

## every

- iterates through an array
- runs a callback on each value in the array
- if the callback returns true for every single value, return false
- otherwise, return true
- the result of the callback will ALWAYS be a boolean

```javascript
var arr = [-1, -2, -3];

arr.every(function(value, index, array) {
  return value < 0;
});

// true

var arr = [-1, 2, -3];

arr.every(function(value, index, array) {
  return value < 0;
});

// false

// implementation
function every(array, callback) {
  for (var i = 0; i < array.length; i++) {
    if (callback(array[i], i, array) === false) {
      return false;
    }
  }
  return true;
}

// usage
function allLowerCase(str) {
  return str.split('').every(function(value) {
    return value === value.toLowerCase();
  });
}

allLowerCase('this is really nice'); // true
allLowerCase('this is Really nice'); // false

function allArrays(arr) {
  return arr.every(Array.isArray);
}

allArrays([[1], [2], [3, 4]]); // true
arrArrays([[1], [2], {}]); // false
```

## reduce

- accepts a callback and an optional second parameter
- iterates through an array
- runs a callback on each value in the array
- the first parameter to the callback is either the first value in the array or the optional second parameter
- the first parameter to the callback is often called 'accumulator'
- the returned value from the callback becomes the new value of accumulator
- whatever is returned from the callback function, becomes the new value of the accumulator

```javascript
[1, 2, 3].reduce(function(accumulator, nextValue, index, array) {
  // [1, 2, 3] - array
  // reduce - method
  // function - callback
  // accumulator - first value in array or optional second parameter
  // second value in array or first, if optional second parameter is passed
  // index - each index in the array
  // array - the entire array

  // whatever is returned inside here, will be the value of accumulator in the next iteration
}, optional second parameter)

// example
var arr = [1, 2, 3, 4, 5];

arr.reduce(function(accumulator, nextValue) {
  return accumulator + nextValue;
});

// accumulator  nextValue  returned value
//      1           2           3
//      3           3           6
//      6           4           10
//      10          5           15

// adding a second parameter
var arr = [1, 2, 3, 4, 5];

arr.reduce(function(accumulator, nextValue) {
  return accumulator + nextValue;
  // not returning - undefined
}, 10);

// accumulator  nextValue  returned value
//      10          1           11
//      11          2           13
//      13          3           16
//      16          4           20
//      20          5           25
```

Using reduce to return different data types

```javascript
var names = ['Tim', 'Matt', 'Colt', 'Elie'];

names.reduce(function(accumulator, nextValue){
  return accumulator += ' ' + nextValue;
}, 'The instructors are');

// accumulator               nextValue       returned value
// The instructors are          Tim       The instructors are Tim
// The instructors are Tim      Matt      The instructors are Tim Matt
//                              ...
// The instructors are Tim Matt Colt Elie

var arr = [5, 4, 1, 4, 5];

arr.reduce(function(accumulator, nextValue) {
  if (nextValue in accumulator) {
    accumulator[nextValue]++;
  } else {
    accumulator[nextValue] = 1;
  }
  return accumulator;
}, {});

// {5: 2, 4: 2, 1: 1}
```

Usage of reduce

```javascript
function sumOddNumbers(arr) {
  return arr.reduce(function(accumulator, nextValue) {
    if (nextValue % 2 !== 0) {
      accumulator += nextValue;
    }
    return accumulator;
  }, 0);
}

sumOddNumbers([1, 2, 3, 4, 5]); // 9

function createFullName(arr) {
  return arr.reduce(function(accumulator, nextValue) {
    accumulator.push(nextValue.first + ' ' + nextValue.last);
    return accumulator;
  }, []);
}

createFullName([{first: 'Colt', last: 'Steele'}, {first: 'Matt', last: 'Lane'}]);
// ['Colt Steele', 'Matt Lane']
```