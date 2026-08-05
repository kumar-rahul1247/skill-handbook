# JavaScript Q&A 📚

<br>
<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5">What is JavaScript?</span>
    </summary>
    </br>

 JavaScript is a lightweight, high-level programming language used to make web pages interactive.  It runs in the browser and can also be used on the server side with Node.js.
</details>

---

<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5">What is Closure in JavaScript?</span>
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
        <span style="font-size: 18px; color: #279CF5">What is Hoisting in Javascript ?</span>
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
        <span style="font-size: 18px; color: #279CF5">How JavaScript work? What is Creation Phase, Execution Phase, Call Stack/Execution Stack.</span>
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
        <span style="font-size: 18px; color: #279CF5">Event loop in Javascript</span>
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

---

<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5">What is the Difference Between setTimeout, setInterval, setImmediate, process.nextTick ?</span>
    </summary>
    </br>

In Node.js, asynchronous operations are managed by the **Event Loop**. 
Methods like `process.nextTick`, `setTimeout`, `setInterval`, and `setImmediate` dictate **when** scheduled callbacks execute relative to the event loop's phases and microtask queues.

### **Execution Priority Overview**

```text
1. Microtask Queue (Highest Priority)
   ├── process.nextTick()
   └── Promise callbacks (.then, async/await)
─────────────────────────────────────────────
2. Event Loop Phases (Macrotasks)
   ├── Timers Phase    ---> setTimeout(), setInterval()
   ├── Poll Phase      ---> I/O callbacks, network, fs
   └── Check Phase     ---> setImmediate()
```
### **1. `setTimeout`**

  - **Description:**  
    The `setTimeout()` method sets a timer which executes a function or specified piece of code **once** the timer expires.

  - **Syntax:**
    ```js
    var timerId = setTimeout(function, delayInMs, arg1, arg2, ...);
    ```

  - **Example:**
    ```js
    function greet(name) {
      console.log("Hello, " + name + "!");
    }

    // Schedule greet to run once after 2000ms (2 seconds)
    var timerId = setTimeout(greet, 2000, "Alice");

    // Cancel the timeout before it runs
    // clearTimeout(timerId);
    ```

---

### **2. `setInterval`**

  - **Description:**  
    The `setInterval()` method repeatedly calls a function or executes a code snippet, with a fixed time delay between each call.

  - **Syntax:**
    ```js
    var intervalId = setInterval(function, delayInMs, arg1, arg2, ...);
    ```

  - **Example:**
    ```js
    var count = 1;

    var intervalId = setInterval(function() {
      console.log("Tick: " + count);
      count++;

      // Stop after 3 ticks
      if (count > 3) {
        clearInterval(intervalId);
      }
    }, 1000);

    // Output:
    // Tick: 1 (after 1s)
    // Tick: 2 (after 2s)
    // Tick: 3 (after 3s)
    ```

### **3. `setInterval` vs Recursive `setTimeout`**

Using `setInterval` can cause issues if the task inside takes longer to finish than the interval delay itself (causing calls to queue up or overlap). A common pattern to prevent this is **Recursive `setTimeout`**, which guarantees a fixed gap *between* executions.

  - **`setInterval` (Fixed Interval Rate):**
    ```js
    // Function runs every 1000ms regardless of how long task() takes
    setInterval(function task() {
      // If this takes 1500ms, execution overlaps
    }, 1000);
    ```

  - **Recursive `setTimeout` (Guaranteed Gap Delay):**
    ```js
    // Guarantees a full 1000ms pause AFTER task() finishes running
    setTimeout(function task() {
      // Perform work here...
      
      setTimeout(task, 1000); // Reschedule after completion
    }, 1000);
    ```

### **4. `setImmediate` vs `process.nextTick`**

In Node.js, both `process.nextTick` and `setImmediate` allow you to schedule callbacks asynchronously, but they operate at different stages of the execution cycle and with different priorities.

---

#### **Quick Comparison**

| Feature | `process.nextTick` | `setImmediate` |
|---|---|---|
| **Queue / Phase** | Microtask Queue (Node-specific) | Check Phase (Macrotask) |
| **Execution Timing** | Runs **immediately** after the current synchronous script, before the Event Loop continues or moves to the next phase. | Runs on the **next tick/turn** of the Event Loop, right after I/O events are processed. |
| **Priority** | **Higher** (executes before `setImmediate` and timers). | **Lower** (executes in the Check phase of the Event Loop). |
| **Starvation Risk** | High (recursive calls can block I/O completely). | Low (allows the Event Loop to continue processing other tasks). |

---

#### **1. `process.nextTick`**

  - **Description:**  
    Executes a callback immediately after the current operation completes, bypassing the Event Loop phases entirely. It is used to run microtasks before the Event Loop yields to I/O or timers.

    *NOTE*: Recursive calls to `process.nextTick()` will starve the Event Loop, blocking all file I/O, network requests, and timers.

  - **Syntax:**
    ```js
    process.nextTick(function, arg1, arg2, ...);
    ```

  - **Example:**
    ```js
    console.log("1. Start");

    process.nextTick(function() {
        console.log("2. NextTick Callback");
    });

    function greet(user, role) {
        console.log(`Hello, ${user}! Your role is: ${role}`);
    }

    // Passing "Alice" as 'user' and "Admin" as 'role'
    process.nextTick(greet, "Alice", "Admin");

    console.log("3. End");

    // Output:
    // 1. Start
    // 3. End
    // 2. NextTick Callback
    // Hello, Alice! Your role is: Admin
    ```

---

#### **2. `setImmediate`**

  - **Description:**  
    Schedules execution for the **Check Phase** of the Event Loop. It runs after I/O events (like reading files or network requests) are handled on the current loop iteration.

  - **Syntax:**
    ```js
    var immediateId = setImmediate(function, arg1, arg2, ...);
    
    // To cancel:
    // clearImmediate(immediateId);
    ```

  - **Example (Inside I/O Callback):**
    ```js
    var fs = require("fs");

    fs.readFile(__filename, function() {
      setTimeout(function() {
        console.log("setTimeout");
      }, 0);

      setImmediate(function() {
        console.log("setImmediate");
      });
    });

    // Output (Guaranteed inside I/O callbacks):
    // setImmediate
    // setTimeout
    ```

    ```
    ┌──────────────────────────┐
    ┌───>│    1. Timers Phase       │ (setTimeout / setInterval)
    │    └────────────┬─────────────┘
    │                 │
    │    ┌────────────┴─────────────┐
    │    │    2. Pending Callbacks  │
    │    └────────────┬─────────────┘
    │                 │
    │    ┌────────────┴─────────────┐
    │    │    3. Poll (I/O) Phase   │ <--- YOU ARE HERE (fs.readFile callback runs)
    │    └────────────┬─────────────┘      When fs.readFile finishes, loop moves directly to:
    │                 │
    │    ┌────────────┴─────────────┐
    │    │    4. Check Phase        │ <--- setImmediate runs HERE NEXT!
    │    └────────────┬─────────────┘
    │                 │
    └─────────────────┘
    ```
---

#### **3. Execution Priority Order**

When combined, asynchronous tasks execute in a strict hierarchy:

```js
console.log("1. Synchronous");

setTimeout(function() {
  console.log("4. setTimeout");
}, 0);

setImmediate(function() {
  console.log("5. setImmediate");
});

process.nextTick(function() {
  console.log("2. process.nextTick");
});

Promise.resolve().then(function() {
  console.log("3. Promise Microtask");
});

console.log("6. Synchronous End");

// Output:
// 1. Synchronous
// 6. Synchronous End
// 2. process.nextTick
// 3. Promise Microtask
// 4. setTimeout
// 5. setImmediate
```

```
START OF NODE.JS PROCESS
                  │
                  ▼
    ┌───────────────────────────┐
    │  Execute Main Sync Script │  <--- Schedules setTimeout(..., 0) & setImmediate(...)
    └─────────────┬─────────────┘
                  │
                  ▼
  ┌───────────────────────────────┐
  │     Microtask Queue Check     │  <--- process.nextTick() & Promises run first
  └───────────────┬───────────────┘
                  │
                  ▼
       ENTER EVENT LOOP PHASES
                  │
                  ▼
    ┌───────────────────────────┐
    │     1. Timers Phase       │  <--- setTimeout(..., 0) RUNS HERE FIRST!
    └─────────────┬─────────────┘
                  │
                  ▼
    ┌───────────────────────────┐
    │   2. Pending / I/O Phase  │
    └─────────────┬─────────────┘
                  │
                  ▼
    ┌───────────────────────────┐
    │     3. Poll Phase         │  <--- Handles I/O callbacks & waiting
    └─────────────┬─────────────┘
                  │
                  ▼
    ┌───────────────────────────┐
    │     4. Check Phase        │  <--- setImmediate() RUNS HERE LATER!
    └─────────────┬─────────────┘
                  │
                  ▼
         (Loop repeats...)
```
</details>

---

<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5">What is the Difference Between call, apply, and bind</span>
    </summary>
    </br>

In JavaScript, `call`, `apply`, and `bind` are methods that allow you to control the `this` value in which a function is executed. While their purposes are similar, they differ in:
- how they handle arguments
- when the function is invoked.

### `call`

  - **Description:**  
    The `call()` method invokes a function immediately, allowing you to specify the value of `this` and pass arguments individually (comma-separated).

    *NOTE*: The `call()` method invokes the function immediately and does not return a new function.  

  - **Syntax:**
    ```js
    func.call(thisArg, arg1, arg2, ...)
    ```

  - **Example:**
    ```js
    var student1 = { firstName: "Emma", lastName: "Rossi" };
    var student2 = { firstName: "Rey", lastName: "Belly" };

    function sayHello(greet1, greet2) {
      console.log( greet1 + " " + this.firstName + " " + this.lastName + ", " + greet2 );
    }

    sayHello.call(student1, "Hyie", "All Good ?"); // Hyie Emma Rossi, All Good ?
    sayHello.call(student2, "Hyie", "All Good ?"); // Hyie Rey Belly, All Good ?
    ```


### `apply`

  - **Description:**  
    The `apply()` method is similar to `call()`, but it takes the function arguments as an array (or array-like object) instead of individual arguments.

  - **Syntax:**  
    ```js
    func.apply(thisArg, [argsArray])
    ```

  - **Example:**
    ```js
    var student1 = { firstName: "Emma", lastName: "Rossi" };
    var student2 = { firstName: "Rey", lastName: "Belly" };

    function sayHello(greet1, greet2) {
      console.log( greet1 + " " + this.firstName + " " + this.lastName + ", " + greet2 );
    }

    sayHello.apply(student1, ["Hyie", "All Good ?"]); // Hyie Emma Rossi, All Good ?
    sayHello.apply(student2, ["Hyie", "All Good ?"]); // Hyie Rey Belly, All Good ?
    ```


### `bind`

  - **Description:**  
    The `bind()` method creates a new function with a specific `this` value and, optionally, persists initial arguments.
      - The `bind()` method returns a new function with a permanently set `this` value.
      - The first argument defines the `this` context.
      - Additional arguments are stored and used when the new function runs.

  - **Syntax:**  
    ```js
    var boundFunc = func.bind(thisArg, arg1, arg2, ...)
    ```

  - **Example:**
    ```js
    var student1 = { firstName: "Emma", lastName: "Rossi" };
    var student2 = { firstName: "Rey", lastName: "Belly" };

    function sayHello(greet1, greet2) {
      console.log( greet1 + " " + this.firstName + " " + this.lastName + ", " + greet2 );
    }

    var sayHelloStudent1 = sayHello.bind(student1);
    var sayHelloStudent2 = sayHello.bind(student2);

    sayHelloStudent1("Hello", "How are you?"); // Hello Emma Rossi, How are you?
    sayHelloStudent2("Hello", "How are you?"); // Hello Rey Belly, How are you?
    ```

#### Summary

  | Method | Invokes Function Immediately? | How Arguments Are Passed         | Returns      |
  |--------|-------------------------------|----------------------------------|--------------|
  | `call` | Yes                           | Comma-separated                  | Function's result |
  | `apply`| Yes                           | Array or array-like object       | Function's result |
  | `bind` | No                            | (Optional) preset, then rest     | New function      |

</details>

---