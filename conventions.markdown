Use Array.prototype.slice instead of [].slice because it doesn't allocate a new array, therefore using less memory.

Assume input is valid, but catch and identify the error when it's not valid.

Generally prefer assuming perfect conditions, and then catching and identifying bad input errors specifically.

Prefer variables instead of passing arguments into an iife (immediately invoked function expression):
```javascript
(function(){
  var window = this; // this === window in the browser, this === global in node.
  var undefined;
  //...
})();

// DON'T do this:
(function(window, undefined) {
  // what does window equal? You have to scroll all the way down..
  //...
})( this );
```


Commenting format:
I generally space and capitalize comments on their own line:
```
// Comment on it's own line.
```
and do no space and no capitalization when at the end of a line:
```
if (something === SomeOtherThing) { //small case
```


Also, this: http://benalman.com/news/2012/05/multiple-var-statements-javascript/
