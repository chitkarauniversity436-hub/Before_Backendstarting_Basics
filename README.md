# Before_Backendstarting_Basics

1). Synchronous JavaScript

First understand synchronous.

Synchronous means:
JavaScript executes code one line at a time, in order.

Example:
console.log("A");
console.log("B");
console.log("C");

Output:

A
B
C

JavaScript does:

Run A
 ↓
Finish A
 ↓
Run B
 ↓
Finish B
 ↓
Run C
 ↓
Finish C

So we can say:

Synchronous = wait for the current task to finish before starting the next task.




2). Why do we need Asynchronous JavaScript?

Imagine this:
console.log("Start");

for(let i = 0; i < 10000000000; i++) {
    // very long task
}
console.log("End");

The second console.log() has to wait until the loop finishes.

That's okay for a small task.

But imagine your backend needs to:

Get data from MongoDB
Read a file
Call another API
Send an email
Wait for a network response

These operations can take time.

If JavaScript waited for every operation, your application could become slow.

That's why we have asynchronous JavaScript.


3).Asynchronous JavaScript

Asynchronous means:

Start a time-consuming task and don't necessarily block the rest of the program while waiting for it.

Example:

console.log("Start");

setTimeout(() => {
    console.log("Task completed");
}, 2000);

console.log("End");

Output:

Start
End
Task completed

Notice something important:

Start
 ↓
setTimeout starts
 ↓
End
 ↓
2 seconds later
 ↓
Task completed

JavaScript didn't wait for the 2 seconds.



4).setTimeout()

setTimeout() is an easy way to understand asynchronous behavior.

setTimeout(() => {
    console.log("Hello");
}, 2000);

This means:

Execute this function after approximately 2 seconds.

Example:

console.log("1");

setTimeout(() => {
    console.log("2");
}, 2000);

console.log("3");

Output:

1
3
2

Not:

1
2
3

because the callback inside setTimeout runs later.

5). Important: JavaScript is Single-Threaded

This is an important backend concept.

JavaScript traditionally executes your JavaScript code using one main thread.

You can imagine:

JavaScript
    │
    ▼
One main worker

So you may ask:

"Then how can JavaScript handle multiple things?"

That's where the JavaScript runtime environment comes in.

In the browser, this involves browser APIs.

In Node.js, Node provides mechanisms such as its event loop and asynchronous I/O facilities.

For now, remember:

JavaScript
    ↓
Executes JavaScript code
    ↓
Event Loop + Runtime APIs
    ↓
Handles asynchronous operations

You'll study the Node.js Event Loop in your syllabus later.
