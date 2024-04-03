---
creation_date: 2024年03月29日
banner: "![[daily-note-banner.gif]]"
banner_icon: "🌞"
tags: "#笔记"
banner_y: 0.4705
---

# Nodejs

## Project Structure and Architecture
https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs

According to https://www.youtube.com/watch?v=fc6o1gwqZuA
**File Hierarchy for single microserves** (server)
```python 
app/
	app.js
	controllers/
	loaders/
	models/
	routers/        
	schemas/        # define db schema
	services/
```

**File Hierarchy for Monorepos with multiple microserves**(server)
```
app/
	app.js
	microservice_1/
		controllers/
		loaders/
		models/
		routers/        
		schemas/      
		services/
	microservice_2/
```

**3 Layer Approach**
BAD! Not Scalable ... Client -> Routes -> Controllers -> DB
- here the controller contacts the DB, ok for small projects
GOOD! Client -> Controller -> Service -> Model -> DB
- controller is designed to receive and send response, not designed for business logic
- so service should do the business logic, when it needs to make a entry, then it uses model to make entry into DB

**Pub-Sub** using EventEmitter!

**Testing** - unit testing for services and controllers.

Logging - Morgan
Application Monitoring - Sentry, DataDog, AppSignal


Express
- CORS
  Same Origin Policies
  - Cookies vs Local Storage vs Session Storage

**Security Note**: While local storage and session storage are convenient for storing tokens, they're susceptible to XSS (Cross-Site Scripting) attacks. Cookies, when used with the `HttpOnly` flag, can mitigate some of these risks. However, cookies are vulnerable to CSRF (Cross-Site Request Forgery) attacks if not properly protected. It's essential to understand these security implications and implement additional protections as necessary, such as using secure, HttpOnly cookies and implementing anti-CSRF tokens.

Solution?

Ensuring that tokens (such as JWTs) are stored on the client's browser following a server response involves a few steps, both on the server-side and the client-side of your application. Here's how you can make sure the tokens are properly stored and managed:

Sending Tokens from the Server

When your server authenticates a user (for instance, after a successful login), it should send the token back to the client. This can be done in several ways:

- **In the Response Body**: The server can include the token in the body of the response to a successful authentication request.
- **Set-Cookie Header**: For a more secure approach, especially if you want to prevent the JavaScript code from accessing the token directly and thereby reduce the risk of XSS attacks, you can send the token in a cookie by setting the `Set-Cookie` header in your server's response.


**Recommendations**
- **Use Middleware for Authentication:** Since authentication is a generic operation (verifying the identity of a user), it's efficiently handled by middleware. This keeps your application DRY (Don't Repeat Yourself) and secure by ensuring that all requests are authenticated before reaching your business logic.
- **Choose Based on Authorization Complexity:** For authorization, the decision is more nuanced. If your application has straightforward, role-based access control that applies globally, middleware is an elegant solution. However, if authorization rules are complex, context-dependent, and vary significantly across different parts of your application, handling these checks within your service layer might provide the necessary flexibility.
- **Combine Approaches When Necessary:** It's also possible to use a hybrid approach. Use middleware for basic authentication and global authorization checks, and perform more specific, fine-grained authorization logic within your service functions as needed



In a three-layer architecture consisting of controllers, services, and models typically used in Node.js/Express applications, the distribution of error handling (such as `try-catch` blocks) depends on the type of errors you expect and where you can best handle them. Here’s a general guideline on how to approach error handling in each layer:

### 1. **Controller Layer**

The controller layer is responsible for handling incoming HTTP requests and sending responses to the client. It acts as a bridge between the client and your application's business logic (services).

- **Error Handling:** It's common to place `try-catch` blocks in the controller layer to catch any exceptions that occur during the execution of business logic or data access operations. This allows you to send appropriate HTTP responses back to the client, such as `500 Internal Server Error` for unexpected exceptions or more specific errors like `404 Not Found` if a resource doesn’t exist.
- **Responsibility:** Controllers should handle errors related to HTTP request processing and delegate business logic errors to the service layer.

### 2. **Service Layer**

The service layer contains the core business logic of your application. It orchestrates data retrieval and manipulation by communicating with the model layer.

- **Error Handling:** While it's possible to use `try-catch` blocks in services, often the role of error handling here is more about throwing custom errors or exceptions that are specific to your business logic. For example, if a certain operation cannot be completed due to business rules, you might throw a custom error that the controller layer can catch and handle appropriately.
- **Responsibility:** Services should validate business rules and throw exceptions when those rules are violated. They can also catch errors from the model layer (data access errors) and either handle them or rethrow them as more abstract exceptions that the controller can understand and manage.

### 3. **Model Layer**

The model layer is typically responsible for data access and manipulation—communicating with databases, external APIs, or other data sources.

- **Error Handling:** This layer should handle errors related to data access, such as connection errors, query errors, or other data source-specific exceptions. In many cases, it's appropriate to catch these errors, log them, and throw an exception that can be handled by the service layer.
- **Responsibility:** Models should ensure that only valid data access operations are attempted and handle any exceptions related to data integrity, availability, or source communication errors.

### General Advice

- **Centralized Error Handling:** Consider implementing a centralized error handling mechanism in your application. Express, for example, allows you to define error-handling middleware that can catch any unhandled errors from any part of the application. This is useful for logging errors, sending generic error responses, and keeping your error-handling code organized.
- **Custom Error Classes:** Create custom error classes that extend the built-in Error class. This approach lets you add additional properties or methods to your errors, making it easier to handle specific types of errors differently (e.g., distinguishing between a "not found" error and a "validation" error).
- **Async Middleware:** If you're using asynchronous operations in your controllers, make sure to use or write middleware that can catch and forward errors from promise rejections to your error-handing middleware.

Applying `try-catch` strategically across these layers, along with using Express’s error-handling capabilities, can help you manage errors effectively throughout your application. This not only makes your application more robust and easier to debug but also improves the user experience by providing meaningful error messages.

### Authorization
Implementing role-based access control (RBAC) is a crucial aspect of securing your application, ensuring that users can only access resources or perform actions according to their roles. Both strategies you mentioned for integrating RBAC with JWT authentication have their merits and considerations. Let's examine them:

### Strategy 1: Two-Step Process (Token Verification + Role Check)

In this approach, the JWT token is used solely for authentication (i.e., verifying the identity of the user). After authentication, you retrieve the user's details from your database or another data store to check their role and authorize access based on that role.

**Pros:**

- **Flexibility:** You can easily update a user's roles without having to issue a new token. This is useful in systems where roles are changed frequently.
- **Separation of Concerns:** Authentication and authorization are kept as separate steps, which can lead to cleaner, more understandable code.

**Cons:**

- **Performance:** Requires an additional query to your datastore to fetch the user's roles on each request, which could impact performance.
- **Complexity:** Slightly more complex to implement, as you need to handle both the authentication step and the subsequent role retrieval and checking.

### Strategy 2: Combining ID and Role in One Token

In this strategy, the JWT token includes both the identity of the user (e.g., user ID) and their roles. This way, once the token is verified, you already have the user's roles available for authorization decisions without needing to query your database.

**Pros:**

- **Performance:** Eliminates the need for an extra database query to fetch the user's roles, as they are included in the token.
- **Simplicity:** Simplifies the authorization logic since everything needed for both authentication and authorization is contained in the JWT.

**Cons:**

- **Flexibility:** If a user's roles change, you need to issue a new token. This can be problematic in systems where roles change frequently or dynamically.
- **Token Size:** Including additional information in the token increases its size, which might be an issue if the token is sent in every request header, especially for applications with strict bandwidth requirements.

### Recommendation:

The choice between these strategies depends on your specific application requirements:

- **If roles change infrequently** and you prefer simplicity and performance, **embedding the roles in the JWT** might be the way to go. This approach works well for applications with relatively static roles or where role changes can be easily managed by reissuing tokens.
    
- **If roles are dynamic or change frequently**, or if you prefer keeping authentication and authorization strictly separate for design reasons, **using a two-step process** might be more appropriate. This method offers more flexibility and keeps your token size smaller.
    

Regardless of the chosen strategy, ensure that your implementation follows security best practices, such as using HTTPS for all communications, securely storing sensitive information, and handling tokens securely on the client side.







![[Pasted image 20240402221442.png]]

![[Pasted image 20240402221458.png]]
![[Pasted image 20240402223154.png]]