# JavaScript Q&A 📚

<br>
<details>
    <summary>
        <span style="font-size: 16px; color: #279CF5">What is JavaScript?</span>
    </summary>
    </br>

 JavaScript is a lightweight, high-level programming language used to make web pages interactive.  It runs in the browser and can also be used on the server side with Node.js.
</details>

---

<details>
    <summary>
        <span style="font-size: 16px; color: #279CF5">What is Closure in JavaScript?</span>
    </summary>
    </br>

A closure in JavaScript is a function that remembers and can access variables from its outer (parent) lexical scope, even after that outer function has finished executing and returned.
Core Concept

When a function is created in JavaScript, it retains a reference to its surrounding state (lexical environment). This gives the inner function access to three scope chains:

* Its own local scope (variables defined inside its curly braces).
* Outer function variables (variables defined in the enclosing function).
* Global variables.


***Practical Example***
```

function createCounter() {
    let count = 0; // Private variable in outer scope

    return function() {
        count++; // Inner function retains access to 'count'
        return count;
    };
}

const counter = createCounter(); // createCounter() finishes executing here

console.log(counter()); // Output: 1
console.log(counter()); // Output: 2
console.log(counter()); // Output: 3
```

Under the Hood (Execution Context)

During the Creation Phase of the Execution Context, the JavaScript engine scans the code, allocates memory space for all variables and functions, and sets up references. It does not physically move your code lines up; it simply sets up memory before executing line-by-line in the Execution Phase.
</details>

---
<details>
    <summary>
        <span style="font-size: 16px; color: #279CF5">What is Hoisting in Javascript ?</span>
    </summary>
    </br>

Hoisting is JavaScript's default behavior of moving variable, function, and class declarations to the top of their containing scope (global or local) during the compilation phase, before the code is executed.

**How Different Declarations Behave**

* `var` Declarations:  Hoisted and initialized with `undefined`. You can access a `var` variable before its declaration, but its value will be `undefined` until execution reaches the assignment line.

* `let` and `const` Declarations:  Hoisted, but not initialized. Accessing them before their declaration line throws a `ReferenceError` because they exist in a state called the Temporal Dead Zone (TDZ) from the start of the scope until the line where they are declared.

* Function Declarations:  Fully hoisted along with their function body/implementation. You can safely call the function before its definition in the code.

* Function Expressions & Arrow Functions: Hoisting behavior depends on the variable keyword used (var, let, or const). If declared with `var`, the variable is hoisted as `undefined`, so calling it early throws a TypeError: ... is not a function.

***Practical Example:***

```
// 1. Function Declaration (Fully Hoisted)

greet(); // Output: "Hello!"
function greet() {
    console.log("Hello!");
}


// 2. 'var' Variable (Hoisted with 'undefined')

console.log(num); // Output: undefined
var num = 10;


// 3. 'let' Variable (Hoisted into Temporal Dead Zone)

console.log(age); // Throws ReferenceError: Cannot access 'age' before initialization
let age = 25;


// 4. Arrow Function / Expression with 'var'

sayHi(); // Throws TypeError: sayHi is not a function
var sayHi = () => {
    console.log("Hi!");
};

```
Under the Hood (Execution Context)

During the Creation Phase of the Execution Context, the JavaScript engine scans the code, allocates memory space for all variables and functions, and sets up references. It does not physically move your code lines up; it simply sets up memory before executing line-by-line in the Execution Phase.
</details>

---

<details>
    <summary>
        <span style="font-size: 16px; color: #279CF5">How JavaScript work? What is Creation Phase, Execution Phase, Call Stack/Execution Stack.</span>
    </summary>
    </br>

JavaScript executes code through a two-phase process managed by the engine, using memory structures to track execution.

    * Creation Phase
    * Execution Phase

Every time JavaScript code runs, it creates an Execution Context in two distinct phases:

   * **Creation Phase**: Before running any code, the engine scans the scope to allocate memory.

     * Memory is set aside for variables and functions.

     * Function declarations are fully stored in memory (hoisted).

     * `var` variables are allocated memory and initialized to undefined.

     * `let` and `const` variables are allocated memory but left uninitialized, putting them in the Temporal Dead Zone (TDZ).

     * The this keyword binding and the lexical environment reference are established.

   * **Execution Phase**: The engine runs the code line-by-line, top-to-bottom.

     * Variables are assigned their actual values as code reaches their assignment lines.

     * Functions are invoked, which creates new execution contexts for each call.


    Call Stack (Execution Stack)

    The Call Stack (also known as the Execution Stack) is a LIFO (Last In, First Out) data structure that tracks where the program is in its execution path.

    +-----------------------------------+
    |   multiply() Execution Context    |  <- Top of stack (Currently running)
    +-----------------------------------+
    |    calculate() Execution Context  |
    +-----------------------------------+
    |  Global Execution Context (GEC)   |  <- Base of stack
    +-----------------------------------+
</details>

---
<details>
    <summary>
        <span style="font-size: 16px; color: #279CF5">Event loop in Javascript</span>
    </summary>
    </br>

The event loop is an important concept in JavaScript that enables asynchronous programming by handling tasks efficiently. Since JavaScript is single-threaded, it uses the event loop to manage the execution of multiple tasks without blocking the main thread.

JavaScript executes code synchronously in a single thread. However, it can handle asynchronous operations such as fetching data from an API, handling user events, or setting timeouts without pausing execution. This is made possible by the event loop.

### Working of Event Loop

The event loop continuously checks whether the call stack is empty and whether there are pending tasks in the callback queue or microtask queue.

![Event Loop ](/assets/event-loop.png)

![Event Loop ](/assets/event_loop_in_javascript.webp)

* **Call Stack**: JavaScript has a call stack where function execution is managed in a Last-In, First-Out (LIFO) order.

* **Web APIs (or Background Tasks)**: These include setTimeout, setInterval, fetch, DOM events, and other non-blocking operations.

* **Callback Queue (Task Queue or Macrotask Queue)**: When an asynchronous operation is completed, its callback is pushed into the task queue.

* **Microtask Queue**: Promises (.then(), .catch(), .finally()) and other microtasks are placed here. The microtask queue is always fully executed (drained) before moving to the next macrotask.

* **Event Loop**: It continuously checks the call stack and, if empty, moves tasks from the queue to the stack for execution.

### Microtask Queue vs. Macrotask (Task) Queue
To avoid any confusion during interviews or code execution analysis, keep this distinction in mind:

| Feature | Microtask Queue | Macrotask Queue (Task Queue / Callback Queue) |
| :--- | :--- | :--- |
| **Origin Term** | Introduced in ES6 / V8 Engine | HTML Standard / Event Loop Specification |
| **Examples** | `Promise.then()`, `queueMicrotask()`, `MutationObserver`, `process.nextTick (Node.js)` | `setTimeout`, `setInterval`, `setImmediate`, I/O, UI rendering, DOM events |
| **Priority** | Higher Priority | Lower Priority |
| **Event Loop Behavior** | Empties the **entire queue** (including nested microtasks) before moving to the next macrotask. | Executes **one** task per Event Loop tick, then moves to microtasks/rendering. |
</details>