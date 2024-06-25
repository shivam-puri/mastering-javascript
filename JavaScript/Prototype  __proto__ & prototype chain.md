Awesome Resource - https://youtu.be/1UTqFAjYx1k?t=838. So awesome that I did this : 

![[Colorcode channel comment.png]]

![[Prototype&proto.excalidraw| 1000]]

# Let me tell you something CRAZZYY!!
suppose `Human` is a class and `me` is an object of `Human` class, then.. Then, THENNN!!! :
 `Human.prototype === me.__proto___` is `true`!!!

***==Prototype is a property that only exists in constructor function or class==***
***==__proto__ is a property that exists in all objects==***


# First of all, Understanding Inheritance in JavaScript

==In JavaScript, inheritance is a mechanism that allows objects to share properties and methods with each other==. This happens through a prototype system, which works as follows:

- **Objects and Prototypes:** All objects in JavaScript have a prototype. A prototype is an object from which another object inherits properties and methods.
- **Prototype Chaining:** When you try to access a property or method on an object, JavaScript first checks whether that property or method exists on the object itself. If it doesn’t find it, it searches the prototype of that object. This process continues until you find the desired property/method or until you reach the “Object.prototype” object, the last in the **_prototype chain_**.
- `**__proto__**`: The `__proto__` property is a way to directly access the prototype of an object. Although its use is discouraged in production, it helps visualize the prototype pipeline.
- ==**Creating Inheritance:**== ==You can create inheritance in JavaScript in several ways. One of the most common is to use constructor functions or classes (introduced in ES6) to create objects with shared methods and properties through their prototypes.==
- **Objects and Inheritance:** When you create an object from a constructor function or class, it automatically inherits all the properties and methods of the prototype of that function or class.
- **Overwriting and Adding Properties:** Inheriting objects can overwrite or add their own properties and methods, which will not affect prototypes or other inheriting objects.

Concepts such as `__proto__`, Prototype chain and constructor functions were mentioned just to introduce us to what inheritance is (since they go together), but we will see how it works and delve deeper as the text progresses.

Inheritance in JavaScript is based on prototypes, where objects inherit properties and methods from other objects, forming a chain of prototypes. This allows for the creation of reusable code and the efficient organization of functionality in JavaScript.

# The meaning of __proto__ and prototype

## __proto__

==The `__proto__` property is present in all JavaScript objects, it points to the prototype object from which the object was created, allowing the inheritance mechanism to find properties and methods that are not directly in the object, but rather in the prototype==.

The `__proto__` is not part of the ECMAScript standard, but is supported by browsers.

It is worth mentioning that its use is not directly recommended, as it is a “legacy” practice.

```javascript
const me = {  
	name: 'Felipe',  
};  
me.__proto__.sayHello = function() {  
	console.log('Hello!');  
}  
  
me.sayHello(); // Output: Hello!
```

Note that the definition of the `sayHello` method does not exist when we create the `me` object, but we add it to the **_prototype chain_** using `__proto__`.

## prototype

==The `prototype` is a property that only exists in constructor functions (or classes in modern JavaScript)==. ==It is used to define shared properties and methods that will be inherited by all objects created from this constructor function.== And it is a fundamental part of the prototyped inheritance mechanism in JavaScript.

```javascript
function Me(name) {  
this.name = name;  
}  
Me.prototype.sayHello = function() {  
console.log(`Hello! My name is ${this.name}!`);  
}

const me = new Me('Felipe');

me.sayHello(); // Output: Hello! My name is Felipe!
```

# Literal objects, constructor and explicit functions

It is worth mentioning that internally, literal objects and notations become explicit functions (built-in constructors), and we can use what we have seen so far to confirm this:

```javascript
const fn = () => {}  
const arr = []  
const obj = {}  
const str = 'Hello'  
const num = 123  
const bool = true  
// { ... }
fn.__proto__ === Function.prototype   // Output: true  
arr.__proto__ === Array.prototype     // Output: true  
obj.__proto__ === Object.prototype    // Output: true  
str.__proto__ === String.prototype    // Output: true  
num.__proto__ === Number.prototype    // Output: true  
bool.__proto__ === Boolean.prototype  // Output: true
```
  

Here we are comparing the `__proto__` (which is the prototype from which the object was created), with the prototype (a property that only exists in constructor functions). But we can make this clearer by comparing the `__proto__` of our literal notation with that of the explicit function:

```javascript
fn.__proto__ === new Function().__proto__   // Output: true  
arr.__proto__ === new Array().__proto__     // Output: true  
obj.__proto__ === new Object().__proto__    // Output: true  
str.__proto__ === new String().__proto__    // Output: true  
num.__proto__ === new Number().__proto__    // Output: true  
bool.__proto__ === new Boolean().__proto__  // Output: true
```

This rule applies to all construction functions.

## Is everything an object?

The phrase “everything is an object” in Javascript is famous, but how true is it?

In JavaScript, all functions (whether created literally or through constructors) inherit from `Object`. This is because in JavaScript, functions are a type of object. This means that functions have access to all properties and methods that are present in the `Object` prototype. Let's use what we've seen so far to confirm this.

```javascript
const str = '__proto__ and prototype chain';  
  
str.__proto__ === String.prototype; // Output: true
```

Here we saw that our string inherited the properties of the constructor function, but since the constructor function also inherits, by default, from Object, so when we chain the inheritance to compare with the constructor function of Object, we have:

```
str.__proto__.__proto__ === Object.prototype; // Output: true
```

Now let’s imagine even more…

```
str.__proto__.__proto__ === Object.prototype && {}.__proto__ === Object.prototype;  
// Output: true
```

Cool, right?

## Is every object null?

As we see it, everything is an object, but what happens when we try to reach the end of the chain? Where does our object inherit from? We will see!

```
console.log(str.__proto__.__proto__.__proto__); // Output: null
```

At the end of the chain (`__proto__`), we receive a `null` return, as we have reached the end of the inheritance.

# What does prototype chain mean?

As mentioned, **_prototype chain_** is basically the basis for inheritance in Javascript. But how does this work?

When we try to look for a property in an object, the engine that runs Javascript will try to access the object itself to see if the property exists, if the property does not exist, it will look in the object’s prototype, if it does not exist in the object’s prototype object, it will look for the prototype of the prototype and so on… generating a true **_prototype chain_** 🤯

```
const me = {  
	name: 'Felipe',  
};  
  
me.age;        // Output: undefined  
me.sayHello(); // Output: (Error) me.sayHello is not a function
```

This message is famous for JS developers, but what happened behind the scenes was exactly this, after declaring the literal object `me` (), we tried to access the `age` property using `me.age` and received `undefined` because age does not exist in the variable declaration or in the prototype tree, and as we saw, when we reached the end of the `__proto__` chain, we received `null`, as it was finished without finding what we were looking for. The same happens with the `sayHello` method

Note that if we try to use the `hasOwnProperty()` function directly on our object, we will get the expected return without having explicitly declared the function:

```
me.hasOwnProperty('age')  // Output: false  
me.hasOwnProperty('name') // Output: true
```
This happens because this method is present in the **_prototype chain_** of our object, if we inspect the variable we will see exactly this:

![Inspect object variable to see hasOwnProperty method](https://miro.medium.com/v2/resize:fit:780/1*GuzdFS0ANBpqrBV7mMOxNg.png)

The same happens with the other methods that we have available in arrays, such as `.find`, `.filter`, `.map`, among others, for example.

![Inspect array variable to see array methods](https://miro.medium.com/v2/resize:fit:875/1*c8xrYcdcnpI2EhTSbxlKvQ.png)

# Prototype chain with constructor functions

Let’s create a simple type of inheritance using constructor functions to make it clearer how this works:

```
function Person() {}  
Person.prototype.name = "Felipe";  
Person.prototype.sayHello = function () {  
  return `Hello! My name is ${this.name}!`;  
};  
  
function Professional() {}  
Professional.prototype = Object.create(Person.prototype);  
Professional.prototype.title = "Developer";
```

Here we create a `Person` function, add the `name` property and the `sayHello` function through the prototype.

Next, we create the function `Professional` whose prototype is inherited from `Person` and add a title as a property.

```javascript
// Note : The output true after strict (===) check means that both are pointing to the exact same things in the memory
console.log(Professional.prototype.__proto__ === Person.prototype); // Output: true;
// This concludes that 
```

As we can see, the behavior was as expected.

But if we try to use the methods we created and inherited directly, we will receive an error similar to `Professional.sayHello is not a function`. This happens because if we don't call `new`, the first `__proto__` will always be the instance of `Function`, without inheriting our classes. To access classes without new, we can access them directly via `prototype`.

```javascript
Professional.prototype.sayHello() // Output: Hello! My name is Felipe!
```
When we call `new`, `__proto__` receives the prototype:

```javascript
const professional = new Professional();  
professional.name;       // Output: Felipe  
professional.title;      // Output: Developer  
professional.sayHello(); // Output: My name is Felipe!
```

## But…

Inheritance in JavaScript can be implemented with either `prototype` or `class` syntax, and both approaches have their advantages and disadvantages. However, the `class` syntax was introduced in ECMAScript 2015 (ES6) to make inheritance and object-oriented programming in JavaScript more intuitive and readable. Let's discuss why the `class` syntax is generally preferred over direct use of `prototype`:

1. **Readability and Maintenance:** The syntax of `class` is more declarative and similar to that of other object-oriented programming languages, making the code easier to read and understand. This makes collaboration and code maintenance easier, as people familiar with object-oriented programming in other languages will find the `class` syntax more accessible.
2. **Clarity of Intent:** The `class` keyword emphasizes the intention of creating a class and defining methods and properties in an object-oriented structure. This makes the code clearer regarding its purpose.
3. **Encapsulation:** The `class` syntax makes it easy to define private class members by using the `#` declaration before the member name. This allows for better encapsulation, which is important for security and code organization.
4. **More Explicit Inheritance:** The `extends` keyword is used explicitly in the `class` syntax to define inheritance from a parent class. This makes it clear which classes are being extended and makes it easier to trace the inheritance hierarchy.
5. **Clearer Builders:** The `class` syntax allows constructors to be defined in a clearer and more standardized way using the name `constructor`. This makes object creation and argument passing more readable.

Although the `class` syntax is generally preferred, it is important to note that both approaches (using `prototype` and `class`) have their place in JavaScript and can be useful in different situations. Choosing between them depends on the specific needs of your project and your team's familiarity with each approach. It is important to understand both forms of inheritance in JavaScript to make informed decisions when writing object-oriented code.

# Conclusion

Inheritance in JS is a complex and confusing topic (both to understand and to write), but it is essential that we understand how it works together with `__proto__` and `prototype`, as they are directly linked even today.

In summary, inheritance in JavaScript is a fundamental concept for understanding how the language works, and it is based on prototypes. Through the **_prototype chain_**, objects inherit properties and methods from other objects, allowing the creation of reusable code and the efficient organization of functionality in JavaScript.

Additionally, we discussed the use of `__proto__` and `prototype` to access object prototypes and constructor functions. We note that the `class` syntax is generally preferred in JavaScript for creating inheritance hierarchies due to its readability, clarity of intent, and support for encapsulation features.

Ultimately, the choice between using `prototype` and `class` syntax depends on project needs and personal preference, but `class` syntax is a more modern and readable approach that is most commonly used in the community. of JavaScript development.

Thank you for staying here, see you later!

### Just an example for FUN

![[Pasted image 20240425131323.png|500]]
![[Pasted image 20240425131317.png|500]]