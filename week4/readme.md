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

So example, the URL /todos provides access to a collection of todo resources, while the URL /todo/1 provides access to a single todo resource. 
A collection of resources is also considered one resource.

REST uses various representations of a resource such as JSON, XML, HTML, and plain text. The most common representation is JSON.

The HTTP Protocol represents an [HTTP message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages) as text, it may look like JSON, or even JavaScript, but it is always text.

![http messages](images/http-messages.png)

##  HTTP Request Methods

Up to this point, we have only used the [GET](https://expressjs.com/en/5x/api.html#app.get) method to retrieve data from a server.

But, there's more! :raised_hands:

Other common HTTP methods that we can use to interact with resources on a server would be:

  - [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST): creates a new resource
  - [PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT): update an existing resource
  - [DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE): delete an existing resource

The following table shows the HTTP methods, Express method and [CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD) operations 
that each HTTP method can perform to interact with resources on a server. 

A **CRUD operation** is a basic operation that we can perform on a resource. CRUD stands for Create, Read, Update, and Delete.

| HTTP Method |  Express Method | CRUD Operation | Description                 |
| ----------- |  -------------- | -------------- | --------------------------- |
| GET         |  app.get()      | Read           | Retrieve a resource         |
| POST        |  app.post()     | Create         | Create a new resource       |
| PUT         |  app.put()      | Update         | Update an existing resource |
| DELETE      |  app.delete()   | Delete         | Delete an existing resource |

___

![questions](images/QA.gif)

>Let's take a quick break to review and answer some questions about RESTful APIs and HTTP methods.

**How do we access a resource on a server?**\
We can use a route to access a resource. A route is a path that we can use to access a resource.

**How does a route specify what to do with a resource?**\
We use an HTTP method to specify what to do with a resource, such as GET, POST, PUT, and DELETE.

**What is an endpoint?**\
An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

**What is the difference between a route and an endpoint?**\
A route is a path that we can use to access a resource. An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

```
// route
app.get('/todos', (req, res) => {
  res.send('GET /todos')
})
// endpoint
GET /todos



```

**What is the difference between a CRUD operation and an HTTP method?**\
A CRUD operation is a basic operation that we can perform on a resource. An HTTP method is a method that we can use to perform a CRUD operation on a resource.
CRUD operations come from the world of databases. HTTP methods come from the world of the web.


**Some examples:**

```
// todos resource

const todos = [
  {
    id: 1,
    description: 'Learn Express',
    completed: false
  },
  {
    id: 2,
    description: 'Learn React',
    completed: false
  },
  {
    id: 3,
    description: 'Learn Redux',
    completed: false
  },
  {
    id: 4,
    description: 'Learn Node',
    completed: false
  }
]

// API endpoints:

`GET /todos` => returns all todos
`GET /todos/1` => returns todo with id 1

`POST /todos` => creates a new todo
`POST /todos/1` => creates a new todo with id 1

`PUT /todos` => updates all todos
`PUT /todos/1` => updates todo with id 1

`DELETE /todos` => deletes all todos
`DELETE /todos/1` => deletes todo with id 1

```