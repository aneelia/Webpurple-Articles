## 3.Tail-recursive functions

A function is *tail-recursive* if the main recursive calls it makes are in tail positions.

For example, the following function is not tail recursive, because the main recursive call in line A is not in a tail position:
```
function factorial(x) {
    if (x <= 0) {
        return 1;
    } else {
        return x * factorial(x-1); // (A)
    }
}
```
factorial() can be implemented via a tail-recursive helper function facRec(). The main recursive call in line A is in a tail position.
```
function factorial(n) {
    return facRec(n, 1);
}
function facRec(x, acc) {
    if (x <= 1) {
        return acc;
    } else {
        return facRec(x-1, x*acc); // (A)
    }
}
```
That is, some non-tail-recursive functions can be transformed into tail-recursive functions.

###3.1 Tail-recursive loops

Tail call optimization makes it possible to implement loops via recursion without growing the stack. The following are two examples.
