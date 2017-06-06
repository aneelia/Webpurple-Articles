ECMAScript 6 offers *tail call optimization*, where you can make some function calls without growing the call stack. This blog post explains how that works and what benefits it brings.

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

**Step 1.** Initially, there are only the global variables id and f on the stack.

![pic1](http://2ality.com/2015/06/tail-call-optimization/stack_frames_1.jpg)

The block of stack entries encodes the state (local variables, including parameters) of the current scope and is called a stack frame.

**Step 2.** In line C, f() is called: First, the location to return to is saved on the stack. Then f’s parameters are allocated and execution jumps to its body. The stack now looks as follows.

![pic2](http://2ality.com/2015/06/tail-call-optimization/stack_frames_2.jpg)

There are now two frames on the stack: One for the global scope (bottom) and one for f() (top). f’s stack frame includes the return address, line C.

**Step 3.** id() is called in line B. Again, a stack frame is created that contains the return address and id’s parameter.

![pic3](http://2ality.com/2015/06/tail-call-optimization/stack_frames_3.jpg)

**Step 4.** In line A, the result x is returned. id’s stack frame is removed and execution jumps to the return address, line B. (There are several ways in which returning a value could be handled. Two common solutions are to leave the result on a stack or to hand it over in a register. I ignore this part of execution here.)

The stack now looks as follows:

![pic4](http://2ality.com/2015/06/tail-call-optimization/stack_frames_2.jpg)

**Step 5.** In line B, the value that was returned by id is returned to f’s caller. Again, the topmost stack frame is removed and execution jumps to the return address, line C.

![pic5](http://2ality.com/2015/06/tail-call-optimization/stack_frames_1.jpg)

**Step 6.** Line C receives the value 3 and logs it.
