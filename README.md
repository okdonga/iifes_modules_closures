# iifes_modules_closures


- syntactical gymnastics we have to go through to get private state
- global state is bad.  why???
- js only has function scope
- in js, functions are first order objects
- that means, functions can be returned as the result of expressions, just like an integer
- variables can hold functions just the same as holding an integer
- and functions are invoked with ()

### Function Declaration

```javascript
// function declaration
function someName() { 
  //
}
```

### IIFE (iffy)


```javascript
// 1. assign a function to a variable
// 2. invoke it later with ()
var someName = (function () {
  //...
});
...
someName();
```

```javascript
// anonymous function expression
(function () {
  //...
})();
```

```javascript 
// avoiding name collisions with external state
(function() {
    var nameUsedCommonlyAcrossLibs = "my stuff";
  //...
})();

// what will you get if you run this in console?
$ nameUsednameUsedCommonlyAcrossLibs


Now, x is only assigned to the value "hello" for the duration of the IIFE. 

```javascript
// binding important variable names for the scope of the IIFE
(function ($) {
     $(document).ready(function () {
          ...
      });
})(jQuery);
