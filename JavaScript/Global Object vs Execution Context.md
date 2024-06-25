
### ==The global object is a part of global execution context, which is a part of the Execution Context==.

![[Global Context and Execution Context | 800]]

The global context and execution context are related but not the same. Let's differentiate them:

1. **Global Object**:
    - ==The global object is an object that exists in JavaScript environments such as web browsers (where it's referred to as `window`) and Node.js (where it's referred to as `global`).==
    - It contains properties and methods that are accessible globally throughout your code.
    - In a web browser, the global object (`window`) includes properties like `document`, `console`, `setTimeout`, `Math`, etc.
    - In Node.js, the global object (`global`) includes properties like `console`, `setTimeout`, `Buffer`, etc.
    - All global variables and functions are attached as properties to the global object.
2. [[Execution Context]]:
    
    - ==An execution context, on the other hand, is a concept that surrounds the environment in which JavaScript code is executed.==
    - Each time a function is invoked, a new execution context is created to manage the execution of that function's code.
    - The execution context includes information such as the function's arguments, local variables, the value of `this`, and a reference to its outer lexical environment (closure).
    - It consists of three main components: the Variable Environment (which contains local variables and function declarations), the Lexical Environment (which represents the scope chain), and the `this` keyword.
    - When a function is called, a new execution context is pushed onto the call stack, and when the function completes, its execution context is popped off the stack.

In summary, ==the global object is a built-in object that holds global variables, functions, and other properties, accessible throughout your code. The execution context, on the other hand, is a runtime concept representing the environment in which code is executed== , including details like local variables, scope, and the value of `this` within a function call. While the global object is a part of the global context, the execution context is more about managing the execution of code, particularly within function calls.
