---
layout:     post
title:      "Closures in JavaScript"
subtitle:   "Understanding the Closure and how it work"
date:       2017-01-13 8:32:51
author:     "Jane"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
header-mask: 0.3
catalog:    true
tags:
    - JavaScript
    - Closures
    - Variable Scope
---

Closures are a fundamental JavaScript concept, if you want to understand what is closures and how it work, first of all, we need to understand some basic concept:

#### Variable Scope

JavaScript variables can belong to the local or global scope.

![Variable-scope](/img/in-article/2017-01-13-Closures-in-JavaScript/variable-scope.jpeg)

* **Global scope**: Any variable `(a)` declared outside of a function,and it can be accessibled from anywhere in your code. 

* **Local scope**: Each function has its own scope, and any variable `(b,c)` declared within that function and  only accessible from that function and any nested functions

Therefore, a function can refer to, or have access to any variables either in its own function scope, of outer (parent) functions or from the global scope.

A function can access all variables defined inside the function, like this:

```Javascript
function myFunction() {
    var a = 4;
    return a * a;
}
```

A function can also access variables defined outside the function,like this:

```Javascript
var a = 4;
function myFunction() {
    return a * a;
}
```
In the first example,  `a` is a local variable, It can only be used inside the function where it is defined. It is hidden from other functions and other scripting code.
While in the second example, `a`is a global variable, can be used (and changed) by all scripts in the page (and in the window). 

However, how to use a local variable out of function, like this:

```Javascript
function myFunction(){
　　　　var a=4;
　　}
alert(a); 
```
The result will return an error. If need to return a local variable, we can define a new function inside the `myFuntion` to achieve the goal, like this:

```Javascript
function myFunction(){
　　　　var a=4;
　　　　function innerFunction(){
　　　　　　alert(a); // 4
　　　　}
　　}
```
Now, `innerFunction` can use variable in `myFunction`, but not vice versa. If return `innerFunction`, the variable in `innerFunction` can be accessed by `myFunction`, like this:

```
function myFunction(){
　　　　var a=4;
　　　　function innerFunction(b){
　　　　　　alert(a+b); // 4+b
　　　　}
       return innerFunction;
　　}

var result=myFunction();
result(3);//4+=7

``` 
The above code has a closure because the anonymous function :`function innerFunction(b){ alert(a+b); }` is declared inside another function, `myFunction` in this example. In JavaScript, if you use the function keyword inside another function, you are creating a closure.

#### Closures

Closures are a fundamental JavaScript concept. It gives you access to an outer function’s scope from an inner function and expose the inner function to others

![Closure-scope](/img/in-article/2017-01-13-Closures-in-JavaScript/definition-closure.png)

To use a closure, simply define a function inside another function and expose it. To expose a function, return it or pass it to another function.The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

In most other common languages, after a function returns, all the local variables are no longer accessible because the stack-frame is destroyed. In JavaScript, if you declare a function within another function, then the local variables can remain accessible after returning from the function you called. 
This is demonstrated above, because we call the function `innerFunction(b)` after we have returned from `myFunction()`. Notice that the code that we call references the variable text, which was a local variable of the function `myFunction()`.
```javascript
function innerFunction(b){ alert(a+b); } // Output of innerFunction.alert();                          
```
Looking at the output of innerFunction.alert(), we can see that the code refers to the variable text. The anonymous function can reference text which holds the value '4+b' because the local variables of myFunction() are kept in a closure.

The magic is that in JavaScript a function reference also has a secret reference to the closure it was created in — similar to how delegates are a method pointer plus a secret reference to an object.


##### Usage
Closure can be used to 
* (1) read local parameter 
* (2) Keep the value of the parameter in the menmory, 

Private data can be emulated by using closures. For example

```javascript
var test = (function(){
        var a = 1;
        var b = 2
        var c = 3
        function read() {
        console.log("Hello")
        }
        function addFunction1(){
                b++;
                alert(a+b);
        }
        function addFunction2(){
                c++;
                alert(a+c);
                read();
        }
        return {
                b:addFunction1,             
                c:addFunction1
        }
})();
test.b();     //4
test.c()      //5
```
#### Final Point
Judging closure, it should meet the following four criterias:
* Whenever you use function inside another function, a closure is used.
* When you use new Function(…) (the Function constructor) inside a function, it does not create a closure. (The new function cannot reference the local variables of the outer function.)
* A closure in JavaScript is like keeping a copy of all the local variables, just as they were when a function exited.
* It is probably best to think that a closure is always created just an entry to a function, and the local variables are added to that closure.
* A new set of local variables is kept every time a function with a closure is called





