## Execution context

==Execution context in JavaScript is the environment in which JavaScript code is executed==. It contains information about the current scope, variables, and functions.

Each time a function is called, a new execution context is created. The execution context for a function contains the following information:

- The current scope: This is the scope in which the function is called.
- The variables: This is a list of all the variables that are accessible to the function.
- The functions: This is a list of all the functions that are accessible to the function.

The execution context for a function is destroyed when the function returns.

==There are two types of execution contexts in JavaScript:==

- Global execution context: This is the execution context for the global scope. It is created when the JavaScript engine starts up.
- Function execution context: This is the execution context for a function. It is created when the function is called.

### How Global Execution context works

![[Execution context Drawing|800]]

The global execution context has two phases : 
1. The memory allocation phase (also called the creation phase)
2. The code execution phase

In the memory allocation phase, the variables and functions are allocated with the memory in the global object (i.e. the `window` object)

Consider the following code : 
```js
var character = "Anya"

function getFav(){
	console.log("Peanuts")
}

console.log(character)
getFav()

```

During the memory allocation phase (i.e. before the code execution) , the javascript engine allocates memory to each and every variables and functions. 

the variables are allocated with a special keyword `undefined` whereas the functions are saved with their as it is body

![[Pasted image 20240502200853.png]]

![[Pasted image 20240502201017.png]]

Now after the memory allocation phase the code execution phase starts and javascript executes the code line by line.

let's see how the above code is executed : 
* the first line updates the `character` present in the global object with the value `Anya`
* The second line is ignored because the function is not invoked.
* During the execution of third line the javascript engine searches for the value of character, and it returns the stored value from the global object and hence logs it to the console.
* The 4th line invokes the function, the `getFav` function is searched in the global object and then executed.

Now that's the case where we accessed the variable after the declaration and initialization.
Consider the example given below : 

```js
console.log(character) // undefined
var character = "Anya"
```

In the above code, the console log, does not through any error, even we are accessing a variable before it's initialization, and further returns `undefined` on the console. 
This is because the variable `character` was already allocated with an `undefined` keyword during the memory allocation phase, and this is the concept behind **HOISTING**

# Hoisting

Hoisting is a JavaScript mechanism where function declarations and variable declarations are moved to the top of their scope before code execution. This means that functions and variables can be used before they are declared.

Now actually the hoisting is just a visualization, what exactly happens behind the scene is what we saw just now in the execution context.

> Whenever a function is invoked in javascript, a whole new execution context (functional execution context) is created, and the [[this]] keyword is set to the function this time. ==(During the global execution context the [[this]] was set to the `Window` object)==. 
> The local variable and local functions of that function are stored in that execution context (which are obviously not available to the outer scope (we are talking about `let` and `const` here. The `var` is an exception here)). If a variable is not available/present in the function's scope, the function looks for the variable in the outer scope.

```js

getFaviourate(); // Peanuts

function getFaviourate(){
	console.log("Peanuts")
}
```

In the above code, the function is invoked before declaration, this is the simplest example of function hoisting.

### Are let and const variables hoisted?

==Yes, let and const declarations are hoisted in JavaScript==, but they aren't stored in the global object. They are stored in a different memory space than global. And they can't be accessed from that memory space until they are initialized.

```js 
var a = 10
let b = 20
```

// before code execution

![[Pasted image 20240502211051.png]]

Here's an analogy for hoisting let and const declarations: they are moved to the top of the block, but they remain locked until you reach the declaration in your code.
==Accessing a variable with let and const before initialization will throw a reference error==. This is also known as the temporal dead zone, where compilers know its existence but can't access it because it's uninitialized.

==In other words, Temporal dead zone is the time since when the `let` or `const` variable was hoisted and till it is initialized with some value==

### Arrow functions and functional expressions

```js
getFav()  // Uncaught TypeError: getFav is not a function
var getFav = function() {
	console.log("Peanuts")
}
```

```js
getFav()  // Uncaught TypeError: getFav is not a function
var getFav = () => {
	console.log("Peanuts")
}
```

Hoisting is when a function declaration is parsed before any code is executed, allowing a function to be called later in the code. ==When a **function expression** is used, the function is saved inside a variable, making it a value==. During hoisting, the variable is hoisted, but the value is not. This means that if the function is called by only writing the variable name, an error will occur.