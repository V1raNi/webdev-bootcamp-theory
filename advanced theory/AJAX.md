# AJAX

Asynchronous Javascript and XML

#### Table of Contents

- [AJAX](#ajax)
      - [Table of Contents](#table-of-contents)
  - [What Is AJAX](#what-is-ajax)
    - [XML and JSON](#xml-and-json)
      - [XML](#xml)
      - [JSON](#json)
  - [Requests](#requests)
    - [XMLHTTP Request (XHR)](#xmlhttp-request-xhr)
      - [Problems with XHR](#problems-with-xhr)
    - [Fetch](#fetch)
      - [Fetch Options](#fetch-options)
      - [Fetch Error Handling](#fetch-error-handling)
      - [Problem with Fetch](#problem-with-fetch)
  - [jQuery and Axios](#jquery-and-axios)
    - [jQuery](#jquery)
      - [$.ajax](#ajax)
      - [$.get](#get)
      - [$.post](#post)
      - [$.json](#json)
    - [Axios](#axios)
      - [Axios Error Handling](#axios-error-handling)

## What Is AJAX

AJAX is not:
- A library
- A framework
- A technology

AJAX is an approach to webdev, a concept, a way of structuring apps

It appeared it 2005 as a way to combine HTML, CSS, JS, DOM, XMLHTTP Requests and create apps that can update without refreshing

With AJAX, websites can send and request data from a server in the background without disturbing the current page (led to today's single page apps with features like infinite scroll)

Making requests with Javascript:
- XMLHTTP Request
- Fetch API
- 3rd Party - jQuery, Axios, etc.

### XML and JSON

They are data formats used in response data (mostly); API's don't respond with HTML, they respond with pure data

#### XML

Extended Markup Language

Semantically similar to HTML, but it does not describe presentation like HTML does

```xml
<item>
  <title>Some title</title>
  <author>Some author</author>
</item>
```

#### JSON

JavaScript Object Notation replaced XML (so mostly it's now AJAJ); it looks almost exactly like JS Obj

```JSON
'item': {
  title: 'Some title',
  author: 'Some author'
}
```

## Requests

Workflow is usually event -> request -> data

### XMLHTTP Request (XHR)

The first, original way to make requests (rarely used)

```javascript
var XHR = new XMLHttpRequest(); // create new instance of request

XHR.onreadystatechange = function() {
  // readyState can have 5 different states (from 0 to 4: unsent, opened, headers_receiver, loading, done)
  if (XHR.readyState == 4 && XHR.status == 200) {
    console.log(XHR.responseText);
  } else {
    console.log('There was a problem!');
  }
};

XHR.open('GET', 'https://api.github.com/zen'); // we tell what type of request we want to do, and URL
XHR.send(); // what we send, we initiate request
```

#### Problems with XHR

1. Ugly, bulky syntax
2. It's 16 years old
3. No Streaming

### Fetch

Is a replacement, update for XHR

```javascript
fetch(url) // basically, this is a request already; it returns a promise
.then(function(res) {
  console.log(res); // whole response object; we can use something like res.status
})
.catch(function(error) {
  console.log(error);
});
```

Parsing JSON with Fetch:

```javascript
fetch(url).then(function(res) {
  return res.json(); // it is promise as well (which is returned to .then after fetch(url))
  // .then (from below) can be inserted here as well (not after the closing brackets of the first .then)
  // functionally they are the same
}).then(function(data) {
  console.log(data); // we can work with data as an object now
}).catch(function() {
  console.log('Problem!');
});
```

#### Fetch Options

```javascript
fetch(url, {
  method: 'POST', // get by default
  body: JSON.stringify({
      name: 'blue',
      login: 'bluecat',
    }) // including object with options
    // we don't have to use stringify, but it makes it easier, we don't have to make manualle eash of this a string, worry about " or '
    // also we can provide headers and other stuff, see more at https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch
})
.then(function(response) {
  // do something
})
.catch(function(error) {
  // handle error
});
```

#### Fetch Error Handling

Syntax:

```javascript
.fetch(url)
.then(function (res) {
  // we put error checking in its own .then
  // first thing we want to do is to ckeck if everything is ok
  if (!res.ok) {
    throw Error(404); // up to us
    // that will trigger .catch below
  }
  return res; // if things are ok, we return response and .then is run
}).then(function (response) {
  console.log('ok');
}).catch(function(error) {
  console.log(error);
});
```

Example:

```javascript
var btn = document.querySelector('button');

btn.addEventListener('click', function() {
  var url = 'https://api.github.com/users/coltasdas';
  fetch(url)
  .then(function() {
    console.log('EVERYTHING IS FINE!');
  })
  .catch(function() {
    console.log('There is a problem');
    // if the user is wrong (not 'Colt'), 'Everything is fine' will be returned anyway
    // we will see 'There is a problem' if there is a problem with a request itself, e.g. if the internet is off, or there is a problem connection, or with credentials, but not with the actual response
  });
});

// we need to rewrite our code to handle different statuses

btn.addEventListener('click', function() {
  var url = 'https://api.github.com/users/coltasdas';
  fetch(url)
  .then(handleErrors)
  .then(function(request) {
    // to check the status we can put if request.status === 200... but we can use request.ok
    // request.ok is a built-in property which can be used instead of 'if status != 200' or something
    if (!request.ok) {
      // throw error
      console.log('Error with response status!')
    }
    // this code will still run
    console.log('EVERYTHING IS FINE!');
  })
  .catch(function() {
    console.log('There is a problem');
  });
});

// refactor using different promises to streamline things
btn.addEventListener('click', function() {
  var url = 'https://api.github.com/users/coltasdas';
  fetch(url)
  .then(handleErrors)
  .then(function(request) {
    if (!request.ok) {
      throw Error(request.status);
    }
    return request // we need to return something if there is no error
  })
  .then(function(request) {
    console.log('Everything is fine');
    console.log(request);
  })
  .catch(function(error) {
    console.log(error); // equals to whatever we 'throw', i.e. here response status
  });
});

// refactor code
var btn = document.querySelector('button');
btn.addEventListener('click', function() {
  var url = 'https://api.github.com/users/coltasdas';
  fetch(url)
  .then(handleErrors)
  .then(function(request) {
    console.log('EVERYTHING IS FINE!');
    console.log(request);
  })
  .catch(function(error) {
    console.log(error);
  });
});

function handleErrors (request) {
  if (!request.ok) {
    throw Error(request.status);
  }
  return request;
}
```

#### Problem with Fetch

The main problem is browser compatibility (IE doesn't have fetch at all)

---

## jQuery and Axios

### jQuery

It can be used not only for manipulating the DOM, but for requests as well (without fetch)

We need to include it first (look for jQuery CDN)

#### $.ajax

This is the main, 'base' method, other three are shorthand methods

It just creates an XHR under the hood
- https://api.jquery.com/jQuery.ajax/ (there are examples at bottom)
- ``Query.ajaxSettings.xhr = function() {
	try {
		return new window.XMLHttpRequest();
	} catch ( e ) {}
};``



```javascript
$.ajax({
  method: 'GET',
  url: 'some.api.com',
  // datatype: 'json',
})
.done(function(res) {
  console.log(res);
  // data is automatically parsed (dataType parameter); we don't need to JSON.parse or data.json().then
  // default: intelligent guess, e.g. JSON
  // it's better to be explicit
})
.fail(function() {
  // do something
  // fail will automatically check response code, i.e. will trigger on 404, not only problem with request itself
})
```

#### $.get

Load data from the server using a HTTP GET request ($.ajax method: 'get')
- https://api.jquery.com/jQuery.get/

```javascript
$("#getBtn").click(function(){
  $.get('https://api.github.com/users/colt')
  // note that parameters are strings, so jQuery.get( url [, data ] [, success ] [, dataType ] )
  .done(function(data){
    console.log(data);
  })
  .fail(function(){
    console.log("ERROR!");
  })
});
```

#### $.post

Load data from the server using a HTTP POST request
- https://api.jquery.com/jQuery.post/

```javascript
$("#postBtn").click(function(){
  var data = {name: "Charlie", city: "Florence"};
  $.post("www.catsarecoolandsoaredogs.com", data)
  .done(function(data){
    console.log("HI!");
  })
  .fail(function(){
    console.log("ERROR!");
  })
});
```

#### $.json

Load JSON-encoded data from the server using a GET HTTP request
- https://api.jquery.com/jQuery.getJSON/

```javascript
$("#getJSONBtn").click(function(){
  $.getJSON("https://api.github.com/users/colt")
  .done(function(data){
    console.log(data);
  })
  .fail(function(){
    console.log("PROBLEM!");
  })
});
```

### Axios

If we don't need whole jQuery (there are many features which could be not needed), we can use some other library for making request, such as Axios

Axios is a lightweight HTTP request library (creates XHR under the hood; we need to install or include it first)
- Make XMLHttpRequests from the browser
- Make http requests from node.js
- Supports the Promise API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Client side support for protecting against XSRF
- https://github.com/axios/axios

```javascript
// basic example
axios.get(url)
.then(function(res) {
  console.log(res.data)
})
.catch(function(e) {
  console.log(e);
})

// usage example (get)
var url = 'https://opentdb.com/api.php?amount=1';
axios.get(url)
.then(function(res){
  console.log(res.data.results[0].question);
})
.catch(function(){
  console.log("ERR");
})
```

#### Axios Error Handling

```javascript
// html:
// <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
// <button>CLICK</button>
// <section id="comments"></section>

var btn = document.querySelector("button");
var section = document.querySelector("#comments");
btn.addEventListener("click", sendRequest);

function sendRequest(){
  axios.get("https://jsonplaaskjldceholder.typicode.com/comments", {
    params: {
      postId: 1 // we can pass in an object with parameters (instead of ?postId=1 etc.)
    }
  })
  .then(addComments)
  .catch(handleErrors)
}

function addComments(res) {
  res.data.forEach(function(comment) {
    appendComment(comment);
  });
}

function appendComment(comment) {
  var newP = document.createElement("p");
  newP.innerText = comment.email;
  section.appendChild(newP);
}

function handleErrors(err) {
    if (err.response) {
      // err.response if used for bad statuses
      console.log("Problem With Response ", err.response.status);
    } else if (err.request) {
      // err.request is used for bad requests
      console.log("Problem With Request!");
    } else {
      console.log('Error', err.message);
    }
  }
```