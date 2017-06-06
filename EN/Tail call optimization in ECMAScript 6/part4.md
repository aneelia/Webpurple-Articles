### 2.2 Tail calls in statements
For statements, the following rules apply.

Only these compound statements can contain tail calls:

* Blocks (as delimited by {}, with or without a label)
* if: in either the “then” clause or the “else” clause.
* do-while, while, for: in their bodies.
* switch: in its body.
* try-catch: only in the catch clause. The try clause has the catch clause as a context that can’t be optimized away.
* try-finally, try-catch-finally: only in the finally clause, which is a context of the other clauses that can’t be optimized away.

Of all the atomic (non-compound) statements, only return can contain a tail call. All other statements have context that can’t be optimized away. The following statement contains a tail call if expr contains a tail call.

```return «expr»;```

### 2.3 Tail call optimization can only be made in strict mode

In non-strict mode, most engines have the following two properties that allow you to examine the call stack:

* func.arguments: contains the arguments of the most recent invocation of func.
* func.caller: refers to the function that most recently called func.

With tail call optimization, these properties don’t work, because the information that they rely on may have been removed. Therefore, strict mode forbids these properties (as described in the language specification) and tail call optimization only works in strict mode.

### 2.4 Pitfall: solo function calls are never in tail position

The function call bar() in the following code is not in tail position:

```js

function foo() {
    bar(); // this is not a tail call in JS
}

```

The reason is that the last action of foo() is not the function call bar(), it is (implicitly) returning undefined. In other words, foo() behaves like this:

```js

function foo() {
    bar();
    return undefined;
}

```

Callers can rely on foo() always returning undefined. If bar() were to return a result for foo(), due to tail call optimization, then that would change foo’s behavior.

Therefore, if we want bar() to be a tail call, we have to change foo() as follows.

```js

function foo() {
    return bar(); // tail call
}

```
