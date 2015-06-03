Some conventions I tend to use personally. Not a comprehensive document.
Google's JS style guide served as a basis for my js style.

ES6! With Babel

And with ES6 closing the gap between javascript and coffeescript, I
think it's pretty worthwhile to stop using *optional* semicolons. See
@izs blog post on the subject:
http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding

Use Array.prototype.slice instead of [].slice because it doesn't allocate a new array, therefore using less memory.

Assume input is valid, but catch and identify the error when it's not valid.

Generally prefer assuming perfect conditions, and then catching and identifying bad input errors specifically.

Prefer variables instead of passing arguments into an ify (immediately invoked function expression):
```javascript
(function(){
  var window = this; // this === window in the browser, this === global in node.
  var undefined;
  //...
})()

// DON'T do this:
(function(window, undefined) {
  // what does window equal? You have to scroll all the way down..
  //...
})( this )
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
