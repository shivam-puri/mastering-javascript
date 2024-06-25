How the `new` keyword works -

![[the new keyword.mp4]]

Try this on your browser console.

```javascript
function SuperElement(type, content) {
    this.el = document.createElement(type);
    this.el.innerText = content;
    this.el.style.color = "white"
    document.body.append(this.el)
    this.el.addEventListener('click', () => {console.log(this.el)})
}

const arr = ["armin", "levi", "Sasha", "Hanji", "Konny", "Jean", "Reiner"]

const newElements = arr.map((x) => {
    return new SuperElement("h1", x)
})
```

# Explanation
Constructor function is the function to create an object. In javascript, to force a function to return an object, a keyword ‘new’ should be put before the function call.

Javascript engine does the magic below when it detects the key word ‘new’ before the function:

1. It creates an empty object A.
2. 2. It binds ‘this’ variable of constructor function to the object A.==
3. The properties and methods accessed by ‘this’ are attached to the object A.
4. Javascript engine sets prototype of object A to the prototype of the constructor function. (important!)
5. If the code in the function does not explicitly return an objet, javascript engine returns object A at the end of the function.

In the code snippet below, Person function is a constructor function. You may notice that name ‘Person’ is capitalized. The intention is to remind you that this function is a constructor function, and when called it is supposed to return an object. Bear in mind that, you need to add the keyword ‘new’ to make constructor function to return you an object.

![](https://miro.medium.com/v2/resize:fit:875/1*BKjy87-2vnDHPC1earSxyQ.png)

That said, some programmers could call the function without the key word ‘new’. Well, what happens? Since Javascript engine does not detect the ‘new’ keyword, it does not do any magic above, and returns undefined instead.

![](https://miro.medium.com/v2/resize:fit:875/1*kkufEZyNCxhdnkWQn7xJww.png)

We can avoid this by modifying constructor function:

![](https://miro.medium.com/v2/resize:fit:875/1*uO_3V9tJ0ZD0LI-Hi7x2FA.png)