## 2. Checking whether a function call is in a tail position

We have just learned that tail calls are function calls that can be executed more efficiently. But what counts as a tail call?

First, the way in which you call a function does not matter. The following calls can all be optimized if they appear in a tail position:
* Function call: func(···)
* Dispatched method call: obj.method(···)
* Direct method call via call(): func.call(···)
* Direct method call via apply(): func.apply(···)
 
 ### 2.1 Tail calls in expressions
 
Arrow functions can have expressions as bodies. For tail call optimization, we therefore have to figure out where function calls are in tail positions in expressions. Only the following expressions can contain tail calls:

* The conditional operator (? :)
* The logical Or operator (||)
* The logical And operator (&&)
* The comma operator (,)

Let’s look at an example for each one of them.

#### The conditional operator (? :)

```const a = x => x ? f() : g();```

Both f() and g() are in tail position.

#### The logical Or operator (||)  
```const a = () => f() || g();```

f() is not in a tail position, but g() is in a tail position. To see why, take a look at the following code, which is equivalent to the previous code:

```
const a = () => {
    let fResult = f(); // not a tail call
    if (fResult) {
        return fResult;
    } else {
        return g(); // tail call
    }
};
```

The result of the logical Or operator depends on the result of f(), which is why that function call is not in a tail position (the caller does something with it other than returning it). However, g() is in a tail position.

#### The logical And operator
```const a = () => f() && g();```
f() is not in a tail position, but g() is in a tail position. To see why, take a look at the following code, which is equivalent to the previous code:
```
const a = () => {
    let fResult = f(); // not a tail call
    if (!fResult) {
        return fResult;
    } else {
        return g(); // tail call
    }
};
```
The result of the logical And operator depends on the result of f(), which is why that function call is not in a tail position (the caller does something with it other than returning it). However, g() is in a tail position.
#### The comma operator (,)  
```const a = () => (f() , g());```
f() is not in a tail position, but g() is in a tail position. To see why, take a look at the following code, which is equivalent to the previous code:
```
const a = () => {
    f();
    return g();
}
```
