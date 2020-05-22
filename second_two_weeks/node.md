# ■■■ Day 16
# Express
## Introduction
```js
const express = require('express');
const app = express();
app.get('/', (req, res) => { res.status(200).json({greeting: "Hello"})}); // note json
app.listen(3000, () => { console.log(`running on port 3000`);});
```
To return json you can use `json` funciton instead of `send`.

## API design
REST makes api's easier to consume.
To build RESTful architecture you need to 
    1. sepate api into logical resources
        Resource is an object with data.
    2. expose structured URLs
        Urls should be structured and easy to read and discover.
    3. use http methods (verbs)
        Urls should only contain an address to a resource(s), and not the methods
        Methods should be specified separately, and there can be many for the same 
        resource. It's common practice to use plural name for resource, and 
        specify specific resource with id: `/tours/:id`
    4. send json
    5. be stateless
        `All state is handled on the client` and not on the server.
        Each request should contain all the information to process it.
            e.g : If a user is logged in, the user state of being logged in should be 
            saved in the browser and not on the server.

Difference between put and patch: Client has to send the `whole` object for `put`, but
can send part of the object for patch.

Best practice RestFul resource url's
```js
/users/3/tours
/users/3/tours/4
```

## Requests handling
Different version of api for backward compatibility: `app.get('/api/v1/tours')`

1. Get requests
    ```js
    app.get('/api/v1/tours', (req, res) => {
        res.status(200).json({
            status: 'success',      // make easy for client to get errors
            results: tours.length, // make it easy for client to understand amount
            data: {
                tours
            }
        })
    });
    ```
2. Post requests
    `body` property on the request contains the body of the request.
    ```js
    app.use(express.json()); // allow body to json parsing
    
    app.post('/api/v1/tours', (req, res) => {
        let newId = tours.length;
        let tour = {...req.body, id: newId}; // note req.body
        tours.push(tour);
        
        res.status(201).json({ // 201 means created*
            status: 'success', 
            data: {
                tour
            }
        }); 
    });
    ```
3. Responding to URL parameters
    ```js
    app.get('/api/v1/tours/:id', (req, res) => {
        const tour = tours.find(t => t.id == req.params.id); // parameters are in params
        res.status(tour ? 200 : 404).json({ 
            status: tour ? 'success' : 'fail', 
            data: {
                tour
            },
            message: tour ? '' : 'No such tour exists'
        });
    });
    ```
    You cant define `multiple` parameters as well as `optional` ones.
    ```js
    // review is an optional second parameter
    app.post('/api/v1/tours/:id/:review?', (req, res) => { 
    ```

4. Patch and Delete requests
    ```js
    app.patch('/api/v1/tours/:id', (req, res) => {
        res.status(200).json({status: "success", data: {tour: "updated tour"}})
    }); 
    
    app.delete('/api/v1/tours/:id', (req, res) => {
        res.status(204)                     // note 204 status code
            .json({status: "success", data: {tour: "updated tour"}})
    });
    ```

## Routes refactoring
```js 
app.route('/api/v1/tours/:id')
    .get((req, res) => {/*...*/})
    .post(fn); // you can pipe verbs for the single resource
```

## Middleware cycle and custom middleware
All middleware together is called `middleware stack`, 
order of middleware matters.

1. custom middleware
    ```js
    app.use(mdwr); // user middleware 

    app.use((req, res, next) => {
        console.log("hello from custom middleware");
        next(); 
    });
    ```
2. Middleware cycle
    If you don't call `next`, server will `never respond` to the client.
    If you don't specify specific route for the middleware, the middleware will
        execute for `every` request.
    Middleware can return to the client before the request has gone through every 
        middleware.

    You can define `new properties` on the request object and read them down the 
    middleware pipeline in other middleware functions.

3. 3rd party middleware
    e.g : morgan
    ```js
    const morgan = require('morgan'); // this package loggs information 
    //...
    app.use(morgan('dev'));
    ```

## Creating multiple routers
```js
const tourRouter = express.Router(); // first create a new Router, which is a middleware

tourRouter.route('/') //  /api/v1/tours
    .get(reqHandler)
    .post(reqHandler)

tourRouter.route('/:id') //  /api/v1/tours/:id
    .patch(reqHandler)
    .delete(reqHandler)
    
// then 
app.use('/api/v1/tours', tourRouter); // address will be PREpended to tourRouter route
```

## Better file structure
```js
// in userRoutes.js in routes directory
const express = require('express');
const userRouter = express.Router();

userRouter.route('/')
    .get(getAllUsers)
    .post(createUser);
    
userRouter.route('/:id')
    .get(getUser)
    .patch(updateUser)
    .delete(deleteUser);
    
module.exports = userRouter;
```

You should also create `controllers` folder and put functions themselves there.
You can call files in controllers folder (model)Controller. 

Also create a file called `server.js` and call app.listen from there.
This file will be responsible for combining all the things you have together.

## Param middleware
Is a middleware that runs only if you have special parameters in you middleware.
```js
const router = express.Router();
router.param('id', (req, res, next, parameterValue) => {
    console.log(parameterValue);
});
// note it's router.param, not router.route
```

Sometimes you may wanna need to prevent middleware from passing control to the 
next middleware. In this case you can just `return` from the middleware something.
```js
return res.status(404).json(/**/);
```

Param middleware runs before other middleware and allows you to 
short-cirquit the request in case the parameters are invalid.

You should define param middlewre in your controllers folder and 
add it to your routes like you saw above.

Each function in your `controllers` folder shoudl not care about validation, 
their purpose should be to do what they should do -> business logic.

 
## Chaining middleware for the same route
```js
router.route('...')
    .get(userController.getUser); // runs single function for single route

// how to call multiple functions for the same route and verb?
// e.g you want to validate not parameters but request body
router.route('...')
    .get(userController.getUser)
    .post(userController.validateUser, userController.postUser); 
```
just use `COMMA`

## Serving static files
You have to use built-in middleware.
```js
app.use(express.static(`${__dirname}/public`));
```

## Environment variables
You can check environment with `app.get('env')` which returns "env" environment
variable. You can set environment from the console like this:
```cli
NODE_ENV=development node app.js
```

You can also create a configuration file -> `config.env` in the root of your app.
```c
// config.env content:
NODE_ENV=development
USER=jonas
PASSWORD=1235151
PORT=3000
```

Uppercase is a convention for environment variables.

To use this configuration file you can use npm package called `dotenv`
```js
const dotenv = requre('dotenv');
dotenv.config({path: './config.env'});

// this command reads the file and assigns configuration variables into env. variables
```

Then you can make confitional logging like this:
```js
if(process.env.NODE_ENV === 'development')
    app.use(morgan('dev'));
    
// conditional port
const port = process.env.PORT || 3000;
```

**Note** you should read config file into env vars `before` you use app in your server
file.




















































































































