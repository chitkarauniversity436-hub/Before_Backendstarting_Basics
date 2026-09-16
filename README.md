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
