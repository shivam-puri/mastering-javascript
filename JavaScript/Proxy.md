### `Generally, Proxy means to act on behalf of another`

`The Proxy object allows you to create an object that can be used in place of the original object, but which may redefine fundamental `Object` operations like getting, setting, and defining properties. Proxy objects are commonly used to log property accesses, validate, format, or sanitize inputs, and so on.`

You create a `Proxy` with two parameters:

- `target`: the original object which you want to proxy
- `handler`: an object that defines which operations will be intercepted and how to redefine intercepted operations.

For example, this code creates a proxy for the `target` object.

```javascript
const target = {
	message1 : "hello",
	message2 : "everyone"
}

const handler1 = {}

const proxy1 = new Proxy(target, handler1)

// Because the handler is empty, this proxy behaves just like the original target:
console.log(proxy1.message1); // hello
console.log(proxy1.message2); // everyone
```

`To customise the proxy we define handler functions`

`Handler functions are sometimes called traps, presumably because they trap calls to the target object. The very simple trap in handler below redefines all property accessors`

```javascript
const target = {
  message1: "hello",
  message2: "everyone",
};

const handler2 = {
  get(target, prop, receiver) {
    return "world";
  },
};

const proxy2 = new Proxy(target, handler2);

console.log(proxy2.message1); // world
console.log(proxy2.message2); // world

```

`Below is an example how you can redefine internal methods of an object for your desired properties`

```javascript
const monster1 = {
  secret: 'easily scared',
  eyeCount: 4,
};


const handler1 = {
// Redefining getter operation
// target = monster1 object
  get: function (target, prop, receiver) {
  // If the property asked === secret
    if (prop === 'secret') {
      return `${target.secret.substring(0, 4)} ... shhhh!`;
    }
// ...arguments, spreads the arguments passed to get fn
    return Reflect.get(...arguments); //Reflect,get() is a built-in JavaScript method that allows you to invoke the default behavior for property access on an object.
  },
};

const proxy1 = new Proxy(monster1, handler1);

console.log(proxy1.eyeCount);
// Expected output: 4

console.log(proxy1.secret);
// Expected output: "easi ... shhhh!"

```

## More on Proxy
* ### https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy

## Related
* ### [[Object Internal Methods]]
