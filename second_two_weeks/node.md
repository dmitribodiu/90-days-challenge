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
        Methods shoudl be specified separately, and there can be many for the same 
        resource. It's common practice to use plural name for resource, and 
        specify specific resource with id: `/tours/:id`
    4. send json
    5. be stateless
        `All state is handled on the client` and not on the server.
        Each request should contain all the information to process it.
            If a user is logged in, the user state of being logged in should be 
            saved in the browser and not on the server.

Difference between put and patch: Client has to send the `whole` object for `put`, but
can send part of the resource for patch.

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
    app.use(express.json());
    
    app.post('/api/v1/tours', (req, res) => {
        let newId = tours.length;
        let tour = {...req.body, id: newId};
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
        const tour = tours.find(t => t.id == req.params.id);
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
    // review is an optional parameter
    app.post('/api/v1/tours/:id/:review?', (req, res) => { 
    ```

4. Patch requests
    


























































































































































