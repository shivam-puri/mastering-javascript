        
        
Standard input ( stdin) is the default input device that a program uses to read data in computer programming`
#### The JavaScript interpreter performs automatic garbage collection for memory management. This means that a JavaScript programmer generally does not need to worry about destruction or deallocation of objects or other values

JavaScript supports an object-oriented programming style. Loosely, this means that rather than having globally defined functions to operate on values of various types, the types themselves define methods for working with values. To sort the elements of an array a, for example, we don’t pass a to a sort() function. Instead, we invoke the sort() method of a:

```javascript
a.sort()  // The object-oriented version of sort(a)
```

```javascript
console.log(typeof null) // object
```

`The null value represents the intentional absence of any object value.`
----

## The Prototype Chain

`In programming, inheritance refers to passing down characteristics from a parent to a child so that a new piece of code can reuse and build upon the features of an existing one. JavaScript implements inheritance by using objects. Each object has an internal link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype and acts as the final link in this prototype chain. It is possible to mutate any member of the prototype chain or even swap out the prototype at runtime, so concepts like static dispatching do not exist in JavaScript.`

--- 
## Division by 0, NaN and Infinity

`Division by zero is not an error in JavaScript: it simply returns infinity or negative infinity. There is one exception, however: zero divided by zero does not have a well-defined value, and the result of this operation is the special not-a-number value, NaN. NaN also arises if you attempt to divide infinity by infinity, take the square root of a negative number, or use arithmetic operators with non-numeric operands that cannot be converted to numbers`

---
## const num = 10 & const num = new Number(10)

`The main difference between ``const num = 10 and  const num = new Number(10)`` is that the former creates a primitive number variable while the latter creates a Number object.

`- A primitive number variable is a variable that stores a number value directly. It is the most efficient way to store numbers in JavaScript.
`- A Number object is a wrapper object that stores a number value. It is less efficient than a primitive number variable, but it provides some additional functionality, such as methods for converting numbers to strings and vice versa.

`In most cases, you should use `const num = 10` to create a number variable. There is no need to use `const num = new Number(10)` unless you need to use the additional functionality provided by the Number object`

----

## NaN

`The not-a-number value has one unusual feature in JavaScript: it does not compare equal to any other value, including itself. This means that you can’t write x === NaN to determine whether the value of a variable x is NaN. Instead, you must write x != x or Number.isNaN(x). Those expressions will be true if, and only if, x has the same value as the global constant NaN`


```javascript
const num = NaN;

// no output
if(num == NaN) console.log("1. Num is NaN");

// console logs
if(num != num) console.log("2. Num is NaN")

const num2 = new Number(NaN);

if(Number.isNaN(num2)) console.log("3. num2 is NaN");         // does not print
if(Number.isNaN(num)) console.log("4. num is NaN");           // prints


```


```javascript
const z1 = 0;
const z2 = -0
z1 === z2 // true
1/z1 === 1/z2  // false
```

---- 

## Primitive datatypes are Immutable in JavaScript

```javascript
let string = "shivam"
string = string + " puri"
console.log(string) // shivam puri
```

`what's happening in the code is not modifying the original string but creating a new string by concatenating puri to the original string shivam, and then assigning that new string to the variable string`.

`So, you're not actually changing the original string; you're creating a new one and assigning it to the same variable. This is why it seems like the string is being modified, but in reality, a new string is being created.`

`The same goes for all primitive datatypes`
#### More @ [[Mutability & Immutability]]

----

## Comparing Two Objects

  `Objects are not compared by value: two distinct objects are not equal even if they have the same properties and values. And two distinct arrays are not equal even if they have the same elements in the same order.` 
#### `Two object values are the same if and only if they refer to the same underlying object.`

```javascript
let a = [];
let b = a;            // now both a and b points to the same reference
b[0] = 1;
console.log(a[0])    // 1, (the change in b is also reflected in a because both are pointing to the same reference)
a == b               // true
```


```javascript
let a = [1, 2, 3];
let b = [];              // a discinct array we will copy into

for(let i = 0; i < a.length; i++) {
	b[i] = a[i];         // copy an element of a into b
}
a == b     // false. 
console.log(b)     // [1, 2, 3]
let c = Array.from(b)   // In ES6, copy arrays from Array.from()

```

----
`Variables and constants declared as part of a for, for/in, or for/of loop have the loop body as their scope, even though they technically appear outside of the curly braces.`

---

## Why curly braces are used to export multiple functions from a module

In JavaScript's module system (introduced in ECMAScript 2015, or ES6), curly braces (`{}`) serve a specific purpose when dealing with named exports. They allow you to explicitly list the functions, variables, classes, or objects you want to make available for use in other modules.

JavaScript provides two main ways to export elements from a module:

1. **Named Exports:** Used for exporting multiple items or giving a custom name to an exported item. These require curly braces in both the export and import statements.
2. **Default Export:** Used for exporting a single entity as the default export of a module. This doesn't require curly braces in the import statement.

In your example:
```
export { capitalize, roundToDecimalPlace };
```

You're performing named exports of two functions: `capitalize` and `roundToDecimalPlace`. This means other modules can import these functions specifically:

```
import { capitalize, roundToDecimalPlace } from './yourModule.js'; // Assuming 'yourModule.js' is the file containing the exports
```

Here, curly braces are necessary on the import side because you're importing multiple named exports.
If you had a single function to export as the default, you wouldn't use curly braces:

```
export default greet; // Assuming 'greet' is the function
```

Then, importing would be:
```
import greet from './yourModule.js';
```
---

**Traditional Client-Side JavaScript**:
* In traditional client-side JavaScript, when a global variable or constant is declared within a `<script>` tag in an HTML document, it becomes accessible to all subsequent `<script>` tags within the same HTML document.
* This means that if you define a global variable `x` in one `<script>` tag, you can access it in any other `<script>` tag within the same HTML document.
* The scope of the global variable is limited to the HTML document in which it is defined. Any script running within that HTML document can access and modify the global variable.

**JavaScript Modules (Client-Side and Node.js)**:
    
* With the introduction of JavaScript modules, the scope of global variables behaves differently.
* In JavaScript modules (both in client-side and Node.js environments), the scope of a global variable is limited to the file in which it is defined. This means that a global variable declared in one module is not automatically accessible to other modules.
* Each module has its own scope, and variables declared within a module are only accessible within that module unless explicitly exported.
* This modular approach helps in encapsulating code and avoiding naming conflicts between different modules.


---
## Objects vs Instances

In the jungle of code, sometimes we trip over a vine of confusion called objects and instances. Basically, think of an object like a blueprint or a prototype in JavaScript or Java. It's like a badass model for a Lamborghini. It has properties like color, model, max speed, you get the drill, and methods like accelerate, brake, or change gear.

But here's the kicker, that model ain't gonna drive you anywhere, it's just a scheme. Now imagine, you took that model and you crafted your own Lamborghini. This specific Lambo you got, it's an instance of the 'Lamborghini' object. It's a real deal, you can drive it, paint it any color, change its oil and grind gears. It's an individual manifestation, with its own unique properties, born out of the object blueprint, yet not restricted by it.

In tech terms, the object defines a class of data, with its structure (properties) and actions (methods). The instance, on the other hand, it's an actual data entity, buzzing with life, crafted from the class, but living its own unique life.

---

## Methods vs Internal Methods

In JavaScript, methods are functions that are defined on objects. They are used to perform actions on the object. For example, the `toString()` method is a method that is defined on all objects in JavaScript. It is used to convert an object to a string.

Internal methods are methods that are defined by the JavaScript engine. They are not accessible from JavaScript code. For example, the `[[GetPrototypeOf]]` internal method is used to get the prototype of an object.

The main difference between methods and internal methods is that methods are accessible from JavaScript code, while internal methods are not. Internal methods are used by the JavaScript engine to implement the language.

Here is a table summarizing the key differences between methods and internal methods:

| Feature       | Method                             | Internal Method                     |
| ------------- | ---------------------------------- | ----------------------------------- |
| Accessibility | Accessible from JavaScript code    | Not accessible from JavaScript code |
| Definition    | Defined by the programmer          | Defined by the JavaScript engine    |
| Purpose       | Used to perform actions on objects | Used to implement the language      |
|               |                                    |                                     |

Here are some examples of methods:

JavaScript

```
// The toString() method converts an object to a string.const obj = {  name: "John Doe",  age: 30,};console.log(obj.toString()); // "[object Object]"// The push() method adds an element to the end of an array.const arr = [1, 2, 3];arr.push(4);console.log(arr); // [1, 2, 3, 4]
```

Here are some examples of internal methods:

JavaScript

```
// The [[GetPrototypeOf]] internal method gets the prototype of an object.const obj = {  name: "John Doe",  age: 30,};const prototype = obj.[[GetPrototypeOf]];console.log(prototype); // {}
```

---
