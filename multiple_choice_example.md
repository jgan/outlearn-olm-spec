// Challenge :: Code execution order ::
{  
Consider the code sample below:

```
var fs = require('fs') // require is a special function provided by node
var myNumber = undefined // we don't know what the number is yet since it is stored in a file

function addOne() {
  fs.readFile('number.txt', function doneReading(err, fileContents) {
    myNumber = parseInt(fileContents)
    myNumber++
  })
}

addOne()

console.log(myNumber)
```

When the code gets executed, which line will be executed first?

( ) `myNumber++`
( ) `console.log(myNumber)`  
( ) One or both of them will not be executed

<!-- () `myNumber++` {{selected: Note that myNumber++ is passed as a callback method to an asynchronous method}, {unselected: You correctly noticed that myNumber++ is executed later despite being located earlier in the code}}  
(x) `console.log(myNumber)`  {{selected: You correctly noticed that this line get executed first because the preceding line is part of a callback method that is delayed because a file read operation.}, {unselected: Note that readFile is an asynchronous method which delays the execution of the previous line}}  
() One or both of them will not be executed {{selected: There is nothing in the code that would keep both lines from being executed}, {unselected: You are correct that asynchronous functions will eventually complete their execution}}  
|| Remember that Javascript has both synchronous and asynchronous methods ||  
|| Remember that readFile is an asynchronous method and anything that accesses disk will generally be asynchronous because it takes longer to complete ||   -->
}
