## Javascript Timers, Callbacks and Extras

For this assignment you will be combining everything you have learned so far in JavaScript to implement the following functions....but first - let's make sure we understand what we are talking about!

### Part 1 - Definitions 

Please define these terms in 1-3 sentences.

1. Function expression

  A function expression is a statement creating a function (with or without a name) and storing it in a variable. Because of hoisting, functions that are created this way are "undefined" until they are assigned a value (the function body). 

2. Function declaration

  A function declaration is a way of creating a named function without the use of the keyword 'var'. Because of the way the JavaScript interpreter works, these functions are evaluated before running other code, so that even if they are called before they are explicitly declared, they will still work.

3. Asynchronous code

  Asynchronous code is code that is not necessarily executed in order. Basic examples of functions that use asynchronous code are setTimeout() and setInterval().

4. Higher Order Function

  A higher order function is a function with a callback. In otherwords, a function that accepts another function as an argument/parameter.

5. The Call Stack

  The call stack is the list of currently running functions. It is a last-in-last-out structure.  

6. The Heap

  The heap is where any variables or data that may be used in the future of the program are held. It has no particular order.

7. The Queue

  The queue is a first-in-first-out structure that holds tasks to be sent to the call stack when it is clear. It gets its tasks from the heap. 

### Part 2 - Concepts

Please answer the following questions in 2-4 sentences.

- What is the difference between these two functions?

 The first is a function expression, the second is a function declaration. In the first case, the variable sayHi will be hoisted, and with contain  "undefined" until it is assigned a value. In the second case, the function will be evaluated before running the code, and thus can be called from anywhere in the program.

```js
 var sayHi = function(){
   return "Hi!";
 }
 
 function sayHello(){
   return "Hello!";
 }
```

- What is the difference between using the `var` keyword inside of a function vs. outside of a function?

 Using the 'var' keyword within a function will limit its scope to that function. It will be accessible anywhere in that function and in any functions within that function. Using the var keyword outside a function will make that variable accessible in that function (as long as it is  declared before the function references it.). 

- Why do we need/use higher order functions? What does it enable us to do? 

  In the example from class, we used a higher order function when we created the "math" function. This function accepted a callback function as one of its arguments and was able to then perform all the operations of the previously defined four functions. It can make our functions much more powerful and versatile. It can also allow us to pass functions in as arguments that we do not yet know the value of. In our class example, we could have done something like:
  math(add(2+3), subtract(3,1), multiply), even if we did not yet know what the value of the arguments was.

- Explain in your own words, what happens to the `stack` and `queue` when the following code is run (please also include what it will output)
  
  -logFirst() gets added to the call stack
  -console.log("first") gets added to call stack
  -console.log("first") is executed and gets removed from the stack ("first" is logged)
  -logFirst() is complete and is removed from the stack
  -setTimeout(logSecond,0) is added to the callstack
  -setTimeout(logSecond,0) sends logSecond to the heap and gets removed from the callstack
  -logThird is added to the callstack, and after 0 milliseconds, logSecond is sent to the queue
  -console.log("third") is added to the call stack
  -console.log("third") is executed and gets removed from the callstack ("third" is logged) (logSecond is still in the queue)
  -logThird() is now complete and gets removed from the call stack
  -the stack is now clear, and logSecond moves from the queue to the stack
  -console.log("second") is added to the stack
  -console.log("second") is executed and removed from the stack ("second" is logged)
  -logThird is removed from the stack.


```js
function logFirst(){
  console.log("first");
}

function logSecond(){
  console.log("second");
}

function logThird(){
  console.log("third");
}

logFirst();
setTimeout(logSecond,0);
logThird();
```

### Part 3 - Application (this will be tough!)

- Write a function called `liftoff` that starts at 10 and `console.log`s the previous number after a second. Once the count is at 0, the function should stop decrementing and `console.log("liftoff!")`

var i = 10;

var timerId = setInterval(liftoff, 1000);

function liftoff(){
  if(i === 0)
  {
    console.log("liftoff!");
    clearInterval(timerId);
    return;
  }
  console.log(i);
  i--;
}

- Write a function called `each` which takes in an array and a callback, and invokes the callback on each element in the array.  This function should **not** return anything. This function should accept an array as its first parameter and a callback function as its second.

```js
function each( arr, callback)
{
   for(var i = 0; i < arr.length; i++)
   {
    callback(arr[i]);
   } 

}


For example: 

```js
 each([1,2,3], function(element){
  	// this will console.log 1,2,3
    console.log(element);
    // but it will return undefined
 })
```

- Write a function called `transform` which takes in an array and a callback and **returns** a new array with the results of the invoked callback on each element in the array. This function should accept an array as its first parameter and a callback function as its second.

function transform(arr, callback)
{
  var newArr = [];
  for(var i = 0; i < arr.length; i++)
  {
    newArr.push(callback(arr[i]));
  }
  return newArr;
}


For example: 

```js
 transform([1,2,3], function(element){
  	return element * 3;
  	// this will return [3,6,9]
 })
```

### Bonus

- Refactor your each and transform function so that the callback function accepts the the index as a second parameter and the array you are iterating over as a third parameter 

function each( arr, i, callback)
{
   for(; i < arr.length; i++)
   {
    callback(arr[i]);
   } 

}

function transform(arr, i, callback)
{
  var newArr = [];
  for(; i < arr.length; i++)
  {
    newArr.push(callback(arr[i]));
  }
  return newArr;
}


- Write a function called `filter` which takes in an array and a callback and **returns** a new array with the results of the invoked callback on each element in the array **only** if they pass a condition. The callback you pass in **MUST** return `true` or `false`

function filter(arr, callback){
  var newArr = [];
  
  for(var i = 0; i < arr.length; i++)
  {
    if(callback(arr[i]))
    {
      newArr.push(arr[i]);
    }
  }

  return newArr;
}


For example:

```js
filter([1,2,3,4], function(element){
  return element % 2 === 0;
  // this will return [2,4] 
}) 
```

