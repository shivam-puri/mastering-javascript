
In JavaScript, a callback function is a function that is passed as an argument to another function and is executed after some operation has been completed. Callback functions are a fundamental concept in JavaScript, especially in asynchronous programming, where they are commonly used to handle asynchronous tasks such as fetching data from a server, processing files, or handling user interactions.

Here's how callback functions work:

1. **Passing a Function as an Argument**: In JavaScript, functions are first-class citizens, which means they can be assigned to variables, passed as arguments to other functions, and returned from functions. When you pass a function as an argument to another function, it's called a callback function.
    
2. **Execution of the Callback Function**: The function receiving the callback function as an argument can then invoke (or "call back") the callback function at some point during its execution. This usually happens after some asynchronous operation has been completed, or when a certain condition is met.
    
3. **Handling Asynchronous Operations**: Callback functions are commonly used to handle asynchronous operations, where the result of the operation is not immediately available. Instead of waiting for the operation to complete synchronously, you provide a callback function that will be called once the operation has finished, allowing you to continue executing other code in the meantime.

```js
function fetchData(callback) {
    // Simulate fetching data asynchronously
    setTimeout(() => {
        const data = ['apple', 'banana', 'orange'];
        callback(data);
    }, 1000);
}

// Define a callback function
function processData(data) {
    console.log('Data received:', data);
}

// Call the fetchData function and pass the processData function as a callback
fetchData(processData);

```

In this example:

- The `fetchData` function simulates fetching data asynchronously using `setTimeout`.
- The `processData` function is defined separately and passed as a callback to the `fetchData` function.
- When the data is available (after 1 second in this case), the `fetchData` function calls the `processData` function and passes the fetched data to it.

Callback functions are widely used in JavaScript to handle events, asynchronous operations, and to create reusable and modular code. However, they can sometimes lead to callback hell (nested callbacks) when dealing with multiple asynchronous operations. This is where newer asynchronous patterns like [[Promises]] and async/await come in handy, offering cleaner and more manageable alternatives to callback-based code.

> Javascript is a synchronous and single-threaded language, which means it can only do one thing at a time in a specific order, but due to callbacks we can do async things inside JavaScript


> Time Tide & JavaScript waits for None.

```js

// The setTimeout function stores the callback function in a separate space and attaches a timer with the callback function, after 5000 ms callback is called
// It does not wait here for 5000 ms to expire.
setTimeout(function() {
	console.log("5 sec timer ended")
}, 5000)

function x(y) { 
	console.log("x") 
}

x(function y(){ 
	console.log("y") 
});

```

Now in the video below, we are debugging on three break-points,  line : `2`, `3` and`12
Initially the debugger is on breakpoint `2`

Notice how the `setTimeout (async)` suddenly pops up in the call stack out of no where in 5 seconds.  

![[callstack-settimeout-callback.mp4]]


 Each and every operation is performed in the call stack. this call stack is what we call the main thread. If an operation takes too much time in the call stack preventing other functions/operations to run, we call it blocking the main thread. 


```js
const button = document.getElementById("button")
console.log(button)

button.addEventListener("click", function getPeanut(){
	console.log("Peanut")
})
```

![[anya_surprised_excited.png|right|120]] In the above example I put the debugger in the line `4` (`console.log("clicked")`) and the moment I click the button, the function `getPeanut` callback gets pushed into the call stack. 


[Closures with event listener | Also a popular interview question](https://youtu.be/btj35dh3_U8?list=PLxnjbfm5MCHFbRlyVCAqpJFdIzPN_IPID&t=811)


![[how closures are stored|1000]]

#### Why do we need to remove event listeners?
Event listeners are heavy, it takes memory. and even when the call stack is empty the program doesn't free up the memory. (even if the call stack is empty, you'll find the click event in the above example in the event listener tab.)

Related - [[Async JS and Event Loop]]



---

For the below example see how the closure works in event listeners tab

```js
const button = document.getElementById("button")
const button2 = document.getElementById("button2")

function handleAddEventListener(){
	let count = 0;
	button.addEventListener("click", function getPeanut(){
		console.log("Peanut", ++count)
	})

	button2.addEventListener("click", function eatPeanut(){
		if(count){ console.log("Peanuts left", --count) }
		else console.log("No Peanuts left 🥺")
	})
}

handleAddEventListener()
```


# Try this out
```js
function step1(callback) {
  setTimeout(function() {
    console.log("Step 1 completed");
    callback();
  }, 1000);
}

function step2(callback) {
  setTimeout(function() {
    console.log("Step 2 completed");
    callback();
  }, 1000);
}

function step3(callback) {
  setTimeout(function() {
    console.log("Step 3 completed");
    callback();
  }, 1000);
}

function step4(callback) {
  setTimeout(function() {
    console.log("Step 4 completed");
    callback();
  }, 1000);
}



step1(function(){
	step2(function(){
		step3(function(){
			step4(function(){
				console.log("This is the end..")
			})
		})
	})
})
```
