# JavaScript Event Loop

## What I Learned

The Event Loop coordinates the execution of synchronous code, tasks, and microtasks. It checks when the Call Stack is empty and allows queued callbacks to run.

## Execution Order

JavaScript executes synchronous code first.

```text
Synchronous code
      ↓
Call Stack becomes empty
      ↓
Microtask Queue
      ↓
Promise callback
      ↓
Task Queue
      ↓
setTimeout callback
```

Microtasks are processed after the current task finishes and before the next task starts.

This is why the Promise callback runs before the `setTimeout` callback.

## Experiment

### Code

```js
console.log('1');

setTimeout(() => {
  console.log('2');
}, 0);

Promise.resolve().then(() => {
  console.log('3');
});

console.log('4');
```

### Actual Output

```text
1
4
3
2
```

### Why?

1. `console.log('1')` runs synchronously.
2. The `setTimeout` callback is scheduled as a task.
3. The Promise callback is scheduled as a microtask.
4. `console.log('4')` runs synchronously.
5. The Call Stack becomes empty.
6. The microtask queue is processed, so `3` is printed.
7. The next task runs, so `2` is printed.

## Key Takeaways

**Synchronous Code**
JavaScript executes synchronous code one statement at a time.

**Microtask Queue**
Contains callbacks such as Promise handlers (`then`, `catch`, and `finally`). Microtasks are processed after the current task finishes and before the next task starts.

**Task Queue**
Contains tasks such as callbacks from `setTimeout`, `setInterval`, and DOM events.

**Event Loop**
Coordinates the execution of tasks and microtasks by checking when the Call Stack is available and allowing queued callbacks to run.
