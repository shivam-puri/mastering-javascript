

`global object in JavaScript, which is an object that exists in the global scope and holds properties and methods that are accessible from anywhere in your JavaScript code.`

Here's a breakdown of the key points mentioned:

1. **Definition of a global object**: A global object always exists in the global scope of a JavaScript environment.
    
2. **Global object in different environments**: Depending on the environment in which JavaScript code is running, the global object may have different names and interfaces. For example:
    
    - In a web browser, the global object is typically the `Window` object.
    - In a web worker, it's the `WorkerGlobalScope` object.
    - In Node.js, it's an object called `global`.
3. **Accessing the global object**: The `globalThis` global property provides a standard way to access the global object regardless of the execution environment.
    
4. **Creating properties of the global object**:
    - Variables declared with `var` at the top level (outside of any function) become properties of the global object. For example, `var x = 10;` creates a property `x` on the global object.                        `This totally makes sense and supports the let const and var story`
    - Function declarations at the top level also become properties of the global object.
    - However, variables declared with `let` or `const` at the top level do not create properties of the global object. (***And hence they can only be accessed only in the file in which they are declared***) This is different from `var`.
5. **Automatic addition to global scope**: Properties of the global object are automatically added to the global scope. This means you can access these properties directly without referencing the global object explicitly.
    

Overall, this passage explains how the global object works in JavaScript and how it interacts with different environments and variable declarations. Understanding this concept is crucial for writing JavaScript code that behaves consistently across various environments.`

```javascript
var erenJeager = 10;                   // Exists in global scope
let mikasa = 20                        // Does not exists in global scope
var fn = function(){console.log("hey")}   // exists in global scope
function kill(){ console.log("kills");}  // exists in global scope

var save = function(){console.log("Save")}   // exists in global scope, but not if declared with let or const
var attack = () => {console.log("attack")}   // - || -

console.log(globalThis)
```


## Global Variables || Global Constants

- When we talk about a "global variable" or "global constant," we mean a variable or constant declared outside of any code block (like a function or loop). These declarations reside at the top level of a script file.

- In JavaScript, variables declared with `var` keyword used to have global scope regardless of where they were declared within a script. However, this behavior led to issues like accidental overwriting and hoisting quirks. 

- Whenever a `var` is declared in the global space it becomes a property of the global object. But the same is not true with `let` and `const`

- With the introduction of `let` and `const` in ES6 (ES2015), JavaScript gained better control over variable scoping. Variables declared with `let` or `const` still have global scope if declared outside of any code block, but their scope is limited to the script file (or module) they are defined in, as opposed to the entire HTML document.

Consider two JavaScript files  *script1.js* and *script2.js*  present inside an HTML document 

```javascript
// script1.js
var erenBday = "mar 30"
let mikasaBday = "feb 10"
const arminBday = "nov 3"
console.log("script1's globalThis", globalThis) // only contains erenBday

// script2.js
console.log("script2's globalThis", globalThis) // contains erenBday from script1

// Actually both the script1 and script2 are a part of the same HTML document and therefore they share the same global object.
```

Functions are also stored in the global objects as it is. 

```javascript
function getErenKills() {console.log("1.6 billion kills")}  // present in the global object (functions are stored as it is during the memory execution phase of execution context) 

var  getMikasaKills = function() {console.log("21 kills")}  // present in the global object (this is a VAR expression and var are stored in the global object) 
const getArminKills = () => {console.log("10k kills")}      // absent in the global object

console.log(getErenKills()); // 1.6 billion kills 😭
```