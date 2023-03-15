# Week 4

## Table of Contents
  - link 1
  - link 2
  - link 3
  - link 4

## Objectives
- RESTful APIs
- CRUD HTTP Requests
- Using Postman
- Error handling
- MVC
- Controllers
- Routers

## Representational state transfer (REST)

When a web API follows the [constraints of REST](https://restfulapi.net/rest-architectural-constraints/), the API called a RESTful API. :sleeping:

A RESTful API is all about URLs, which provide access to resources such as HTML, CSS, image, JSON, that are returned by a request.

There are [design principles](https://apiguide.readthedocs.io/en/latest/build_and_publish/use_RESTful_urls.html) that each RESTful API usually follows: 

A few of the most important principles are:
  - Use nouns to identify resources (e.g. /todos)
  - Use plural nouns to identify collections of resources (e.g. /todos)
  - Use HTTP methods to specify what to do with a resource (e.g. GET /todos, POST /todos, PUT /todos/:id, DELETE /todos/:id)
  - Use route parameters to specify which resource to access (e.g. /todos/:id)
  - Use query parameters to filter resources (e.g. /todos?completed=true)


So for example, the URL /todos provides access to a collection of todo resources, while the URL /todo/1 provides access to a single todo resource. 
A collection of resources is also considered one resource.

REST uses various representations of a resource such as JSON, XML, HTML, and plain text. The most common representation is JSON.

The HTTP Protocol represents an [HTTP message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages) as text, it may look like JSON, or even JavaScript, but it is always text.

![http messages](images/http-messages.png)

##  HTTP Request Methods

Up to this point, we have only used the HTTP request method [GET](https://expressjs.com/en/5x/api.html#app.get) method to retrieve data from a request to a server.

But, there's more! :raised_hands:

Other common HTTP methods that we can use to interact with resources on a server would be:

  - [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST): creates a new resource
  - [PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT): update an existing resource
  - [DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE): delete an existing resource

The following table shows thew common HTTP methods, Express method and [CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD) operations 
that each HTTP method can perform to interact with resources on a server. 

A **CRUD operation** is a basic operation that we can perform on a resource. CRUD stands for Create, Read, Update, and Delete.

>A table showing the common HTTP methods, Express methods, CRUD operations, and their descriptions:

| HTTP Method |  Express Method | CRUD Operation | Description                 |
| ----------- |  -------------- | -------------- | --------------------------- |
| GET         |  app.get()      | Read           | Retrieve a resource         |
| POST        |  app.post()     | Create         | Create a new resource       |
| PUT         |  app.put()      | Update         | Update an existing resource |
| DELETE      |  app.delete()   | Delete         | Delete an existing resource |

>A table showing how we would map a route to its URL path and HTTP method:

| Route name   |  URL path       | HTTP Method    | Description                                   |
| ------------ |  -------------- | -------------- | --------------------------------------------- |
| Index (list) |  /todos         | GET            | Return a list of todos.                       |
| Create       |  /todos         | POST           | Create a new todo.                            |
| Read         |  /todos/:id     | GET            | Return the todo with the speficied id,        |
|              |                 |                |  or return 404 if not found.                  |
| Update       |  /todos/id      | PUT            | Update and existing todo with data in request |
| Delete       |  /todos/:id     | DELETE         | Delete the todo with the specified id,        |
|              |                 |                |  or return 404 if not found.                  |

___

![questions](images/QA.gif)

>Before continuing, let's take a brief intermission to review some common questions about RESTful APIs jargon.

**How do we access a resource on a server?**\
We can use a route to access a resource. A route is a path that we can use to access a resource.

**How does a route specify what to do with a resource?**\
We use an HTTP method to specify what to do with a resource, such as GET, POST, PUT, and DELETE.

**What is an endpoint?**\
An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

**What is the difference between a route and an endpoint?**\
A route is a path that we can use to access a resource. An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

```
// route:
/api/v1/todos/:id

// endpoint:
GET /api/v1/todos/:id
POST /api/v1/todos/:id
PUT /api/v1/todos/:id
DELETE /api/v1/todos/:id
```

**What is the difference between a CRUD operation and an HTTP method?**\
A CRUD operation is a basic operation that we can perform on a resource. An HTTP method is a method that we can use to perform a CRUD operation on a resource.
CRUD operations come from the world of databases. HTTP methods come from the world of the web.

**What is a payload?**\
A payload is the data that we send to a server in a request. The payload is the data that we send to a server in a request.

```
// payload:
{
  "id": 1,
  "title": "Buy milk",
  "completed": false
}
```
___
### Routes 

Before we get knee deep into the other HTTP methods, let's take a step back talk about how we can structure our routes a bit more efficiently.

We can use [Express router](https://expressjs.com/en/guide/routing.html#express-router) to group routes together. 
A router is a mini Express application that we can use to group routes together.

The router file will contain all of the routes for a specific resource. For example, we can create a router file for a todo resource.

![routes file](images/routes-file.png)

We would import the router into our app file and use it to handle all of the routes for our todo resource.

>The [todo resource](https://jsonplaceholder.typicode.com/todos) is just another example I grabbed from [JSON Placeholder](https://jsonplaceholder.typicode.com/).  You can use this pattern for any resource. :nerd_face:

```
// app.js
const express = require('express');
const app = express();

const todoRouter = require('./routes/todos');

app.use('/todos', todoRouter);

// ... rest of the app
```


```
// routes/todos.js
const express = require('express');
const router = express.Router(); // => instantiate a router object

router.get('/', (req, res) => { // => attaching a route to the router object
  // ... handle the request 
});

router.get('/:id', (req, res) => { // => attaching a route to the router object
  // ... handle the request
});

module.exports = router;
```


This way, we can keep our routes organized and separate from our main app file. :sunglasses:

>You can have as many router files as you want. For example, you can have a router for your todo resource, a router for your user resource, and a router for your blog resource.

You can also have a route that starts with a specific **starting point**. This is called a route prefix. 
In the example below, we have a route prefix that starts with either /api/v1/todos, /api/v1/users, and /api/v1/blogs.

```
// app.js
const express = require('express');
const app = express();

const todoRouter = require('./routes/todos');
const userRouter = require('./routes/users');
const blogRouter = require('./routes/blogs');

app.use('/api/v1/todos', todoRouter); // => route prefix that starts with /api/v1/todos
app.use('/api/v1/users', userRouter); // => route prefix that starts with /api/v1/users
app.use('/api/v1/blogs', blogRouter); // => route prefix that starts with /api/v1/blogs

app.listen(8000, () => {
  console.log('Server is listening on port 8000');
});
```

>**Note:** /api/v1 is a common route prefix that we use to indicate that we are using version 1 of our API. 