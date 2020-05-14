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
                return after(...args); // calls another function after the first call
            }
        };
    }
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
for each function application. Advantega is `cleaner code`.

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

Another reason for imputity may imply innner function state, which may 
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
    Injection allows for easier later `testing`.

3. Using just the result of impure functions
    You can just pass the result from functions, which will be just value(s).

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
        const average = (a, i) => b => (a += b) / ++i;
        const avg = arr.reduce(average(arr[0], 1))
        let sn = average(5, 1); // initialization -> average, total items
        ```
    3. Calculating several values at once
        You could return an array instead of a single value.
    
2. Folding left and right

































































































s