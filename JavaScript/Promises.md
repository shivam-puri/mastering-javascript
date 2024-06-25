Promises in JavaScript are a mechanism for handling asynchronous operations. ==They were introduced to solve the problem of callback hell==, where multiple nested callbacks made code hard to read and maintain.

Here's a detailed explanation of promises in JavaScript:

# Understanding Promise States

Just to review, a promise can be created with the constructor syntax, like this:

```js
let promise = new Promise(function(resolve, reject) {
  // Code to execute
});
```

The constructor function takes a function as an argument. This function is called the `executor function`.

```js
// Executor function passed to the 
// Promise constructor as an argument
function(resolve, reject) {
    // Your logic goes here...
}
```

The executor function takes two arguments, `resolve` and `reject`. These are the callbacks provided by the JavaScript language. Your logic goes inside the executor function that runs automatically when a `new Promise` is created.

For the promise to be effective, the executor function should call either of the callback functions, `resolve` or `reject`. We will learn more about this in detail in a while.

The `new Promise()` constructor returns a `promise` object. As the executor function needs to handle async operations, the returned promise object should be capable of informing when the execution has been started, completed (resolved) or retuned with error (rejected).

A `promise` object has the following internal properties:

1. `state` – This property can have the following values:

- `pending`: Initially when the executor function starts the execution.
- `fulfilled`: When the promise is resolved.
- `rejected`: When the promise is rejected.

2.  `result` – This property can have the following values:

- `undefined`: Initially when the `state` value is `pending`.
- `value`: When `resolve(value)` is called.
- `error`: When `reject(error)` is called.

These internal properties are code-inaccessible but they are inspectable. This means that we will be able to inspect the `state` and `result` property values using the debugger tool, but we will not be able to access them directly using the program.

![promise_state_inspect](https://www.freecodecamp.org/news/content/images/2020/11/promise_state_inspect.png)

Able to inspect the internal properties of a promise

A promise's state can be `pending`, `fulfilled` or `rejected`. A promise that is either resolved or rejected is called `settled`.

### How promises are resolved and rejected

Here is an example of a promise that will be resolved (`fulfilled` state) with the value `I am done` immediately.

```js
let promise = new Promise(function(resolve, reject) {
    resolve("I am done");
});
```

The promise below will be rejected (`rejected` state) with the error message `Something is not right!`.

```js
let promise = new Promise(function(resolve, reject) {
    reject(new Error('Something is not right!'));
});
```

An important point to note:

> A Promise executor should call only one `resolve` or one `reject`. Once one state is changed (pending => fulfilled or pending => rejected), that's all. Any further calls to `resolve` or `reject` will be ignored.

```js
let promise = new Promise(function(resolve, reject) {
  resolve("I am surely going to get resolved!");

  reject(new Error('Will this be ignored?')); // ignored
  resolve("Ignored?"); // ignored
});
```

In the example above, only the first one to resolve will be called and the rest will be ignored.


# How to handle a Promise once you've created it

A `Promise` uses an executor function to complete a task (mostly asynchronously). A consumer function (that uses an outcome of the promise) should get notified when the executor function is done with either resolving (success) or rejecting (error).

The handler methods, `.then()`, `.catch()` and `.finally()`, help to create the link between the executor and the consumer functions so that they can be in sync when a promise `resolve`s or `reject`s.
  
![consumer_executor](https://www.freecodecamp.org/news/content/images/2020/11/consumer_executor.png)

## How to Use the `.then()` Promise Handler

The `.then()` method should be called on the promise object to handle a result (resolve) or an error (reject).

It accepts two functions as parameters. Usually, the `.then()` method should be called from the consumer function where you would like to know the outcome of a promise's execution.

```js
promise.then(
  (result) => { 
     console.log(result);
  },
  (error) => { 
     console.log(error);
  }
);
```

If you are interested only in successful outcomes, you can just pass one argument to it, like this:

```js
promise.then(
  (result) => { 
      console.log(result);
  }
);
```

If you are interested only in the error outcome, you can pass `null` for the first argument, like this:

```js
promise.then(
  null,
  (error) => { 
      console.log(error)
  }
);
```

However, you can handle errors in a better way using the `.catch()` method that we will see in a minute.

### Example of `.then()` promise handler

when `x` has a value <= 5

![[Pasted image 20240503184059.png]] 

When `x` has a value > 5

![[Pasted image 20240503184903.png]]

---
Let's look at a couple of examples of handling results and errors using the `.then` and `.catch` handlers. We will make this learning a bit more fun with a few real asynchronous requests. We will use the [PokeAPI](https://pokeapi.co/) to get information about Pokémon and resolve/reject them using Promises.

First, let us create a generic function that accepts a PokeAPI URL as argument and returns a Promise. If the API call is successful, a resolved promise is returned. A rejected promise is returned for any kind of errors.

We will be using this function in several examples from now on to get a promise and work on it.

```js
function getPromise(URL) {
  let promise = new Promise(function (resolve, reject) {
    let req = new XMLHttpRequest();
    req.open("GET", URL);
    req.onload = function () {
      if (req.status == 200) {
        resolve(req.response);
      } else {
        reject("There is an Error!");
      }
    };
    req.send();
  });
  return promise;
}
```

Example 1: Get 50 Pokémon's information:

```js
const ALL_POKEMONS_URL = 'https://pokeapi.co/api/v2/pokemon?limit=50';

// We have discussed this function already!
let promise = getPromise(ALL_POKEMONS_URL);

const consumer = () => {
    promise.then(
        (result) => {
            console.log({result}); // Log the result of 50 Pokemons
        },
        (error) => {
            // As the URL is a valid one, this will not be called.
            console.log('We have encountered an Error!'); // Log an error
    });
}

consumer();
```

Example 2: Let's try an invalid URL

```js
const POKEMONS_BAD_URL = 'https://pokeapi.co/api/v2/pokemon-bad/';

// This will reject as the URL is 404
let promise = getPromise(POKEMONS_BAD_URL);

const consumer = () => {
    promise.then(
        (result) => {
            // The promise didn't resolve. Hence, it will
            // not be executed.
            console.log({result});
        },
        (error) => {
            // A rejected prmise will execute this
            console.log('We have encountered an Error!'); // Log an error
        }
    );
}

consumer();
```


## How to Use the `.catch()` Promise Handler

You can use this handler method to handle errors (rejections) from promises. The syntax of passing `null` as the first argument to the `.then()` is not a great way to handle errors. So we have `.catch()` to do the same job with some neat syntax:

```js
// This will reject as the URL is 404
let promise = getPromise(POKEMONS_BAD_URL);

const consumer = () => {
    promise.catch(error => console.log(error));
}

consumer();
```

If we throw an Error like `new Error("Something wrong!")`  instead of calling the `reject` from the promise executor and handlers, it will still be treated as a rejection. It means that this will be caught by the `.catch` handler method.

This is the same for any _synchronous_ exceptions that happen in the promise executor and handler functions.

Here is an example where it will be treated like a reject and the `.catch` handler method will be called:

```js
new Promise((resolve, reject) => {
  throw new Error("Something is wrong!");// No reject call
}).catch((error) => console.log(error))
```

## How to Use the `.finally()` Promise Handler

The `.finally()` handler performs cleanups like stopping a loader, closing a live connection, and so on. The `finally()` method will be called irrespective of whether a promise `resolve`s or `reject`s. It passes through the result or error to the next handler which can call a .then() or .catch() again.

Here is an example that'll help you understand all three methods together:

```js
let loading = true;
loading && console.log('Loading...');

// Gatting Promise
promise = getPromise(ALL_POKEMONS_URL);

promise.finally(() => {
    loading = false;
    console.log(`Promise Settled and loading is ${loading}`);
}).then((result) => {
    console.log({result});
}).catch((error) => {
    console.log(error)
});
```

# What is the Promise Chain?

The  `promise.then()` call always returns a promise. This promise will have the `state` as `pending` and `result` as `undefined`. It allows us to call the next `.then` method on the new promise.

When the first `.then` method returns a value, the next `.then` method can receive that. The second one can now pass to the third `.then()` and so on. This forms a chain of `.then` methods to pass the promises down. This phenomenon is called the `Promise Chain`.

![image-105](https://www.freecodecamp.org/news/content/images/2020/12/image-105.png)

Promise Chain

Here is an example:

```js
let promise = getPromise(ALL_POKEMONS_URL);

promise.then(result => {
    let onePokemon = JSON.parse(result).results[0].url;
    return onePokemon;
}).then(onePokemonURL => {
    console.log(onePokemonURL);
}).catch(error => {
    console.log('In the catch', error);
});
```

Here we first get a promise resolved and then extract the URL to reach the first Pokémon. We then return that value and it will be passed as a promise to the next .then() handler function. Hence the output,

```shell
https://pokeapi.co/api/v2/pokemon/1/
```

The `.then` method can return either:

- A value (we have seen this already)
- A brand new promise.

It can also throw an error.

Here is an example where we have created a promise chain with the `.then` methods which returns results and a new promise:

```js
// Promise Chain with multiple then and catch
let promise = getPromise(ALL_POKEMONS_URL);

promise.then(result => {
    let onePokemon = JSON.parse(result).results[0].url;
    return onePokemon;
}).then(onePokemonURL => {
    console.log(onePokemonURL);
    return getPromise(onePokemonURL);
}).then(pokemon => {
    console.log(JSON.parse(pokemon));
}).catch(error => {
    console.log('In the catch', error);
});
```

In the first `.then` call we extract the URL and return it as a value. This URL will be passed to the second `.then` call where we are returning a new promise taking that URL as an argument.

This promise will be resolved and passed down to the chain where we get the information about the Pokémon. Here is the output:

![image-159](https://www.freecodecamp.org/news/content/images/2020/11/image-159.png)

Output of the promise chain call

In case there is an error or a promise rejection, the .catch method in the chain will be called.

A point to note: Calling `.then` multiple times doesn't form a Promise chain. You may end up doing something like this only to introduce a bug in the code:

```js
let promise = getPromise(ALL_POKEMONS_URL);

promise.then(result => {
    let onePokemon = JSON.parse(result).results[0].url;
    return onePokemon;
});
promise.then(onePokemonURL => {
    console.log(onePokemonURL);
    return getPromise(onePokemonURL);
});
promise.then(pokemon => {
    console.log(JSON.parse(pokemon));
});
```

We call the `.then` method three times on the same promise, but we don't pass the promise down. This is different than the promise chain. In the above example, the output will be an error.

![image-160](https://www.freecodecamp.org/news/content/images/2020/11/image-160.png)