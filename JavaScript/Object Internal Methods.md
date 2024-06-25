### `Object.defineProperty`

`Object.defineProperty()` is a method in JavaScript that allows you to precisely control how properties are added or modified on an object.

When you normally add a property to an object using assignment (`object.property = value`), it becomes a regular property that can be easily seen, changed, and removed. These properties show up when you loop through all the properties of the object (`for...in` loop, `Object.keys()`, etc.).

However, with `Object.defineProperty()`, you have more control. By default, properties added this way are not writable (you can't change their value), not enumerable (they won't show up when you loop through the properties), and not configurable (you can't delete them or change their attributes later).

```javascript
const anime = {
	name : "eren jeager",
	kills : "8 billion",
	love : "mikasa ackerman"	
}

Object.defineProperty(anime, "best_friend" , {
	value : "armin arlet"
} )

anime.best_friend = "hangee" // this does not changes the bestfriend field because its not writable now. It can only be changed again using object.defineProperty

console.log(anime.best_friend) // armin arlet

// ERROR : cannot redefine property "best_friend"
Object.defineProperty(anime, "best_friend", {
	value : "hangee"
})

console.log(anime.best_friend)     // armin arlet
console.log(anime)                 // { name: 'eren jeager', kills: '8 billion', love: 'mikasa ackerman' } (the best_friend property will be hidden while console.logging & mapping the whole object )
```

When you use `Object.defineProperty()`, it employs the `[[DefineOwnProperty]]` internal method to define or modify a property on an object. ***This internal method is specifically designed for defining properties*** and is different from the `[[Set]]` internal method used in regular property assignment (`object.property = value`).


More to learn on Object Internal Methods. Refer the MDN Docs.