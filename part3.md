## 2. Checking whether a function call is in a tail position

We have just learned that tail calls are function calls that can be executed more efficiently. But what counts as a tail call?

First, the way in which you call a function does not matter. The following calls can all be optimized if they appear in a tail position:
