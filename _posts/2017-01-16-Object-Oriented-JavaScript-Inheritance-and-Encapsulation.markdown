---
layout:     post
title:      "OOP in JavaScript"
subtitle:   "Object Oriented JavaScript: Inheritance and Encapsulation"
date:       2017-01-16 11:38:05
author:     "Jane"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
header-mask: 0.3
catalog:    true
tags:
    - JavaScript
    - OOP
    - Inheritance
    - Encapsulation
---

Object Oriented Programming (OOP)refers to using self-contained pieces of code to develop applications. We call these self-contained pieces of code objects, better known as Classes in most OOP programming languages It consist of three main principle: `Inheritance`,`Polymorphism`, and `Encapsulation` 



Objects can be thought of as the main actors in an application, or simply the main “things” or building blocks that do all the work. As we know by now, objects are everywhere in JavaScript since every component in JavaScript is an Object, including Functions, Strings, and Numbers. `Class` concept in In JavaScript (except JavaScript ES5 and ES6)，JavaScript adopt `Function` to simulate class. 

In this article, we are concerned with only `Inheritance` and `Encapsulation`, since only these two concepts apply to OOP in JavaScript, particularly because, in JavaScript, objects can encapsulate functionalities and inherit methods and properties from other objects. 


Based on the object, the principle should be explained like this: 
* Inheritance : objects can inherit features from other objects.
* Polymorphism : objects can share the same interface—how they are accessed and used—while their underlying implementation of the interface may differ.
* Encapsulation :each object is responsible for specific tasks.



#### Encapsulation

Encapsulation refers to enclosing all the functionalities of an object within that object so that the object’s internal workings (its methods and properties) are hidden from the rest of the application. This allows us to abstract or localize specific set of functionalities on objects. 



##### (1) Constructor
whenever you want to create objects with similar functionalities (to use the same methods and properties), you encapsulate the main functionalities in a Function and you use that Function’s constructor to create the objects.
In JavaScript you first create an object (Function is also an object), then you can augment your own object or create new objects from it. For example: there are two Cat Object: `Cat1` and `Cat2`, both of them have two property: `name` and `color` ; Here, we can define like this:

Defining a Cat prototype object which has two properity－－name and color：

```javascript
function Cat(name,color){
       this.name=name;
       this.color=color;
    }
```
Now, there are two Instance object：
```javascript
var cat1 = new Cat("Jenny","White");
var cat2 = new Cat("Mike","Yellow");

alert(cat1.name); // Jenny
alert(cat1.color); // White
```
Every function has a constructor property, and this property points to the constructor of the function. Here, we can use `constructor` property and `instanceof` to verify the relationship between prototype and instance object:

```javascript
alert(cat1.constructor == Cat); //true
alert(cat1 instanceof Cat); //true
```
##### (2) Prototype

Every object in JavaScript (where everything is an object, even functions) has an internal prototype property, which points in fact to a prototype object. prototype object is one object, on matter how many objects will point it, it only be assgined one piece of monery. Therefore, we can define some fixed property and method in prototype object to save the monery.

For example: we add a `type` property and `eat` method for the cat, like this 
```javascript
function Cat(name,color){
       this.name = name;
       this.color = color;
    }
    Cat.prototype.species= "Animal"
    Cat.prototype.eat = function(){alert("cat like eating mouse")
};
```
Now, every instance own `eat()` method:
```javascript
cat1.eat(); // cat like eating mouse
cat2.eat(); // cat like eating mouse
alert(cat1.eat == cat2.eat); //true
```
First of all, looking up in the object, if a method or property is missing on an object, it is searched on its prototype,  If the prototype-object also doesn't have such a property or method, its prototype is checked in turn, thus walking the original object's prototype-chain until a match is found or its end is reached.

![prototype](/img/in-article/2017-01-16-Object-Oriented-JavaScript-Inheritance-and-Encapsulation/prototype.gif)


Here, three kinds of method can be used to verify the prototype :

* **isPrototypeOf()** vertify the relationship between prototype and instance

```javascript
alert(Cat.prototype.isPrototypeOf(cat1)); //true　
alert(Cat.prototype.isPrototypeOf(cat2)); //true
```
* **hasOwnProperty()** vertify the property is a self-property or inherit from prototype object

```javascript
alert(cat1.hasOwnProperty("name")); // true　
alert(cat1.hasOwnProperty("species")); // false
```
* **in** vertify whether the instance have this property.

```javascript
alert("name" in cat1); // true
alert("type" in cat1); // true
```
`in` also can be used to traversal all the property.
```javascript
for(var prop in cat1) { 
         alert("cat1["+prop+"]="+cat1[prop]); }
```

#### Inheritance

Inheritance refers to an object being able to inherit methods and properties from a parent object (a Class in other OOP languages, or a Function in JavaScript). 

##### Inheritance between Constructor
How to achieve the inheritance between two objects, for example:
```javascript
function Animal(){
       this.species = "Animal";
    }
```
and 
```javascript
function Cat(name,color){
       this.name = name;
       this.color = color;
   }
```
In the following section, I will explain how to make `Cat` inherit `Animal`
###### (1) Using `call` or `apply`

Binding parent's contructor to child's object, like this:

```javascript
function Cat(name,color){
       Animal.apply(this, arguments);
       this.name = name;
       this.color = color;
    }
var cat1 = new Cat("Jenny","White");
alert(cat1.species); // animal
```
###### (2) Using `prototype` property
If Cat‘s prototype points to an Animal's object', all the property of Animal will be inherited by Cat, Like this:
```javascript
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;//alert(Cat.prototype.constructor == Animal); ----true
var cat1 = new Cat("Jenny","White"); //alert(cat1.constructor == Animal); ---- true
alert(cat1.species); // animal
```
Here is one thing that we need to pay attention is: 
If replace the default prototype object, like this:
```javascript
step1: o.prototype = {};
```
next step, we need to add `constructor` proporty for new prototype object, and assign this property to the previous constructor, like this 
```javascript
step2: o.prototype.constructor = o;
```

###### (3) Inherit `prototype` property
Make Cat‘s prototype points to an Animal's prototyle', all the property of Animal's prototype will be inherited by Cat, Like this:
```javascript
function Animal(){ }
Animal.prototype.species = "animal";
```
And 
```javascript
Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;
var cat1 = new Cat("Jenny","White"); 
alert(cat1.species); // animal
```
Compared with (2), we don't need to create `Animal` object, therefore, Saving the memory. However, the drawback is if we mpdify the Cat.prototype, the Animal.prototype will be changed. 

If we use an `empty obejct` as an agent, the drawbck will be overcome. For example 
```javascript
var F = function(){};
F.prototype = Animal.prototype;
Cat.prototype = new F();
Cat.prototype.constructor = Cat;
```
Above method can be encapuslate in a function, like this:

```javascript
function extend(Child, Parent) {
       var F = function(){};
       F.prototype = Parent.prototype;
       Child.prototype = new F();
       Child.prototype.constructor = Child;
       Child.uber = Parent.prototype;
    }
 ```
 When using it 

 ```javascript
extend(Cat,Animal);
var cat1 = new Cat("Jenny","White"); 
alert(cat1.species); // animal
```

###### (4) Using Copy
we also can copy all the propory and method from parent's object to child'object, Like this:
* **Define an animal prototype property:**

```javascript
function Animal(){ }
Animal.prototype.species = "Animal";
```
* **Define a function to implement copy**

```javascript
function extend2(Child, Parent) {
       var p = Parent.prototype;
       var c = Child.prototype;
       for (var i in p) {
          c[i] = p[i];
          }
       c.uber = p;
    }
```

 When using it 

 ```javascript
extend(Cat,Animal);
var cat1 = new Cat("Jenny","White"); 
alert(cat1.species); // animal
```