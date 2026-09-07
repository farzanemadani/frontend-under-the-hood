# JavaScript Closures

## What I Learned

A closure is a function that remembers and can access variables from its outer scope, even after the outer function has finished executing.

## How Closures Work

Each call to `createCounter()` creates a new closure with its own `count` variable.

```text
createCounter()       createCounter()
      ↓                     ↓
  count = 0             count = 0
      ↓                     ↓
 counter1              counter2
```

`counter1` and `counter2` have independent `count` variables.

## Experiment

### Code

```js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1());
console.log(counter1());
console.log(counter1());

console.log(counter2());

console.log(counter1());
```

### Actual Output

```text
1
2
3
1
4
```

### Why?

Each call to `createCounter()` creates a new `count` variable and a new closure.

`counter1` keeps a reference to its own `count`:

```text
counter1 → count = 0
```

When `counter1()` is called:

```text
count = 0
   ↓
count++
   ↓
1
```

Calling `counter1()` again uses the same `count`:

```text
1 → 2 → 3
```

`counter2` has its own independent closure and its own `count`:

```text
counter2 → count = 0
```

Therefore, calling `counter2()` returns `1` without affecting `counter1`.

When `counter1()` is called again, its `count` continues from `3`:

```text
3 → 4
```

## Private State

Closures can be used to create private state.

The `count` variable cannot be accessed directly from outside the closure:

```js
console.log(counter1.count); // undefined
```

However, the returned function can access and modify `count` through the closure:

```js
console.log(counter1()); // 5
```

This allows a function to maintain state without exposing the state directly.

## Key Takeaways

**Closure**

A closure is a function that remembers and can access variables from its outer scope.

**Independent State**

Each call to a function that creates a closure can create a separate closure with its own state.

**Persistent State**

Variables captured by a closure remain accessible even after the outer function has finished executing.

**Private State**

Closures can be used to keep variables private and expose controlled access through functions.
