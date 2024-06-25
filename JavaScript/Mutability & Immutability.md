
Mutability refers to data types that can be accessed and changed after they've been created and stored in memory. Immutability, on the other hand, refers to data types that you can't change after creating them – but that you can still access in the memory.`

# Prerequisite

![[Untitled-2024-01-12-2318.png]]


The primitive data types are stored in stack of the program and are ***immutable*** i.e. it is impossible to change them.

```javascript
let main_character = "eren jeager";
let side_character = main_character;

console.log(side_character) // eren jeager
side_character = "armin arlet"
console.log(side_character)  // armin arlet
console.log(main_character) // eren jeager (The changes in side_character do not affect the main_character variable & vice versa. Hence they are immutable).

// This is because while declaring a primitive data type a whole new copy is made in the stack i.e. totally unique and unrelated to the variable it is copied from.
```

Whereas in case of non primitive datatypes (reference datatypes) the data is stored in the heap memory and the reference (address) of this data is stored inside a variable in the stack memory` 

```javascript
const anime = {
	name : "AOT",
	mc : "eren jeager"
}
// This line of code passes the reference stored in anime variable to anime2
const anime2 = anime; // any changes in anime2 will also be reflected in anime object because both variable points the same object therefore they are mutable

anime2.mc = "levi ackerman"
console.log(anime.mc); // levi ackerman
```

### *Cloning of Objects*

```javascript
// cloning
const anime3 = Object.assign({}, anime); // any changes in anime3 will not affect anime or any variable that contains the same pointer as anime. Because a new object is created in the heap and the reference is passed to anime3


console.log(anime3);

anime3.mc = "mikasa ackerman"

console.log(anime3.mc)   // mikasa Ackerman
console.log(anime2.mc)	 // levi Ackerman
console.log(anime.mc)	 // levi Ackerman

// The cloning of object can also be achieved by spread operator 
const anime4 = {...anime3}

// BTW the goat, the real MC is still Eren Jeager
```


### Related

* #### [[Object Internal Methods]]
