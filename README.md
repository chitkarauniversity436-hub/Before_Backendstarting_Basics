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
