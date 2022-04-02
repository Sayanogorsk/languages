
## Callbacks
A “callback-based” style of asynchronous programming:
* Many functions are provided by JavaScript host environments that allow you to schedule asynchronous actions. 
* the second argument is a function (usually anonymous) that runs when the action is completed.

```
    function loadScript(src, callback) {
      let script = document.createElement('script');
      script.onload = () => callback(null, script);
      script.onerror = () => callback(new Error(`Script load error for ${src}`));
      document.head.append(script);
    }

    loadScript('/my/script.js', function(error, script) {
      if (error) {
        // handle error
      } else {
        // script loaded successfully
      }
    });
```

* Callback in callback
*  the “error-first callback” style: The first argument of the callback is reserved for an error if it occurs. Then callback(err) is called.
* Both contribute to "callback hell" or "pyramid of doom"

## Promise
A promise is a special JavaScript object that links the “producing code” and the “consuming code” together. 
* Promises allow us to do things in the natural order. 
* We can call .then on a Promise as many times as we want.


### Producer
The constructor syntax for a promise object is:
```
    let promise = new Promise(function(resolve, reject) {
      // executor (the producing code, "singer")
    });
```

* The function passed to new Promise is called the executor. When new Promise is created, the executor runs automatically and attempts to perform a job. 
* When it is finished with the attempt, it calls resolve if it was successful or reject if there was an error. resolve and reject are callbacks provided by JavaScript itself.


### Consumer: then, catch, finally
Consuming functions can be registered (subscribed) using methods .then, .catch and .finally.
* Then: 
The first argument of .then is a function that runs when the promise is resolved, and receives the result. The second argument of .then is a function that runs when the promise is rejected, and receives the error.
```
    promise.then(
      function(result) { /* handle a successful result */ },
      function(error) { /* handle an error */ }
    );
```
* Catch: 
If we’re interested only in errors, then we can use null as the first argument: .then(null, errorHandlingFunction). Or we can use .catch(errorHandlingFunction), which is exactly the same:
* Finally:
The call .finally(f) is similar to .then(f, f) in the sense that f always runs when the promise is settled: be it resolve or reject.

### Promises Chaining
* As the result is passed along the chain of handlers
* A handler, used in .then(handler) may create and return a promise. In that case further handlers wait until it settles, and then get its result.

### Error Handling with Promises

## Async/Await
There’s a special syntax to work with promises in a more comfortable fashion, called “async/await”. 
* The word “async” before a function means one simple thing: a function always returns a promise. Other values are wrapped in a resolved promise automatically.
* The keyword await makes JavaScript wait until that promise settles and returns its result. It’s just a more elegant syntax of getting the promise result than promise.then. 
* If it’s an error, an exception is generated — same as if throw error were called at that very place.
