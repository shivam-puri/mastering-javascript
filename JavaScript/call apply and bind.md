### The Context 

```javascript
const name1 = {
	firstName : "eren",
	lastName : "jeager",
	printFullName : function(){
		console.log(this.firstName + " " + this.lastName);
	}
}

const name2 = {
	firstName : "mikasa",
	lastName : "ackerman",
	printFullName : function(){
		console.log(this.firstName + " " + this.lastName);
	}
}

name1.printFullName() // eren jeager
name2.printFullName()  // mikasa ackerman
```
the problem is code, lack of reusability
### Using function call()
in the last method we had to mention the `printFullName` function in both objects, and things were repeating here.
A better approach for this can be call() method of function, lets see how : 
### example 1 -

![[Pasted image 20240501175823.png]]

### example 2 -

```javascript
let getFullName = function(homeTown){
	console.log(this.firstName + " " + this.lastName, " from " + homeTown);
}

const name1 = {
	firstName : "eren",
	lastName : "jeager",
	displayKills : function(kills) {
		console.log(kills + " killed")
	}
}

const name2 = {
	firstName : "mikasa",
	lastName : "ackerman",
}


// the first argument passes the this keyword for the getFullName function and then you can have desired numbers of arguments/parameters
getFullName.call(name1, "Shiganshina District");
getFullName.call(name2, "Wall Maria");

name1.displayKills("1.6 billion")
// this is called function borrowing.
// name1.displayKills access the displayKills fn in name1 and then .call(name2, 21) sets the this keyword to name2 for this call and 21 as an argument for the call
name1.displayKills.call(name2, 21);

```

### Using function apply()

The same as call but we pass arguments as an array of elements

![[Pasted image 20240501175929.png]]

This is the same as function call(), the only difference here is, all the arguments in function.apply() (except the first argument i.e. the this argument) are passed as an array.

So I'm copying the above code and just changing call() to apply() and no. of arguments to an array of arguments.

```javascript
let getFullName = function(homeTown){
	console.log(this.firstName + " " + this.lastName, " from " + homeTown);
}

const name1 = {
	firstName : "eren",
	lastName : "jeager",
	displayKills : function(kills, titans, humans) {
		console.log(kills + " killed" + "," + titans + " titans " + " & " + humans + " humans")
	}
}

const name2 = {
	firstName : "mikasa",
	lastName : "ackerman",
}


// the first argument passes the this keyword for the getFullName function and then you can have
// desired numbers of arguments/parameters

// arguments other than this keyword are passed as an array
getFullName.apply(name1, ["Shiganshina District"]);
getFullName.apply(name2, ["Wall Maria"]);

name1.displayKills("1.6 billion", 23, "1.6 billion")
// arguments other than this keyword are passed as an array
name1.displayKills.apply(name2, [21, 16, 5]);
```

### Using function.bind()
The **`bind()`** method of [[The Function Object|Function]] instances ==creates and returns a new function that==, when called, calls this function with its `this` keyword set to the provided value, and a given sequence of arguments preceding any provided when the new function is called.

### example 1 -

![[Pasted image 20240501180228.png]]

### example 2

```javascript
const name1 = {
	firstName : "eren",
	lastName : "jeager",
	displayKills : function(kills, titans, humans) {
		console.log(kills + " killed" + "," + titans + " titans " + " & " + humans + " humans")
	}
}

const name2 = {
	firstName : "mikasa",
	lastName : "ackerman",
}

// the bind method binds a method with an object and returns us a copy of that.
let displayKillsOfName2 = name1.displayKills.bind(name2, 21, 16, 5);

displayKillsOfName2(); // 21 killed,16 titans & 5 humans

```