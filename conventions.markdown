

Use Array.prototype.slice instead of [].slice because it doesn't allocate a new array, therefore using less memory.

Assume input is valid, but catch and identify the error when it's not valid.

Generally prefer assuming perfect conditions, and then catching and identifying bad input errors specifically.
