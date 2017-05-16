ECMAScript 6 offers tail call optimization, where you can make some function calls without growing the call stack. This blog post explains how that works and what benefits it brings.

### 1. What is tail call optimization?
To understand what tail call optimization (TCO) is, we will examine the following piece of code. I’ll first explain how it is executed without TCO and then with TCO.

```js
function id(x) {
    return x; // (A)
}
function f(a) {
    let b = a + 1;
    return id(b); // (B)
}
console.log(f(2)); // (C)
```
