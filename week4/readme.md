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

## RESTful APIs

When an web API follows the [constraints of REST](https://restfulapi.net/rest-architectural-constraints/), the API called a RESTful API. :sleeping:

A RESTful API is all about URLs, which provide access to resources such as HTML, CSS, image, JSON, that are returned by a request.

There are [design principles](https://apiguide.readthedocs.io/en/latest/build_and_publish/use_RESTful_urls.html) that each RESTful API usually follows: 

A few of the most important principles are:
  - Use nouns to identify resources (e.g. /todos)
  - Use plural nouns to identify collections of resources (e.g. /todos)
  - Use HTTP methods to specify what to do with a resource (e.g. GET /todos, POST /todos, PUT /todos/:id, DELETE /todos/:id)
  - Use route parameters to specify which resource to access (e.g. /todos/:id)
  - Use query parameters to filter resources (e.g. /todos?completed=true)
  - Use HTTP body to specify the data to send to the server (e.g. JSON, XML, etc.)


##  HTTP Request Methods

Up to this point, we have only used the GET method to retrieve data from a server.

But there are other HTTP methods that we can use to interact with resources on a server.

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

>Let's answer some common questions to help you put together a mental model of these concepts.

**What is a resource?**

A resource is an object or representation of something. For example, a todo is a resource. A todo can have an id, a description and a completed property (among other things). 
These are all properties of the todo resource. 

**How do I access a resource?**

We can use a route to access a resource. A route is a path that we can use to access a resource.

**How does a route specify what to do with a resource?**

We use an HTTP method to specify what to do with a resource, such as GET, POST, PUT, and DELETE.
An HTTP method combined with a route specifies what to do with a resource.
This is called an endpoint.

**So, what is an endpoint exactly?**

An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

**What is the difference between a route and an endpoint?**

A route is a path that we can use to access a resource. An endpoint is a combination of an HTTP method and a route. An endpoint specifies what to do with a resource.

**How does a route know which resource to access?**

We can use a route parameter to tell the server which resource to access. A route parameter is a variable that we can use to access a resource.

**How do we know which HTTP method to use?**

**What is a CRUD operation?**

CRUD stands for Create, Read, Update, and Delete. These are the four basic operations that we can perform on a resource.
This is also known as the CRUD principle.

**What is the difference between a CRUD operation and an HTTP method?**

A CRUD operation is a basic operation that we can perform on a resource. An HTTP method is a method that we can use to perform a CRUD operation on a resource.
It is important to note that not all CRUD operations have an HTTP method associated with them.

CRUD operations come from the world of databases. HTTP methods come from the world of the web.
We can use HTTP methods to perform CRUD operations on a resource.

**What is the difference between a CRUD operation and an HTTP method?**

A CRUD operation is a basic operation that we can perform on a resource. An HTTP method is a method that we can use to perform a CRUD operation on a resource.




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