# IIFEs, Closures, Modules

These are the syntactical gymnastics we have to go through to get useful OO stuff in js, such as
  - private state
  - public API
  - encapsulation (global state is bad.  why???)
  - namespacing

## Pertinent js recap

- js only has function scope
- In js, functions are first order objects
  - functions can be passed as params
  - functions can be returned as the result of other functions
  - variables can hold functions
- Functions are invoked with `()`
- Anything wrapped in `()` becomes an expression

### Function declaration and invocation

```javascript
// function declaration
function someName() { 
  // ...
}
```

```javascript
// function expression
// (defines an anonymous function and assigns it to a var)
var someName = (function() {
  // ...
});
```

```javascript
// function invocation
someName();
```

## IIFE (iffy)

This is an IIFE

```javascript
(function() {
  //...
})();
```

- An anonymous function expression
- The parens around the declaration turn it into an expression
- The parens at the end cause it to be immediately invoked
- Avoids polluting global scope
- Allows us to hide private state
- Avoids variable hoisting
- This is going to be built in to ES6 as an [Arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```javascript
(function() {
  var something = "my internal stuff";
  //...
})();

// what will the following line log?
console.log(something);
```

## JS Module Pattern

- We can use IIFEs as building blocks for modules
- We can hide internal state
- We can expose a public API
- We can provide namespacing
- This is a standard pattern for writing JS libraries

```javascript
// mathy.js
var mathy = (function() {
  
  var topSecret = "shhhhhh";
  
  var factorial = (function(n) { 
    return Math.factorial(n);
  });
  
  var square = (function(n) {
    console.log("square");
    secretFunctionTime();
    console.log("square out");
    return n*n;
  });
  
  var log = (function(n) {
    return Math.log(n);
  });
  
  var secretFunctionTime = (function() {
    console.log(topSecret + ", can't call this from outside");
  });
  
  return {
    factorial: factorial,
    square: square,
    log: log
  }
})();

// What will these log...?
console.log(mathy.square(4));
// ????
console.log(mathy.secretFunctionTime)
// ????
console.log(mathy.topSecret)
// ????
```

## Closures

- In js, if we use the function keyword inside another function, we are creating a closure
- The local variables in the inner function can remain accessible after returning from the outer function
- A closure is a function having access to the parent scope, even after the parent function has closed
- A closure is a special kind of object that combines two things: a function, and the environment in which that function was created
- Environment consists of any local variables that were in-scope at the time that the closure was created

```javascript
var returnData = (function() {
  // only created once
  var largeDataSetTakesForeverToGet = [1,2,3]; // this could be an ajax call
  
  return function() { 
    console.log(largeDataSetTakesForeverToGet) 
  };
})();

// getting that same variable back each time
returnData();
returnData();
returnData();
```

- This internal state can be mutable, but only through API we provide

```javascript
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();

add();
add();
add();
```

#### Closure as function factory

- We can initialize a closure with state
- A closure with its own state is created for each function invocation

```javascript
function messageAfter(seconds) {
  return function(thingToMessage) {
    return 'Waited ' + seconds + ' seconds to say: ' + thingToMessage;
  };
}

var messageAfter5Seconds = messageAfter(5);
var messageAfter2Seconds = messageAfter(2);

messageAfter5Seconds("dragonfliez 4eva");
messageAfter2Seconds("dragonfliez 4eva");

messageAfter(7)("dragonfliez 4eva");
messageAfter(453764576)("dragonfliez 4eva");
```


## Resources

- [IIFEs](http://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
- [MDN on closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [Currying and closures in JS](http://engineering.cerner.com/blog/closures-and-currying-in-javascript/)
- [Modules, module loaders, ES6 built in modules](https://www.airpair.com/javascript/posts/the-mind-boggling-universe-of-javascript-modules)
