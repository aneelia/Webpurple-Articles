ECMAScript 6 offers tail call optimization, where you can make some function calls without growing the call stack. This blog post explains how that works and what benefits it brings.

## 1. What is tail call optimization?
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
## 1. Normal execution
Let’s assume there is a JavaScript engine that manages function calls by storing local variables and return addresses on a stack. How would such an engine execute the code?

Step 1. Initially, there are only the global variables id and f on the stack.

///pic///

The block of stack entries encodes the state (local variables, including parameters) of the current scope and is called a stack frame.

Step 2. In line C, f() is called: First, the location to return to is saved on the stack. Then f’s parameters are allocated and execution jumps to its body. The stack now looks as follows.
