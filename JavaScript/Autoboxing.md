 
```javascript
let flag = false; 
const str = flag.toString(); 
console.log(str, typeof str); // false, string
```

### ***Methods are allocated to objects, and can be accessed by using . operator. but here flag is a variable, (primitive datatype in JS) its not an object so how the .toString() works here?***

---

You're absolutely right that primitive data types in JavaScript (like `false` in this case) don't inherently have methods like `toString()`. However, JavaScript provides a mechanism called **auto-boxing** (or coercion) when you try to use a method on a primitive value.

Here's what happens behind the scenes in your code:

1. **Auto-boxing:** When you use `flag.toString()`, JavaScript temporarily converts the primitive value `false` (boolean) into a corresponding wrapper object, typically of type `Boolean`. This object does have the `toString()` method.
2. **Method Call:** The `toString()` method is then called on the temporary Boolean object, which converts the internal boolean value (`false`) to a string representation ("false").
3. **Unboxing:** After the method call, JavaScript discards the temporary object and returns the resulting string value ("false").

So, in the output you see:

```
false string
```

The first value (`false`) is the original Boolean value, and the second value (`string`) is the type of the converted string returned by `toString()`.

**Key Points:**

- Auto-boxing is a convenience feature that allows you to use object methods on primitive values.
- JavaScript automatically handles the creation and disposal of the temporary wrapper object.
- This behavior can sometimes lead to unexpected results if you're not aware of it.

**In summary:**

While primitive data types in JavaScript don't have methods themselves, auto-boxing provides a way to interact with them using methods defined on their corresponding wrapper objects.

![[Drawing 2024-04-30 17.46.06.excalidraw | 1000]]