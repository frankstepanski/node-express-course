# Week 4

## Request/Response Cycle, REST Methods, Postman and MVC

### Request and Response Cycle

Let's review how APIs work and look more at the [request and response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#http_flow) cycle.


**HTTP Request**

A HTTP request is made up of a few parts:
  - The HTTP method - GET, POST, PUT, DELETE
  - The URL
  - The HTTP version 
  - The request headers
  - The request body 

  ![request](images/request.png)



**HTTP Response**

A HTTP response is made up of a few parts:
  - The HTTP version
  - The status code
  - The status message
  - The response headers
  - The response body

  ![response](images/response.png)




### Postman



### Error Handling Middleware

When your app is in error mode, all regular middleware is ignored and Express will execute only error-handling middleware functions. 
To enter error mode, simply call next with an argument. It's convention to call it with an error object, as in next(new Error ("Something bad happened!")).

These middleware functions take four arguments instead of two or three. The first one is the error (the argument passed into next), 
and the remainder are the three from before: req, res, and next. You can do anything you want in this middleware. 
When you're done, it's just like other middleware: you can call res.end or next. 

Calling next with no arguments will exit error mode and move onto the next normal middleware; calling it with an argument will continue onto the next error-handling middleware if one exists.