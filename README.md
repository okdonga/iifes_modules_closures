# IIFEs, Modules, Closures

IIFEs and modules are the syntactical gymnastics we have to go through to get usefull OO stuff in JS
  - private state, 
  - encapsulation (global state is bad.  why???)
  - namespacing

## Pertinent JS recap

- js only has function scope
- in js, functions are first order objects
  - functions can be passed as params
  - functions can be returned as the result of expressions
  - variables can hold functions
- functions are invoked with ()
- anything wrapped in () becomes an expression

### Function declaration and invocation

```javascript
// function declaration
function someName() { 
  //
}
```

```javascript
// function expression
// (defines an anonymous function and assigns it to a var)
var someName = (function () {
  //...
});
```

```javascript
// function invocation
someName();
```

## IIFE (iffy)

This is an IIFE

```javascript
(function () {
  //...
})();
```

- anonymous function expression
- the parens around the declaration turn it into an expression
- immediately invoked
- avoids polluting global scope
- allows us to hide private state
- avoids variable hoisting

```javascript
var something = "external stuff";
(function () {
  var something = "my internal stuff";
  //...
})();
// what will the following line log?
console.log(something);
```

- Binding important variable names for the scope of the IIFE

```javascript
(function ($, window, document, undefined) {
  // ...
})(jQuery, window, document);
```

- Defensive semicolon

```javascript
;(function () {
  //...
})();
```

## JS Module Pattern

- We can use IIFEs as building blocks for modules
- In addition to hiding internal state, modules also allow us to expose a public API and provide namespacing
- This is a standard pattern for writing JS libraries

```javascript

// mathy.js
var mathy = (function() {
  
  var factorial = (function(n) { 
    return Math.factorial(n);
  });
  
  var square = (function(n) {
    console.log("square");
    privateFunction();
    console.log("square out");
    return n*n;
  });
  
  var log = (function(n) {
    return Math.log(n);
  });
  
  var privateFunction = (function() {
    console.log("can't call this from outside");
  });
  
  return {
    factorial: factorial,
    square: square,
    log: log
  }
})();
// ...
> mathy.square(4);
4
> mathy.privateFunction
undefined
```


## Closures

- in js, if you use the function keyword inside another function, you are creating a closure
- the local variables in the inner function can remain accessible after returning from the outer function
- a special kind of object that combines two things: a function, and the environment in which that function was created
- environment consists of any local variables that were in-scope at the time that the closure was created.
```javascript
function greet(name){
    var text = 'Hello ' + name; // local variable
    return function(){ console.log(text); }
}

var printGreeting = greet("pocket gophers");

console.log(printGreeting.toString());
printGreeting();
```

Closure as function factory

- you can initialize your closure with state

```javascript
function messageAfter(seconds) {
  return function(thingToMessage) {
    return 'Waited ' + seconds + ' seconds to say: ' + thingToMessage;
  };
}

var messageAfter5Seconds = messageAfter(5);
var messageAfter2Seconds = messageAfter(2);

messageAfter5Seconds("pocket gophers));
messageAfter2Seconds("pocket gophers));

messageAfter5Seconds(5)("pocket gophers");
messageAfter2Seconds(2)("pocket gophers");
```

## Resources

- [IIFEs](http://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
- [modules, module loaders, ES6 built in modules](]https://www.airpair.com/javascript/posts/the-mind-boggling-universe-of-javascript-modules)
- [MDN on closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
