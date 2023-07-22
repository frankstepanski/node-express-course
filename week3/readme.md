# Week 3

## Table of Contents
  - [Getting Started with Express](#getting-started-with-express)
  - [Routing](#routing)
  - [Middleware](#middleware)
  - [JSON](#json)
  - [Deployment](#deployment)
  - [Code](#code)

## Objectives
- Understand the basics of Express
- Learn how to create a basic Express server
  - Understand how to use the app.listen() method
  - Using node and nodemon to run the server
  - Separating the Express app from the server
- Create a basic Express route (GET)
  - Types of routes
  - How to use the app.get() method
  - The importance of the route order
  - 404 Not Found route
  - Implement route and query parameters
  - Understand how JSON data is sent to the client
- Middleware
  - What is middleware
  - Types of middleware
  - How to use middleware
  - How to create custom middleware
- Deployment
  - What is deployment
  - Setup an account on Render
  - How to deploy an Express app to Render

## Getting Started with Express

Although you can build full web servers with nothing but Node's built-in 
[http module](https://nodejs.org/api/http.html#http), most codebases will use a framework. 
The API exposed by the http module is pretty minimal and doesn't do a lot of heavy lifting for you.

Building a web application becomes a simpler task with the addition of web frameworks. 
A web framework in Node.js is a module that offers structure to your application that
allows you to build and customize the way your application feels without
worrying about building certain features from scratch.

[Express](https://expressjs.com/), is a Node web server framework that promotes itself as "fast, unopinionated, and minimalist". Express can be thought of as an abstraction layer on top of Node's built-in HTTP server.

Express allows you to concentrate on your application's business logic without having to worry too much about web server technicalities such as URL routing, parsing data, setting HTTP headers, and so on.

>Other popular Node web server frameworks include [Nest](https://nestjs.com/), [Koa](https://koajs.com/),
[Socket.io](https://socket.io/), [Fastify](https://www.fastify.io/) and others.

### Create a basic Express server

First we need to initialize a new npm project and install Express. 

Let us create a folder (I called mine getting-started) and open it in VS Code. 
Open up the Terminal in VS Code and run the following commands:

```
npm init
npm i express

```

![npm init](images/vsc-npm-init.png)

>Because Express is unopinionated, it does not force you to do things in a certain way. If you see something that is diferent in how I set up my project, that's ok!

We create a **src** folder that will hold the source code that directly relates to the running of the server. The **app.js** file will hold configuration and **server.js** is where you will run your Express server.

>Again, you can name the files whatever you want (e.g. unopinionated), but these are commmon names.

![folder structure](images/folder-structure.png)

**app.js:**

We want to export our [app](https://expressjs.com/en/5x/api.html#app) so that we can use it in our server.js file.
```
const express = require("express"); 
const app = express(); // the express() function returns an Express application instance

module.exports = app;
```

**server.js:**

This file is responsible for running the server. We will use the app.listen() method to start the server. The code will "listen" on the specified port. The [app.listen()](https://expressjs.com/en/5x/api.html#app.listen_path_callback) method takes two arguments, the port number and a callback function that will be called once the server is listening.

A [server](https://www.computerhope.com/jargon/s/server.htm) is just some software that is always running that **listens** for requests from other programs. When a request is received, the server performs some action and sends a response back to the requesting program.

A **port** is a way of having multiple applications listen on the same server. Each application will listen on a different port. 

>Think of a port as an apartment number. The mailman will deliver the mail to the correct apartment by looking at the mailbox number. The apartment number is the port number. The mailman is the server. The mail is the request. The apartment is the client.

![cat mailbox](images/cat-mailbox.gif)

```
const { PORT = 8000 } = process.env;
const app = require("./app");

const listener = () => console.log(`Listening on Port ${PORT}!`);
app.listen(PORT, listener);
```

The [process.env](https://nodejs.org/dist/latest-v8.x/docs/api/process.html#process_process_env) object is a global Node object that stores environment variables.
The PORT variable is set to 8000 if it is not already set. The app variable is set to the Express instance exported from app.js.
The listener variable is a callback function that will be called once the server is listening.

>Environment variables are variables that can contain sensitive (e.g. api keys, database url, etc.) data in your project. 

The callback function will log a message to the console once the server is listening.

___

**Starting the server:**

At the command line, run the following command:

```
node src/server.js
```

![express app running](images/node-server.png)

**EADDRINUSE error:** :scream:

If you get an error like this:

![node instances error](images/node-instances-error.png)

Either of two things may be happening:

  - You already have an application already running on port 8000, and you can just change the port number in your server.js file.
  
  - As you have been testing new changes to your Express application, mulitple instances of your server may have been started. 

For the second option, you can use this command to find which process is using the port: :eyes:

```
lsof -i tcp:8000

```

![node instances running](images/list-node-instances.png)

And then run this command to kill the process replacing the **<PID>** with the process ID number:

```
kill -9 <PID>
```

>Instead of using the Terminal, both Mac and Windows have a GUI tool that can do the same thing. Mac has the [Activity Monitor](https://support.apple.com/en-gb/guide/activity-monitor/welcome/mac) and Windows has the [Task Manager](https://learn.microsoft.com/en-us/shows/inside/task-manager) that you can use to find and kill processes.

**The nodemon package**

Every time you make a change to one of your files, you will need to re-start your server in order for the changes to take effect
You can automate this by using the [nodemon](https://www.npmjs.com/package/nodemon) package.

The nodemon package will monitor any file changes and re-start the server for you. 

To install nodemon, run the following command:

```
npm i nodemon --save-dev
```

To use nodemon, you have to add a script in your "scripts" object in your **package.json** file.

![nodemon package.json](images/nodemon-package.png)

You can then run the following command on your command line:

```
npm run dev
```

![express app running](images/nodemon-server.png)

Now, everytime we make a changes to our files, nodemon will restart the server for us. :partying_face:

![nodemon restarting](images/nodemon-restarting.png)

**Stopping the server**

To stop the server, press **Ctrl + C** in your terminal window.

>Remember, we are on the server now, so we will be referencing the terminal more to help us troubshoot errors. :nerd_face:

___

## Routing 

Once the Express server is running and listening for requests, we need to define how the server should 
respond to any given request. 

To tell our Express server how to deal with any given request, we register a series of routes. 
Routes define the control flow for requests based on the request's path and HTTP verb.

For example, if your Express server receives a **GET** request at **/about**, we will use a route to define the appropriate functionality
for that HTTP verb (GET) and path (/about).

The path is the part of a request URL after the hostname and port number, so in a request to localhost:8000/about, 
the path is /about (in this example, the **hostname** is localhost, the port number is 8000).

The HTTP verb is always included in the request, and it is one of a [number of options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) used to specify expected functionality. 

GET requests are used for retrieving resources from a server.

Express uses [app.get()](https://expressjs.com/en/4x/api.html#app.get) to register routes to match GET requests. 
Express routes take up to three arguments, a path (usually a string), a callback function to handle the request and send a response, and a callback argument to the middleware funcvtion, called "next" by convention (optional).

To send a response, we use the [res.send()](https://expressjs.com/en/5x/api.html#res.send) method which sends a response to the client.

>The callback will have three arguments (req, res, next) which we will delve deeper in to later.

Let's add the following route to app.js: 

```
app.get("/about", (req, res, next) => {
  res.send("About Code the Dream!");
});
```

![first route](images/first-route.png)

You can test this route by visiting **localhost:8000/about** in your browser.

![about route](images/about-route.png)

We can also add some HTML to the response:

```
app.get("/about", (req, res, next) => {
  res.send("<h1>About Code the Dream!</h1>");
});
```

![about route html](images/about-route-html.png)

>**Order of routes** - The order of routes is important.  Express searches through routes in the order that they are registered in your code.
The first one that is matched will be used, and its callback will be called.

**Headers and Browser Dev Tools**

Web requests are more complicated than just the URL. Both the request and response can have additional data to be sent back and forth, 
and this process is invisible to most users. This metadata is generally referred to as [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).

You can use the browser's developer tools to view the response from the server:

![devtools](images/browser-devtools-about-tab.png)

If you click on an individual request from the list you will see details about the header server response:

![devtools headers](images/browser-devtools-headers.png)

Then you can click on the Response tab and you will see the data that was sent back to the browser:

![devtools response](images/browser-devtools-response.png)

>If you see a [304 status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304) instead of a 200, don't worry. That just means that the browser has a cached version of the page and is not requesting a new copy from the server.

![304 status code](images/browser-304-about.png)


There are other tools you can use to view the headers (and much, much more), such as [Postman](https://www.postman.com/). Next Week! :zany_face:

>The 404 Status on the **favicon.ico** is normal. The browser is trying to load the favicon.ico file from the root of the server but we don't have one.

When we try to visit a route that is not defined, we get a 404 error:

**404 Error Handler** :broken_heart:

If no routes are matched on a client request, the Express server will send a [404 Not Found response](https://expressjs.com/en/starter/faq.html#how-do-i-handle-404-responses), to the client.

![404 browser error](images/browser-404-error.png)

![404 terminal](images/404-error.png)

> That's not a useful or pretty looking message. How can we make this better? :thinking:

We can add a catch-all route as the last route defined in our app.js file to handle any requests that are not matched by a route defined above. This will be, if effect, our 404 error handler.

A 404 error handler route is defined with app.use() since it considered more "middleware" as this is for any non-existing routes.

**Note:** You could use app.get() and just use `*` as the path, but this is not recommended.

Let's add a 404 error handler to our app.js file:

```
app.use((req, res, next) => {
  res.status(404).send("<h1>404: Page not found</h1>");
});
```

Here we are using the [res.status()](https://expressjs.com/en/5x/api.html#res.status) method to set the status code to 404 and 
sending a custom message to the client.

![404 browser handler](images/browser-404-handler.png)

**response.end()**

A special, but not highly used but good to know method of the response object is [response.end()](https://expressjs.com/en/5x/api.html#res.end) which is used to end the response process.
There are many ways to end a response process: Calling response.send(), response.json(), etc. 

>What is the difference between response.end() and response.send()?

The difference is that **response.end()** ends the response process, but it does not send any data back to the client.

>When would this be used? It is normally used in middleware where you need to end the response (i.e. error handling or logging, etc.)
___

### Status Codes

HTTP status codes are a standard way of communicating the status of a request.

When executed, each operation will return a [status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), indicating whether the invocation has been
successful or not. Successful invocation is indicated by an HTTP 2xx status code, while operations
that are not executed correctly indicate this with erroneous status code — 4xx if the error is
at the client side, or 5xx when the server fails to process a valid request.

>100-199: Informational response; unlikely will return this range\
>200-299: Success; the request was received, understood and processed\
>300-399: Redirection; a resource at a different url was substituted\
>400-499: Client error; problem in how client submitted request\
>500-599: Server error: request accepted, but server error prevented fullfillment

By default, Express will send a [200 status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) when a response is sent,
but sometimes, we may want to respond with a different status code.

We can implicitly set the status code by using the [res.status()](https://expressjs.com/en/4x/api.html#res.status) method,
which is a good practice to follow.

```
app.get("/about", (req, res, next) => {
  res.status(200).send("<h1>About Code the Dream!</h1>");
});
```
___

## Middleware

A web framework like Express.js operates through functions called [middleware](https://expressjs.com/en/guide/using-middleware.html) that sit between 
an HTTP interaction on the web and the Node.js platform. Middleware is a general term applied to code that assists in listening for, analyzing, filtering, and handling HTTP communication
before data interacts with application logic. That is, middleware is code that runs between the request and the response. 

Middleware can take many hats such as authentication, logging, database access, and much more.

>So an Express application is basically a series of middleware function calls. :white_check_mark:

Middleware gets between the [request-response cycle](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) and helps manage the request and can help determine how the server should respond. 
This can be thought of as a middleware pipeline where Express puts multiple pieces of middleware together through a series of functions.

These functions have access to the request object (**req**), the response object (**res**), and the **next** middleware
function in the application's request-response cycle. The next middleware function is commonly denoted by a variable named next.

[Middleware](https://expressjs.com/en/guide/writing-middleware.html) functions are callback functions. Express uses the **use()** method to register middleware functions and 
calls them in the order they are registered.

>Middleware is the most confusing and most important part of Express. Don't worry, once we start building middleware ourselves in upcoming lessons, you will start to understand it better. :sweat_smile:

Middleware functions can perform the following tasks:
  - Execute any code.
  - Make changes to the request and the response objects.
  - End the request-response cycle.
  - Call the **next** middleware function in the stack.

Types of Middleware:
  - Application-level middleware
  - Router-level middleware
  - Error-handling middleware
  - Built-in middleware

If you write your own middleware, it will have this signature:

```
const middleware = (req, res, next) => {
  // Middleware function body
  // next() is called to move to the next middleware function
  // OR return next() to immediately jump out of the callback
}
```

:rotating_light:  Always include and call **next()** in your middleware functions. If not, your Express server will hang (i.e. just stop working). :rotating_light:

>Using **next()** will execute the code after the current middleware function is finished. 
Using **return next()** will immediately jump out of the callback the code below will be unreachable. Normally next() is used and only return next() when in some sort of conditional statement.

**Commonly Used Middleware**

A few of the commonly used middleware packages that we will be using in this course are:

  - [body-parser](https://www.npmjs.com/package/body-parser): provides parsing for HTTP request bodies; parsing both URL-encoded and JSON-encoded bodies, as well as others.
  - [morgan](https://www.npmjs.com/package/morgan): provides HTTP request logging; logs HTTP requests to the console.
  - [helmet](https://www.npmjs.com/package/helmet): security middleware functions that set HTTP response headers to help protect your app from some well-known web vulnerabilities.
  - [static](https://expressjs.com/en/starter/static-files.html): serves static files such as images, CSS files, and JavaScript files.
  - [json](https://expressjs.com/en/api.html#express.json): built-in middleware that parses incoming requests with JSON payloads and is based on body-parser.


![middleware](images/middleware.png)

**Using Middleware**

To test out a middleware function, we can create a simple one that logs the request method and the request path to the console.
We would write this function in the app.js file, right after the other requires:

```
const logRequest = (req, res, next) => {
  console.log("A request is being made!");
  next(); 
};
```

And register this middleware function with the [.use()](https://expressjs.com/en/5x/api.html#app.use) method of the [app](https://expressjs.com/en/5x/api.html#app) object.

```app.use(logRequest);```

Now, visiting **localhost:8000/about** in your browser will now log the message to the console.

![custom middleware](images/custom-middleware.png)

Not very exciting, but it's a start. :thumbsup:

Let's implement the third party logging middleware [Morgan](https://www.npmjs.com/package/morgan).

 - First, we have to install it:

```
npm install morgan
```

- Second, we require it at the top of the app.js file, right after the other requires:

```
const express = require('express');
const app = express();
const morgan = require("morgan");
```

- Then finally, we register the middleware with the use() method (right after the require statement):

```
app.use(morgan("dev"));
```

Now, we will get a more detailed log of the request in the console (you will see lots more useful stuff later).  :wink:

![morgan middleware](images/morgan-console.png)

___

### Route Parameters

Routes become more powerful when they can be used dynamically. 

Express provides this with 
[route parameters](https://expressjs.com/en/guide/routing.html#route-parameters). Parameters are route path segments 
that begin with **:** in their Express route definitions. They act as wildcards, matching any text at that path segment. 

For example **/todos/:id** will match both **/todos/1** and **/todos/7**.

Express parses the parameters, extracts the value, and attaches them as an object to the request object: [req.params](https://expressjs.com/en/api.html#req.params). 
This object's keys are any parameter names in the route, and each key's value is the actual value of 
that field per request.

```
const cars = { 
  honda: { model: "Accord", price: 30000 }, 
  lexus: { model: "LX", age: 90000 } 
};

// GET /cars/lexus
app.get('/cars/:name', (req, res, next) => {
  console.log(req.params.name); // => lexus
  console.log(cars[req.params.name]); // => { model: 'LX', age: 90000 }
  res.send(cars[req.params.name]);
});
```

In the above code, a [.get()](https://expressjs.com/en/5x/api.html#app.get) route is defined to match the **/cars/:name** path. When a GET request arrives for **/cars/lexus**, 
the callback is called. Inside the callback, req.params will be an object containing the key name and the value lexus, 
which was present in the actual request path.

The appropriate car is retrieved by name (the object key) from the cars object and sent back to the client with [res.send()](https://expressjs.com/en/5x/api.html#res.send).

So now if we visit **localhost:8000/cars/lexus** in our browser, we will see the following:

![route param](images/route-param.png)

And in our terminal:

![route param](images/terminal-route-param.png)

___

### Query Parameters

A [query string](https://en.wikipedia.org/wiki/Query_string) is text that comes at the end of a URL following a question mark **?**. 
It provides additional information to a given route. 

A query parameter is a key-value pair in a query string. The key and the value are strings separated by an equals sign =. 

```
// GET /search?q=javascript
app.get('/search', (req, res, next) => {
  console.log(req.query.q); // javascript
  res.send(`You searched for: ${req.query.q}`);
});
```

![query param](images/query-param.png)

In the above example, the query parameter key is **q** and the value is **javascript**. We can access these query parameters by
using the [req.query](https://expressjs.com/en/api.html#req.query) object. The contents of URL query strings are
stored in an object called **req.query**. This object will be empty if there are no parameters, otherwise it will be 
an object of key value pairs.

___

### Putting Route and Query Parameters Together

We can use the query and route paramters together to create a more dynamic route. 
This is useful when we want to filter the data we are retrieving from a database.

The following route will match **/cars/lexus?color=red**. 
The route parameter **name** will be lexus, and the query parameter **color** will be red.
 
```
// GET /cars/lexus?color=red
app.get('/cars/:name', (req, res, next) => {
  console.log(req.params.name); // => lexus
  console.log(req.query.color); // => red
  console.log(cars[req.params.name]); // => { model: 'LX', age: 90000 }
  res.send(cars[req.params.name]);
});
```

![route and query params](images/route-query-params.png)

![route and query params](images/terminal-route-query-params.png)

>But which is better? Route or Query parameters? (The answer is it depends.) :smiling_imp:

Route parameters are used to identify a resource, while query parameters are used to filter the results of a query.
Meaning, you can use query parameters to get a subset of the data from a resource.

___

### Serving Static files

Most web applications contain static files that return the same response to all users. These could include images, favicons, CSS stylesheets, client-side JavaScript, pre-rendered HTML pages, or any other asset.
Express allows you to define a **single directory** that contains **static assets** and returns any file that matches the URL path.

To access these static files, you would use the [static method](https://expressjs.com/en/starter/static-files.html) from the [express](https://expressjs.com/en/5x/api.html#express) module. 
This method takes an absolute path to the folder containing your static files.

>Yes, more middleware!!! :partying_face:

As with any other middleware function, you need to tell your Express app **to use** this feature.
  
  - Add a **public** folder in your root project folder

  ![publich folder](images/public-folder.png)

  - Find a **cute** cat image and create an **images** folder inside the public folder. Then put the cat image in the images folder.
 
  - Add the line of code below to the app.js file (before your route definitions):

  ```
  app.use(express.static("public"))
  ``` 
Then visit **localhost:8000/images/cat.jpg** in your browser. You should see the cat image. :heart_eyes_cat:

![publich folder](images/static-cat.png)

>**Note:** The **public** folder name is a convention. 

___

## JSON

For REST APIs to be useful, they need to work with data in a format that is **easy** to consume. This is where [JSON](https://www.json.org/json-en.html) comes in.

JSON (JavaScript Object Notation) is a common format for storing and transmitting data over the internet. It is based on a subset of the JavaScript Programming Language, 
more specifically the [JavaScript object literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#objects_and_properties).

It is easy for humans to read and write to this format as well as easy for machines to parse and generate. We all like easy, right? 
Just like the [song](https://www.youtube.com/watch?v=0XRrYgByp28) says, "It's easy like Sunday morning". :sunglasses:

**Serialization and Deserialization**

The process of sending data in JSON format to REST API (request or response) is called **serialization**. 

>Serialization is the process of converting an object into a stream of bytes to store the object or transmit it to memory.

To do this in JavaScript we can use the [JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) method.

```
const car = {
  model: 'LX',
  age: 90000
};

const json = JSON.stringify(car);
```

And voila! The car variable now holds a regular javascript object that we can work with in our code.

The process of parsing received JSON data from a REST API is called **deserialization**.

>Deserialization is the reverse process where the byte stream is used to recreate the actual JavaScript object in memory. 

To do this in JavaScript we can use the [JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) method.

```
const json = '{"model":"LX","age":90000}'; // => this would be coming from a REST API
const car = JSON.parse(json);
```

**Serving JSON**

The state of an application is all the data that the application must keep track of in order to work.
This data is usually stored in a database, but for small applications, and for our immediate purpose, it can be stored in a .js file.

Step 1:

Let's create a **data** folder and create a **users-data.js** file in it.

To make it easy, let's grab some data from the popular [JSONPlaceholder API](https://jsonplaceholder.typicode.com/). 

Head over to their website, scroll down a bit and click on the [users](https://jsonplaceholder.typicode.com/users) link under Resources.

![JSONPlaceholder Resources](images/json-placeholder-users.png)

... and copy that data into your users-data.js file.

![data folder](images/data-folder.png)

Step 2:

In **users-data.js** add a module.exports statement for the array of users to export it.

![json data property](images/user-data.png)

Step 3:

Now that we have the data in a .js file, we need to import it into our app.js file.

```
const users = require('./data/users-data.js'); 
```

Step 4:

Then finally, we create a route that will return the users data.

```
  // ... other middleware and routes

app.get('/users', (req, res, next) => {
  res.send({ data: users });
});
```

>Why use a **data** property? 

This is because you are following a simplified version of the [JSON:API specification](https://jsonapi.org/), a common pattern for APIs returning JSON. 

The APIs that you build will always return an object with either a data property or an errors property. 
Any information sent to the API will also be an object with a data property. 

Now visit **localhost:8000/users** in your browser. You should see the JSON data. 

![users data browser](images/browser-users-data.png)

If we look in our browser's dev tools, we can see that the **Content-Type** is **application/json**. We didn't set this, it was set automatically by Express.
Using the [res.send()](https://expressjs.com/en/api.html#res.send) method, Express will automatically set the **Content-Type** header based on the data type of the argument.

![browser users dev tool](images/browser-users-dev-tools.png)

Great, we displayed all of our users, but what if we want just one particular user? Maybe a user with a specific **id**?

We can reference the **id** property to get a specific user by passing that value as a **route parameter**. :cowboy_hat_face:

```
app.get('/users/:id', (req, res, next) => {
  const { id } = req.params;
  const foundUser = users.find(user => user.id === Number(id));
  if (foundUser) {
      res.send({ data: foundUser });
  } else {
      next(`User not found! ID: ${id}`);
  }
});
```

Then visit **localhost:8000/users/5** in your browser. You should see the JSON data for the user with the **id of 5**.

![users data params](images/browser-user-params.png)

If you tried to visit **localhost:8000/users/50** in our browser, you would get an error because there is no user with an **id of 50**. :expressionless:

![user params error](images/browser-user-error.png)

The **next()** function with an error message is a way to tell Express that there was an error and to pass the error to the next middleware function.
Since we didn't create a middleware function to handle errors, Express will automatically send a 500 error to the browser along with the error message.

![500 error](images/terminal-500-error.png)

**res.json()**

In the previous example, we used the [res.send()](https://expressjs.com/en/api.html#res.send) method to send the JSON data to the browser.
We techncially should have used the [res.json()](https://expressjs.com/en/api.html#res.json) method instead because the data we were sending was a JavaScript object.

It worked anyway because Express automatically converts JavaScript objects to JSON.

But we can also use res.json() to send JSON from data that is not even an object at all. :exploding_head:

```
app.get('/greeting', (req, res, next) => {
  const greeting = "Hello World!";
  res.json(greeting);
});
```

Then visit **localhost:8000/greeting** in your browser. You should see "Hello World" sent as JSON. 

![greeting json](images/browser-greeting.png)

Checking our browser's dev tools, we can see that the **Content-Type** is **application/json**.

![res.json](images/browser-json-greeting.png)

>When would we use res.json() over res.send()? Use res.json() when you want to send a JSON response that was created from a JavaScript object or not even an object at all.

**JSON Browser Extensions**

Although we can view JSON in the browser, it is not very readable.  To make it easier to read, we can install a browser extension that will format the JSON for us.
There are many extensions that do this, a very popular one for the Chrome browser is [JSON Formatter](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en).

![json chrome extension](images/json-browser-extension.png)

>As mentioned earlier, we will talk about another tool to interact with APIs and JSON next week. So stay tuned! :star_struck:

___

## Deployment

Let's try to deploy what we have learned so far to the cloud. :cloud:
This way we can share our app with the world. :earth_americas:

When we choose a cloud provider we have to aware of what we are deploying:

  - Is it a React/Next app? If so, we can use a React hosting service like [Vercel](https://vercel.com/) or [Render](https://render.com/)
  - Is it a Node.js app? If so, we can use a Node.js hosting service like [Render](https://render.com/) or [Heroku](https://www.heroku.com/).
  - Is it a Java app? If so, we can use a Java hosting service like [AWS](https://aws.amazon.com/).
  - The list goes on and on...

>We are using Render over Heroku because Heroku's free-service has ended.

**Render Deploy Steps**

Before we start, Render requires a [specific version of Node.js](https://render.com/docs/node-version). Refer to the link for more information.

>You have to implement one of the options described in the link in your code before you deploy.

**Step 1**

Create a free account on [Render](https://render.com/).

![render](images/render.png)

**Step 2**

Once logged in, in your Dashboard, click on **New +** and select **Web Service**. 

![render new web service](images/render-web-service.png)

**Step 3**

Select a repository to deploy, if you do not see the repo in the list, click on **Configure account** under Github.
This will install the Render app on your Github account, then you can select the repo.

**Step 4**

After you select the repo, you will be taken to the **Configure** page. Here you can set the **Name** of your app and the **Branch** to deploy.

The **Build Command** will be $npm i, the **Start Command** will be $node src/server.js.
The **Runtime** will be Node.js and select **Instance Type** will be Free. 

![render configuration](images/render-config.png)

>Note: We are instructing Render how to install our dependencies and start our app. 

**Step 5**

You will then see the build process start. Once it is complete, you will see a green check mark and a link to your app. \
The link will be **your-app-name.onrender.com**.

The build process can take a few minutes, so be patient (remember, we are using the free account).

![render build process](images/render-build-process.png)

**Step 6**

Now, in your dashboard you should now see your deployed app(s) listed. Click on the app to see the details.

![render dashboard deployed app list](images/render-dashboard-app-list.png)

**Step 7**

Test out a few of the routes you have created. :smile:

>We will continue to deploy our app as we add more functionality to it. For now, we just wanted to test out deployment.

Grab a :coffee: or a beverage :tropical_drink: of choice and enjoy the rest of your day. :sunglasses:

![congrats](images/congrats.gif)

## Code

Checkout the [repo](https://github.com/frankstepanski/node-express-course-code/tree/main/week3/src) for this week's lesson.