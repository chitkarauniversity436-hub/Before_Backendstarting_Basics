# Before_Backendstarting_Basics 
// Do it before 25

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

6). Callback

Now we come to callbacks.

A callback is simply:

A function that we give to another function so that it can be called later.

Example:

function greet(name, callback) {
    console.log("Hello " + name);

    callback();
}

function finish() {
    console.log("Finished");
}

greet("Tarun", finish);

Output:

Hello Tarun
Finished

Here:

finish

is passed as a callback.


7). Callback with setTimeout
console.log("Start");

setTimeout(() => {
    console.log("Data received");
}, 2000);

console.log("End");

The function:

() => {
    console.log("Data received");
}

is a callback.

It is executed later.



8. Why callbacks became a problem

Suppose you have several asynchronous operations.

getUser(function(user) {

    getOrders(user, function(orders) {

        getPayment(orders, function(payment) {

            sendEmail(payment, function() {

                console.log("Done");

            });

        });

    });

});

It becomes deeply nested.

This is called:

Callback Hell 😵

It can become difficult to:

Read
Debug
Maintain
Handle errors

That's one reason Promises became important.

9. Promise

A Promise represents the eventual result of an asynchronous operation.

Think of a Promise as:

"I don't have the result right now, but I'll give you the result later."

Imagine ordering food 🍔.

You order food
     ↓
Restaurant says:
"Your order is being prepared."
     ↓
       Promise
     ↓
 ┌───────────────┐
 │               │
 ▼               ▼
Success        Failure
  ↓               ↓
Food arrives   Order failed

A Promise has three states:

1. Pending

Operation is still running.

⏳ Pending
2. Fulfilled

Operation succeeded.

✅ Fulfilled
3. Rejected

Operation failed.

❌ Rejected
10. Creating a Promise
const promise = new Promise((resolve, reject) => {

    let success = true;

    if(success) {
        resolve("Task completed");
    }
    else {
        reject("Task failed");
    }

});

Here:

resolve()

means:

Success

and:

reject()

means:

Failure

11. Using a Promise with .then()
const promise = new Promise((resolve, reject) => {

    resolve("Data received");

});

promise.then((data) => {
    console.log(data);
});

Output:

Data received

.then() runs when the Promise is successfully resolved.

12. Handling Errors with .catch()
const promise = new Promise((resolve, reject) => {

    reject("Something went wrong");

});

promise
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error);
    });

Output:

Something went wrong

So:

.then()  → Success
.catch() → Error
13. Promise Example with Delay
function getData() {

    return new Promise((resolve, reject) => {

        setTimeout(() => {
            resolve("Data received");
        }, 2000);

    });

}

getData()
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error);
    });

Flow:

getData()
   ↓
Promise created
   ↓
Waiting 2 seconds
   ↓
resolve()
   ↓
.then()
   ↓
"Data received"
14. async / await

Promises are useful, but .then() chains can sometimes become difficult to read.

async/await gives us cleaner syntax for working with Promises.

Example:

function getData() {

    return new Promise((resolve) => {

        setTimeout(() => {
            resolve("Data received");
        }, 2000);

    });

}

Now:

async function showData() {

    const data = await getData();

    console.log(data);

}

showData();

Output after approximately 2 seconds:

Data received
15. What does async mean?

When you write:

async function showData() {

}

you are saying:

This function works asynchronously and returns a Promise.

Example:

async function hello() {
    return "Hello";
}

Even though you return a normal string:

"Hello"

the function actually returns a Promise.

16. What does await mean?

This is the most important part.

const data = await getData();

means roughly:

"Wait for this Promise to settle before continuing this async function."

For example:

async function getUser() {

    console.log("Start");

    const data = await getData();

    console.log(data);

    console.log("End");
}

Inside this async function, the code after await waits for the Promise result.

But the entire JavaScript application doesn't freeze.

This distinction is important.

17. Error Handling with async/await

Use:

try
catch

Example:

async function getData() {

    try {

        const data = await fetchData();

        console.log(data);

    }
    catch(error) {

        console.log(error);

    }

}

Think:

try
 ↓
Run asynchronous operation
 ↓
Success → continue
 ↓
Failure → catch
18. Callback → Promise → async/await

This is the evolution you should remember.

Callback
getData(function(data) {
    console.log(data);
});

↓

Promise
getData()
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.log(error);
    });

↓

async/await
async function main() {

    try {

        const data = await getData();

        console.log(data);

    }
    catch(error) {

        console.log(error);

    }

}

async/await doesn't replace Promises. It is syntax built around Promises.

19. Why is this VERY important for Backend?

Imagine your Node.js server receives:

1000 requests

Many requests may require:

Database
   ↓
Network
   ↓
File system
   ↓
Another API

These operations take time.

Node.js uses asynchronous programming so it can handle I/O efficiently rather than simply sitting idle while every operation finishes.

Example:

app.get("/users", async (req, res) => {

    const users = await User.find();

    res.json(users);

});

Here:

await User.find();

waits for the database operation's Promise within that request handler, while Node.js can continue handling other work.

This is why you will see async/await everywhere in backend development.

20. Now JSON

Your syllabus also needs JSON.

JSON stands for:

JavaScript Object Notation

It is a common format for sending data between frontend and backend.

Normal JavaScript Object
const user = {
    name: "Tarun",
    age: 20
};
JSON
{
    "name": "Tarun",
    "age": 20
}

They look similar, but JSON is data format, not a JavaScript object.

21. Why do we need JSON?

Suppose React asks your Node.js backend:

"Give me the user's information."

Backend might send:

{
    "name": "Tarun",
    "age": 20,
    "course": "Backend"
}

The frontend receives that data and displays it.

So:

React
   │
   │ Request
   ▼
Node.js / Express
   │
   │ Database
   ▼
MongoDB
   │
   ▼
Node.js
   │
   │ JSON Response
   ▼
React

JSON is extremely common in REST APIs.

22. JSON.stringify()

Converts a JavaScript object → JSON string.

const user = {
    name: "Tarun",
    age: 20
};

const jsonData = JSON.stringify(user);

console.log(jsonData);

Result:

{"name":"Tarun","age":20}
23. JSON.parse()

Converts JSON string → JavaScript object.

const jsonData = '{"name":"Tarun","age":20}';

const user = JSON.parse(jsonData);

console.log(user.name);

Output:

Tarun

Remember:

JSON.stringify()
JavaScript Object
       ↓
    JSON String

and:

JSON.parse()
JSON String
       ↓
JavaScript Object
Debug
Maintain
Handle errors

That's one reason Promises became important.
