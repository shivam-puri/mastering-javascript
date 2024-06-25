# My friend asked why write reusable code?

![[findandreplace vscode.mp4]]

### An example of factory person

![[Pasted image 20240430161720.png]]


A **factory function** is any function which is not a class or constructor that returns a (presumably new) object. ==In JavaScript, any function can return an object. When it does so without the== ==`new`== ==keyword, it’s a factory function.==

Factory functions have always been attractive in JavaScript because they offer the ability to easily produce object instances without diving into the complexities of classes and the `new` keyword.

With a factory function, you can create as many user objects as you want. If you’re building a chat app, for instance, you can have a user object representing the current user, and also a lot of other user objects representing all the other users who are currently signed in and chatting, so you can display their names and avatars, too.

Let’s turn our `user` object into a `createUser()` factory:

```javascript
const createUser = ({ userName, avatar }) => ({  
  userName,  
  avatar,  
  // method to change username
  setUserName (userName) {  
    this.userName = userName;  
    return this;  
  }  
});

console.log(createUser({ userName: 'echo', avatar: 'echo.png' }));

/*{  
  "avatar": "echo.png",  
  "userName": "echo",  
  "setUserName": [Function setUserName]  
}  
*/
```
another example
```javascript
function createAnime(name, mc, genre) {
    return {
        name, mc, genre,
        setAnime (name) {
            this.name = name
            return this // to return the updated object on whom this setAnime method was called
        }
    }
}

const aot = createAnime("AOT", "eren", "action")
aot.setAnime("attack on humans") // returns : {name: 'attack on humans', mc: 'eren', genre: 'action', setAnime: ƒ}
```
---
### Function mixins

A function mixin in JavaScript is a design pattern that allows objects to inherit functionality from a function. This can help to reduce functional repetition and increase function reuse.

##### let's understand this with an example : 

we have a class -
```javascript
class Person{
    constructor(name){
        this.name = name;
    }
}
```

we create a person `p1` of class `Person`

```javascript
const p1 = new Person("eren jeager")
```

Now we want to add a new method in Person class (say, `sayHi`)
so we create a function `sayHiMixin` . and then pass Person as a target class to add the method `sayHi` in : 

```javascript
function sayHiMixin (target){
    target.prototype.sayHi = function(){
        return `Hey! my name is ${this.name}`
    }
}

sayHiMixin(Person)
```

And now, 

```javascript
p1.sayHi() // Hey! my name is eren jeager
```

### Another example

![[Pasted image 20240430160943.png| 500]]
![[Pasted image 20240430160956.png|500]]

---

Related - [[De-structuring and default parameters]]
If you wanna go advanced -  [Factory Functions for Mixin Composition](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1#:~:text=Factory%20Functions%20for%20Mixin%20Composition)
#### Source
<iframe src="https://www.youtube.com/embed/lE_79wkP-1U?list=PL1PqvM2UQiMoGNTaxFMSK2cih633lpFKP" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 100%; height: 100%;"></iframe>
