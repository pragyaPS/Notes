
### **Fibonacci Sequence**
- Recursive Fibonacci function:
  ```javascript
  function fib(n) {
    if (n < 2) return n;
    return fib(n - 1) + fib(n - 2);
  }
  ```
  - Time Complexity: O(2^n) (exponential).
  - Space Complexity: O(n) (due to call stack).

- Optimized Recursive Fibonacci (Memoization):
  ```javascript
  function fib(n, memo = {}) {
    if (n in memo) return memo[n];
    if (n < 2) return n;
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
    return memo[n];
  }
  ```
  - Time Complexity: O(n).
  - Space Complexity: O(n).

- Iterative Fibonacci:
  ```javascript
  function fib(n) {
    if (n < 2) return n;

    let prev = 0, curr = 1;
    for (let i = 2; i <= n; i++) {
      [prev, curr] = [curr, prev + curr];
    }
    return curr;
  }
  ```
  - Time Complexity: O(n).
  - Space Complexity: O(1).

- Reduce-based Fibonacci:
  ```javascript
  function fib(n) {
    if (n === 0) return 0;
    if (n === 1) return 1;

    return Array.from({ length: n - 1 }).reduce(
      ([prev, curr]) => [curr, curr + prev],
      [0, 1]
    )[1];
  }
  ```
  - Time Complexity: O(n).
  - Space Complexity: O(n) (due to the array).

---

### **Custom `call` Implementation**
- Example of creating a custom `.call` function:
  ```javascript
  Function.prototype.myCall = function (thisArg, ...argArray) {
    thisArg = thisArg || globalThis; // Ensure `thisArg` is valid
    const uniqueKey = Symbol("fn"); // Create a unique property
    thisArg[uniqueKey] = this;      // Temporarily assign the function

    const result = thisArg[uniqueKey](...argArray); // Call the function
    delete thisArg[uniqueKey];      // Clean up
    return result;
  };
  ```
- Usage:
  ```javascript
  function getAge() {
    return this.age;
  }

  const person = { age: 34 };
  console.log(getAge.myCall(person)); // Output: 34
  ```

---

### **`ref` vs `state` in React**
- **Use `ref` for:**
  - Values not part of rendering logic (e.g., timer IDs, DOM elements).
  - Persistent mutable storage without causing re-renders.

- **Use `state` for:**
  - Values that affect rendering (e.g., UI elements, data).
  - Triggering re-renders when values change.

#### **Example: Stopwatch**
```javascript
import { useState, useRef } from 'react';

export default function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    intervalRef.current = setInterval(() => {
      setNow(Date.now());
    }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>Start</button>
      <button onClick={handleStop}>Stop</button>
    </>
  );
}
```
- **Why Use `ref` for Timer ID?**
  - `ref` avoids unnecessary re-renders.
  - `state` would trigger a re-render on every interval update, which is inefficient.

---

### **Key JavaScript Methods**

#### **`slice`**
- **Non-mutating**: Extracts a portion of an array without modifying the original array.
- Syntax:
  ```javascript
  const newArray = arr.slice(start, end);
  ```
  - `start`: Starting index (inclusive).
  - `end`: Ending index (exclusive).
  - Supports negative indices.

- Example:
  ```javascript
  const arr = [1, 2, 3, 4, 5];
  console.log(arr.slice(1, 4)); // Output: [2, 3, 4]
  console.log(arr.slice(-3, -1)); // Output: [3, 4]
  ```

#### **`pop` vs `shift`**
- **`pop`**:
  - Removes the last element of an array.
  - Mutates the array.
  - Example:
    ```javascript
    const arr = [1, 2, 3];
    arr.pop(); // Removes 3
    console.log(arr); // [1, 2]
    ```

- **`shift`**:
  - Removes the first element of an array.
  - Mutates the array and shifts the remaining elements.
  - Example:
    ```javascript
    const arr = [1, 2, 3];
    arr.shift(); // Removes 1
    console.log(arr); // [2, 3]
    ```

#### **`Array.from`**
- Creates a new array from an iterable or array-like object.
- Syntax:
  ```javascript
  Array.from(arrayLike, mapFn);
  ```
- Example:
  ```javascript
  const arr = Array.from({ length: 5 }, (_, i) => i * 2);
  console.log(arr); // [0, 2, 4, 6, 8]
  ```

#### **`padStart` and `padEnd`**
- **`padStart`**: Pads the beginning of a string.
- **`padEnd`**: Pads the end of a string.
- Example:
  ```javascript
  console.log("5".padStart(3, "0")); // Output: "005"
  console.log("5".padEnd(3, "0"));   // Output: "500"
  ```

---

### **Other Concepts**
- **`this` in Custom `myCall`:**
  - Represents the function on which `myCall` is invoked.
  - The environment might display additional metadata for debugging purposes.

---

### **Recommendations**
- Use `state` for values that directly affect rendering.
- Use `ref` for persistent, mutable values that don’t affect rendering, such as DOM references or interval IDs.
- Optimize recursive solutions using memoization when dealing with overlapping subproblems (e.g., Fibonacci).

Let me know if you’d like further clarification or examples on any of these topics! 😊

