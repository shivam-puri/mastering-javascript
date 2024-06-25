In a regular functions , the arguments property (or variable whatever) is always available as an array of arguments inside the function whether any arguments are passed or not.

![[Pasted image 20240501182958.png]]

But in arrow function that's not the case :

![[Pasted image 20240501183130.png]]

If you need access to the arguments passed to an arrow function, you can use the rest parameter syntax `...args`:

This syntax allows you to gather up all the arguments passed to the arrow function into an array called `args`, which you can then manipulate as needed.

Just like this : 

![[Pasted image 20240501183432.png]]

---

### Can u tell me how arguments is available in the regular functions?

In regular functions, the `arguments` object is a special array-like object that is automatically available within the function's scope. It provides access to all the arguments passed to the function, ***regardless of whether they were explicitly declared as named parameters or not.***

![[Pasted image 20240501183959.png]]

### When not to use Arrow functions

![[Pasted image 20240501203033.png]]

Arrow functions in JavaScript ==should not be used as methods, constructors, or event handlers==. They also should not be used with the `arguments` object, [[call apply and bind]] methods.


### Why arrow functions can't be used as constructors
You cannot have named arrow functions, they are always anonymous.
And therefore Constructor functions can't be an arrow functions because they are named!.

![[Pasted image 20240501185348.png|700]]

----
### Why arrow functions can't be used as methods 

==Before reading the cases, keep this in mind that, The arrow function doesn't have its own `this` context, so it inherits the `this` context from its `lexical scope`==
#### Consider the three cases  -

#### Case 1

![[Pasted image 20240501194113.png]]

#### Case 2

![[Pasted image 20240501194147.png]]

#### Case 3

![[Pasted image 20240501194250.png]]

Explanation :

Case 1 - 
* In the case of the object literal, `this` refers to the global object because the object is created in the global scope. (Refer [[Arrow functions#A Confusing thing]] for more clarity)
* But on logging `me.getThis`, we get the this pointer pointing to the `me` object. This is because, when the `getThis` function was called as a method, the this was set to `me` (because `me.getThis` was called. Still confused? Refer [[this]])

Case 2 - 
* The arrow function doesn't have its own `this` context, so it inherits the `this` context from its `lexical scope`, and the lexical scope's context was global, (since the object was declared globally).
* Hence when printed` me.getThis()` it logged the `window` i.e. the `global` object

Case 3 - 
* In this case, `me.getThis()` was called, so the anonymous function inside the `getThis` method pointed to the `me` object. and the arrow function inside the `setTimeOut` function inherited it's lexical scope i.e. the `getThis` function this time, which is pointing to `me` 
* And hence on logging `me.getThis()` , the object pointer printed.
---
### Why not to use arrow functions for prototypes

![[arrowfunction and prototype.mp4]]
### Why not to use arrow functions as event handlers

Important ! Don't Skip

https://youtu.be/ajTvmGxWQF8?list=PL1PqvM2UQiMoGNTaxFMSK2cih633lpFKP&t=1509

---
#### A Confusing thing  


![[Pasted image 20240501190954.png|600]]

![[Pasted image 20240501191007.png|600]]

In the case of the object literal, `this` refers to the global object because the object is created in the global scope. In the case of the constructor function, `this` refers to the instance being created (`p1`), and assigning `this` to a property inside the constructor captures a reference to the instance itself.

Keep this in mind before reading Real fun.