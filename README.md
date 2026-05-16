# Closure Diagram — How it works

This document explains the JavaScript closure concept and describes how to read and interpret the interactive diagram in `closure.html`.

---

## What is a closure?

A closure is the combination of a function and the lexical environment in which that function was declared. The lexical environment contains any local variables that were in-scope at the time the closure was created. Closures allow inner functions to access outer function variables even after the outer function has finished executing.

Key points:

- A closure captures variable bindings (not values) from its declaring scope.
- Each call to a function creates a new lexical environment.
- Closures are commonly used for data privacy, function factories, and callbacks.

Example:

```js
function makeCounter() {
  let count = 0; // captured by the returned function
  return function() {
    count += 1;
    return count;
  };
}

const c1 = makeCounter();
console.log(c1()); // 1
console.log(c1()); // 2

const c2 = makeCounter();
console.log(c2()); // 1  (separate lexical environment)
```

---

## Purpose of the diagram

The diagram visualizes the runtime relationship between functions, their lexical environments (scopes), and the variables that live in those environments. It helps you trace:

- Which variables are accessible to a function at a given time.
- When separate instances of the same function share or have distinct captured state.
- The lifetime of variables across asynchronous callbacks and event handlers.

---

## Diagram components (legend)

- Node — Function: represents a function object; labeled with the function name (or &lt;anonymous&gt;).
- Node — Environment (box): groups the variables that belong to a particular lexical scope (activation record).
- Arrow — Scope link: points from a function to its parent environment (the lexical chain).
- Edge — Reference: indicates that a variable in one environment is referenced by another function.
- Color hints:
  - Green: local variables created in the current environment.
  - Blue: closed-over variables (captured by one or more closures).
  - Gray: globals or external references.

Interactive hints:

- Hover a function node to highlight its closure variables and scope chain.
- Click a step/trace button to advance execution and watch environments appear or be garbage-collected.

---

## How to read a typical trace

1. Identify the function node you care about (e.g., a returned inner function).
2. Follow its scope link(s) upward to find the lexical environments it closes over.
3. Inspect variables in those environments — captured variables will be highlighted.
4. Advance the execution trace to see how variables mutate over time and how environments are created/destroyed.

Example scenario (counter factory):

- `makeCounter()` node creates an Environment A containing `count`.
- The returned inner function node has a scope link to Environment A.
- Multiple calls to `makeCounter()` create Environment A1, A2, ... each with its own `count`.

---

## Common patterns and pitfalls shown by the diagram

- Loop closures (using `var`): the diagram shows many functions referring to the same environment, explaining why all callbacks see the final loop value.
- Memory retention: captured variables stay alive as long as a reachable closure references them; the diagram highlights retained environments.
- Capturing objects vs. primitives: both are shown, but object mutations will be visible across closures that reference the same object.

---

## Using the demo

1. Open `closure.html` in a browser or serve the folder and visit the file.
2. Choose an example from the sidebar (counter, timers, factory functions).
3. Hover and click nodes to inspect environments and variables.
4. Use the step controls to replay creation and invocation events.

Notes:

- The demo intentionally keeps visuals simple: function nodes and boxed environments map directly to the language runtime concepts.
- If a variable is shown in blue, multiple closures hold references to it.

---

## Further reading

- MDN: Closures — https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures
- "You Don't Know JS (Scope & Closures)" — Kyle Simpson

---

If you'd like, I can:

- Add annotated screenshots for each example in the README.
- Expand explanations for asynchronous examples (timers, promises).
