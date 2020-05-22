# ■■■ Day 16
# JS ninja >>>>>
# Objects and collections <<<
# OOP with prototypes
A `prototype` is an object to which the search for a particular property
can be delegated to.

## Prototypes
When developing software, we strive not to reinvent the wheel, so we want to reuse as
much code as possible.

To test whether an object has access to a particular property,
we can use the `in` operator. In JavaScript, the object’s prototype
property is an internal property that’s not directly accessible

Instead, the built-in method `Object.setPrototypeOf` takes in two object arguments
and sets the second object as the prototype of the first.

It’s important to emphasize that every object can have a prototype, and an object’s
prototype can also have a prototype, and so on, forming a `prototype chain`.

## Object construction and prototypes
We’d like to be able to consolidate the set of properties and
methods for a class of objects in one place.

`new` operator, applied to a `constructor function`

every function has a `prototype object` that’s automatically set as
the prototype of the objects created with that function.
```js
function Ninja(){}
Ninja.prototype.swingSword = _ => true;
    
const n = new Ninja();
n.swingSword(); // true
```

Every function has a built-in `prototype` object, which we can `freely modify`
Ninja prototype initially has only `constructor` property that references back to function

Ninja.prototype -> Ninja.prototype
Ninja.prototype.Constructor -> Ninja

`all` objects created with the Ninja constructor will have access to the swingSword
method. Now that’s code reuse!

1. Instance properties
    In addition to exposing properties via the prototype, we can
    initialize values within the constructor function via the `this` parameter.
    ```js
    function Ninja(){
        this.swung = false;
        this.swingSword = function(){//each instance has it's own funciton(not efficient)
            return !this.swung;
        };
    }
    ```
    You should only define methods on the prototype (if you were to ever do that)

2. Object typing via constructors
    By using the `constructor` property, we can access the function that was used to
    create the object. This information can be used as a form of `type` checking
    It can be done with `instanceOf` operator which gives us a way to determine whether an
    instance was created by a particular function constructor
    
    We can also use `constructor` property of an instance object:
    instance.constructor -> prototype.constructor -> functionConstructor
    
## Achieving inheritance
Inheritance is a form of reuse in which new objects have access
to properties of existing objects. 

The best technique for creating such a prototype chain is to use an
instance of an object as the other object’s prototype
```js
SubClass.prototype = new SuperClass();  // (new SuperClass()).constructor
                                        // will point to SuperClass
new SubClass instanceOf SuperClass;     // true
```
This preserves the prototype chain

```js
function Ninja(){}
Ninja.prototype = new Person();

var ninja = new Ninja();
```
When we try to access the dance method through the ninja object, the JavaScript runtime
will first check the ninja object itself. Because it doesn’t have the dance property,
its prototype, the person object, is searched.

```js
Ninja.prototype = Person.prototype; // don't do that
```
Any subsequent change to ninja prototype will also change the person prototype
which may be bad.

1. The problem of overriding the constructor property
    Because you override the prototype property, you `lose constructor` property
    
    Just add this property back again. You should also take care about enumerability
    of that property (it should not be visible in for loop), and define it using 
    `Object.defineProperty(o, {})`
    
2. The instanceof operator
    The instanceof operator works by checking whether the current prototype of the
    Ninja function is in the `prototype chain` of the ninja instance.

## Using ES6 classes
1. Class keyword
    In the constructor’s body, we can access the newly created `instance` with the `this`
    keyword, and we can easily add new properties, such as the name property.
    Within the class `body`, we can also define `methods` that
    will be accessible to all instances.
    ```js
    class Human {
        constructor(name) {   // own properties
            this.name = name;
        }
        
        sayHi(){                // instance methods, can access this
            console.log(`hi, i am ${this.name}`);
        }
        
        static getHeight(human) {   // static methods accessed throught Human.getHeight()
            return human.height;
        }
    }
    ```
    
    Static methods translate to assigning methods to constructor functions themselves
    `Human.getHeight = function getHeight() {...}`
    
2. Implementing inheritance
    ```js
    class Ninja extends Person {    // extends
        constructor(name, weapon){
            super(name);            // super(...args) calls new Person.bind(this)(...args)
            this.weapon = weapon;
        }
        wieldWeapon(){
            return true;
        }
    }
    ```

# Controlling access to objects
you’ll learn techniques for
controlling access to and monitoring all of the changes that occur in your objects.
    
## Getters and setters
1. Defining getters and setters
    ```js
    const ninjaCollection = {
        ninjas: ["Yoshi", "Kuma", "Hattori"],
        get firstNinja(){ // getter
            report("Getting firstNinja");
            return this.ninjas[0];
        },
        set firstNinja(value){ // setter takes value
            report("Setting firstNinja");
            this.ninjas[0] = value;
        }
    };
    ```
    
    native getters and setters allow us to
    specify properties that are accessed as standard properties, but that are methods
    whose execution is triggered immediately when the property is accessed

    es6 syntax:
    ```js
    class NinjaCollection {
        constructor(){
            this.ninjas = ["Yoshi", "Kuma", "Hattori"];
        }
        get firstNinja(){
            report("Getting firstNinja");
            return this.ninjas[0];
        }
        set firstNinja(value){
            report("Setting firstNinja");
            this.ninjas[0] = value;
        }
    }
    ```
    
    If the code is in nonstrict mode, assigning a value to a property with `only a getter`
    achieves `nothing`; the JavaScript engine will silently ignore our request.
    If, on the other hand, the code is in `strict` mode, the JavaScript engine will
    `throw` a type error

    You can also degine setters and getters with `Object.defineProperty`
    it may be useful when defining "private" properties.

## Using proxies to control access
A proxy is a `surrogate` through which we control access to another object.
It enables us to define custom actions that will be executed when an object
is being interacted with.

You can think of proxies as almost a `generalization of getters and setters`;
but with each getter and setter, you control access to only a single object property,
whereas proxies enable you to generically handle `all interactions` with an object,
including even method calls.

But proxies are powerful.
They allow us to easily add profiling and performance measurements to our code,
autopopulate object properties in order to avoid pesky null exceptions, and to wrap
host objects such as the DOM in order to reduce cross-browser incompatibilities.

In JavaScript, we can create proxies by using the built-in `Proxy` constructor

proxy that intercepts all attempts to read and write to properties of an object:
```js
const empreror = { name: "Hitachi" }
const representative = new Proxy(empreror, {
    get(target, key) {
        log('Reading' + key + "Through proxy")
            return key in target 
                ? target[key]
                : "Don't bother the emperor!";
    },
    set(target, key, value) {
        report("Writing " + key + " through a proxy");
        target[key] = value;
    }
})
```

This is the `gist` of how to use proxies: Through the Proxy constructor, we create a
proxy object that controls access to the target object by activating certain
traps(get/set/etc...), whenever an operation is performed directly on a proxy.

We can intercept many operations, not only get and set.

Another example:
```js
// logging
function makeLoggable(target){
    return new Proxy(target, {
        get: (target, property) => {
            report("Reading " + property);
            return target[property];
        },
        set: (target, property, value) => {
            report("Writing value " + value + " to " + property);
            target[property] = value;
        }
    });
}

// measure performance
let f = new Proxy(functionToMeasure, { // note, you can pass fs as well as objs
    apply: (target, thisArg, args) => { // TRAP called on apply
        console.time("time1");
        const result = target.apply(thisArg, args);
        console.timeEnd("time2");
        return result;
    }
});

// autopopulate properties
function Folder() {
    return new Proxy({}, {
        get: (target, property) => {
            report("Reading " + property);
            if(!(property in target)) 
                target[property] = new Folder();
            
            return target[property]; } }); }
// now you can access long.long.long.fileStructure = somethign.
// even if you didn't have some properties in the middle of the pipe before, they 
// will be created

// implement negative array indeces
return new Proxy(array, {
    get: (target, index) => {
        index = +index;
        return target[index < 0 ? target.length + index : index];
    },
    set: (target, index, val) => {
        index = +index;
        return target[index < 0 ? target.length + index : index] = val; // notice assign
    }
});
```

- Performance costs
    A proxy can define traps, functions that will be executed whenever a
    certain operation is performed on a proxy, it introduces a significant amount
    of additional processing that impacts performance.
    proxies are around `50 times slower`

# Dealing with collections
## Arrays
1. Searching 
    `find` method
    ```js
    const elem = arr.find(v => v == smth);
    ```

## Maps
Maps map a key to a specific value.

1. Don't use objects as maps
    all objects have prototypes; even if we define new, empty objects as our maps,
    they still have access to the properties of the prototype objects.
    You don't want to access them accidentally.

    In addition, with objects, keys can only be string values.
    if you want to create a mapping for any other value, that value will be silently
    converted into a string without anyone asking you anything!
    ```js
    const firstElement = document.getElementById("firstElement");
    const secondElement = document.getElementById("secondElement");
    const map = {};
    
    map[firstElement] = "smth";
    map[secondElement] = "smth else"; // this will override map[firstElement]
                                    // because both fE and sE have the same string
                                    // representation - "[object HTMLDivElement]"
    ```
    
2. Creating a map
    Use `set` method to add new mapping.
    `delete` to delete mapping
    
    `get` to get elements
    `has` to check a mapping for existence
    
    `size` property to get the amount of mappings
    ```js
    const ninjaIslandMap = new Map();
    ninjaIslandMap.set(ninja1, { homeIsland: "Honshu"});
    ```
    
    if you use object as a key, new map is created for every object instance.
    even if two objects have the same properties, they will map to different 
    values.

## Sets
In many real-world problems, we have to deal with collections of `distinct` items

1. Your first set
    ```js
    const ninjas = new Set(Enumerable);
    ```
    Discrete mathematic's funcitons on sets you have to implement yourself.

# ■■■ Day 17
# Regular expressions <<<
## What's good about them?
Then allow to solve pattern matching problems elegantly.

## Regex refresher
Literal regex is used for compile time regex.
Constructor regex is used to create regex from a sting at runtime.

1. Five flags: 
    1. i - case insensitive
    2. g - global
    3. m - multiline (e.g in textarea)
    4. y - stiky matching (from the `last` position)
    5. u - enable unicode point excapes (\uXXXX)

2. Terms and operators
    1. [abs] - one of the set
    2. [^abs] - one BUT from the set
    3. [a-m] - one from range
    4. \X - special character literal (\^)
    5. /^/, /$/ - begin and end
        - `repetition` operators
    6. /t?est/ - (X?) - optional character
    7. X+ - one or many
    8. X* - 0 or many
    9. X{4} - exact number of times
    10. X{1,3} - range of times 
    11. X{1,} - open-ended range of times
        - Repetition operators can be `greedy` (by default) and non-greedy.
            greedy consume all characters they can find, non-greedy - only minimum
            amount. To make operator non-greedy add a `question-mark` to it.
    12. X+? - one or many non-greedy
        - `special character`
    13. . - any character but whitespace
    14. \d, \D - any digit, not digit
    15. \w, \W - any alphanumeric, not alphanumeric
    16. \s, \S - any whitespace, not whitespace
    17. \b, \B - word boundary, inside a word
        - `grouping and capturing`
    18. (ab)+ - applies to sequence of a -> b instead of to a single character
        - `alternation`
    19. a+|b+ - sequence of a's or a sequence of b's
        - `backreferences`
    The notation for such a term is the backslash followed by the number of
    the capture to be referenced, beginning with 1, such as `\1, \2,` and so on

    e.g : 
        - `/^([dtn])a\1/` -> `\1` is not known until evaluation time.
        - `/<(\w+)>(.+)<\/\1>/` -> catches xml tags with non-empty body.

## Capturing matching segments
Determining whether a string matches a pattern is an obvious first step
and often all that we need, but determining `what` was matched is also
useful in many situations.
    
1. Simple captures
    string example -> transform:translateY(15px)
    task -> capture translateY value
    solved -> `/translateY\(([^\)]+)\)/`
        - translateY\( -> matches translateY(
        - ([^\)]+) -> matches everything that is `not` ) and occures more than once.
        - \) -> matches )

2. Using global expressions
    If you use global flag with a `match` function, the funciton will return 
    all matches in the whole string, but won't return individual matches separately
    (in case you have multiple capturing groups)

    If you don't use the global flag, the match funciton will return all your 
    capturing groups but only from the first match.
    
    To get access to `all captuiring groups from the whole string` you need another 
    method : `exec`, which you can repeatedly call on the same string.
    each subsequent call progresses to the next global match.
    Each call returns the next match `and` its captures.
    ```js
    while ((match = tag.exec(html)) !== null) {...}
    ```

3. Referencing captures
    We can refer to portions of a match that we’ve captured in two ways: one within the
    match itself, and one within a replacement string (where applicable).
    `/<(\w+)([^>]*)>(.*?)<\/\1>/g;`
    Matches tags.
    
    `assert("fontFamily".replace(/([A-Z])/g, "-$1"`
    backreference within a replacement string
    (transforms pascalCase to hyphen-case)

4. Non-capturing groups
    To indicate that a set of parentheses shouldn’t result in a capture, the regular
    expression syntax lets us put the notation `?:` immediately after the opening bracket.
    ```js
    const pattern = /((?:ninja-)+)sword/;
    ```
    causes only the outer set of parentheses to create a capture

## Replacing using functions
The `replace` method of the String object is powerful and versatile.

let’s say that we want to replace all uppercase characters in a string with X
```js
"ABCDEfg".replace(/[A-Z]/g,"X")
```
But perhaps the most powerful feature presented by replace is the ability to provide
a `function` as the replacement value rather than a fixed string. The function will
be invoked on every match. 
Parameters : (matchLength, capture1, capture2 ..., indexOfTheMatch, sourceString)

The value `returned` from the function serves as the replacement value.

> common problem

1. Matching everything including new line:
    `[\S\s]*` - everything that is and isn't a whitespace character

