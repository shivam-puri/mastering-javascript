# De-structuring

Pay special attention to the function signature:

```
const createUser = ({ userName, avatar }) => ({
```
In this line, the braces (`{`, `}`) represent object de-structuring. This function takes one argument (an object), but de-structures two formal parameters from that single argument, `userName`, and `avatar`. Those parameters can then be used as variables in the function body scope. You can also de-structure arrays:
 
```
const swap = ([first, second]) => [second, first];console.log( swap([1, 2]) ); // [2, 1]
```
And you can use the rest and spread syntax (`...varName`) to gather the rest of the values from the array (or a list of arguments), and then spread those array elements back into individual elements:

```
const rotate = ([first, ...rest]) => [...rest, first];console.log( rotate([1, 2, 3]) ); // [2, 3, 1]
```


## Default parameters

![[Pasted image 20240430120446.png]]

A better look of the function : 

![[Pasted image 20240430120624.png]]

The last `= {}` bit just before the parameter signature closes means that if nothing gets passed in for this parameter, we’re going to use an empty object as the default. When you try to destructure values from the empty object, the default values for properties will get used automatically, because that’s what default values do: replace `undefined` with some predefined value.

Without the `= {}` default value, `createUser()` with no arguments would throw an error because you can’t try to access properties from `undefined`.


![[Pasted image 20240430120855.png]]

## computed property keys

![[Pasted image 20240430121630.png]]