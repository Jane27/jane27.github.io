---
layout:     post
title:      "Callback function, Single-Threaded Model in JavaScript"
subtitle:   "Understanding the concept of Callback function, Single-Threaded Model in Javascript"
date:       2017-02-01 8:32:51
author:     "Jane"
header-img: "img/post-bg-e2e-ux.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Callback function
    - Single-Threaded Model
    - Event Loop
---

### Callback function
##### What is Callback Function
The above-linked Wikipedia article defines it nicely:
> Callback Function is a reference to executable code, or a piece of executable code, that is passed as an argument to other code.

A callback is a function that is passed as an argument to another function and is executed after its parent function has completed.
A function that accepts other functions as arguments is called a higher-order function, which contains the logic for when the callback function gets executed.

To illustrate callbacks, let's start with a simple example:
```javascript
function createQuote(quote, callback){ 
  var myQuote = "I like to eat " + quote + "!";
  callback(myQuote); 
}

function logQuote(quote){
  console.log(quote);
}

createQuote("vegetables", logQuote); // Result in console: I like to eat vegetables!
```
In the above example, createQuote is the higher-order function, which accepts two arguments, the second one being the callback. The logQuote function is being used to pass in as our callback function. When we execute the `createQuote` function, notice that we are not appending parentheses to logQuote when we pass it in as an argument. This is because we do not want to execute our callback function right away, we simply want to pass the function definition along to the higher-order function so that it can be executed later.

Also, we need to ensure that if the callback function we pass in expects arguments, that we supply those arguments when executing the callback. In the above example, that would be the callback(myQuote); statement, since we know that logQuote expects a quote to be passed in.

Additionally, we can pass in anonymous functions as callbacks. The below call to createQuote would have the same result as the above example:
```javascript
createQuote("vegetables", function(quote){ 
  console.log(quote); 
});
```
Incidentally, you don't have to use the word "callback" as the name of your argument, Javascript just needs to know that it's the correct argument name. Based on the above example, the below function will behave in exactly the same manner.
```javascript
function createQuote(quote, functionToCall){ 
  var myQuote = "Like I always say, " + quote;
  functionToCall(myQuote);
}
```
##### why use Callback Function

Most of the time we are creating programs and applications that operate in a synchronous manner. In other words, some of our operations are started only after the preceding ones have completed. Often when we request data from other sources, such as an external API, we don't always know when our data will be served back. In these instances we want to wait for the response, but we don't always want our entire application grinding to a halt while our data is being fetched. These situations are where callback functions come in handy.

Let's take a look at an example that simulates a request to a server:
```javascript
function serverRequest(query, callback){
  setTimeout(function(){
    var response = query + "full!";
    callback(response);
  },5000);
}

function getResults(results){
  console.log("Response from the server: " + results);
}

serverRequest("The glass is half ", getResults); // Result in console after 5 second delay: Response from the server: The glass is half full!
```
In the above example, we make a mock request to a server. After 5 seconds elapse the response is modified and then our callback function getResults gets executed. To see this in action, you can copy/paste the above code into your browser's developer tool and execute it.

Also, if you are already familiar with setTimeout, then you've been using callback functions all along. The anonymous function argument passed into the above example's setTimeout function call is also a callback! So the example's original callback is actually executed by another callback. Be careful not to nest too many callbacks if you can help it, as this can lead to something called "callback hell"! As the name implies, it isn't a joy to deal with.



#### Single-Threaded Model

Javascript is single threaded. But, they're also event driven, which creates a slightly different scenario than what you'd traditionally call single or multi-threaded applications. 

In JavaScript, almost all I/O is non-blocking. This includes HTTP requests, database operations and disk reads and writes; the single thread of execution asks the runtime to perform an operation, providing a callback function and then moves on to do something else. When the operation has been completed, a message is enqueued along with the provided callback function. At some point in the future, the message is dequeued and the callback fired.

##### Event Loop

The decoupling of the caller from the response allows for the JavaScript runtime to do other things while waiting for your asynchronous operation to complete and their callbacks to fire. 

JavaScript runtimes contain a message queue which stores a list of messages to be processed and their associated callback functions. These messages are queued in response to external events (such as a mouse being clicked or receiving the response to an HTTP request) given a callback function has been provided. If, for example a user were to click a button and no callback function was provided – no message would have been enqueued.

In a loop, the queue is polled for the next message (each poll referred to as a “tick”) and when a message is encountered, the callback for that message is executed.

![event-loop](/img/in-article/2017-02-01-Callback-function-Single-Threaded-Model-in-JavaScript/event-loop.png)

The calling of this callback function serves as the initial frame in the call stack, and due to JavaScript being single-threaded, further message polling and processing is halted pending the return of all calls on the stack.

![ProcessFlow](/img/in-article/2017-02-01-Callback-function-Single-Threaded-Model-in-JavaScript/ProcessFlow.png)

As described in the picture, the main parts of JavaScript Event loop are:

* The Call stack, which is a usual call stack that contains all called functions.
* The Event handlers queue, a queue of event handlers that are waiting to be executed.
* The Event loop, which is a process that checks whether events handlers queue is not empty and if it is – calls * top event handler and removes it from queue.
* The JavaScript Web APIs: those APIs that are provided by the browser that are responsible for filling the Event handlers queue, providing many features such as the ability to make an AJAX request and e.g. the implementation of Mozilla has a great documentation about Web APIs and it could be found by the link.






