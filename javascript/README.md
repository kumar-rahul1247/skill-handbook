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
        <span style="font-size: 18px; color: #279CF5">What is Closure ?</span>
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
        <span style="font-size: 18px; color: #279CF5">What is Hoisting ?</span>
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
        <span style="font-size: 18px; color: #279CF5">What is Event loop ?</span>
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
        <span style="font-size: 18px; color: #279CF5">What is the Difference between setTimeout, setInterval, setImmediate, process.nextTick ?</span>
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
        <span style="font-size: 18px; color: #279CF5">What is the Difference between call, apply, and bind method ?</span>
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

<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5"> What Temporal Dead Zone (TDZ) ?</span>
    </summary>
    </br>

  ### Technical Definition
  > **TDZ** is the time period when a variable exists, but accessing it before its **initialization line** throws an error.

  ---

  ### Internal Mechanism
  > It is **declared, but uninitialized**. 
  > *(Accessing it during this window triggers a `ReferenceError: Cannot access variable before initialization`)*

  ---

  ### Standard Definition
  > The **Temporal Dead Zone (TDZ)** is the time period between entering a scope and initializing a variable (`let` or `const`), during which accessing that variable throws a `ReferenceError`.

  ---

  ### TL;DR
  > **In short:** It is the zone where a variable exists, but cannot be used yet.

  ```
┌──────────────────────────────────────────────┐
│  Scope Entered  ──>  Variable Hoisted        │
├──────────────────────────────────────────────┤
│                                              │
│  TEMPORAL DEAD ZONE (TDZ)                    │
│  - Variable exists in scope                  │
│  - Memory reference is uninitialized         │
│  - Read/Write throws ReferenceError          │
│                                              │
├──────────────────────────────────────────────┤
│  let x = 10;   ──>  Initialization Line      │
└──────────────────────────────────────────────┘
│  TDZ Ends      ──>  Normal Execution         │
--──────────────────────────────────────────────
  ```

### Technical Lifecycle: `var` vs `let` / `const`

* `var` Lifecycle: Declaration and Initialization happen simultaneously at scope entry.

```javascript
  // 1. Declaration + Initialization (hoisted as undefined)
  console.log(a); // Output: undefined
  var a = 5;      // 2. Assignment
```

* `let` / `const` Lifecycle: Declaration is hoisted, but Initialization is deferred to the actual line of code.
```javascript
  // 1. Declaration (hoisted, TDZ begins)
  console.log(b); // Uncaught ReferenceError: Cannot access 'b' before initialization
  let b = 5;      // 2. Initialization + Assignment (TDZ ends)
```

### Key Technical Behaviors

* Temporal, Not Spatial: The TDZ is bounded by time (execution order), not physical line numbers.

```javascript
  const fn = () => console.log(val); // Fine: fn is defined, not called yet
  let val = 100;                     // TDZ ends here
  fn();                              // Output: 100
```

* Typeof Safety Break: Standard typeof normally returns "undefined" for undeclared variables, but inside the TDZ it throws a ReferenceError.

```javascript
  console.log(typeof undeclaredVar); // Output: "undefined"
  console.log(typeof tdzVar);        // ReferenceError
  let tdzVar;
```
</details>

----


<details>
    <summary>
        <span style="font-size: 18px; color: #279CF5">Variable Declarations: `var`, `let`, and `const` </span>
    </summary>
    </br>

 In JavaScript, variables can be declared using three different keywords: `var`, `let`, and `const`. 
 While they all serve the primary purpose of storing data. 
 They differ significantly in terms of **scope**, **hoisting**, **re-declaration**, and **mutability**.

## Quick Summary Comparison

  | Feature | `var` | `let` | `const` |
  | :--- | :--- | :--- | :--- |
  | **Scope** | Function Scope | Block Scope | Block Scope |
  | **Hoisting** | Hoisted (initialized as `undefined`) | Hoisted (in Temporal Dead Zone) | Hoisted (in Temporal Dead Zone) |
  | **Re-declaration** | Allowed in the same scope | Not allowed in the same scope | Not allowed in the same scope |
  | **Re-assignment** | Allowed | Allowed | Not allowed |
  | **Initial Value** | Optional | Optional | **Mandatory** upon declaration |

---

## 1. `var` (Function Scoped & Legacy)

  `var` is the original variable declaration keyword in JavaScript (prior to ES6/ES2015).

  ### Key Characteristics:
  * **Function Scope:** `var` variables are scoped to the nearest enclosing function. They ignore block structures like `if` statements or `for` loops.
  * **Hoisting:** `var` declarations are hoisted to the top of their function or global scope and initialized with `undefined`.
  * **Re-declaration & Re-assignment:** You can re-declare and re-assign a `var` variable within the same scope without throwing an error.

### Code Examples:

  ```javascript
  // Scope behavior
  function varScopeExample() {
    if (true) {
      var x = 10;
    }
    console.log(x); // 10 (accessible outside the block!)
  }

  // Hoisting behavior
  console.log(a); // undefined (declaration hoisted, but not value)
  var a = 5;

  // Re-declaration
  var b = 1;
  var b = 2; // Allowed
  console.log(b); // 2
  ```

---

## 2. `let` (Block Scoped & Re-assignable)

  Introduced in ES6, `let` provides modern block-level scoping and safer variable handling.

  ### Key Characteristics:
  * **Block Scope:** `let` variables are restricted to the block (enclosed by `{}`) in which they are declared (e.g., inside `if` statements, loops, or functions).
  * **Temporal Dead Zone (TDZ):** `let` variables are hoisted, but accessing them before their declaration results in a `ReferenceError`.
  * **No Re-declaration:** You cannot re-declare a `let` variable within the same block scope.

### Code Examples:

```javascript
  // Scope behavior
  if (true) {
    let y = 20;
    console.log(y); // 20
  }
  // console.log(y); // ReferenceError: y is not defined

  // Temporal Dead Zone (TDZ)
  // console.log(c); // ReferenceError: Cannot access 'c' before initialization
  let c = 10;

  // Re-assignment vs Re-declaration
  let count = 1;
  count = 2; // Allowed (re-assignment)
  // let count = 3; // SyntaxError: Identifier 'count' has already been declared
```

---

## 3. `const` (Block Scoped & Immutable Reference)

  Also introduced in ES6, `const` stands for "constant" and is used to declare values that should not be re-assigned.

  ### Key Characteristics:
  * **Block Scope:** Same block-scoping behavior as `let`.
  * **Must Be Initialized:** A `const` variable must be assigned a value during declaration.
  * **Immutable Reference (Not Immutable Value):** You cannot re-assign a `const` variable to a new value or memory address. However, if the value is an **object or array**, its properties/elements can still be modified.

  ### Code Examples:

```javascript
  // Initialization requirement
  // const Z; // SyntaxError: Missing initializer in const declaration
  const Z = 100;

  // Re-assignment attempt
  // Z = 200; // TypeError: Assignment to constant variable.

  // Objects and Arrays with const
  const user = { name: 'Alice', age: 25 };

  // Modifying properties IS allowed:
  user.age = 26; 
  user.city = 'New York';
  console.log(user); // { name: 'Alice', age: 26, city: 'New York' }

  // Re-assigning the object reference is NOT allowed:
  // user = { name: 'Bob' }; // TypeError: Assignment to constant variable.
  ```

---

## Detailed Feature Comparison

### A. Scope: Function vs. Block
```javascript
  function scopeDemo() {
    if (true) {
      var varVariable = "I am var";
      let letVariable = "I am let";
      const constVariable = "I am const";
    }

    console.log(varVariable); // Works: "I am var" - Its functional scope
    // console.log(letVariable); // ReferenceError - Reason: let declared in if block and accessing outside block
    // console.log(constVariable); // ReferenceError - Reason: const declared in if block and accessing outside block
  }
```

### B. Hoisting & Temporal Dead Zone (TDZ)
  Hoisting moves declarations to the top of their scope during compilation.
  * `var` is hoisted and initialized as `undefined`.
  * `let` and `const` are hoisted, but remain uninitialized in the **Temporal Dead Zone (TDZ)** until the code reaches their declaration line.

```javascript
  console.log(varNum); // Output: undefined - Hoisted 
  var varNum = 10;

  console.log(letNum); // Throws ReferenceError - Hoisted but Temoral Dead Zone Feature throw error
  let letNum = 20;
```

---

## Best Practices & Recommendations

  1. **Default to `const`:** Use `const` for most variable declarations. It prevents accidental re-assignments and makes intent clear.
  2. **Use `let` when re-assignment is needed:** Use `let` for variables that will change over time, such as loop counters, flags, or accumulator values.
  3. **Avoid `var`:** Modern JavaScript code bases generally avoid `var` entirely due to unpredictable scoping issues and hoisting side effects.
</details>