#Execution context

## Execution Context
Running your code requires first compling to determine what it does and if the code is valid which is translated with a set of instructions which is then implemented in a way for the computer to understand.
The only time an execution context in created, is first when the JS engine starts interpreting your code, and when a function is invoked.

## Global execution context

Global execution context first creates a creation phase which consits of a global object and a this object which are both created for us. In the code we are running in the creation phase memory allocation is set up for variables which are in the beginning set to undefined and functions are set into memory as entirely.
Once creation phase has been completed the execution phase then sets up memory space for variables and functions which is known  as hoisting. 
```
//example of hoisting
//New execution context is created.    
sayHello(); //returns "Hello" 
console.log(hi); // returns undefined as as initially the creation phase set this value to undefined and has not reached creation phase yet.
var hi = "Hi"
function sayHello() {
    console.log("Hello");
}
```

## Function execution context

The function execution context is created when a function is invoked and the execution context will run when a function is invoked by the javascript engine. Everytime a function is called/invoked it will get its own execution context. As stated earlier, the global creation phase will create:
Global object
A this object
Set up memory allocation for variables and functions
Place function declarations in memory and variable declarations to undefined.
Function invocations don't create a global object such as in the creation phase which we don't want multiple global objects every time a function is invoke.
Once a function has finished executing, it gets removed from the 'call stack'  once it has finished going through both the creation and execution phase. This is because Javascript is single threaded (From our perspective as programmers)
```
var sayHey = "Hey"
function sayHello() {
    console.log("Hello");
    var myName = "Ben"
} ```
In this example, sayHey exists in both the global execution context as well as the sayHello() execution context because it is passed in as an argument.
Variables declared inside a function live inside that function execution enviroment and not inside the global execution.

#Javascript 'this' Keyword
## Implicit Binding
```
let sayNameMixin = (obj) => {
    obj.sayName = function() {
        console.log(this.name);
    }
};
let person1 = {
    name: 'Mike',
    age: 20
};
let person2 = {
    name: 'Joe',
    age: 25
};
sayNameMixin(person1);
sayNameMixin(person2);
person1.sayName(); //Outputs Mike
person2.sayName(); //Outputs Joe
```
Implicit Binding refers to when the function is invoked, the object standing before the left of the dot is what the this keyword will be bound too. In our case person.sayName(); our person object will refer to the this keyword.

## Explicit Binding

Explicit Binding (explicitily state where the 'this' keyword refers to in a function)

```
let sayName = function() {
    console.log(`My name is ${this.name}`);
};
let mike = {
    name: 'Mike',
    age: 20
};
sayName.call(mike); //outputs 'My name is Mike

// we can also use apply and bind in explicit functions
//sayName.apply(mike, [args]);
//sayName.bind(mike, [args]); //returns a brand new function
```
In an explicit binding context we're explicitly defining what the 'this' keyword is by calling the sayName function and this is set to the Mike object.

## New binding

```
let Animal = (color, name, sound) => {
    this.color = color;
    this.name = name;
    this.sound= sound;
};

let cow = new Animal('black', 'Ana', 'Mooo');
```

This inside a constructor function is an object and when a new function is invoked with the 'new' keyword the this keyword inside the Animal constructor function is then bound to the new object being constructed.

## Default binding
```
let sayName = () => {
    console.log(this.age); //set to the window/global object
};

let person = {
    name: 'Mike'
};

sayName(); // returns undefined
window.age = 'Mike'; //this is now set to 'window'
```


If 'this'  is not bound to anything it will default to and refer to the window object 

# Prototypes

Objects are the foundation of the Javascript programming language and almost every aspect in Javascript is considered an object. Objects are key value pairs and the most common way to create an object is with curly braces {} and you can then add methods and properties to an object using the dot notation.
```
let person = {}; //creates new object
let person = new Object(); // Creates a object constructor
names.friend1 = "Henry"; //assign a property to an object.
names.bestfriend = "Jill";
```
To make our program more flexible we can encapsulate this logic inside of a function so we can then invoke it when we need to create a new person in our case. This function can be thought of as a constructor function as it is responsible for creating a new object. Constructor functions, by convention should always start with a capital letter which contain property value pairs and can be thought of as a blueprint to create other objects that also will use proprties in the constructor function.
```
function Person (name, age) { //function constructor blueprint
    this.name = name; //Key value pairs
    this.age = age;
    this.stats = function() {
        return 
    }
}
```

Prototypes are a property that every function has which allow us to share methods across instances of a function. But instead of managing seperate objects for methods, prototypes allow us for a way to create multiple object to be linked together and have access to each other's properties and methods.
```
let person = new Object();
Object.getPrototypeOf(person);
person.__proto__ //Legacy feature - Should not be used in production.
```
Above shows 2 different ways to get the prototype of an object and will output methods and properties available to the object. 

# ES6 Classes

ES6 classes was introduced as an easier way to write object oreintated code in javascript. It is also important to note, although the class syntax may look a lot like other programming syntax's, it is not the same. Behind the scenes it is still prototype based.

### ES5 prototype constructor
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.greet = function() {
    return `My name is ${this.name}. I'm ${this.age} years old`;
}
const instance = new Person("sam", 24);
console.log(instance) //My name is Sam. Im 24 years old!
```

### ES6 class prototype constructor

```
class Person {
    constructor(name, age) {
    this.name = name;
    this.age = age;
    }
    greet() {
         return `My name is ${this.name}. I'm ${this.age} years old`;
     }
}
const instance = new Person("Sam", 24);
console.log(instance) // My name is Sam I'm 24 years old!
```

It is important to note that everything related to the Person object, will sit inside this block. rather than having Person.prototype... as a method outside.
Using classes makes code look much simplier and less complicated. You don't have to create new prototypes of the Person object to set a new function to it and also elimates the use of using 'call' when we wish to inherit properties from another prototype.

## ES6 Inheritance 

### Inheritance in ES5
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}

//Constructor function
function Programmer(name, age, language) {
    Person.call(this, name, age);
    this.language = language;
}
Programmer.prototype = Object.create(Person.prototype);
Person.prototype.constructor = Programmer;
Programmer.prototype.greet = function() {
    return `${Person.prototype.greet.call(this)} I code in ${this.langauge}
}
```

Inheriting class objects can get messy, first we create a Programmer constructor which inherits and create properties from the Person prototype and call methods we wish to use from the Person prototype 

### Inheritances in ES6
```
class Person {
    constructor(name, age) {
    this.name = name;
    this.age = age;
    }
    greet() {
         return `My name is ${this.name}. I'm ${this.age} years old`;
     }
}
```
//extends inherits from the Person object
```
class Programmer extends Person {
    constructor(name, age, language)
    //super refers to the parent class which is Person
    super(name, age);
    this.language = language;
}
const Mike = new Programmer("Mike", 25, "C++");
greet() {
    //Super.greet() is reusing greet from its parent class
    return `${super.greet()} I code in ${this.language});
}
console.log(Mike.greet()) //Output: My name is Mike, Im 25 years old, I code in C++
```

In ES6, instead of creating a seperate constructor function as shown above, we inherit the properties from the Person object using 'extends', create our constructor with the properties we wish to use and call 'super' which refers to the parent class with it's class properties.


