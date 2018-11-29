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



