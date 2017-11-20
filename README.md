# Prototypes in JS

## Initial steps

Let's review the basics of objects in JavaScript.


![First Example](img/1.jpg)


This way of making an object is simplest. You can just read the code for an object like this and immediately understand it. Its defining curly brackets contain properties, which follow the format `key: value`. Objects help us label and group together data in a way that other data structures can't. Arrays, for instance, would have a hard time accounting for the color, model, VIN number, and tire size of a car. They were designed for sequence-oriented data, not description-oriented data.

Now we encounter a conundrum. Let's say that we want to make an object with the same fields but different values. How could we make `student_2`? Let's say `student_2` is for Mary Jane Bukowski, to pick a name at random. How would we do this? One way would be to copy and paste the properties from `student_1` into a new `student_2`, replacing George's properties with Mary's new ones. This method is inefficient, non-programmatic, and likely to create errors. Luckily, JS has a good solution.

![Second Example](img/2.jpg)

Let's think for a second. What is it that `student_1` and `student_2` would have in common? Their keys! So we need something that can store the names of keys that we want to apply to different objects with different values. This is exactly the purpose that a constructor in JavaScript serves. A constructor is just a function like any other. There is no magic going on in the constructor function itself. The magic takes place outside it. Take a look at line 15 in the second image above. We're passing in a bunch of values into something called `new Student`. What this evaluates to is an empty object (at least initially). The `this` keyword (which is just a placeholder for some kind of containing object) in each assignment line of the constructor function gets changed to this new object, and properties are then assigned to it like you would manually assign them.

Constructors emulate the basic behavior of classes and instances in other languages. But be careful! A constructor is not literally a class. It's just a function that *constructs* a new object according to the blueprint described in its code block.

## Prototypes

Constructors actually write properties onto the objects they create. `student_1` in the second image above truly has a property of its own called `first_name`, as we can see in the results on line 16. And really the only thing that you need to understand about prototype inheritance is that it works differently. So how does it work?

Prototype inheritance, in contrast to a property writing system, is an object referencing system. (This referencing is formally known as delegation.) In order to understand this better, let's look at a crazy object.

![Third Example](img/3.jpg)

In `crazyObj` we don't have a bunch of keys that we'd like to see in other objects with different values. We have a pile of data. Imagine that each return statement in each crazy function was replaced with 200 lines of unbearably complex calculations and assignments. Imagine that `crazyArray1` is actually filled with millions of elements. Now imagine a constructor whose job it is to just assign these ridiculous values to the crazy functions and the crazy array of a new object. Imagine how long it might take to go through this process for 100 separate objects, and how much memory it would require! This is the kind of thing that makes browsers crash and computers lag, so a browser-oriented language like JS has to have a good way of dealing with this issue.

Put simply, the solution is just to reference `crazyObj`. Instead of copying over *all* that data, it is just referenced from within an object that is associated with `crazyObj`. Let's look at a separate example of prototypal inheritance in action to understand this better.

![Fourth Example](img/4.jpg)

`a` is defined as an empty object. It has no properties of its own. `b` and `c` have properties. So first we're specifying that `b` is the prototype of `a`. We do this by `Object.setPrototypeOf(a, b);`. This is the functional equivalent of saying that `a.__proto__ = b;`. So here's what will happen if we provide `a.name`. First, it will look for the `name` value in `a` itself. Since it's empty it's not there. Then, it will find out which object the `__proto__` of `a` refers to. Then it will look for the property (in this case, `name`) in the prototype. And, voila! Through this mechanism `a.name` is a valid expression, even though `a` doesn't actually have any properties. `__proto__` is the property that can be set to provide a reference to another object. The properties of the prototype can be referenced (using dot or bracket notation) in the same way that we reference the actual properties of an object.

In this example we have a prototype chain. `c` is the prototype of `b`, and `b` is the prototype of `a`. Therefore, `a` can access all the properties of `b` and `c`. And therefore, `a.color` is a perfectly valid expression.

## Bringing it all together

Now I'm going to blow your mind. Wait for it... A constructor is something that facilitates both the property writing scheme and the object referencing scheme. It's a two-in-one. But how is this possible? It's possible because a constructor is just a function. Every function in JS has a property called `prototype`. You can think of this like a built-in place for all the stuff we don't want to pass into each new object, but would rather reference for efficiency's sake. There's no conceptual difference between `<constructor>.prototype` and `b` in the last example. They're both objects that we're using to contain referenced data. Any object made by a constructor will automatically have a `__proto__` property. This property points to `<constructor>.prototype`.

So now, hopefully, you can fully understand code that looks like

    function testConstructor() {
        this.bananas = false;    // instance stamping part
        this.strawberries = true; }
    testConstructor.prototype.meow = function () {    // object referencing part
        return "Meow!"; };
    const newObj = new testConstructor();

For whatever reason we might have, we decided to put the `meow` function in the prototype of the constructor rather than stamping it as a value onto a new object. Methods are customarily added to the constructor's prototype, but it won't break the system to add `this.meow = function...` to the constructor's code block. In production, some methods could be hundreds of lines long, so this custom makes some sense.

## Making sense of `Array.prototype.slice.call(arguments);`

## References

* [Beej's article on JS prototypes](http://beej.us/blog/data/javascript-prototypes-inheritance/)
* [Beej's blog post on Array.prototype.slice.call(arguments)](https://github.com/LambdaSchool/BeejWiki/wiki/Arrays,-prototypes,-slices,-calls)
* [YouTube: Prototypes in Javascript - p5.js Tutorial](https://www.youtube.com/watch?v=hS_WqkyUah8)
* [YouTube: proto vs prototype - Object Creation in JavaScript P5](https://www.youtube.com/watch?v=DqGwxR_0d1M)
* [Constructors in JS](http://adripofjavascript.com/blog/drips/constructors-in-javascript.html)
* [Wikipedia: Prototype-based programming](https://en.wikipedia.org/wiki/Prototype-based_programming)
