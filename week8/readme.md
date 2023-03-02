# Week 8

## Table of Contents
  - link 1
  - link 2
  - link 3
  - link 4

## Objectives




### Authentication

Authentication is the verification of who you are. For example, let's say you've gone to a concert. 
At the front door, the security guard asks to see your ticket and ID in order to verify that the name on your ID matches the name on your ticket. 
This is an example of authentication.

Authentication relies on one or more factors to verify identity, and these factors come in three main types:

 - Knowledge is something you know, like a username and password.
 - Possession is something you have, like a security card or mobile device
 - Inherence is something you are, which generally refers to biometric data such as fingerprints.

Authentication that relies on a single factor, such as a simple username/password combo, is called Single-Factor Authentication, and is becoming increasingly insecure.

Authentication that requires multiple factors, such as a username/password combo and a code sent to a mobile device, is known as Multi-Factor Authentication. 
This is distinct from Multi-Step authentication, which requires multiple types of authentication within a single factor, such as a password and a PIN.

Authentication has been around long before computers, in forms such as simple passphrases, but the widespread adoption of computers caused a dramatic increase in the need for reliable, secure forms of authentication. 
In a relatively short span of time, we went from passwords and printed ID cards to complex systems relying on multiple factors of authentication (some of which you might not even realize are in use).

**Basic Authentication**
Authentication is about proving identity through one or more factors, but how does the proof actually work? Most methods for authentication use the concept of challenges and responses. 
This pattern can be found everywhere you look, from password-based authentication on websites to verification codes sent to your phone to physical locks that require a key or combination to unlock.

Responses to authentication prompts can be categorized into:

 - Knowledge-Based: "Something You Know"
 - Possession-Based: "Something You Have"
 - Inherence-Based: "Something You Are"

**Single Sign-On & OAth2**

The most recent advancement in authentication is Single Sign-On, also known as SSO. If you've ever used the 'Sign in with Google/Facebook/Etc' buttons on websites, you've used SSO. In fact, Codecademy lets you use SSO to log in.

With SSO, you authenticate with one service yourself, then use that service to authenticate with other services, but those services never actually have access to the secrets you use to authenticate. This makes it just as secure, if not more, than creating a password for the website itself.

The current standard for SSO is OAuth 2.0, the protocol that powers the 'Sign in with…' buttons.

##Sessions & Cookies vs LocalStorage

HTTP(S) protocol on its own is stateless, meaning requests and responses are just relaying information back and forth with no knowledge of a specific user.

But web developers want to create engaging, personalized experiences for users. This means there needs to be a system that associates the requests with a specific user and does so in a secure way. This is where sessions come in!

A web session refers to a series of user interactions over a time frame. Session data is stored server-side and associated with a session ID.

Think of a session as short-term memory for a web application. 


It's a bit clunky for the client to remember to tack the session ID onto every request. Because of this, the session ID is often kept client-side in the form of session cookies. Cookies are tiny pieces of data — text files of max 4kb — the browser stores that are automatically sent with HTTP requests to a web application. 
Cookies are set by the HTTP response header in key-value pairs:

```
Set-Cookie: Key=Value
```

A session cookie is set with the first HTTP response from the server and persists until the browser is closed or the cookie expires. They look like this in the HTTP header:

```
Set-Cookie: sessionID=34jgL79b
```

This is roughly how a session is implemented with cookies:

1. A user goes to a site. The web server creates a session and a session ID.
2. In the server's response, it tells the browser to store a cookie with the session ID (should not include any personal information).
3. The session ID cookie automatically attaches to each subsequent HTTP request to the server.
4. When the server reads the session ID cookie sent with the next HTTP request, it returns the session data associated with the ID.
5. The process continues as long as the session is active.
6. The session and session ID cookie expires after a user closes out the browser, logs out, or a predetermined session length (i.e. an hour) passes.

Reading cookie data can involve some tedious syntax and relying on cookies to be attached for each HTTP request can affect a website's performance. Cookies are also quite limited in storage. Cookies were the only option for storing miscellaneous data outside of the database until HTML5 came around with localStorage and sessionStorage.

localStorage is a newer form of client-side storage. These browser files also store data as key-value pairs, and web applications can choose to store up to 5MB of data in localStorage. localStorage does not interact with the server, but is instead accessed and modified by simple client-side JavaScript code. localStorage will persist even after a user exits the browser, and will continue to persist until the browser cache is deleted.

sessionStorage, which uses the same syntax as localStorage, can hold session data. This storage clears once the browser closes, so, for many use cases, this is more secure.

localStorage/sessionStorage security depends on the general security of your web application's code against common web attacks. 

More On Session Security
Users and web developers should be concerned with session hijacking, an attack in which an attacker steals session identifiers and gains access to the web server as a different person.

Why hijack a session? If a user is in an authenticated session on their bank's website, the attacker could transfer funds from a user's bank account. Another scenario is an attacker could hijack the session of an authorized admin on an organization's website and steal data.

Below, we will introduce a couple of secure practices for implementing sessions that prevents hijacking attack

Define Session Expiry
The shorter a session is, the less time an attacker has to hijack a session. This is usually done by setting an expiry on the session cookie. It’s also important to implement an automatic session expiration on the backend.

A timeout dictates how long a session can stay open. The session timeout after an idle period is a common feature on bank websites! Other environments that require high security even implement an absolute timeout where a user’s session ends regardless of activity.

Make Session IDs Difficult to Hack
Session IDs are just like passwords — the longer and more random, the better. According to OWASP, session identifiers should be at least 128 bits long. This helps prevent brute-force attacks where a hacker uses multiple bots to guess IDs.

In order to make the session ID random, ensure it does not contain personally identifying information and that the algorithm to generate an ID doesn’t follow a predefined pattern that makes it easier to guess.

Securing Cookies
Session cookies can be made more secure if they expire. This decreases the timeframe where an attacker could steal the session identifier.

You should also use the Secure, HttpOnly, and SameSite attributes in the Set-Cookie HTTP header.

### HTTP Security Headers

HTTP responses can contain headers with extra information that tells the client (browser) how to behave. Security-related headers are added in server-to-client responses to reflect a policy that the website wants to implement, like enforcing HTTPS communications over HTTP, or limiting whether a browser is allowed to render the current webpage in an iframe on another site.

Security headers help protect both the web application and the user. They can help prevent web attacks like Cross-Site Scripting (XSS). It’s important to stay updated about these headers and configure them server-side.

Having security headers configured well also increases a website’s trustworthiness, which in turn makes it rank higher in web searches (SEO).

### Introduction to Sessions in Express

Whether you are browsing through an online shop or accessing your bank account online, your user data persisted across page loads thanks to sessions. Without them, every time you reloaded the window you would be logged out or lose your cart!

### Installing express-session

In order to implement sessions within an Express application, we can use the NPM module, express-session, as a middleware.

Once the session middleware is implemented, each user that navigates to our app will have a unique session generated for them. This allows us to store their session data server-side under a session identifier and easily retrieve it.

```
npm install express-session
```

From here, we import the session module and store it in a variable:

```const session = require("express-session")```

express-session Configuration
Now that we have express-session installed, we can configure the middleware and implement it in app.js. Let’s explore a few of the options we can configure:

secret: The secret property is a key used for signing and/or encrypting cookies in order to protect our session ID.
The next two properties determine how often the session object will be saved.

resave: Setting this option to true will force a session to be saved back to the session data store, even when no data was modified. Typically, this option should be false, but also depends on your session storage strategy.

saveUninitialized: This property is a boolean value. If it’s set to true, the server will store every new session, even if there are no changes to the session object. This might be useful if we want to keep track of recurring visits from the same browser, but overall, setting this property to false allows us to save memory spa

```
app.use(
  session({
    secret: "D53gxl41G",
    resave: false,
    saveUninitialized: false,
  })
);
```

Note that we are using a hardcoded string of characters for the secret property. Usually, this random string should be stored securely in an environment variable, not in the code.

The resave and saveUninitialized properties are set to false in order to avoid saving or storing unmodified sessions. With those options put in place, we have the most basic setup of our middleware!

Storing Session Data
Now that the middleware will create sessions, let’s configure how the middleware saves session data.

Sessions are typically stored in three different ways:

In memory (this is the default storage)
In a database like MongoDB or MySQL
A memory cache like Redis or Memcached
Whenever a user makes a request from the same client with a valid session identifier, the server retrieves the valid session information.

express-session provides an in-memory store called, MemoryStore(). If no other store is specified, then this is set as the default storage. Let’s explore how we would add this to the middleware.

We can instantiate a new store like so:

const store = new session.MemoryStore();
Once instantiated, we can add it in the configuration of our session:

app.use(
  session({
    secret: "secret-key",
    resave: false,
    saveUninitialized: false,
    store,
  })
);

### JSON Web Tokens

JSON Web Tokens are self-contained JSON objects that compactly and securely transmit information between two parties. They are secure because they are digitally signed using a secret or a public/private key pair.

As a reminder, JSON, or JavaScript Object Notation, is essentially a slightly stricter version of a Javascript object. A JSON object should be enclosed in curly braces and may contain one or more key-value pairs.

Components of a JWT
A JWT is made up of three components:

Header
Payload
Signature

JWT Header
A JWT header contains the type of the token we’re creating and the signing algorithm that will be used.

Type: The type of this token will always be “JWT”. The Internet Assigned Numbers Authority, or IANA, coordinates internet protocol resources across the globe. The “JWT” type aligns with the media type “application/jwt“.

Algorithm: The signing, or hashing, algorithm used might vary. Some commonly used algorithms are HMAC-SHA256, represented by "HS256", RSA with SHA-256, represented by "RW256", and ECDSA with SHA-256, represented by "ES256".

Here's an example of a header specifying the HMAC-SHA256 algorithm:

```
{
  'alg': 'HS256',  
  'typ': 'JWT'
}
```

JWT Payload
A JWT payload contains claims about an entity. A claim is a statement or piece of information and the entity is often a user.

There are three types of claims a JWT payload can contain:

Registered Claims: These are predefined claim types that anyone can use in a JWT.
Public Claims: These are custom claim types that are created by a developer and can be used publicly. They should be registered to avoid collisions, also known as repeated claims.
Private Claims: These are custom claim types that are not registered or public. They are only used between parties that have agreed to use them.
Here’s an example payload using some common registered claims:


```
{
    'sub': '1234567890',
    'name': 'Harine Cooper',
    'admin': false,
    'iat': 1620924478,
    'exp': 1620939187
   }
```

JWT Signature
A JWT signature is used to verify that the JWT wasn't tampered with or changed. It can be created taking the encoded header, the encoded payload, a secret, and using the hashing algorithm to create a hash from those elements.

The secret is a symmetric key known by the sender and receiver of this token.

In this example, we will use jwt.io's JWT debugger to create our final JWT.

Improperly Storing a JWT

Do not store your JWT in localStorage as an attacker could use Cross-Site Scripting attacks to steal local data.

Storing your JWT in a cookie may seem like a solution to this, but could expose your data to a Cross-Site Resource Forgery attack. Additionally, if a user has disabled cookies in their browser, the application is now unable to store the JWT.

Why Use JWTs?
JWTs are used for:

Authorization: They’re often used for SSO.
Information Exchange: If a server received a valid JWT, it knows the sender is who they say they are and the information hasn’t been tampered with.
So, why use JWTs?

Parsing JSON is easier than some alternatives like XML or SAML.
JWTs are small, scale well, and are easier for mobile devices to process.
Why are some reasons we might not want to a JWT?

A mix of a public and private key-pair adds security, but can also add complexity.
Sensitive information, like passwords or Social Security Numbers, should not be stored client-side, even if it is encoded.

OAuth
OAuth is an authorization framework that provides specific authorization flows which allow unrelated servers to access authenticated resources without sharing any passwords. It works by allowing applications to authenticate with third-party services in exchange for an access token which can be passed with an HTTP request to access protected content.

In this lesson, we will learn how to implement OAuth 2.0, which is a rewrite of OAuth 1.0 that simplifies the process and introduces the following four OAuth Roles:

Resource Owner: the user who authorizes an application to an account
Resource Server: the API server that accepts access tokens and verifies their validity
Authorization Server: the server that issues access tokens
Client: the application that requests the access tokens

