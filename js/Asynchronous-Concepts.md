# Asynchronous Concepts:

## Question 1 : How JS executes the code?

**Answer** :- Firstly, javascript runs in a single-threaded, synchronous manner, which has only one call stack to execute the code. To understand how to execute the code, we need to understand some concepts, lets see them and understand with an example as well:

1. **Execution Context** - whenever a code is executed, an execution context gets created, which is pushed into the call stack and the code gets executed according to that. There are mainly two types of execution context:

   - **Global Execution Context** - this gets created first and is pushed into the call stack, which starts executing the code line by line. It is created by default.
   - **Function Execution Context** - while executing the code line by line, if it recognize a function invocation, another execution context is created and is placed on top of the stack above the global execution context.

2. **Call Stack** - inside the javascript engine there is a call stack present through which the code gets executed. Javascript has only one call stack to execute the whole code. It manages the execution contexts. The call stack is a stack data-structure which uses the Last-In-First-Out(LIFO) method to execute the code. Whenever a function is called, a new execution context is created and pushed onto the stack. When the function returns, the context is popped from the stack.

3. **Event Loop, Callback Queue and Micro-task Queue** - the event loop, callback queue and micro-task queue is required when we write asynchronous javascript code.

   - **Callback Queue** - whenever we write a block of code which needs to be executed after some time, it gets stored in the web API section and once the timer is up it gets pushed into the callback queue, and denotes that it is ready to be called once the call stack is empty(the whole code has been executed). This is a callback queue.
   - **Event Loop** - On the other hand, event loop works like a gatekeeper, it continuously monitors the call stack to see when it gets empty. Once it gets empty, the event loop sees the callback queue, and if there is any function waiting to be executed, the event loop sends it to the call stack and that function gets executed.
   - **Micro-task Queue** - this is similar to callback queue, the only difference is that, the high priority callback functions are pushed into the micro-task queue, and the event loop gives priority to the micro-task queue, meaning it will call any function waiting in the micro-task queue first, and then move on to the callback queue. By default, Promises enter the micro-task queue.

4. **Execution Flow** -

- **Global Execution Context Creation**: The global context is created, and global code starts executing.
- **Function Calls**: When a function is called, a new execution context is created and pushed onto the call stack.
- **Execution of Function Code**: The code inside the function is executed, and if it calls another function, a new context is created and pushed onto the stack.
- **Completion of Function Execution**: Once the function completes, its context is popped from the call stack.
- **Asynchronous Operations**: When an async operation (like setTimeout or Promise) is encountered, it's handed off to the browser APIs. The function continues executing while the async operation runs in the background.
- **Event Loop**: After the async operation is completed, its callback is pushed to the callback queue. The event loop checks if the call stack is empty and then pushes the callback context onto the call stack for execution.

5. **Example** :

```
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 1000);

Promise.resolve('Promise').then(console.log);

console.log('End');

```

Output :

```
Start

End

Promise

Timeout

```

6. **Execution Steps** :

- **Global Execution Context**: The global context is created and console.log('Start') is pushed onto the call stack and executed.
- **setTimeout**: The setTimeout function is called, and the callback is registered with the browser API to be executed after 1000 ms. The context is popped off the stack.
- **Promise**: The Promise.resolve('Promise') is called and its then callback is registered to be executed once the promise resolves.
- **console.log('End')**: This line is executed, logging End.
- **Promise Callback Execution**: The event loop finds the Promise callback in the micro-task queue, and it's pushed onto the call stack and executed, logging Promise.
- **setTimeout Callback Execution**: After 1000 ms, the event loop finds the setTimeout callback in the task queue, pushes it onto the call stack, and it's executed, logging Timeout.

## Question 2 : What is diff between Sync & Async ?

**Answer** :-

**Sync(Synchronous)**

- In synchronous programming, tasks are executed one after another. Each task must complete before the next one starts.
- The execution follows a sequential order.
- If a task takes a long time to complete, it blocks the entire execution of the program until it finishes.
- **Example of synchronous code**:

```
console.log('Start');


function slowTask() {
for (let i = 0; i < 1e9; i++);
}

slowTask();

console.log('End');

```

Output :

```

Start
End (after a delay)

```

**Async(Asynchronous)**

- In asynchronous programming, tasks can start and finish independently of the main program flow. The program can continue executing other tasks while waiting for async tasks to complete.
- Async tasks do not block the main thread.
- Long-running tasks (like network requests(using fetch) or file I/O) are handled asynchronously, allowing the program to remain responsive.
- Asynchronous operations typically use callbacks, promises, or async/await syntax to handle the completion of tasks.
- **Example of asynchronous code**:

```
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 1000);

console.log('End');

```

Output :

```
Start
End
Timeout (after 1 second)

```

- **Example Combining Sync and Async**:

```
console.log('Start');

function slowSyncTask() {
  for (let i = 0; i < 1e9; i++); // Simulate a slow sync task
  console.log('Sync Task Completed');
}

slowSyncTask();

setTimeout(() => {
  console.log('Timeout');
}, 1000);

console.log('End');

```

Output:

```
Start
Sync Task Completed (after a delay)
End
Timeout (after 1 second)

```

## Question 3 : What are the ways to make the code Async?

**Answer** :- There are several ways to make the code async. Here are few approaches:

1. **Callbacks**:

A callback is a function passed as an argument to another function, which is then executed inside the outer function to complete some kind of action. -**Example**:

```
console.log('Start');

function doAsyncTask(callback) {
  setTimeout(() => {
    console.log('Async Task Complete');
    callback();
  }, 1000);
}

doAsyncTask(() => {
  console.log('Callback Executed');
});

console.log('End');

```

Output:

```
Start
End
Async Task Complete
Callback Executed

```

2. **Promises**:

A promise is an object which represents the eventual completion (or failure) of an asynchronous operation and its resulting value. Promises provide a more readable and maintainable way to handle async operations compared to callbacks.

- **Example**:

```
console.log('Start');

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise Resolved');
  }, 1000);
});

promise.then((message) => {
  console.log(message);
});

console.log('End');

```

Output:

```
Start
End
Promise Resolved

```

3. **Async/Await**:

Async and await provide a way to write asynchronous code that looks synchronous. An async function returns a promise, and the await keyword can be used to wait for the promise to resolve.

- **Example**:

```
console.log('Start');

async function doAsyncTask() {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Async/Await Resolved');
    }, 1000);
  });

  const result = await promise;
  console.log(result);
}

doAsyncTask();

console.log('End');

```

Output:

```
Start
End
Async/Await Resolved

```

4. **Using the fetch API**:

The fetch API is used to make network requests and is inherently asynchronous. It returns a promise that resolves to the Response object representing the response to the request.

- **Example**:

```
console.log('Start');

async function fetchData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

fetchData();

console.log('End');

```

Output:

```
Start
End
{...data from the API...}

```

5. **setTimeout and setInterval**:

These functions allow you to schedule code to run after a delay (setTimeout) or at regular intervals (setInterval).

- **Example**:

```
console.log('Start');

setTimeout(() => {
  console.log('Timeout Executed');
}, 1000);

console.log('End');

```

Output:

```
Start
End
Timeout Executed

```

## Question 4 : What are Web Browser APIs?

**Answer** :- In simple words, web browser APIs lets us use the superpowers, such as setTimeout(), fetch(), DOM APIs, console, etc. These are not part of javascript but we can use these functions with the help of APIs.

## Question 5 : What is event loop?

**Answer** :- Event loop works as a gatekeeper. It continuously monitors, whether the call stack is empty or not. Once the call stack gets empty, it first checks the mucro-task queue, and if there are any callback functions waiting in the queue it puts them in the call stack to be executed. As soon as the micro-task queue gets empty, it then sees the callback queue and sends the callback functions waiting to be executed in the call stack.

- **Example**:

```
console.log('Start');

setTimeout(() => {
  console.log('Timeout Callback');
}, 1000);

fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(data => console.log('Fetch Data:', data));

console.log('End');

```

Output:

```
Start
End
Fetch Data: { ... }
Timeout Callback

```

- **Execution steps**:

  - console.log('Start') is executed and "Start" is printed.
  - setTimeout is called, its callback is registered, and the timer starts.
  - fetch is called, and the request is sent. The promise callbacks are registered.
  - console.log('End') is executed and "End" is printed.
  - The setTimeout timer completes after 1000ms and its callback is placed in the queue.
  - The fetch request completes, and its then callbacks are placed in the micro-task queue.
  - The call stack is empty after the initial execution.
  - The event loop first processes the micro-task queue (promises), executing the fetch callbacks.
  - The event loop then processes the task queue, executing the setTimeout callback.

## Question 6 : What is a callback hell ?

**Answer** :- Callback hell, also known as "Pyramid of Doom," occurs when multiple nested callback functions are used in asynchronous programming, leading to code that is difficult to read, maintain, and debug. This situation often arises when performing several asynchronous operations in sequence, where each operation depends on the completion of the previous one.

- **Example**:

```
const fs = require("fs");
const path = require("path");

function convertFileContent(src, dest, transform, callback) {
    fs.readFile(src, 'utf-8', (err, data) => {
        if (err) {
            return callback(err);
        }

        const transformedData = transform(data);
        fs.writeFile(dest, transformedData, (err) => {
            if (err) {
                return callback(err);
            }

            callback(null, dest);
        });
    });
}

function appendToFile(file, content, callback) {
    fs.appendFile(file, content, (err) => {
        if (err) {
            return callback(err);
        }

        callback(null);
    });
}


function readFilesAndDelete(fileList, callback) {
    fs.readFile(fileList, 'utf-8', (err, data) => {
        if (err) {
            return callback(err);
        }

        const files = data.trim().split('\n');

        let filesDeleted = 0;

        files.forEach((file) => {
            fs.unlink(file, (err) => {
                if (err) {
                    return callback(err);
                }

                filesDeleted++;
                if (filesDeleted === files.length) {
                    callback(null);
                }
            });
        });
    });
}

function fsProblem2(srcFile) {
    const filenames = 'filenames.txt';
    const upperCaseFile = 'uppercase.txt';
    const lowerCaseFile = 'lowercase.txt';
    const sortedFile = 'sorted.txt';

    convertFileContent(srcFile, upperCaseFile, (data) => data.toUpperCase(), (err, upperFile) => {
        if (err) {
            return console.error(err);
        }

        appendToFile(filenames, `${upperFile}\n`, (err) => {
            if (err) {
                return console.error(err);
            }

            convertFileContent(upperFile, lowerCaseFile, (data) => data.toLowerCase().split('.').join('.\n'), (err, lowerFile) => {
                if (err) {
                    return console.error(err);
                }

                appendToFile(filenames, `${lowerFile}\n`, (err) => {
                    if (err) {
                        return console.error(err);
                    }

                    convertFileContent(lowerFile, sortedFile, (data) => data.split('\n').sort().join('\n'), (err, sortedFile) => {
                        if (err) {
                            return console.error(err);
                        }

                        appendToFile(filenames, `${sortedFile}\n`, (err) => {
                            if (err) {
                                return console.error(err);
                            }

                            readFilesAndDelete(filenames, (err) => {
                                if (err) {
                                    return console.error(err);
                                }

                                console.log('All transformations complete and files deleted successfully.');
                            });
                        });
                    });
                });
            });
        });
    });
}
```

- Problems with Callback Hell:

  - The code becomes hard to read due to the nested structure.
  - Updating or modifying the code becomes challenging.
  - Managing errors in deeply nested callbacks is complex and often requires additional logic.
  - Adding more asynchronous operations increases the complexity and deepens the nesting, exacerbating the problem.

## Question 7 : What is inversion of control in callbacks?

**Answer** :-

- Inversion of Control in the context of callbacks refers to the design pattern where the control flow of a program is inverted, meaning that instead of the code calling the necessary functions directly and managing the flow, it hands over the control to some framework, library, or piece of code (such as a callback function). This entity then takes over the execution at appropriate times or in response to certain events.

- When using callbacks, you are essentially passing a piece of code (the callback function) to another function or library, which will execute the callback when the time is right (e.g., after an asynchronous operation completes). This contrasts with a traditional control flow where your code directly calls each function in sequence.

- As the full control is with a function, framework, or a library, you never know when the nested callback function will be called or even will it ever be called.

## Question 8 : What is a Promise?

**Answer** :- A promise is an object which represents the eventual completion (or failure) of an asynchronous operation and its resulting value. Promises provide a more readable and maintainable way to handle async operations compared to callbacks.

- **Example**:

```
console.log('Start');

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise Resolved');
  }, 1000);
});

promise.then((message) => {
  console.log(message);
});

console.log('End');

```

Output:

```
Start
End
Promise Resolved

```

## Question 9 : How to create a new promise?

**Answer** :- Creating a new promise in JavaScript involves using the 'Promise' constructor, which takes a single function argument. The function is executed immediately by the 'Promise' implementation, passing in two parameters: 'resolve' and 'reject'. You use 'resolve' to fulfill the promise with a value, and 'reject' to reject the promise with a reason (typically an error).

- **Example**: create a simple promise that simulates an asynchronous operation using setTimeout.

```
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve('Operation was successful');
    } else {
      reject('Operation failed');
    }
  }, 2000);
});

// Handling the promise
myPromise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error);
  });

```

## Question 10 : What are different states of a Promise -pending, fulfilled, rejected?

**Answer** :- A Promise in JavaScript has three distinct states that represent the progress and outcome of an asynchronous operation. These states are:

- **Pending**: The initial state of a promise, where the outcome is neither known nor determined. The promise is still ongoing, and no result (value or error) has been produced yet.

- **Fulfilled**: The state of a promise when the asynchronous operation completes successfully. In this state, the promise has a result value that can be accessed.

- **Rejected**: The state of a promise when the asynchronous operation fails. In this state, the promise has a reason for failure, typically represented as an error.

- **Example**:

```
const promise = new Promise((resolve, reject) => {
  const success = true; '// Simulate a successful operation'
  setTimeout(() => {
    if (success) {
      resolve('Operation successful'); '// Fulfilled state'
    } else {
      reject('Operation failed'); '// Rejected state'
    }
  }, 1000); // Simulate an asynchronous operation with a 1-second delay
});

console.log(promise); '// Logs: Promise { <pending> }'

promise
  .then(result => {
    console.log('Fulfilled:', result); '// Logs: Fulfilled: Operation successful'
  })
  .catch(error => {
    console.error('Rejected:', error); '// If it fails, logs: Rejected: Operation failed'
  });

```

## Question 11 : How to consume an existing promise?

**Answer** :- Consuming an existing promise involves using the '.then., '.catch', and optionally '.finally' methods to handle the resolution or rejection of the promise. These methods allow you to define what should happen when the promise is fulfilled, rejected, or completed.

- '.then' Method: Used to handle the fulfillment of the promise. It takes two optional arguments: a callback function for the fulfillment value and a callback function for the rejection reason.

- '.catch' Method: Used to handle the rejection of the promise. It takes one argument: a callback function for the rejection reason.

- '.finally' Method: Used to execute a callback once the promise is settled (either fulfilled or rejected). It takes one argument: a callback function that runs regardless of the promise’s outcome.

- **Example**: Let's see an example with a real-world asynchronous operation using the fetch API, which returns a promise:

```
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => {
    console.log('Data fetched:', data); // Logs the fetched data if successful
  })
  .catch(error => {
    console.error('Fetch error:', error); // Logs the error if there's a problem with the fetch
  })
  .finally(() => {
    console.log('Fetch operation completed'); // Logs when the fetch operation is completed
  });

```

## Question 12 : How to chain promises using '.then'?

**Answer** :- Chaining promises using '.then' allows you to execute asynchronous operations in sequence, where each subsequent operation depends on the result of the previous one. This is achieved by returning a new promise from the '.then' callback, which is then handled by the next '.then' in the chain.

- **Example**: Consider an example where we perform three asynchronous operations sequentially:

  - Fetch user data from an API.
  - Fetch posts made by the user.
  - Fetch comments on those posts.
    Here’s how you can chain these operations:

```
function fetchUser(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
    .then(response => response.json());
}

function fetchPosts(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
    .then(response => response.json());
}

function fetchComments(postId) {
  return fetch(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`)
    .then(response => response.json());
}

// Chain the promises
fetchUser(1)
  .then(user => {
    console.log('User:', user);
    return fetchPosts(user.id);
  })
  .then(posts => {
    console.log('Posts:', posts);
    // Assuming we want to fetch comments for the first post
    if (posts.length > 0) {
      return fetchComments(posts[0].id);
    } else {
      throw new Error('No posts found for the user');
    }
  })
  .then(comments => {
    console.log('Comments on first post:', comments);
  })
  .catch(error => {
    console.error('Error:', error);
  });

```

## Question 13 : How to handle errors in a promise chain using '.catch'?

**Answer** :- Handling errors in a promise chain using '.catch' is crucial to ensure that any errors occurring at any point in the chain are appropriately managed. The '.catch' method is used to handle rejected promises, and it can be placed at the end of the promise chain to catch any errors that occur during any of the asynchronous operations in the chain.

- **Example**: Consider an example where we perform three asynchronous operations and handle any errors that occur during the process:

      - Fetch user data from an API.
      - Fetch posts made by the user.
      - Fetch comments on those posts.

  Here's how to chain these operations and handle errors:

```
function fetchUser(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error('Failed to fetch user');
      }
      return response.json();
    });
}

function fetchPosts(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error('Failed to fetch posts');
      }
      return response.json();
    });
}

function fetchComments(postId) {
  return fetch(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error('Failed to fetch comments');
      }
      return response.json();
    });
}

// Chain the promises with error handling
fetchUser(1)
  .then(user => {
    console.log('User:', user);
    return fetchPosts(user.id);
  })
  .then(posts => {
    console.log('Posts:', posts);
    // Assuming we want to fetch comments for the first post
    if (posts.length > 0) {
      return fetchComments(posts[0].id);
    } else {
      throw new Error('No posts found for the user');
    }
  })
  .then(comments => {
    console.log('Comments on first post:', comments);
  })
  .catch(error => {
    console.error('Error occurred:', error);
  });

```

## Question 14 : '.finally' block in a promise chain.

**Answer** :- The finally block in a promise chain is used to execute code after the promise is settled, regardless of whether the promise was fulfilled or rejected.

- **Example**:

```
new Promise((resolve, reject) => {
    // Simulate an asynchronous operation
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            resolve("Operation successful");
        } else {
            reject("Operation failed");
        }
    }, 1000);
})
.then(result => {
    console.log(result);
    // Handle the successful result
})
.catch(error => {
    console.error(error);
    // Handle the error
})
.finally(() => {
    console.log("Cleanup operations or final tasks");
    // This will run regardless of the outcome
});

```

The '.finally' block is guaranteed to execute after the promise chain is completed, making it a reliable place to put any code that needs to run after the asynchronous operation, no matter its outcome.

## Question 15 : What happens when an Error gets thrown inside '.then' when there is a '.catch'?

**Answer** :- When an error is thrown inside a .then method in a promise chain, the error is propagated to the nearest .catch method that follows in the chain. The .catch method will then handle the error.

- **Example**:

```
new Promise((resolve, reject) => {
    resolve("Initial success");
})
.then(result => {
    console.log(result); // Logs: "Initial success"
    // Simulate an error
    throw new Error("Something went wrong in .then");
})
.catch(error => {
    console.error("Caught in .catch:", error.message);
    // Handles the error
});

```

## Question 16 : What happens when an Error gets thrown inside '.then' when there is no '.catch'?

**Answer** :- When an error is thrown inside a .then method and there is no .catch method to handle the error, the promise chain will result in an unhandled promise rejection.

- **Example**:

```
new Promise((resolve, reject) => {
    resolve("Initial success");
})
.then(result => {
    console.log(result); // Logs: "Initial success"
    // Simulate an error
    throw new Error("Something went wrong in .then");
})
.then(result => {
    console.log("This will not be logged");
});

// No .catch method to handle the error

```

## Question 17 : Why must '.catch' be placed towards the end of the promise chain?

**Answer** :- The '.catch' method should be placed towards the end of the promise chain to ensure that it can handle any errors that occur anywhere in the chain. This placement ensures that any error thrown or any promise that gets rejected will be caught and handled appropriately.

- **Example**:

```
new Promise((resolve, reject) => {
    resolve("Step 1 success");
})
.then(result => {
    console.log(result); // Logs: "Step 1 success"
    return "Step 2 success";
})
.then(result => {
    console.log(result); // Logs: "Step 2 success"
    throw new Error("Error in Step 3");
})
.then(result => {
    // This will not execute because the previous .then threw an error
    console.log(result);
})
.catch(error => {
    console.error("Caught in .catch:", error.message); // Logs: "Caught in .catch: Error in Step 3"
    // Handles the error
});

```

In this example:

    - The promise resolves successfully with "Step 1 success".
    - The first '.then' handles this and returns "Step 2 success".
    - The second '.then' logs "Step 2 success" and throws an error.
    - The third '.then' does not execute because the error was thrown.
    - The '.catch' at the end catches the error and logs the message.

## Question 18 : How to consume multiple promises by chaining?

**Answer** :- To consume multiple promises by chaining, you can sequentially call '.then' methods on each promise, ensuring each promise is resolved one after the other. This is useful when the result of one promise depends on the result of a previous one, or when you need to perform actions in a specific order.

- **Example**:

```
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise1"), 1000);
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise2"), 2000);
});

const promise3 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise3"), 3000);
});

promise1
    .then(result1 => {
        console.log(result1); // Logs: "Result from promise1"
        return promise2;      // Return the next promise
    })
    .then(result2 => {
        console.log(result2); // Logs: "Result from promise2"
        return promise3;      // Return the next promise
    })
    .then(result3 => {
        console.log(result3); // Logs: "Result from promise3"
    })
    .catch(error => {
        console.error("Caught in .catch:", error.message);
    });

```

In this example:

    - promise1 resolves after 1 second and logs its result.
    - The first '.then' method returns promise2.
    - promise2 resolves after 2 seconds and logs its result.
    - The second '.then' method returns promise3.
    - promise3 resolves after 3 seconds and logs its result.
    - If any of the promises reject, the '.catch' method handles the error.

## Question 19 : How to consume multiple promises by Promise.all?

**Answer** :- This method takes an iterable (e.g., an array) of promises and returns a single promise that resolves when all of the promises have resolved or rejects when any one of the promises rejects.

- **Example**:

```
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise1"), 1000);
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise2"), 2000);
});

const promise3 = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Result from promise3"), 3000);
});

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results); // Logs: ["Result from promise1", "Result from promise2", "Result from promise3"]
    })
    .catch(error => {
        console.error("Caught in .catch:", error.message);
    });

```

In this example:

    - Three promises, promise1, promise2, and promise3, are created, each resolving after a different amount of time.
    - Promise.all is called with an array of these promises.
    - The then method logs the array of results once all promises have resolved.
    - If any promise rejects, the catch method handles the error.

## Question 20 : How to do error handling when using promises?

**Answer** :- Error handling in promises is crucial to ensure your code can gracefully handle failures and avoid unhandled promise rejections. Here are several strategies to effectively handle error.

1. Using '.catch' Method :

   - **Example**:

   ```
   const fetchData = () => new Promise((resolve, reject) => {
   // Simulate an asynchronous operation
   setTimeout(() => reject(new Error("Failed to fetch data")), 1000);
   });

   fetchData()
   .then(result => {
       console.log(result);
   })
   .catch(error => {
       console.error("Caught in .catch:", error.message);
   });

   ```

2. Using async/await with try/catch :

- **Example**:

```
const fetchData1 = () => new Promise((resolve) => setTimeout(() => resolve("Data from fetch1"), 1000));
const fetchData2 = () => new Promise((resolve, reject) => setTimeout(() => reject(new Error("Failed in fetch2")), 2000));
const fetchData3 = () => new Promise((resolve) => setTimeout(() => resolve("Data from fetch3"), 3000));

async function fetchAllData() {
  try {
      const data1 = await fetchData1();
      console.log(data1);

      const data2 = await fetchData2();
      console.log(data2);

      const data3 = await fetchData3();
      console.log(data3);
  } catch (error) {
      console.error("Caught in .catch:", error.message);
  }
}

fetchAllData();

```

## Question 21 : Why is error handling the most important part of using a promise?

**Answer** :- Error handling is a crucial part of using promises in JavaScript for several reasons:

1. Ensuring Reliability and Stability
   Proper error handling ensures that your application remains stable and can recover gracefully from unexpected issues. Without handling errors, a single unhandled error can cause an entire chain of operations to fail, leading to an unstable and unreliable application.

2. Preventing Unhandled Promise Rejections
   Unhandled promise rejections can lead to warnings and, in some environments like Node.js, can cause the process to terminate. This can result in a poor user experience and potential data loss.

3. Improving Debugging and Maintenance
   Handling errors allows you to log them and gather necessary information for debugging. This makes it easier to identify and fix issues, improving the maintainability of your code.

4. Graceful Degradation and Recovery
   Error handling enables your application to degrade gracefully when something goes wrong. Instead of crashing or becoming unusable, the application can display a meaningful error message or fallback behavior.

5. Ensuring Correct Flow of Execution
   Proper error handling ensures that the flow of execution is correct and that subsequent operations are only performed if previous ones were successful. This is especially important in sequences of dependent asynchronous operations.

6. User Experience
   Providing informative error messages and handling errors gracefully improves the user experience. Users are more likely to trust and continue using an application that handles errors well.

## Question 22 : How to promisify an asynchronous callbacks based function - eg. setTimeout, fs.readFile?

**Answer** :- To promisify an asynchronous callback-based function means to convert it into a function that returns a promise. This allows you to use the function with modern JavaScript constructs like async/await and promise chaining, making your code cleaner and easier to manage.

- **Example 1**: Promisifying 'setTimeout()'

```
function delay(ms) {
    return new Promise((resolve) => {
        setTimeout(resolve, ms);
    });
}

// Usage with .then
delay(1000).then(() => {
    console.log('Executed after 1 second');
});

// Usage with async/await
(async () => {
    await delay(1000);
    console.log('Executed after 1 second');
})();

```

- **Example 2**: Promisifying 'fs.readFile'

```
const fs = require('fs');

function readFileAsync(path, encoding) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, encoding, (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
}

// Usage with .then
readFileAsync('example.txt', 'utf8')
    .then(data => {
        console.log('File contents:', data);
    })
    .catch(error => {
        console.error('Error reading file:', error);
    });

// Usage with async/await
(async () => {
    try {
        const data = await readFileAsync('example.txt', 'utf8');
        console.log('File contents:', data);
    } catch (error) {
        console.error('Error reading file:', error);
    }
})();

```

## Question 23 : How to use the following promise based functions:

### Promise.resolve -

Promise.resolve creates a promise that is resolved with the given value.

- **Example**:

```
const resolvedPromise = Promise.resolve('Resolved value');

resolvedPromise.then(value => {
    console.log(value); // Output: "Resolved value"
});

```

### Promise.reject -

Promise.reject creates a promise that is rejected with the given reason.

- **Example**:

```
const rejectedPromise = Promise.reject(new Error('Rejected reason'));

rejectedPromise.catch(error => {
    console.error(error.message); // Output: "Rejected reason"
});

```

### Promise.all -

Promise.all takes an array of promises and returns a single promise that resolves when all of the input promises have resolved or rejects when any one of the input promises rejects. The resolved value is an array of the resolved values from the input promises.

- **Example**:

```
const promise1 = Promise.resolve('Result 1');
const promise2 = Promise.resolve('Result 2');
const promise3 = Promise.resolve('Result 3');

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results); // Output: ["Result 1", "Result 2", "Result 3"]
    })
    .catch(error => {
        console.error(error);
    });

```

### Promise.allSettled -

Promise.allSettled takes an array of promises and returns a single promise that resolves when all of the input promises have settled (either resolved or rejected). The resolved value is an array of objects that describe the outcome of each promise.

- **Example**:

```
const promise1 = Promise.resolve('Result 1');
const promise2 = Promise.reject('Error 2');
const promise3 = Promise.resolve('Result 3');

Promise.allSettled([promise1, promise2, promise3])
    .then(results => {
        results.forEach((result, index) => {
            if (result.status === 'fulfilled') {
                console.log(`Promise ${index + 1} resolved with: ${result.value}`);
            } else {
                console.log(`Promise ${index + 1} rejected with: ${result.reason}`);
            }
        });
        // Output:
        // Promise 1 resolved with: Result 1
        // Promise 2 rejected with: Error 2
        // Promise 3 resolved with: Result 3
    });

```

### Promise.any -

Promise.any takes an array of promises and returns a single promise that resolves as soon as any one of the input promises resolves.

- **Example**:

```
const promise1 = Promise.reject('Error 1');
const promise2 = Promise.resolve('Result 2');
const promise3 = Promise.resolve('Result 3');

Promise.any([promise1, promise2, promise3])
    .then(result => {
        console.log(result); // Output: "Result 2"
    })
    .catch(error => {
        console.error(error);
    });

const promise4 = Promise.reject('Error 4');
const promise5 = Promise.reject('Error 5');

Promise.any([promise4, promise5])
    .catch(error => {
        console.error(error); // Output: AggregateError: All promises were rejected
    });

```

### Promise.race -

Promise.race takes an array of promises and returns a single promise that resolves or rejects as soon as any one of the input promises resolves or rejects.

- **Example**:

```
const promise1 = new Promise((resolve) => setTimeout(() => resolve('Result 1'), 500));
const promise2 = new Promise((resolve) => setTimeout(() => resolve('Result 2'), 100));
const promise3 = new Promise((resolve) => setTimeout(() => resolve('Result 3'), 300));

Promise.race([promise1, promise2, promise3])
    .then(result => {
        console.log(result); // Output: "Result 2"
    })
    .catch(error => {
        console.error(error);
    });

const promise4 = new Promise((_, reject) => setTimeout(() => reject('Error 4'), 200));
const promise5 = new Promise((resolve) => setTimeout(() => resolve('Result 5'), 300));

Promise.race([promise4, promise5])
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error); // Output: "Error 4"
    });

```
