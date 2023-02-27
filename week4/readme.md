# Week 4

##  HTTP Request/Response Object, REST Details, Postman and MVC

### HTTP Request Object

When an end user navigates to a URL, the browser automatically generates an [HTTP Request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http_requests) for a resource. 

The first line of the request is the [request line](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#start_line) which contains three parts: the request method, the request URI, and the HTTP version.
  - The request method: tells the server what kind of request is being made.
  - The request URI: tells the server what resource is being requested.
  - The HTTP version: tells the server what version of HTTP is being used.

The request line is followed by a series of request headers which contain additional information about the request.

The request headers are followed by an empty line, which is used to separate the request headers from the request body.

The request body contains the data that is being sent to the server. The request body is optional. If the request method is GET, then the request body is ignored. If the request method is POST, then the request body is required.

![http request](images/http-request.png)

### HTTP Response Object

After a web server receives an HTTP request, it will process and respond to the request. 

The web server first responds with the protocol version in use, the status code and status message. Like HTTP request headers, there are HTTP response headers. 
HTTP response headers often provide the browser with instructions for handling the response and security requirements. 

The response body contains the data that is being sent to the client. The response body is optional. If the response status code is 204, then the response body is ignored. If the response status code is 200, then the response body is required.

![http response](images/http-response.png)


### REST APIs

The most common type of  API you'll come across on the modern web, is the RESTful API. 
REST stands for Representational State Transfer, and refers to an architectural style that guides how in terms of how you should create APIs



### Postman




Every API request needs to include some headers. 
Headers include some of the background information that help the server have some information about the client that is sending the request. 
Sometimes, we will need to modify or add certain headers in order to get an API to do what we want, but often we can just let the tool that we are using send the default headers that it needs to send without worrying about it.

If you want to create or modify resources with an API, you will need to give the server some information about what kind of properties you want the resource to have. This kind of information is usually specified in the body of a request.


In Postman, you can see what headers will be sent with your request by using the Headers tab. You can also modify the headers and add additional ones here as needed. I will get into more details on how headers work and how to use them in future chapters, but for now, you don't need to worry about them too much. The point of mentioning them here is just to make sure you know the terminology. Let's turn our attention instead to the body of an API request.



### Error Handling Middleware

When your app is in error mode, all regular middleware is ignored and Express will execute only error-handling middleware functions. 
To enter error mode, simply call next with an argument. It's convention to call it with an error object, as in next(new Error ("Something bad happened!")).

These middleware functions take four arguments instead of two or three. The first one is the error (the argument passed into next), 
and the remainder are the three from before: req, res, and next. You can do anything you want in this middleware. 
When you're done, it's just like other middleware: you can call res.end or next. 

Calling next with no arguments will exit error mode and move onto the next normal middleware; calling it with an argument will continue onto the next error-handling middleware if one exists.