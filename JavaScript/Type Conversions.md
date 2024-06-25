![[Pasted image 20240421184506.png]]

![[Pasted image 20240421184652.png]]


![[Pasted image 20240421185114.png]]


```javascript
let x;
x + ""             // String(x)
+x                 // Number(x)
x - 0              // Number(x)
!!x                // Boolean(x) Note: Double !

let n = 10;
let binary = n.toString(2);
let octal = n.toString(8);
let hex = n.toString(16);
let decimal = n.toString();

console.log(n, binary, octal, hex, decimal);
console.log(typeof n, typeof binary, typeof octal, typeof hex, typeof decimal);
```