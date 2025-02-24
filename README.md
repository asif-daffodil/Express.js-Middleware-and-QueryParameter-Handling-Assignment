# Express.js Middleware and Query/Parameter Handling Assignment

## Overview
This assignment focuses on creating and using middleware in an Express.js application. You will build an application that handles query strings and route parameters using middleware functions for validation and response control.

## Project Structure
```
- /middlewares
    - checkAge.js
    - validateName.js
- /public
    - (static files like index.html, style.css, etc.)
- index.js
- README.md
```

## Instructions

### 1. Set Up Express.js Server
- Create an `index.js` file to set up a basic Express.js server.
- Serve static files from the `public` directory using `express.static()`.

### 2. Middleware for Name Validation
- Inside the `middlewares` directory, create a file named `validateName.js`.
- In this middleware, check if the `name` query string is present when accessing the `/contact` route.
- If `name` is missing, respond with: `"Please provide your name."`
- If `name` is present, pass control to the next middleware or route handler.

```javascript
// validateName.js
const validateName = (req, res, next) => {
    if (!req.query.name) {
        return res.send("Please provide your name.");
    }
    next();
};

module.exports = validateName;
```

### 3. Middleware for Age Validation
- Inside the `middlewares` directory, modify the `checkAge.js` middleware.
- It should check if an `age` parameter is provided in the `/about/:age?` route.
- If `age` is missing, respond with: `"Please provide your age."`
- If `age` is less than 18, respond with: `"You are not allowed to enter this page."`
- If `age` is 18 or above, pass control to the next middleware or route handler.

```javascript
// checkAge.js
const checkAge = (req, res, next) => {
    const age = req.params.age;
    if (!age) {
        return res.send("Please provide your age.");
    }
    if (age < 18) {
        return res.send("You are not allowed to enter this page.");
    }
    next();
};

module.exports = checkAge;
```

### 4. Routes in `index.js`
- Add the middleware to the respective routes in your `index.js` file.
- Use `validateName.js` for the `/contact` route to check for a `name` query string.
- Use `checkAge.js` for the `/about/:age?` route to check for the age parameter.

```javascript
// index.js
const express = require('express');
const app = express();

// Middleware and static files
app.use(express.static('public'));
const validateName = require('./middlewares/validateName');
const checkAge = require('./middlewares/checkAge');

// Routes
app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.get('/about/:age?', checkAge, (req, res) => {
    res.send('About Us');
});

app.use('/contact', validateName);
app.get('/contact', (req, res) => {
    res.send(`Contact Us, ${req.query.name}`);
});

// Start server
app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```

### 5. Running the Application
- To run the application, install the dependencies using `npm install` and start the server with `node index.js`.
- The server will run on `http://localhost:3000`.

### 6. Testing the Application
- Test the `/contact` route with and without the `name` query parameter. Example: `http://localhost:3000/contact?name=John`.
- Test the `/about/:age?` route with different age values. Example: `http://localhost:3000/about/25` or `http://localhost:3000/about/15`.

## Conclusion
By completing this assignment, you will have a better understanding of middleware, route parameters, and query handling in Express.js. 