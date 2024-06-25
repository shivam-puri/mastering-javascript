A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives you access to an outer function's scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

Consider the following example code:

```javascript
function init() {
  var name = "Mozilla"; // name is a local variable created by init
  function displayName() {
    // displayName() is the inner function, that forms the closure
    console.log(name); // use variable declared in the parent function
  }
  displayName();
}
init();
```

`init()` creates a local variable called `name` and a function called `displayName()`. The `displayName()` function is an inner function that is defined inside `init()` and is available only within the body of the `init()` function. Note that the `displayName()` function has no local variables of its own. However, since inner functions have access to the variables of outer functions, `displayName()` can access the variable `name` declared in the parent function, `init()`.
### Another Example

```javascript
function Human(name){
    function sayHello() {
        console.log(`Hi I'm ${name}`)
    }
    return {
        sayHello
    }
}

const anya = new Human("Anya")
anya.sayHello() // Hi I'm Anya
```

![[anya_serious.png|right|120]]

==Even though the `Human` function has finished executing, the `sayHello` method retains access to the `name` variable due to closure==. This means that `sayHello` "closes over" the `name` variable, capturing its value. So when you call `anya.sayHello()`, it still has access to the `name` variable set during the creation of the `anya` instance, and it can correctly log `"Hi I'm Anya"` to the console.


### Another amazing example

Consider the low quality, repeated code : 

```javascript
document.getElementById('size-12').onclick = function(){
	document.body.style.fontSize = `12px`
}

document.getElementById('size-14').onclick = function(){
	document.body.style.fontSize = `14px`
}
	
document.getElementById('size-16').onclick = function(){
	document.body.style.fontSize = `16px`
}
```

Lets make it less repeatable

```javascript

function fontStyle(size) {
	document.body.style.fontSize = `${size}px`
}

document.getElementById('size-12').onclick = fontSize(12);
document.getElementById('size-14').onclick = fontSize(14);
document.getElementById('size-16').onclick = fontSize(16);

```

But in the above case, the `fontSize` function is invoked and it will return `undefined` to the LHS. we don't want to return `undefined` but instead return a function that sets `document.body.style.fontSize` to the desired size.

This can be achieved by making the below changes in the function
(Just Bear with me)

```javascript
function fontStyle(size) {
	return function() {
		document.body.style.fontSize = `${size}px`
	}
}
```

Now, this would never be possible without the closure property. The closure property allows the inner function (i.e. the function returned from the higher ordered function here) to attain the value of size with it.