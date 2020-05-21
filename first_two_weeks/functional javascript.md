# ■■■ Day 12
# Mastering JavaScript Functional Programming >>>>>
# Becoming Functional
FP is based on producing the desired result by evaluating expressions built
out of functions that are `composed` together

We want our code to be:
    + modular
    + undertandable
    + testable
    + extensible
        Small changes shouldn't imply large, serious refactoring of your code.
    + reusable

FP gives us `all of that`.

- Not all is gold
    we want to use FP to simplify our coding, not to make it more complex
    so sometimes we may want to step off FP paradigm where we need it.
    
JavaScript isn't a purely functional language, but it has all the features
that we need for it to work as if it were:
    1. Functions as first-class objects
    2. recursion
    3. arrow functions
    4. closures
    5. spread operator

# ■■■ Day 13
# Thinking Functionally
## Doing something only once
Solutions:
1. hoping for the best
2. using global flag
    This obviously works, but it has several problems that must be addressed:
        + global variables are evil
        + you need to not to forget to reinitialize the variable
        + testing global variables is hard
3. removing/changing handler
    + tight coupling (bad)
    + all drawbacks from the previous solution
4. disabling the button
5. using closure to allow function calling only once
6. `functional solution`
    + We don't want to modify the original function in any way
    + We need to have a new function that will call the original one only once
    + We want a solution to be general

    we'll write a higher-order function, which we'll be able to apply to any
    function, to produce a new function that will work only once
    
    ```js
    const once = (fun, after) => {
        let called = false;
        return (...args) => {
            if(!called) {
                called = !called; 
                return fun(...args) 
            } else {
                return after(...args); // calls different function after the first call
            }
        };
    }
    // or
    const once = (f, cd = true) => (...args) => cd ? (cd != cd, f(...args)) : cd;
    ```

Exercises
```js
// 1. function "once" without extra variables:
const once = f => {
    return (...args) => {
        let result = f(...args);
        // empty function (you could use use null but it will throw)
        f = () => {};
        return result;
    }
}

// 2. function thisManyTimes
const nTimes = (f, n) => {
    return (...a) => {
        return n ? (f(...a), n--) : () => {};
    }
}
```

# Starting Out with Functions
```js
const altSum3 = x => y => z => x + y + z;
altSum3(1)(2)(3); // 6
```

## Functions as objects
You can replace :
```js
fetch("some/remote/url").then(function(data) {
    processResult(data);
});
// with this
fetch("some/remote/url")
    .then(processResult); // no need to create a function
// BUT
fetch("some/remote/url")
    .then(someObje.processResult.bind(someObje); // when calling METHODS you need to BIND
```
This programming style is called `pointfree` style or tacit style,
and its main characteristic is that you `never` specify the arguments
for each function application. Advantage is `cleaner code`.

## Using functions in FP ways
1. Injection
    ```js
    // strategy pattern
    palabras.sort((a, b) => a.localeCompare(b, "es")); // es => espanol
    ```

2. Stubbing
    an idea that comes from testing that involves replacing a function with
    another that does a simpler job, instead of doing the actual work.

Exercises:
```js
// convert to one-liner
const doAction3 = (state = initialState, action) =>
    (dispatchTable[action.type] && dispatchTable[action.type](state, action)) ||
    state;
// result
const doAction3 = (st = initialState, a) => dt[a.type] ? dt[a.type](st, a) : st;

// make a polyfill for bind
const bind = (f, o) => (...arg) => f.call(o, arg); 
```

# Pure functions
A function is pure if it satisfies two conditions:
    1. Same arguments => same results
    2. No side effects
    
## Referential transparency
Is a property that lets you replace an expression
with its value without changing the results of whatever you were doing

## Functional impurity 
Function is `impure` if it changes or tries to get access to anything that is
not it's arguments, or calls another function that does that.
> If a function uses an impure function, it immediately becomes impure itself.

Another reason for imputity may imply inner function state, which may 
    change output of the functions with the same arguments.
Another reason for impurity is changing arguments passed by `reference`.

Function that uses random generator is obviously impure.

## Advantages of pure functions
1. Order of execution
    When you work with pure functions there's
    no explicit need to specify the order in which they should be called.

2. Memoization
    Since the output of a pure function for a given input is always the same,
    you can cache the function results

3. Self-documentation
4. Unit testing

## Impure functions
Let's consider how we can reduce the number of impure functions, even if doing away with
all of them isn't really feasible. 

1. Avoiding the usage of state
    + provide all the needed info as arguments
    + if you need to update the state, create another special impure function for this
        but your pure function should just return the changed copy

2. Injecting impure functions
    If a function becomes impure because it needs to call another function
    that is itself impure, a way around this problem is to `inject` the required function.
    Injection allows for easier later `testing` (using "mocking" technique)

3. Using just the result of impure functions
    You can just pass the result from impure functions, which will be just value(s).

## Pure function testing
Is very easy.
You may want to separate pure function testing from impure function testing
because they usually care about different things.

# ■■■ Day 14
# Programming declaratively
## Transformations
1. Reducing
    Reducing is much easier to read than hard-coded loops.
    1. Sum an array
        To reduce an array, you must provide a dyadic function(with two args)
        and an initial value
        ```js
        const myArray = [22, 9, 60, 12, 4, 56];
        const sum = (x, y) => x + y;
        const mySum = myArray.reduce(sum, 0); // 163 or ERROR if an array is empty
        
        const sum = (x, y) => (x || 0) + (y || 0); // fix
        ```
    2. Find average
        ```js
        const average = (a, i = 1) => b => (a += b) / ++i;
        let arrAvg = average(0, 0);
        const avg = arr.reduce((_, v) => arrAvg(v))
        let sn = average(5, 1); // initialization -> average, total items
        ```
    3. Calculating several values at once
        You could return an array instead of a single value.
    4. Folding left and right
        The complementary reduceRight() method works just as reduce() does,
        only starting at the end and looping until the beginning of the array.
        There are cases where it's useful, e.g reversing a string.
        ```js
        const sum = (a, b) => a + b;
        const reversStr = str => str.split('').reduceRight((b, v) => b + v, '');
                                                    // or (sum, '')
        ```

# ■■■ Day 15
2. Mapping
    1. Map over loop?
        + you don't need to write any logic which could cause bugs
        + you don't need to track "i" variable
        + new array is produces so your code is pure
    2. Emulating map with reduce
        ```js
        const myMap = (a, f) => a.reduce((b, v) => b.concat(f(v)), []);
        ```
    3. Dealing with arrays of arrays
        In ES2019, two operations were added to JavaScript: `flat`() and `flatMap`().
        
        The `flat`() method creates a new array, concatenating all elements of its
        subarrays to the desired level, which is, by default, 1:
        ```js
        const a = [
            [1, 2],
            [3, 4, [5, 6, 7]]
        ];
        a.flat(); // 1 by default => [1, 2, 3, 4, [5, 6, 7]];
        a.flat(2); // only numbers
        ```
        
        Basically, what `flatMap`() does is first apply a map() function and then
        apply flat() to the result of the mapping operation.
        
        Emulating flat and flatMap:
        ```js
        // concat unflats for us
        const flatOne = arr => arr.reduce((b, v) => b.concat(v), []); 

3. More general looping
    Sometimes you need to do a loop, but the required process doesn't really fit
    map() or reduce(). Use `foreach`.

## Logical higher-order functions
Meaning using predicate as a higher order function
(predicate is a function that returns bool)

Where it can be useful?
    1. Filtering
    2. Searching

## Working with async functions
1. Async-ready looping
    If we cannot directly use methods such as forEach(), map(), and the like,
        we'll have to develop new versions of our own.
    1. Looping over async calls
        ```js
        const forEachAsync = (a, f) => 
            a.reduce((b, v) => b.then(() => f(v)), // apply a func. to result
            Promise.resolve() // base is a resolved promise
        );
        // note! calls are not async against one another but rather subsequent
        ```
    2. Mapping async calls
        ```js
        // use in case the function you want to apply is async and returns a promise
        const mapAsync = (arr, fn) => Promise.all(arr.map(fn));
        // this way promises from function will be unfolded automatically
        ```
    3. Filtering async calls
        First you need to mapAsync and then when you get an array of bool
        you filter the first array with a simple filter.
        ```js
        const filterAsync = (a, f) => Promise.all(arr.map(f))
            .then(b => a.filter((_, i) => !!b[i])); 
        ```
    4. Reducing async calls
        ```js
        const reduceAsync = (a, f, b) => 
            a.reduce((b, v) => b.then(f), Promise.resolve(b)); // error we need to use v
                                                            // now we only use b
        const reduceAsync = (a, f, b) => 
            a.reduce((b, v) => b.then(b => f(b, v)), Promise.resolve(b));
            // don't forget that for reduce you need a dyadic function
        ```

## Currying and partial application
Unary function - one argument
Binary - two
Variadic - variable amount

What is currying?
    Currying is a `process of converting` a function with n number of arguments
    into a nested unary function
    
Why we need it?
    It helps us with `abstraction`.

How to implement?
```js
const curry = (f, args = []) =>  // store arguments in closure
    function fn(a) { 
        (args = args.concat(a)).length == f.length  // add arugments and compare to f len
            ? f(args) 
            : fn;
    }
```