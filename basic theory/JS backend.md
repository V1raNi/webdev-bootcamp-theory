# JS and Backend

Stuff from 'advanced' bootcamp will be italics bolded or separated by stars (in code)

#### Table of Contents

- [JS and Backend](#js-and-backend)
      - [Table of Contents](#table-of-contents)
  - [HTTP in Depth](#http-in-depth)
  - [Command Line](#command-line)
  - [Node.js](#nodejs)
    - [What Is Node.js](#what-is-nodejs)
    - [Node Console](#node-console)
    - [NPM](#npm)
    - [Package.json](#packagejson)
    - [Express framework](#express-framework)
      - [Usage of Express](#usage-of-express)
      - [Folder Structure](#folder-structure)
    - [Routes and Route Parameters](#routes-and-route-parameters)
      - [Splat or Star Route Matcher](#splat-or-star-route-matcher)
      - [Order of Routes](#order-of-routes)
      - [Route parameters](#route-parameters)
    - [Templates and EJS](#templates-and-ejs)
      - [If Statement in EJS](#if-statement-in-ejs)
      - [Loop Statement in EJS](#loop-statement-in-ejs)
    - [Serving Custom Assets in EJS](#serving-custom-assets-in-ejs)
    - [Making EJS more 'DRY'](#making-ejs-more-dry)
      - [Getting rid of '.ejs'](#getting-rid-of-ejs)
      - [Partials](#partials)
    - [Posts Requests and Body Parsing](#posts-requests-and-body-parsing)
  - [APIs](#apis)
    - [Intro to API's](#intro-to-apis)
      - [API Endpoint](#api-endpoint)
    - [JSON and XML](#json-and-xml)
      - [XML](#xml)
      - [JSON](#json)
    - [Making Requests with Node.js](#making-requests-with-nodejs)
  - [Databases](#databases)
    - [Intro to Databases](#intro-to-databases)
    - [SQL (relational) vs NoSQL (non-relational)](#sql-relational-vs-nosql-non-relational)
      - [SQL](#sql)
      - [NoSQL](#nosql)
    - [Data Association](#data-association)
  - [MongoDB](#mongodb)
    - [Running Mongo](#running-mongo)
    - [Mongo Commands](#mongo-commands)
    - [Mongoose](#mongoose)
      - [Usage of Mongoose](#usage-of-mongoose)
      - [***Mongoose Promises***](#mongoose-promises)
    - [Embedding and Referencing Data](#embedding-and-referencing-data)
      - [Embedding](#embedding)
      - [Referencing](#referencing)
  - [REST](#rest)
    - [RESTful routes](#restful-routes)
  - [Cleaning Up](#cleaning-up)
    - [Export](#export)
    - [Routes Clean Up](#routes-clean-up)
  - [Authentication](#authentication)
    - [One Way Hashing](#one-way-hashing)
      - [Example of Hashing](#example-of-hashing)
    - [Sign In (Authentication)](#sign-in-authentication)
    - [JWT (JSON Web Token)](#jwt-json-web-token)
      - [Authentication](#authentication-1)
      - [Sending JWT to Server](#sending-jwt-to-server)
      - [Bcrypt Usage](#bcrypt-usage)
  - [Auth with Passport.js](#auth-with-passportjs)
    - [Getting started](#getting-started)
    - [User](#user)
  - [GIT](#git)
    - [Git basics](#git-basics)
      - [Git init](#git-init)
      - [Git status](#git-status)
      - [Git add](#git-add)
      - [Git commit](#git-commit)
      - [Git log](#git-log)
      - [Git checkout](#git-checkout)
    - [Git revert](#git-revert)
  - [Deploying](#deploying)
    - [Environment Variables](#environment-variables)
  - [Useful Modules](#useful-modules)
    - [Cors](#cors)
    - [Morgan](#morgan)

## HTTP in Depth

The most important thing that's happening when you go to some website is the 'request-response' cycle and it's independent from a browser

HTTP is hypertext transfer protocol

HTTP verbs are various HTTP requests\
They tell the server what to do, what we're requesting, e.g:
- Get request - we are asking server to get us some data
    - the only request accessible from a browser navbar or search bar
    - searching something is a GET request
        - query string starts with ? and is separated by &
- Post reqiest - we are sending some data with a request to server to be added to database or something
    - some sign up forms or something
- Put and Patch requests are used to update some data
    - update a post on Facebook

When a server gets a 'Delete' request, we expect something to be deleted (but it doesn't happen just because of the request)

Important parts of a response:
- Body of the response (what's sent back)
    - e.g. HTML, CSS, and JS when we are visiting some website
- Headers (metadata about the response)
    - e.g. content type, date, status
- Status (it's a number, a part of the protocol, a standardized way of transferring a status of a response)

---

## Command Line

Note: Still not sure what to use: cmd, powershell, gitbash or WSL or even Babun (possibly with ConEmu)

- ls shows all of the contents of the current folder we're inside of
    - ``ls dirname`` to see the contents of specific folder
- cd is used to change directories
    - ``cd ..`` to go back one level
    - you can start writing the name of a directory and hit tab to autocomplete it
- touch is used to create new files
    - ``touch orange.txt``
- mkdir is used to create a new folder
    - ``mkdir FavColors``
- rm is used to delete a specific file
    - ``rm orange.txt``
- rm -rf allows us to remove entire directories
    - ``rm -rf colors/``
    - ``rm -rf /`` deletes everything!
    - -rf is a flag, a way to change what the command does
        - stands for 'recursive force'

---

## Node.js

### What Is Node.js

Node.js is a JS runtime built on Chrome's JS engine
- allowed JS to run not only in the browser
- it is a way to write and run JS on the server side

Node.js's package system, npm, is the largest ecosystem of open source libraries in the world

### Node Console

Based on command line, basically we can write JS code in a command line or run a file with node
- although we can't use some things which are available in a browser console like document, alert, etc.

```bash
$node
>'hello ' + 'world'
>'hello world'
# Ctrl+C twice to exit
$node hello.js
```

It is called REPL - Read-Evaluate-Print Loop - is a simple, interactive computer programming environment that takes single user inputs (i.e., single expressions), evaluates them, and returns the result to the user
- exists in many server side languages, e.g. in ruby

### NPM

NPM stands for Node Package Manager; it is a way to include 'libraries' when we write server-side JS

To install a package (the list could be found at npmjs.com):

```bash
$npm install <package name>
# npm install <package name>@version
```

To include a package:

```javascript
var cat = require('cat-me'); // cat-me is the name of a package
```

### Package.json

Package.json is a file with metadata about the project (description, name, version, license, dependencies)

Dependencies are the packages the application relies and needs to work (at runtime)
- React, Express, Redux, etc.

Devdependencies are modules which are only required during development
- Nodemon, Babel, ESLint, Mocha, etc.

The node modules folder is not included in GitHub repositories\
Rather that uploading all of the packages, we can just include information about the required packages in package.json and install them with one command

When installing a package we can add flags to take the package name and version and save it into our package.json file
- --save (``npm i -S``) to save it as a depenpency
- --save-dev (``npm i -D``) to save it as a devDependency
- After node version 9 we can omit ``--save`` flag

To create a package.json we can use ``npm init`` command and then follow the instructions

```bash
$npm init

name: #name of the folder by default
version: #(1.0.0) by default
description:
enrty point: #the file where our application starts, index.js by default
test command:
git repository:
keywords:
author:
license: #ISC by default
```

---

### Express framework

A library is a code that someone else wrote and we can include and use in our app

A framework is almost the same, but the way that we use it is different\
The most inportant difference is Inversion of Control
- When you call a library, you are in control
    - Library is a collection of functionality that you can call
- When with a framework, the control is inverted: the framework calls you
    - This is called the Hollywood principle: Don't call Us, We'll call You
    - Basically, all the control flow is already in the framework, and there's just a bunch of predefined white spots that you can fill out with your code

Types of frameworks:
- heavy (opinionated)
    - heavyweight framework has little 'blanks' that you fill in; apps are created faster but sometimes you have no idea what's going on behind the scenes
    - Rails
- light (unopinionated)
    - lightweight framework has more blanks and more frequently across the code; more flexible
    - Express

Express is a lightweight web development framework

#### Usage of Express

```javascript
var express = require('express');
var app = express(); // that includes all the contents of express and saves into a variable

// '/' => 'Hi there!'
app.get('/', /*middleware */ function(req, res) {
    // request and response (could be anything, but it's the regular naming policy)
    // request (Obj) contains all the info about the request that was made
    // response (Obj) contains all the info about what we're going to respond with
    res.send('Hi there');
});

// '/bye' => 'Goodbye!'
app.get('/bye', function(req, res) {
    res.send('Goodbye!');
});

// tell Express to listen for requests (start server)
app.listen(3000, function() {
    console.log('Serving app on port 3000');
});

// for c9:
// app.listen(process.env.PORT, process.env.IP, function() {
//     console.log('Server has started');
// });

// process.env.PORT is an enviroment variable which is called PORT, it will return a number which comes from c9
// process.env.IP tells express to listen from a particular IP
```

#### Folder Structure

One pattern of folder structure of an Express app

- handlers
    - functions that we pass to our routes
- routes
    - routes for our app
- middleware
    - middleware functions
- models
    - database models
- views
    - page templates, if we don't use front-end framework
- public
    - assets for our app if we don't use front-end framework
- index.js

### Routes and Route Parameters

Routes are how we listen for a particular request, and then run some other code depending on the request that we get

#### Splat or Star Route Matcher

We use * to have some sort of catch-all, some message we respond to every other route that we didn't specify, e.g. show an error message if page is not found

```javascript
var express = require('express');
var app = express();

// it should be located at the bottom of the app
app.get('*', function(req, res) {
    res.send('You are a star');
});
```

#### Order of Routes

Order of routes matters
- The first route that matches is the only route that will be run
- If we put a * route before anything else, we will never have any other code run

#### Route parameters

Websites (e.g. Reddit) don't have all the routes written in their code, like:

```javascript
app.get('/r/soccer');
app.get('/r/music');
app.get('/r/movies');
```

Instead we create patterns by making use of route (path) parameters (variables)

```javascript
app.get('/r/subredditName'); // works only for '/r/subredditName'
app.get('/r/:subredditName'); // creates a pattern where we listen for get request for '/r/' and anything afterwards

// website.com/r/soccer would work
// website.com/r/soccer/hello wouldn't work because it's a different pattern

app.get('/r/:subredditName/comments/:id/:title/');
```

We can access the data inside of our route handler

```javascript
app.get('/r/:subredditName', function(req, res) {
    var subreddit = req.params.subredditName;
    res.send('WELCOME TO THE ' + subreddit.toUpperCase() + ' SUBREDDIT');
});

// /r/soccer
// WELCOME TO THE SOCCER SUBREDDIT
// /r/cats/
// WELCOME TO THE CATS SUBREDDIT
```

---

### Templates and EJS

When we use Express, to render a page, we don't write plain html files, we need dynamic html files which are called 'templates'

```javascript
app.get('/', function(req, res) {
    res.render('home.ejs'); // we can send back the contents of a file
})
```

EJS stands for Embedded JavaScript
- We need to install a package for it first
- ``npm install ejs --save``
- It lets us embed JS code (variables, if statements, loops, etc.) inside of html

Usually we don't put .ejs files in the same directory with app.js (we create a separate 'views' directory)
- You can use the method set() to redefine express's default settings.
- ``app.set('views', path.join(__dirname, '/yourViewDirectory'));``
    - Or maybe ``app.set('views','./folder1/folder2/views');``

EJS file:

```html
<h1>You fell in love with: <%= thingVar.toUpperCase() %> <h1>
<!-- everything inside of the '<%= %>' brackets will be treated as a JS code -->
```

App file:

```javascript
app.get('/fallinlovewith/:thing'), function(req, res) {
    var thing = req.params.thing;
    res.render('love.ejs', {thingVar: thing}); // we're passing the variable into the .ejs file; we can put multiple pieces of data to be used in our template
});
```

#### If Statement in EJS

There are three types of tags in ejs:
- <%= %>
    - The value will be returned and added to HTML (without HTML)
- <% %>
    - When we do logic, we don't want to add anything into HTML, we just run the code (control flow)
- <%- %>
    - Includes text with HTML tags (evaluates the code)
    - Could be a dangerous practice
    - Inputs should be sanitazied (getting rid of script tags)
        - express-sanitizer
        - ``req.sanitize(req.body)``

```html
<h1>You fell in love with: <%= thingVar.toUpperCase() %> <h1>

<% if(thingVar.toLowerCase() === 'rusty') { %>
        <p>Good choice!</p>
<% } else { %>
        <p>You should've said Rusty!</p>
<% { %>
```

#### Loop Statement in EJS

App file:

```javascript
app.get('/posts', function(req, res) {
    var posts = [
        {title: 'Post 1', author: 'Susy'},
        {title: 'My adorable pet', author: 'Charlie'},
        {title: 'Look at this puppy', author: 'Colt'},
    ];

    res.render('posts.ejs', {posts: posts});
});
```

Posts.ejs:

```html
<h1>The Posts Page</h1>

<%= posts %>
<!-- [object Object], [object Object], [object Object] -->

<% for (var i = 0; i < posts.length; i++) { %>
    <li>
        <%= posts[i].title %> - <strong><%= posts[i].author %></strong>
    </li>
<% } %>
<!--
Post 1 - Susy
My adorable pet - Charlie
Look at this puppy - Colt
-->

<!-- Also we can use forEach: -->
<% posts.forEach(function(post) { %>
    <li>
        <%= post.title %> - <strong><%= post.author %></strong>
    </li>
<% }); %>
```

### Serving Custom Assets in EJS

We can include public assets (css, js) in EJS files

We create another directory called 'public' (can be named anything) and we put app.css in this folder

```html
<link rel="stylesheet" href="app.css">
<!-- it won't work this way since the default policy of Express is to not serve anything aside from the 'views' directory -->
<!-- but we don't need to specify the path (public/app.css) here -->
```

```javascript
app.use(express.static('public')); // we tell Express to serve the contents of 'public' directory
```

### Making EJS more 'DRY'

#### Getting rid of '.ejs'

We can shorten our code by telling Express that we're going to use '.ejs' ahead of time

```javascript
res.render('home.ejs');

app.set('view engine', 'ejs');

res.render('home'); // Express expects these files to be .ejs
```

#### Partials

Partials are the files, templates that we can write and include in other templates
- We can use them to avoid writing html boilerplate or link to css files manually in every .ejs file of our app

To use partials we should create directory 'partials' inside of a 'views' folder and put them there
- Some people call this directory 'layouts'
- The common pratice is to create 'header.ejs' and 'footer.ejs'
    - Although we are not limited to this

Header.ejs:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Demo App</title>
        <link rel="stylesheet" type="text/css" href="/app.css">
    </head>
    <body>
```


Note that paths are important
- There is a difference between '/app.css' and 'app.css'
    - The app won't work properly without '/' in 'href'
- When we add the '/' we are telling to look for app.css not in the same directory the template is in, but in a root directory
- We specify the path when we have different elements, e.g.
    - \<link rel="stylesheet" type="text/css" href="/stylesheets/app.css">
    - \<script src="/scripts/main.js">\</script>

Footer.ejs:

```html
    <p>Copyright 2019</p>
  </body>
</html>
```

Including partials:

```html
<% include partials/header %>
<!-- we don't specify .ejs since we told Express to look for .ejs files at 'Getting rid of '.ejs' section' -->

<h1>This is a Demo App</h1>

<% include partials/footer %>
```

---

### Posts Requests and Body Parsing

```html
<form action="/addfriend" method="POST">
    <input type="text" name="newfriend" placeholder="name">
    <button>I made a new friend</button>
</form>
```

```javascript
app.post('/addfriend', function(req, res) {
    res.send('You have reached the post route');
    console.log(req.body);
});
```

Req.body here returns undefined because express (out of the box)doesn't create the request.body, we need to explicitly tell it to take the request.body and turn it into a JS object called request.body
- There is a package for that called body-parser

```javascript
var bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({extended: true}));

app.post('/addfriend', function(req, res) {
    console.log(req.body.newfriend); // returns what we submitted in the form above
    res.redirect('/friends'); // instead of rendreing a new page or sending something, we often would want to redirect a user
});
```

---

## APIs

### Intro to API's

Application Programming Interface(API) in a broad sense is a set of routines, protocols, and tools for building software and applications

It is a way to connect with other applications, interface which allows two systems to communicate with one another
- It could be a database API (locally)
- Video card API
- API to incorporate graphical elements
- Web APIs

Web APIs generally communicate via HTTP
- Twitter API 'give me all tweets that mention "ice cream"'
- Reddit API 'what is the current top post?'
- Weather API 'what is the weather in New York?'
- Some APIs can be found at programmableweb.com

#### API Endpoint


An endpoint is one end of a communication channel
- When an API interacts with another system, the touchpoints of this communication are considered endpoints
- For APIs, an endpoint can include a URL of a server or service
- Each endpoint is the location from which APIs can access the resources they need to carry out their function
- Endpoints specify where resources can be accessed by APIs and play a key role in guaranteeing the correct functioning of the software that interacts with it
- API performance relies on its ability to communicate effectively with API Endpoints

### JSON and XML

When we use the internet, we make HTTP request and get HTML back

APIs don't respond with HTML because HTML contains info about the structure of the page, and APIs respond with data, not structure
- They use simpler data formats like XML and JSON

#### XML

XML stands for Extended Markup Language
- It is syntacticly similar to HTML, but it does not describe presentation like HTML does

```xml
<person>
    <age>21</age>
    <name>Travis</name>
    <city>Los Angeles</city>
</person>
```

#### JSON

JSON stands for Javascript Object Notation
- JSON looks exactly like JavaScript objects, but everything is a string

```json
{
    "person": {
        "age": "21",
        "name": "Travis",
        "city": "Los Angeles"
    }
}
```

### Making Requests with Node.js

We can make requests from a browser, from a command line

```bash
$curl http://www.google.com
```

Inside of an application we can make requests by using a package 'request'
- usually we recieve data as a string
- to get data as a JSON use ``JSON.parse(data)``

```javascript
var request = require('request');
request('http://www.google.com', function (error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body); // show the HTML for the Google homepage
    }
});

request('http://someAPI.com', function (error, response, body) {
    // another way of writing
    if (error) {
        console.log('Something went wrong');
        console.log(error);
    } else {
        if (response.statusCode == 200) {
            var parsedData = JSON.parse(body);
            console.log(body); // string
            console.log(parsedData); // object
            console.log(['query']['results']); // prints data in 'query -> results'
        }
    }
})
```

---

## Databases

### Intro to Databases

Database is a collection of information/data which has an interface to interact with this data (add, remove, edit data)

### SQL (relational) vs NoSQL (non-relational)

#### SQL

SQL are tabular (we have to define patterns ahead of time) and flat databases

<table>
    USERS TABLE
    <tr>
        <td>id</td>
        <td>name</td>
        <td>age</td>
        <td>city</td>
    </tr>
    <tr>
        <td>1</td>
        <td>John</td>
        <td>24</td>
        <td>NYC</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Helen</td>
        <td>23</td>
        <td>Missoula</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Peter</td>
        <td>34</td>
        <td>Moscow</td>
    </tr>
<table>

<table>
    COMMENTS TABLE
    <tr>
        <td>id</td>
        <td>text</td>
    </tr>
    <tr>
        <td>1</td>
        <td>'come visit Montana!'</td>
    </tr>
    <tr>
        <td>2</td>
        <td>'lol'</td>
    </tr>
    <tr>
        <td>3</td>
        <td>'I love puppies'</td>
    </tr>
    <tr>
        <td>4</td>
        <td>'seriously Montana is great!!'</td>
    </tr>
<table>

If we want to express relationship between users and comments, the only way to do it is through another table

<table>
    USER/COMMENTS JOIN TABLE
    <tr>
        <td>userId</td>
        <td>commentID</td>
    </tr>
    <tr>
        <td>1</td>
        <td>3</td>
    </tr>
    <tr>
        <td>2</td>
        <td>1</td>
    </tr>
    <tr>
        <td>2</td>
        <td>4</td>
    </tr>
    <tr>
        <td>3</td>
        <td>2</td>
    </tr>
<table>

We need to follow the established pattern very closely
- If we want to add a new property to someone (e.g. favColor of Peter), we would need to create a favColor for everyone which should be empty for everyone else (null, undefined, false) - and that's not flexible

#### NoSQL

NoSQL databases are more flexible, we don't have to define any patterns ahead of time, and things could be nested (it's not flat)
- It looks similar to JS, it is BSON (Binary JavaScript Object Notation)
- Flexibility doesn't make it better than SQL, for some cases it's better, for some - not

```javascript
{
    name: "Helen",
    age: 23,
    city: Missoula,
    comments: [
        {text: 'come visit Montana!'},
        {text: 'seriously Montana is great!!'},
        // if we want to add new data we would just push it into this object
        {text: 'why does no one care about Montana?'}
    ],
    favColor: 'purple'
}

{
    name: "Sally",
    age: 23,
    city: Missoula,
    comments: [
        {text: "I don't like Montana at all"},
    ],
    favFood: 'steak'
}
```

### Data Association

- One to one (One item relates to one another item)
    - One book - one publisher
- One to many (One entity is related to many entities)
    - One user - many uploaded photos
- Many to many (Association goes both ways)
    - Students have multiple courses, and each course has many students

---

## MongoDB

### Running Mongo

To run the DB: 
- "C:\Program Files\MongoDB\Server\4.0\bin\mongod.exe" --dbpath="``YOUR PATH``"
- [initandlisten] waiting for connections
- run mongo

### Mongo Commands

- help
- show dbs
    - Shows databases that you have
- use \<db name>
    - Uses the selected db
    - If there is no such db, Mongo creates it
- db.\<collection>.insert({data})
    - Create
    - We add things in by creating collections (they group data together)
    - If there is no such collection, Mongo creates it
    - db.dogs.insert({name: "Rusty", breed: "Mutt"})
- db.\<collection>.find()
    - Read
    - Returns everything in a collection if ()
    - Looks for an item (or items)
    - db.dogs.find({name: "Rusty"})
- db.\<collection>.update()
    - Update
    - Updates data of an item
    - db.\<collection>.update({name: "Lulu"}, {breed: Labradoodle"})
        - Will update the breed of "Lulu" but will lose everything else (name, age, etc.)
    - If we want to update items without overwriting them:
        - db.dogs.update({name: "Rusty", {$set: {name: "Tater", isCute: true}}})
- db.\<collection>.remove()
    - Destroy (delete)
    - db.dogs.remove({breed: "Labradoodle"})
    - Removes everything matching by default
- db.\<collection>.drop()
    - Drops (deletes) the whole collection

### Mongoose

Mongoose is a tool which helps to interact with MongoDB in the JS app
- It is a MongoDB object modeling in Node.js (Object Data Mapper, ODM)

#### Usage of Mongoose

```javascript
var mongoose = require('Mongoose');
mongoose.connect('mongodb://localhost/cat_app'); // creates cat_app if there's no such DB

// *********************
// debug mode allows us to see what's happening at any given point when things are being sent to a database, when they're failing instead of just silently failing
// it's better to set it up in our models/index.js file
mongoose.set('debug', true);
// *********************

// define what a cat looks like (telling JS that we want to be able add cats to the DB)
// this is not defining a table, this is defining a pattern (anything else can be added if needed), by default they are not required
var catSchema = new mongoose.Schema({
    // we can also add an object in, and say type: string, and give it other pieces of data like if it's required, and give a message or if it has some default value
    name: {
        type: String,
        required: 'Name cannot be blank!'
    }
    age: Number,
    temperament: String
});


// we take the schema (pattern) and compile it into a model and save it into a variable (now it is an object which has all the methods we need)
var Cat = mongoose.model('Cat', catSchema); 
// 'Cat' is a single version of the collection name (collection will be 'cats')
// *** don't forget to export it ***

// add a new cat to the DB
var george = new Cat({
    name: 'George',
    age: 11,
    temperament: 'Grouchy',
});

george.save((err, cat) => {
    if (err) {
        console.log('Something went wrong');
        console.log(err);
    } else {
        console.log('We just saved a cat to the DB');
        console.log(cat); // cat is what's coming from the DB, and 'george' isn't (it is what we're trying to save to the DB)
    }
});

// another way to add a cat:
Cat.create({
    name: 'Snow White',
    age: 15,
    temperament: 'Nice'
}, (err, cat) => {
    if (err) {
        console.log(err);
    } else if {
        console.log(cat);
    }
});

// retrieve all cats from the DB
Cat.find({}, (err, cats) => {
    if (err) {
        console.log('Something went wrong');
        console.log(err);
    } else {
        console.log('All the cats are...');
        console.log(cats);
    }
});
```

#### ***Mongoose Promises***

We can use promises instead of callback functions (most of the examples below feature callbacks and ES5 though since they were written before I learned promises and other advanced stuff)

```javascript
mongoose.Promise = Promise; // allows us to use the promise syntax
// db.Todo.find().then instead of callback db.Todo.find(function() {});
// it's better to set it up in our models/index.js file
```

### Embedding and Referencing Data

Embedding or referencing data depends on what you're doing and what your goal is

#### Embedding

```javascript
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/demo');

// user model
var userSchema = new mongoose.Schema({
    email: String,
    name: String
});

var User = mongoose.model('User', userSchema);

// post model
var postSchema = new mongoose.Schema({
    title: String,
    content: String
});

var Post = mongoose.model('Post', postSchema);

// user and post examples:
// {
//     email: 'charile@brown.edu',
//     name: 'Charlie Brown'
// }
// {
//     title: 'Apples',
//     content: 'Apples are delicious'
// }

// embedding data into user schema (should be placed after a postSchema)
var userSchema = new mongoose.Schema({
    email: String,
    name: String,
    posts: [postSchema]
});

// data will look something like this:
// {
//     email: 'charile@brown.edu',
//     name: 'Charlie Brown',
//     posts: [
//        {
//          title: 'Apples',
//          content: 'Apples are delicious'
//        }
//     ]
// }

// adding a new post:
// when creating a new user
var newUser = new User({
    email: 'hermione@hogwarts.edu',
    name: 'Hermione Granger'
});

newUser.posts.push({
    title: 'How to brew a polyjuice potion',
    content: 'Go to potions class to find out!'
});

newUser.save(callback)...;

// adding a new post to existing user (probably can do with arrow functions)
User.findOne({name: 'Hermione Granger'}, function(err, user) {
    if (err) {
        console.log(err);
    } else {
        console.log(user);
        user.posts.push({
            title: '3 things I hate',
            content: 'Voldemort, Voldemort, Voldemort'
        });
        user.save(function(err, user) { // this is what some people call callback hell
            if (err) {
                console.log(err);
            } else {
                console.log(user);
            }
        });
    }
});
```

#### Referencing

```javascript
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/demo');

// post model
var postSchema = new mongoose.Schema({
    title: String,
    content: String
});

var Post = mongoose.model('Post', postSchema);

// user model
var userSchema = new mongoose.Schema({
    email: String,
    name: String,
    posts: [
        {
            type: mongoose.Schema.Types.ObjectId, // post IDs
            ref: 'Post'
        }
    ]
});

var User = mongoose.model('User', userSchema);

// example user and post
User.create({
    email: 'bob@gmail.com',
    name: 'Bob Belcher'
});
Post.create({
    title: 'How to cook burger',
    content: 'Just cook it'
});

// referencing
Post.create({
    title: 'How to not cook a burger',
    content: 'Just do not cook it'
}, function(err, post) {
    // finding the user
    User.findOne({email: 'bob@gmail.com'}, function(err, foundUser) {
        if (err) {
            console.log(err);
        } else {
            // add the post into the user's posts
            foundUser.posts.push(post); // 'post' is the post we've just created
            // save into the db
            foundUser.save(function(err, data) {
                if (err) {
                    console.log(err);
                } else {
                    console.log(data); // inside of the 'posts' array we will have only the id instead of the whole post (as it was with the embedding)
                }
            });
        }
    });
});

// find posts of the user
// populate actually populates the posts array, it looks up the data by IDs which were in the posts array and sticks it into the array
// exec starts the query
User.findOne({email: 'bob@gmail.com'}).populate('posts').exec(function(err, user) {
    if (err) {
        console.log(err);
    } else {
        console.log(user); // we will get a user with full post texts in 'posts' array instead of IDs
    }
});
```

---

## REST

REST (REpresentational State Transfer) is a mapping between HTTP routes and CRUD (Create, Read, Update, Destroy), pattern for defining the routes

### RESTful routes

This pattern makes applications more predictable and reliable to interact with other applications
- Note that browser forms don't support PUT or DELETE requests, they return it as a GET
- We need to include method-override to change that
    - Add ?_method=PUT to the action afterwards

<table>
    <tr style="text-align: left">
        <th>Name</th>
        <th>URL</th>
        <th>HTTP Verb</th>
        <th>Description</th>
        <th>Mongoose Method</th>
    </tr>
    <tr>
        <td>INDEX</td>
        <td>/dogs</td>
        <td>GET</td>
        <td>Display a list of all dogs</td>
        <td>Dog.find()</td>
    </tr>
    <tr>
        <td>NEW</td>
        <td>/dogs/new</td>
        <td>GET</td>
        <td>Display a form to create a new dog</td>
        <td>N/A</td>
    </tr>
    <tr>
        <td>CREATE</td>
        <td>/dogs</td>
        <td>POST</td>
        <td>Create a new dog, then redirect somewhere</td>
        <td>Dog.create()</td>
    </tr>
    <tr>
        <td>SHOW</td>
        <td>/dogs/:id</td>
        <td>GET</td>
        <td>Show info about one dog</td>
        <td>Dog.findById()</td>
    </tr>
    <tr>
        <td>EDIT</td>
        <td>/dogs/:id/edit</td>
        <td>GET</td>
        <td>Show edit form for one dog</td>
        <td>Dog.findById()</td>
    </tr>
    <tr>
        <td>UPDATE</td>
        <td>/dogs/:id</td>
        <td>PUT</td>
        <td>Update a particular dog, then redirect somewhere</td>
        <td>Dog.findByIdAndUpdate()</td>
    </tr>
    <tr>
        <td>DESTROY</td>
        <td>/dogs/:id</td>
        <td>DELETE</td>
        <td>Delete a particular dog, then redirect somewhere</td>
        <td>Dog.findByIdAndRemove</td>
    </tr>
</table>

Nested routes:

- We can't do comments/new because this url doesn't have any info about the particular post (e.g. campground in YelpCamp) we're adding this comment to
- We can nest the comment route (make it depended), associating it with the campground:
    - campgrounds/:id/comments/new GET
    - campgrounds/:id/comments POST

---

## Cleaning Up

### Export

We can clean up the code and make code more modular (we can use models multiple times) with creating separate files and requiring them in our application (we can do this for models, middleware, routes, etc.)

For models:
- Create a directory 'models'
- Separate code into pieces and store them in separate files
    - Note that you need to require all the dependencies in every file-module
- ``module.exports = ``
    - Similar to 'return' of the function
    - You export something out of the model (like post schema that you've created in post.js)
- To require:
    - ``var Post = require('./models/post');``
    - To reference the current directory we use './
    - ***When we require a folder, it will automatically look for index.js***
- ***Models/Index.js***
    - We can put all the needed models into this file so that we require only index.js
    - ``var mongoose = require('mongoose');``
    - ``mongoose.set('debug', true);``
    - ``mongoose.connect('mongodb://localhost:27017/todo-api', {useNewUrlParser: true});``
    - ``mongoose.Promise = Promise;``
    - ``module.exports.Todo = require('./todo');``
        - Requiring todo.js file and exporting it out
        - When we require the models directory, we're getting require/todo which in turn has schema and model
        - Basically, we're just getting the model out

### Routes Clean Up

We can separate the code in the big file with routes into several categorized ones:

```javascript
// inside of the index route:
var express = require('express');
var router = express.Router(); // new instance of the express router

// if there are dependencies:
var User = require('../models/user');
var passport = require('passport');

// *********************
// if we use index.js in models:
var db = require('../models'); // now we can access to a model as db.Model (e.g. db.Todo.find())
// *********************

// replace all 'app' to 'router'
// we're adding routes to router, not in the app itself
// export
module.exports = router;

// inside of app.js
var indexRoutes = require('./routes/index');
app.use(indexRoutes);

// other way to reduce duplication even more (doesn't really work for index routes since thay have no common things in their routes):
app.use('/campgrounds', campgroundRoutes);
// in campgrounds route file replace all '/campgrounds' in routes with '/', e.g.:

router.get('/', (req, res) => { // not /campgrounds
  // get campgrounds from DB
  Campground.find({}, (err, allCampgrounds) => {
    if (err) {
      console.log(err);
    } else {
      res.render('campgrounds/index', {
        campgrounds: allCampgrounds
      });
    }
  });
});

router.get('/new', (req, res) => { // not /campgrounds/new
  res.render('campgrounds/new');
});

// for comments:
app.use('/campgrounds/:id/comments', campgroundRoutes);
// although it won't find IDs (req.params.id), and to fix that we need to put this in the comments file (it will merge params from campgrounds and comments):
var router = express.Router({mergeParams: true});
```

---

## Authentication

### One Way Hashing

It's converting data into a fixed length has string; we can only recreate the hash if we know the original data

Once we have the has, we can't go back and figure out what the original data was

It's applicable for saving passwords on our server - we never save actual password; the data that the site saves should be just hash and nothing else

#### Example of Hashing

Password 'password' -> given to a hash library 'bcrypt' -> $2a$10$9Mconplm8A780pY6iB2q.eBwkdldFbnz2tSH2uqHEi5B9KTpR3O8.

### Sign In (Authentication)

Once we've saved that password as a has, the process of signing is like this:

$2a$10$9Mconplm8A780pY6iB2q.eBwkdldFbnz2tSH2uqHEi5B9KTpR3O8. (in database) -> user provides a password ('password') -> password goes through 'bcrypt' -> hash is generated again -> compare those hashes -> log in

### JWT (JSON Web Token)

jwt.io

Users don't want to enter their passwords on every page; we need some proof that we have logged in in the past

We can use JWTs as proof that we've logged in before - JWT is a web standard for storing signed data (sometimes called 'jot')

JWT Format:

```
Header: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
Payload: eyJ1c2VySWQiOiIxMjM0In0.
Signature: kud-czcx6yOSSQgB0lKbibHNFmlAJwrV8iRQ1Ha-r-Q
```

Signature is used to make sure that the header and the payload has not been tempered with; both the header and the payload, and then also a secret key are inputs to generate the signature
- A secret key is a string that we store on the server but we don't give people access to it (use env variables, install dotenv for connecting to .env file)

The server is the one that keeps track of that secret key, and only it can know about the secret
- If a hacker were to get access to that key then the hacker could also make fake JWTs with whatever header and payload that they want
  - So the thing that keeps this secure is the fact that this signature generation can only be done by the server, it can't be done by the client of by anyone else

#### Authentication

The server verifies the payload and the header to make sure it hasn't been tempered with, and if it hasn't, it looks in the payload to see that it has something like a user ID stored, and then if that user ID is there, that verifies that we've logged in in the past

When the user signs in, we give the generated JWT back to the client; every authenticated request after that the client needs to send us the JWT back (if we're the server)
- As long as the client sends a JWT for which that signature matches the signature that we generate on the server, we are authenticated
- Once we look in this payload, we can see what the user ID is to figure out which user is authenticated
- If something is changed (header, payload, signature) it will lead to an invalid signature problem
  - If the header or payload are different, since they are inputs to generate signature, it will result in the server trying to generate a different signature

#### Sending JWT to Server

The standard way to send the JWT is to include the JWT in the authorization header, and the first part of this header is typically 'Bearer' (type portion of authorization)

HTTP Header: Authorization: Bearer \<JWT> (e.g. Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0In0.kud-czcx6yOSSQgB0lKbibHNFmlAJwrV8iRQ1Ha-r-Q)

#### Bcrypt Usage

To see how we use one way hashing with bcrypt is used, go to 'advanced/warbler/warbler-server/models/user.js', '../handlers/auth.js', '../routes/auth.js'

## Auth with Passport.js

Passport.js is used for user authentication
- There are different modules: passport-local (user and password), password-facebook (for facebook auth), etc.

### Getting started

```javascript
var express = require('express'),
    mongoose = require('mongoose'),
    passport = require('passport'),
    LocalStrategy = require('passport-local'),
    passportLocalMongoose = require('passport-local-mongoose'),
    User = require('./models/user');

var app = express(); // this should be on top so the code below would know about app
app.set('view engine', 'ejs');
app.use(passport.initialize());
app.use(passport.session()); // these two lines setting passport up
app.use(require('express-session')({
    secret: 'Pick any sentence, a few English words', // this is used to encode and decode the information in the session
    resave: false,
    saveUninitialized: false // these two lines are required
}));

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser()); // these two methods are responsible for reading the session, unencoding data from it (deserialize), and encode and put it back into session (serialize); the methods are added when we plugged passportLocalMonggose into the User model

// in user model:
var mongoose = require('mongoose'),
    passportLocalMongoose = require('passport-local-mongoose');

var UserSchema = new mongoose.Schema({
    username: String,
    password: String
});

UserSchema.plugin(passportLocalMongoose); // it takes this package and adds a bunch of methods to it in order to have a user auth

module.exports = mongoose.model('User', UserSchema);
```

```javascript
// ====================
// ROUTES
// ====================

app.get('/', (req, res) => {
  res.render('home');
});

app.get('/secret', isLoggedIn, (req, res) => {
  res.render('secret');
});

// ====================
// AUTH ROUTES
// ====================

// show sign up form
app.get('/register', (req, res) => {
  res.render('register');
});
// handling user sign up
app.post('/register', (req, res) => {
  req.body.username
  req.body.password
  User.register(new User({username: req.body.username}), req.body.password, (err, user) => {
    // we make a new User object and we only pass in username (we don't save password to the database straight away)
    // we pass the password as a second argument, it will hash the password and store it (hash will be password, and 'salt' is used to 'unhash' it)
    if (err) {
      console.log(err);
      return res.render('register');
    } else {
      passport.authenticate('local')(req, res, function() {
        // this logs user in, takes care of the session, stores the correct info, runs the serializeUser using the 'local' strategy (can be other, like twitter, facebook)
        res.redirect('/secret'); // example of the page seen only for registered user
      });
    }
  })
});

// LOGIN ROUTES
// login form
app.get('/login', (req, res) => {
  res.render('login');
});
// login logic
app.post('/login', passport.authenticate('local', {
  // this is a middleware, it is the code that runs before our route callback
  successRedirect: '/secret',
  failureRedirect: '/login'
}), (req, res) => {
});

// log out
app.get('/logout', (req, res) => {
  req.logout();
  res.redirect('/');
});

function isLoggedIn(req, res, next) {
  // it is a middleware, args are according to usual naming convention
  // requst obj, response obj, next - next thing that needs to be done (we don't have to set it up, by putting it in as a middleware, express knows what to do next and handles this)
  if (req.isAuthenticated()) {
    return next();
  }
  res.redirect('login');
}
```

### User

Sending the user info to templates:

```javascript
// to one page:
app.get('/campgrounds', (req, res) => {
  // get campgrounds from DB
  Campground.find({}, (err, allCampgrounds) => {
    if (err) {
      console.log(err);
    } else {
      res.render('campgrounds/index', {campgrounds: allCampgrounds, currentUser: req.user}); // sending info about current user
    }
  });
});

// to all pages
app.use(function(req, res, next) {
  res.locals.currentUser = req.user; // whatever we put inside of res.locals is what's available inside of our template
  // req.user is created by express, it contains info about the current user (id and username), undefined if none
  next(); // moving to the next code
});
```

---

## GIT

GIT is a version control system, it helps you and other collaborators work on the big projects and see the history of the progect changes

GitHub is the largest website with git repositories

### Git basics

#### Git init

Git init is used to 'tell' git that this folder exists, initialize git in the directory
- It works only for this folder and nested folders, and doesn't know about any higher level folders
- Repo is repository, one per project
- Hidden folders start with .
    - Use ls -a to see them
- It makes the directory .git and tracks all of our changes in the files inside of the directory
- .git has all of the git information
- If you initialized it in the wrong directory
    - rm -rf
- Initializing a repo in a folder doesn't mean that it automatically knows and tracks every file and every change that you make
    - E.g. you can tell git to avoid tracking some files with sensitive information (actually just git wouldn't know about it, it would ignore it)

#### Git status

It 'asks' git for a status, usually typed before everything else

#### Git add

Git add is used to tell git about the files we want to track
- git add app.js
- git add .
    - Is used to add all of the stages not stages to commit

#### Git commit

Git commit is used to save the checkpoint, save the files that are added (tracked)
- If there were untracked files before this commit, reverting back to it would make it look like there were any other files which were added later
- git commit -m "add app file"
    - -m is message, every commit should have a message (in present tense)
- Changes should be staged to commit
    - It's a two stage process: modifying the file and then commiting it won't do anything (because we can commit multiple files)
    - If we added a file, and modified some other files, commiting without adding the modified files would make git commit only the new file
    - We need to add files with git add again before commiting

#### Git log

Git log gives all the history, the log of all the commits that we've made in the repo
- Opens a new terminal interface
    - Press Enter or arrows to scroll, you can't type any regular commands
    - To exit press 'Q'


#### Git checkout

It's used for going checking something out, or viewing, to change branches
- If you have some changes, and you go back to view old code, you will lose it so either commit or overwrite
- git checkout \<commit id> (e.g. 71abf9b67466c45eba560660a00216d17b8d3c09) is used to move from master branch
- HEAD detached at \<commit id>
    - HEAD is a point where you are, and here this point is at some older version of the code
        - O -> O -> O -> O (normal workflow, master at the last O)
        - O -> HEAD -> O -> Master (after 'detaching')
    - You can try ls to see that there are no any files which were created past this commit
    - When we use checkout we check the older code and then come back to where we were (sort of transports you back in time)
    - If you try to make changes when HEAD is detached, we run into a problem since we have to make a decision
    - To get back to master: git checkout master

### Git revert

Sometimes you want to revert changes, get back to some old code
- When it comes to deleting changes, it's pretty rare, most developers don't use it often
- There are several ways of getting back to old code (depending if you want to keep commits past that point, or not, etc.)
- One of them git revert --no-commit \<commit we want to revert to>..HEAD and then git commit -m "undo changes" (or smth)
    - This will revert everything from the HEAD back to the commit hash
    - It will recreate that commit state in the working tree as if every commit since had veen walked back
    - --no-commit lets git revert all the commits at once
    - This is a safe and easy way to rollback to a previous state: no history is destroyed, so it can be used for commits that have already been made public
        - We can get back to it using git log
    - If you try to revert while having some unstaged changes, it will give a warning

---

## Deploying

### Environment Variables

Environment is where the code is being run, and its variable e.g. process.env.PORT (PORT is the variable name, usually in CAPS)

Variables that are changed when we change environment (server) of the app (avoiding hardcoding things like ports, IPs, etc.); they are hidden and not exposed to anyone and not related to Node or something else, it's a universal concept

Creating:

```bash
# local
$export DATABASEURL=mongodb://localhost:27017/yelp_camp
# on heroku you need to config vars in settings or
$heroku config:set DATABASEURL=mongodb://Dmitry:Rustyiscool1@ds213615.mlab.com:13615/yelpcamp
```

Accessing:

```javascript
process.env.DATABASEURL;
mongoose.connect(process.env.DATABASEURL);
// setting a default value (if a value doesn't exist), it's a good practice
var url = process.env.DATABASEURL || 'mongodb://localhost:27017/yelp_camp';
```

## Useful Modules

### Cors

This a bit of middleware that's going to allow us to make requests that are cross origin (Cross-Origin Resourse Sharing) - we tell our server to send headers in the response that notify the browser that it can actually allow for a cross-origin request

So by default the origin policy is a security policy on the web says that we cannot make those requests because we don't want to write any kind of malicious javascript on another domain

E.g. our backend is going to be starting on localhost 3001, and our frontend application is going to have a different port number, and when there are requests from one domain to another right off the bat, it's a violation of the same origin policy because those port numbers are different

They might both be local hosts but they're still going to be different ports, so we want to allow requests from different domains to come into our API and to do that we're going to use this Cors module - a middleware that's going to allow for certain headers to be sent and set to allow for cross origin requests

### Morgan

Morgan allows us to see request that are coming in to our data (logging)