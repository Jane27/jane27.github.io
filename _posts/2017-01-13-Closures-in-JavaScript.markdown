---
layout:     post
title:      "Closures in JavaScript"
subtitle:   "understanding the Closure and how it work"
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
　　　　function innerFunction(){
　　　　　　alert(a); // 4
　　　　}
       return innerFunction;
　　}

var result=myFunction();
result();//4

``` 
This is `closure`

#### Closures

Closures are a fundamental JavaScript concept. It gives you access to an outer function’s scope from an inner function and expose the inner function to others

![Closure-scope](/img/in-article/2017-01-13-Closures-in-JavaScript/definition-closure.png)

To use a closure, simply define a function inside another function and expose it. To expose a function, return it or pass it to another function.The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

To summary,

* Closures contain a function `(outer)` and a reference to the environment in which the function `(inner)` was created.
* A closure is formed when an outer function exposes an inner function.
* Closures can be used to easily pass parameters to callback functions.
* Private data can be emulated by using closures.  This is common in object-oriented programming and namespace design.





