## 1.2 Tail call optimization 

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

If you look at the previous section then there is one step that is unnecessary – step 5. All that happens in line B is that the value returned by id() is passed on to line C. Ideally, id() could do that itself and the intermediate step could be skipped.

We can make this happen by implementing the function call in line B differently. Before the call happens, the stack looks as follows.

///pic6///

If we examine the call we see that it is the very last action in f(). Once id() is done, the only remaining action performed by f() is to pass id’s result to f’s caller. Therefore, f’s variables are not needed, anymore and its stack frame can be removed before making the call. The return address given to id() is f’s return address, line C. During the execution of id(), the stack looks like this:

///pic7///

Then id() returns the value 3. You could say that it returns that value for f(), because it transports it to f’s caller, line C.

Let’s review: The function call in line B is a *tail call*. Such a call can be done with zero stack growth. To find out whether a function call is a tail call, we must check whether it is in a tail position (i.e., the last action in a function). How that is done is explained in the next section.
