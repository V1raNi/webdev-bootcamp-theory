# Testing Basics

Unit tests test parts of an application (units)

Very commonly each unit is tested individually and independently to ensure that application is running as expected

What we need for testing:
- A framework to write tests
- A way of describing the code we are testing
- A tool where we can make assertions or expectations about our code

#### Table of Contents

- [Testing Basics](#testing-basics)
      - [Table of Contents](#table-of-contents)
  - [TDD and BDD](#tdd-and-bdd)
    - [Test Driven Development](#test-driven-development)
    - [Behavior Driven Development](#behavior-driven-development)
  - [Types of Tests](#types-of-tests)
    - [Unit Tests](#unit-tests)
    - [Integration Tests](#integration-tests)
    - [Acceptance Tests](#acceptance-tests)
    - [Stress Testing](#stress-testing)
  - [Jasmine](#jasmine)
    - [Jasmine Syntax and Matchers](#jasmine-syntax-and-matchers)
      - [Matchers](#matchers)
    - [Writing Better Tests with Hooks](#writing-better-tests-with-hooks)
      - [beforeEach](#beforeeach)
      - [afterEach](#aftereach)
      - [beforeAll/afterAll](#beforeallafterall)
    - [Testing Larger Test Suites](#testing-larger-test-suites)
      - [Nesting Describe](#nesting-describe)
      - [Pending Tests](#pending-tests)
      - [Number of Expect's](#number-of-expects)
    - [Mocking](#mocking)
    - [Spies](#spies)
      - [Creating a Spy](#creating-a-spy)
      - [Testing Parameters](#testing-parameters)
      - [Returning a Value](#returning-a-value)
        - [Why Use a Spy](#why-use-a-spy)
      - [Testing frequency](#testing-frequency)
    - [Clock](#clock)
      - [setTimeout](#settimeout)
      - [setInterval](#setinterval)
      - [Testing Async Code](#testing-async-code)
  - [Mocha](#mocha)

## TDD and BDD

### Test Driven Development

The idea of TDD is that we write test before we write our app code

This is a contrast to other testing methodologies which involve writing code and then testing it

With TDD we follow a pattern called 'Red Green Refactor'
1. Write the tests
2. Since we don't have any code yet, these tests sould fail after we see the tests fail (red)
3. Write the code necessary to pass the tests (green)
4. Refactor code as necessary

As long as the tests are still passing we can be reasonably confident that we aren't introducing new bugs into our program 

As our applications grow larger and we add more features, we will still have a solid suite of tests which ensure that we are not adding additional bugs to our application

The development process may be a bit longer with writing tests first the code written is much more likely to be bug free and maintainable as the code base grows

### Behavior Driven Development

Jasmine is a behavior driven javacript framework (BDD)

BDD is a subset of TDD; the two ideas are not mutually exclusive - we can have BDD without TDD and vice versa

The idea behind behavior driven development or BDD is that when writing tests we describe the behavior of our functionality and not just what we expect the result to be

When we use functions like 'describe' and 'it', the first parameter to each of these is a string for how we want to describe the behavior of what we are testing

BDD involves being verbose with our style and describing the behavior of our functionality (this is helpful when testing the design of some software instead of just testing or describing the expected output we can be much more descriptive in our tests)

## Types of Tests

### Unit Tests

These are tests which are written for one small component or unit of our application, they are meant to test the individual pieces of the app

Unit tests are written for the purpose of proving that the units or parts of our application behave as expected before they're put together

### Integration Tests

Having passed the unit tests, our application may fail when our units are combined

This leads us to integration testing which is meant to test the integration of our units or larger parts of our application; here we test more than one unit and how they function together

Integration testing builds off of unit testing

### Acceptance Tests

Acceptance testing involves performing tests on the full system which could be using your application in the browser or on a device to see whether the application's functionality satisfies a specification provided; here we are simply testing to see if something is acceptable or not

The purpose of acceptance tests is to evaluate the entire business or system requirements, not to test how one unit or how multiple units are integrated together


### Stress Testing

The idea behind stress testing is to determine how effective our applications can be under unfavorable conditions; these conditions include systems going down, high traffic, or other types of scenarios that may not be
common but can happen to an application

## Jasmine

It is a testing framework which works with all kinds of JS environments (browser, node.js) and has a simple syntax (and even has an interface)
- Tests are usually not written inside of HTML, instead we have a couple of js files (one of them spec file wthich contains tests, and other one is the code we're testing)

```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Jasmine Tests</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine.css">
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine-html.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/boot.js"></script>
</body>
</html>
```

### Jasmine Syntax and Matchers

Essential keywords:
- describe
  - is given by Jasmine, we use it to organize our tests (as well as other keywords)
  - 'let me describe ... to you'
  - very often we have one describe block per function/unit that we're testing
- it
  - is used inside of describe functions
  - inside of the it function we write code that explains in more detail what we expect this piece of functionality to do
  - 'let me tell you about ...'
  - each it function corresponds to a test, and is often called a 'spec'
- expect
  - lives inside of the it function
  - inside of expect function we make expectations or assertions about that functionality we are testing
  - if one of our expectations isn't met, the test (or spec) fails
  - 'here's what I expect'

Example:

```
describe('Earth')
  it('isRound')
    expect(earth.isRound.toBe(true))
  it('is the third planet from the sun')
    expect(earth.numberFromSun).toBe(3)
```

In code:

```javascript
var earth = {
  isRound: true;
  numberFromSun: 3
}

describe("Earth", function() {
  // string, callback
  it("is round", function() {
    expect(earth.isRound).toBe(true)
    // returns an object which we can attach other methods to
    // these methods attached onto the result of expect function are called matchers
    // here the matcher is toBe (result of expect function === value we pass to toBe function)
  });
  it("is the third planet from the sun", function() {
    expect(earth.numberFromSun).toBe(3)
  });
});
```

#### Matchers

Matchers are functions we attach to the result of the expect function

List of matchers:
- toBe/not.toBe
  - === values
- toBeCloseTo
  - compares two values, and accepts second parameter for precision
  - useful if we only care if something is similar, but not exactly the same
- toBeDefined
  - check if certain variables have specific value, and not undefined
- toBeFalsey/toBeTruthy
  - when we expect a value when converted to a boolean to be true or false
- toBeGreaterThan/toBeLessThan
  - comparing numbers
- toContain
  - if a value is contained in an array
  - sometimes we don't know all the values in an array, but want to make sure that something is there
- toEqual
  - when compating two objects with ===, we will not always get what we expect even if they look the same
  - if the two objects are different references in memory, === is false
    - ``arr1 = [1, 2, 3]``
    - ``arr2 = [1, 2, 3]``
    - ``arr1 === arr2 // false``
    - ``arr3 = []``
    - ``arr4 = arr3``
    - ``arr3 === arr4 // true, they are the same reference in memory``
  - toEqual accepts two different objects, and if their values inside are the same, it will return true (even if they are different references in memory)
  - used mostly for objects and arrays
- jasmine.any() (not really a matcher, but a useful tool especially in type checking)
  - if we want to make sure some value is an array or function, or constructed from a specific function, we can use jasmine.any() to check the type
  - typeof doesn't work for arrays (arrays are a type of object)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine.css">
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/jasmine-html.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jasmine/2.6.2/boot.js"></script>
  <script>
    describe("Jasmine Matchers", function() {
      it("allows for === and deep equality", function() {
        expect(1+1).toBe(2);
        expect([1,2,3]).toEqual([1,2,3]);
      });
      it("allows for easy precision checking", function() {
        expect(3.1415).toBeCloseTo(3.14,2);
      });
      it("allows for easy truthy / falsey checking", function() {
        expect(0).toBeFalsy();
        expect([]).toBeTruthy();
      });
      it("allows for easy type checking", function() {
        expect([]).toEqual(jasmine.any(Array));
        expect(function(){}).toEqual(jasmine.any(Function));
      });
      it("allows for checking contents of an object", function() {
        expect([1,2,3]).toContain(1);
        expect({name:'Elie', job:'Instructor'}).toEqual(jasmine.objectContaining({name:'Elie'}));
      });
    });
  </script>
</body>
</html>
```

### Writing Better Tests with Hooks

#### beforeEach

Example of a test with much repetition:

```javascript
decribe('#push', function() {
  it('adds elements to an array', function() {
    var arr = [1,3,5]; // we can't simply define arr once since it will be changed, and tests will give unexpected results
    arr.push(7);
    expect(arr).toEqual([1,3,5,7]);
  });

  it('returns the new length of the array', function() {
    var arr = [1,3,5];
    expect(arr.push(7)).toBe(4);
  });

  it('adds anything into the array', function() {
    var arr = [1,3,5];
    expect(arr.push({})).toBe(4);
  });
});
```

Jasmine has a way to avoid repeating: beforeEach
- It accepts a callback that will be run before a callback to each 'it' function

```javascript
decribe('Arrays', function() {
  var arr;
  beforeEach(function() {
    arr = [1,3,5];
  });
});
// Notice here that we are declaring the 'arr' variable inside of the callback to 'describe' and then assigning it to an array inside of a callback to beforeEach
// The reason we do this is because of scoping in javascript
// If we place the 'var' keyword inside of the callback to before each then that variable would only be accessible inside of the callback to beforeEach and nowhere else
// If we declare the variable outside of the beforeEach, we can access it inside of inner functions

// refactored result:
decribe('Arrays', function() {
  var arr;
  beforeEach(function() {
    arr = [1,3,5];
  });
  it('adds elements to an array', function() {
    arr.push(7);
    expect(arr).toEqual([1,3,5,7]);
  });

  it('returns the new length of the array', function() {
    expect(arr.push(7)).toBe(4);
  });

  it('adds anything into the array', function() {
    expect(arr.push({})).toBe(4);
  });
});

// Now notice that we only have to define 'arr' once and each callback to the it function has an 'arr' variable that gets redefined each time
```

#### afterEach

The same way that we can run code before each 'it' callback, we can run after each 'it' callback

afterEach runs after each 'it' callback, and is useful for teardown

```javascript
describe('Counting', function() {
  var count = 0;

  beforeEach(function() {
    count++; // we are adding one to our count variable before each 'it' callback
  });

  afterEach(function() {
    count = 0; // we are resetting our count to 0 after each 'it' callback
  });

  it('has a counter that increments', function() {
    expect(count.toBe(1));
  });

  it('gets reset', function() {
    expect(count).toBe(1);
  });
});

// The idea here is that we can use afterEach to reset variables used between tests
// This is also called a teardown and is useful when ensuring that your data stays the same between tests
// Very commonly we see teardown code when testing databases to ensure that we start and end with the same sample data
```

#### beforeAll/afterAll

If we you want to create a variable that persists among all tests, we can use the beforeAll and afterAll functions
- They run before or after all tests, and do not reset in between
- This is not as common since it can lead to unintended side effects but sometimes it is needed

```javascript
var arr = [];
beforeAll(function() {
  arr = [1,2,3];
});

describe('Counting', function() {
  it('starts with an array', function() {
    arr.puch(4);
    expect(1).toBe(1);
  });
  it('keeps mutating that array', function() {
    console.log(arr); // [1,2,3,4]
    arr.push(5);
    expect(1).toBe(1);
  });
});

describe('Again', function() {
  it('keeps mutating the array again', function() {
    console.log(arr); // [1,2,3,4,5]
    expect(1).toBe(1);
  });
});

// Notice that the code is similar to beforeEach, but the 'arr' variable we create only gets defined once and is shared amongst all the tests
```

### Testing Larger Test Suites

#### Nesting Describe

Describe is how we can express a specific high level topic that we will test; we can also nest describe blocks if we are describing quite a few things

```javascript
// here we are describing arrays, and then describing specific methods for arrays 
// inside of each of these inner 'describe' blocks we can place 'it' blocks

describe('Array', function() {
  var arr;
  beforeEach(function() {
    arr = [1,3,5];
  });

  describe('#unshift', function() {
    it('adds an element to the beginning of an array', function() {
      arr.unshift(17);
      expect(arr[0]).toBe(17);
    });
    it('returns the new length', function() {
      expect(arr.unshift(1000)).toBe(4);
    });
  });

  describe('#push', function() {
    it('adds elements to the end of an array', function() {
      arr.push(7);
      expect(arr[arr.length-1]).toBe(7);
    });
    it('returns the new length', function() {
      expect(arr.push(1000)).toBe(4);
    });
  });
});

// the idea here is that we want to test arrays but arrays are a very large topic
// so we'll break it down into smaller describe blocks, and in each of these describe blocks will cover individual array methods
```

#### Pending Tests

We can mark tests as pending when we don't know exactly what we'll be testing or if we do not want to run a specific test

We can mark a test as pending by either omitting a callback function to the 'it' function, adding the 'pending'
function inside, or simply placing an X before the 'it' function

```javascript
describe('Pending specs', function() {

  xit('can start with an xit', function() {
    expect(true).toBe(true);
  });

  it('is a pending test if there is no callback function');

  it('is pending if the pending function is invoked inside the callback', function() {
    expect(2).toBe(2);
    pending();
  });

});
```

#### Number of Expect's

If the testing of one unit needs more than one 'expect' - use more; just make sure we are not testing several aspects of a unit which do not belong together in a single one test case

```javascript
// not great

describe('Earth', function() {
  it('is round and has a method to check what number it is from the sun', function() {
    expect(earth.isRound()).toBe(true);
    expect(earth.howFarFromSun).toBe(jasmine.any(Function));
    expect(earth.howFarFromSun()).toBe(3);
  });
});

// better

describe('Earth', function() {
  it('is round', function() {
    expect(earth.isRound()).toBe(true);
  it('has a method to check what number it is from the sun', function() {
    expect(earth.howFarFromSun).toBe(jasmine.any(Function));
    expect(earth.howFarFromSun()).toBe(3);
  });
});
```

### Mocking

When unit testing, we strive to isolate specific functionality and how this functionality behaves under a variety of circumstances

Unfortunately, many functions or objects may depend on other parts of our application; these can include other functions and data sources, and even previously executed functions; this is what mocking is used for

To isolate the behavior of the object you want to replace the other objects by mocks that simulate the behavior of the real objects; this is useful if the real objects are impractical to incorporate into the unit test

A mock is a fake object that poses as a function without having to go through the overhead of creating the real object; when we create a mock object, it creates a fake object that takes the place of the real object

We can then define what methods are called and their return values from within our mock object

Mocks can be used to retrieve certain values like how many times the mock function was called, what value the function returned, and how many parameters the function was called with

In short, mocking is creating objects that simulate the behavior of real objects.

- Some people differenciate mocking and stubbing (stub being a 'minimal' simulated object which implements just enough behavior to allow the object under the test to execute a test)
- See more: https://stackoverflow.com/questions/2665812/what-is-mocking


### Spies

In Jasmine mocks are referred to as spies

- Jasmine has test double functions called spies
- A spy can stub (mimic) any function and track calls to it and all its arguments
- Spies only exist in the 'describe' or 'it' block in which they are defined
- Spies are removed after each spec
- There are special matchers for interacting with spies
- A spy listens to method calls on your objects and can be asked if and how a method got called later on
- More info: https://blog.codeship.com/jasmine-spyon/

#### Creating a Spy

There are two ways to create a spy in Jasmine: the spyOn() function and the jasmine.createsBy() function
- spyOn() can only be used when the method exists on an object 
- jasmine.createsBy() will return a brand new function

```javascript
// when we are spying on an existing function, make sure to use spyOn()

function add(a, b, c) {
  return a + b + c;
}
// this function is attached to the 'window' object (it's in the global scope)

// spy on the 'add' function and test to see if our spy has been called
decribe('add', function() {
  var addSpy, result;
  beforeEach(function() {
    addSpy = spyOn(window, 'add');
    result = addSpy();
  });
  it('can have params tested', function() {
    expect(addSpy).toHaveBeenCalled();
  });
});

// Notice here that we don't care about any kind of return value; we just want to make sure that our function is called
// So if our function had to go through an expensive process like connecting to a database we wouldn't have to worry about any of that now, we're just faking the behavior of this function
```

#### Testing Parameters

We can use toHaveBeenCalledWith matcher to test default parameters or the type of certain parameters

```javascript
function add(a, b, c) {
  return a + b + c;
}

decribe('add', function() {
  var addSpy, result;
  beforeEach(function() {
    addSpy = spyOn(window, 'add');
    result = addSpy();
  });
  it('can have params tested', function() {
    expect(addSpy).toHaveBeenCalled();
    expect(addSpy).toHaveBeenCalledWith(1, 2, 3);
  });
});
```

#### Returning a Value

We can use .and.callThrough function to make sure that we store the return value instead of just undefined (if our spy actually returns the correct value)

```javascript
function add(a, b, c) {
  return a + b + c;
}

decribe('add', function() {
  var addSpy, result;
  beforeEach(function() {
    addSpy = spyOn(window, 'add').and.callThrough();
    result = addSpy(1, 2, 3);
  });
  it('can have a return value tested', function() {
    expect(result).toEqual(6);
  });
});
```

##### Why Use a Spy

Why would we bother to use a spy to test a return value instead of the actual function?

It's possible that the original function takes a while to run or depends on other objects that we cannot access

With unit testing we want to test small units of our code so we should not be invoking lots of functions that depend on other ones; We should strive to use dummy data whenever possible to speed up our tests

#### Testing frequency

We can test the frequency or how many times a function is called; this is useful if we want to make sure a function is only called a certain amount of times or maybe we want the function to only be invoked once

To test this we can use the 'calls' object on the spy to access the count or see if the function has been called at
all

```javascript
function add(a, b, c) {
  return a + b + c;
}

decribe('add', function() {
  var addSpy, result;
  beforeEach(function() {
    addSpy = spyOn(window, 'add').and.callThrough();
    result = addSpy(1, 2, 3);
  });
  it('can have a return value tested', function() {
    expect(addSpy.calls.any()).toBe(true);
    expect(addSpy.calls.count()).toBe(1);
    expect(result).toEqual(6);
  });
});
```

### Clock

Clock is used to test time dependent code and asynchronous code (this includes setInterval, setTimeout, as well as operations that use dates)
- It is installed by jasmine.clock.install()
  - This is commonly done in a beforeEach callback
- We need to make sure to uninstall the clock after we are done to restore the original functions (after each spec to make sure we do not have any unintended side effects)
  - Usually done in afterEach callback


#### setTimeout

```javascript
describe('a simple setTimeout', function() {
  var sample;
  beforeEach(function() {
    sample = jasmine.createSpy('sampleFunction'); // dummy function
    jasmine.clock().install();
  });

  afterEach(function() {
    jasmine.clock().uninstall();
  });

  it('is only invoked after 1000 ms', function() {
    setTimeout(function() {
      sample();
    }, 1000);
    jasmine.clock().tick(999); // moving clock by a certain amount of time
    expect(sample).not.toHaveBeenCalled();
    jasmine.clock().tick(1);
    expect(sample).toHaveBeenCalled();
  });
});
```

#### setInterval

```javascript
describe('a simple setInterval', function() {
  var dummyFunction;

  beforeEach(function() {
    dummyFunction = jasmine.createSpy('dummyFunction');
    jasmine.clock().install();
  });

  afterEach(function() {
    jasmine.clock.uninstall();
  });

  it('check to see the number of times the function is invoked', function() {
    setInterval(function() {
      dummyFunction();
    }, 1000);
    jasmine.clock().tick(999);
    expect(dummyFunction.calls.count()).toBe(0);
    jasmine.clock().tick(1000);
    expect(dummyFunction.calls.count()).toBe(1);
    jasmine.clock().tick(1);
    expect(dummyFunction.calls.count()).toBe(2);
  });
});
```

#### Testing Async Code

Jasmine also has support for running specs that require testing async code (this includes timers like setInterval, setTimeout as well as HTTP request with something like ajax)

beforeAll, afterAll, beforeEach, afterEach, and 'it' take an optional single argument (commonly called 'done') that sould be called when the async work is complete

Test will not complete until the 'done' function is invoked (Jasmine has an internal timer which it runs before the test fails to make sure that the test is not waiting too long)
- Jasmine will wait 5 seconds by default; the internal timer can be modified with jasmine.DEFAULT_TIMEOUT_INTERVAL (it is done rarely)

```javascript
function getUserInfo(username) {
  return $.getJSON('https://api.github.com/users/' + username);
}

describe('#getUserInfo', function() {
  // passing a parameter 'done' to the callback of the 'it' function
  it('returns the correct name for the user', function(done) {
    // invoking the getUserInfo function and passing the user name of elie, and then attaching a .then to handle the resolved promise and passing a callback to .then which contains the data we get back from the GitHub API
    getUserInfo('elie').then(function(data) {
      // inside of this callback we test to see if the name property on the data we got back is the name which is expected
      expect(data.name).toBe('Elie Schoppik');
      done(); // making sure the test doesn't time out
    });
  });
});
```

---

## Mocha