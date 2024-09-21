### What Are Status Codes, and How Are They Used in API Responses (e.g., 200, 404, 500)?

**Status codes** are part of the HTTP protocol and are used in API responses to indicate the result of a client’s request. They provide a standardized way for the server to inform the client about the outcome of their request—whether it was successful, caused an error, or needs further action. Status codes are three-digit numbers, and each falls into one of five categories based on its first digit.

### Categories of HTTP Status Codes

1. **1xx Informational**: Indicates that the request has been received and understood. These codes are rarely used in modern APIs.
   - **Example**: `100 Continue`—The client should continue with its request.

2. **2xx Success**: Indicates that the request was successfully received, understood, and processed.
   - **Example**: `200 OK`—The request was successful, and the server has returned the requested data.
   
3. **3xx Redirection**: Indicates that further action is needed from the client, often involving redirection to another resource.
   - **Example**: `301 Moved Permanently`—The resource has been moved to a new URL.

4. **4xx Client Errors**: Indicates that there was an issue with the client’s request, such as a bad syntax or invalid input.
   - **Example**: `400 Bad Request`—The request was invalid or cannot be processed by the server.
   - **Example**: `404 Not Found`—The requested resource could not be found.

5. **5xx Server Errors**: Indicates that there was an issue on the server, such as an internal error or service unavailability.
   - **Example**: `500 Internal Server Error`—An unexpected condition was encountered, and the server could not fulfill the request.

### Common HTTP Status Codes in API Responses

Here are some of the most frequently encountered status codes when working with APIs:

#### 1. **200 OK**
   - **Meaning**: The request was successful, and the server returned the requested data.
   - **Use in API**: This is the most common status code for successful GET requests, where data is fetched from the server. It can also be used for successful POST requests that result in data creation.
   - **Example**:
     ```json
     {
       "status": 200,
       "message": "Request successful",
       "data": {
         "id": 123,
         "name": "Product A"
       }
     }
     ```

#### 2. **201 Created**
   - **Meaning**: The request was successful, and a new resource has been created as a result.
   - **Use in API**: Typically used for POST requests when a new resource (e.g., user, product, etc.) is successfully created.
   - **Example**:
     ```json
     {
       "status": 201,
       "message": "Resource created successfully",
       "data": {
         "id": 123,
         "name": "New Product"
       }
     }
     ```

#### 3. **204 No Content**
   - **Meaning**: The request was successful, but there is no content to return.
   - **Use in API**: Common for DELETE requests when a resource has been successfully deleted or when an update operation doesn’t require a response body.
   - **Example**:
     ```json
     {
       "status": 204,
       "message": "Resource deleted"
     }
     ```

#### 4. **400 Bad Request**
   - **Meaning**: The request was malformed or contains invalid data.
   - **Use in API**: Used when the server cannot process the request due to a client error, such as invalid syntax or missing required parameters.
   - **Example**:
     ```json
     {
       "status": 400,
       "error": "Invalid input",
       "message": "The 'email' field is required."
     }
     ```

#### 5. **401 Unauthorized**
   - **Meaning**: The client must authenticate itself to get the requested response.
   - **Use in API**: Typically used when the request requires authentication, but the client has failed to provide valid credentials.
   - **Example**:
     ```json
     {
       "status": 401,
       "error": "Unauthorized",
       "message": "Authentication is required."
     }
     ```

#### 6. **403 Forbidden**
   - **Meaning**: The server understands the request, but it refuses to authorize it.
   - **Use in API**: Used when the client does not have permission to access the requested resource, even if authenticated.
   - **Example**:
     ```json
     {
       "status": 403,
       "error": "Forbidden",
       "message": "You do not have permission to access this resource."
     }
     ```

#### 7. **404 Not Found**
   - **Meaning**: The requested resource could not be found on the server.
   - **Use in API**: Used when the client requests a resource that does not exist or is not available at the specified endpoint.
   - **Example**:
     ```json
     {
       "status": 404,
       "error": "Not Found",
       "message": "The requested resource was not found."
     }
     ```

#### 8. **405 Method Not Allowed**
   - **Meaning**: The request method (e.g., GET, POST) is not supported for the requested resource.
   - **Use in API**: Used when the client tries to perform an operation (such as a POST request) that is not allowed for that endpoint.
   - **Example**:
     ```json
     {
       "status": 405,
       "error": "Method Not Allowed",
       "message": "POST requests are not allowed for this resource."
     }
     ```

#### 9. **409 Conflict**
   - **Meaning**: The request conflicts with the current state of the resource.
   - **Use in API**: Typically used when there is a versioning conflict, such as trying to update a resource that has been modified by another process.
   - **Example**:
     ```json
     {
       "status": 409,
       "error": "Conflict",
       "message": "The resource has already been modified."
     }
     ```

#### 10. **500 Internal Server Error**
   - **Meaning**: The server encountered an unexpected condition that prevented it from fulfilling the request.
   - **Use in API**: Used when something goes wrong on the server, such as a bug or an unhandled exception.
   - **Example**:
     ```json
     {
       "status": 500,
       "error": "Internal Server Error",
       "message": "An unexpected error occurred on the server."
     }
     ```

#### 11. **502 Bad Gateway**
   - **Meaning**: The server, while acting as a gateway or proxy, received an invalid response from the upstream server.
   - **Use in API**: Typically used when a server is down or unreachable.
   - **Example**:
     ```json
     {
       "status": 502,
       "error": "Bad Gateway",
       "message": "The server received an invalid response from the upstream server."
     }
     ```

#### 12. **503 Service Unavailable**
   - **Meaning**: The server is currently unavailable (e.g., overloaded or under maintenance).
   - **Use in API**: Used when the server cannot process requests, typically during downtime or overload.
   - **Example**:
     ```json
     {
       "status": 503,
       "error": "Service Unavailable",
       "message": "The server is temporarily unavailable."
     }
     ```

### How Status Codes Are Used in API Responses

In RESTful APIs, status codes are returned along with a response body, providing a standardized way for the server to inform the client about the outcome of their request. The status code tells the client whether the request was successful, whether there was a client-side error, or whether the server encountered an issue while processing the request.

#### Example API Response

For a successful GET request:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```

For an invalid POST request with missing parameters:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "status": 400,
  "error": "Bad Request",
  "message": "The 'name' field is required."
}
```

### Conclusion

Status codes are vital in APIs as they provide clear feedback to clients about the outcome of their requests. Using the appropriate status codes helps ensure that clients can handle different situations appropriately, such as retrying requests, showing error messages, or confirming success. By adhering to standard HTTP status codes, API developers can maintain consistent and predictable communication between servers and clients.