Resource : https://youtu.be/fVXp7ZWjlO4

```javascript
console.log(this)
```


![[Pasted image 20240430094701.png]]

---

1. **The `this` Keyword**:
    - In JavaScript, `this` refers to the context in which a piece of code, typically a function's body, is supposed to run.
    - It allows functions to access and operate on the properties of the object to which they belong.
2. **Dynamic Binding**:
    
    - ==The value of `this` in JavaScript is dynamically determined at runtime, based on how a function is invoked, not how it is defined.==
    - When a function is invoked as a method of an object (e.g., `obj.method()`), `this` points to the object that the method is attached to.
    - When invoked as a standalone function (e.g., `func()`), `this` typically refers to the global object (`window` in browsers, `global` in Node.js) in non-strict mode, and `undefined` in strict mode. `And this is true for all functions, doesnt matter if a fn is global or local`
    - ![[Pasted image 20240501174702.png]]
1. **Methods to Set `this`**:
    
    - The [[call apply and bind|Function.prototype.bind()]] method can create a new function with a specified `this` value, which doesn't change when the function is invoked.
    - The [[call apply and bind|apply()]] and [[call apply and bind|call()]] methods allow you to explicitly set the `this` value for a particular function call.
4. **[[Arrow functions]]**:
    
    - Arrow functions have a different behavior regarding `this`. They inherit `this` from the parent scope at the time they are defined, not when they are invoked.
    - This means that the value of `this` inside an arrow function is determined by the surrounding lexical scope.
    - Arrow functions do not have their own `this` binding. Therefore, their `this` value cannot be changed using [[call apply and bind|call, apply or bind]].
    - This behavior makes arrow functions particularly useful for preserving the context of `this` in callbacks and avoiding issues with dynamically changing `this` in nested functions.

In summary, the value of `this` in JavaScript is dynamic and depends on how a function is invoked. ==Arrow functions inherit `this` from the surrounding lexical scope, making them useful for preserving context, while regular functions have their `this` value determined by the way they are called==.


### Standalone function

```javascript
console.log(this)  // //Window {window: Window, self: Window, document: document, name: '', location: Location, …}

function printName(name, kills){
	console.log("name : ", name, " kills : ", kills);
	console.log(this)  //Window {window: Window, self: Window, document: document, name: '', location: Location, …}
	function anotherFun(){
		console.log(this); // //Window {window: Window, self: Window, document: document, name: '', location: Location, …}
	}
	anotherFun();
}
printName("eren", "1.6 billion")
```

### Method of an object

```javascript
class Character {
	constructor(name, kills){
		this.name = name;
		this.kills = kills
	}

	print(){
		console.log(this)
		console.log("name : ", this.name, " kills : ", this.kills);
	}
}


const eren = new Character("eren", "1.6 billion");
eren.print() // Character {name: 'eren', kills: '1.6 billion'}
```

---
# `this` and callback functions

Here in this case, the callback function may be present inside the Person constructor but it's [[this|this]] pointer points to the global object.

Because when you pass a function as an argument to `setTimeout`, it's no longer called as a method of an object; it's called as a standalone function, and its `this` value defaults to the global object (`Window` in a browser environment).

```javascript
function Person(name){
    this.name = name
    this.speak = function(){
        console.log("tatakae", this)
    }

    setTimeout(function(){
        console.log("logging this reference of callback fn", this)
    }, 2000)
}

const erenJeager = new Person('Eren Jeager') //after 2s :  logging this reference of callback fn Window {window: Window, self: Window, document: document, name: '', location: Location, …}

erenJeager.speak() // tatakae Person {name: 'Eren Jeager', speak: ƒ}
```

This can be fixed using 2 ways (or more probably)

## 1. using [[call apply and bind|bind()]] 
```javascript 
function Person(name){
    this.name = name
    this.speak = function(){
        console.log("tatakae", this)
    }

    setTimeout(function(){
        console.log("logging this reference of callback fn", this)
    }.bind(this), 2000)
}

const erenJeager = new Person('Eren Jeager') //after 2s :  logging this reference of callback fn Person {name: 'Eren Jeager', speak: ƒ}
```

## 2. using [[Arrow functions]]

##### Note! that the bind, call or apply cannot be used on arrow functions

```javascript
function Person(name){
    this.name = name
    this.speak = function(){
        console.log("tatakae", this)
    }

    setTimeout(() => {
        console.log("logging this reference of callback fn", this)
    }, 2000)
}

const erenJeager = new Person('Eren Jeager') //after 2s :  logging this reference of callback fn Person {name: 'Eren Jeager', speak: ƒ}
```