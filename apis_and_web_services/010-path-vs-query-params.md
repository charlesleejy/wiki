### Path Parameters and Query Parameters in an API

Path parameters and query parameters are two key methods of passing information to an API endpoint. Understanding the difference between these two types of parameters is essential for effective API design and usage.

### 1. **Path Parameters**

**Definition**: 
Path parameters (also known as route parameters) are part of the **URL path** itself. They are used to identify specific resources in an API, and they form part of the URL structure. Path parameters are typically used when you want to specify a specific resource or a group of resources.

**Example**:
Consider an API that retrieves user details based on the user ID. The path parameter in this case is the `user_id` that is embedded directly in the URL.

```plaintext
GET /users/{user_id}
```

When calling the API, you replace `{user_id}` with an actual value:

```plaintext
GET /users/123
```

In this case, `123` is the path parameter, which tells the API that the request is specifically for the user with the ID `123`.

**Use Cases**:
- Identifying specific resources: `/users/{user_id}` for retrieving a particular user.
- Accessing sub-resources: `/users/{user_id}/orders` to get orders of a specific user.
- Dynamic values in the URL: `/products/{product_id}/reviews` for accessing reviews of a specific product.

**Advantages**:
- Path parameters help maintain clean, readable, and hierarchical URLs.
- Ideal for referring to resources and identifying unique entities.
  
### 2. **Query Parameters**

**Definition**: 
Query parameters are **key-value pairs** that are appended to the **end of the URL**. They are typically used to filter, sort, or provide additional information to the server. Query parameters follow a `?` in the URL and are separated by `&`.

**Example**:
Consider an API that lists all users but allows filtering based on optional criteria like `age` and `status`.

```plaintext
GET /users?age=25&status=active
```

In this example:
- `age=25` is a query parameter that filters the users by age.
- `status=active` filters the users by their status.

You can add as many query parameters as needed to refine the API request.

**Use Cases**:
- Filtering results: `/products?category=electronics&price=low` to filter products by category and price.
- Sorting results: `/users?sort=age&order=desc` to sort users by age in descending order.
- Pagination: `/items?page=2&limit=20` to get the second page of items with 20 items per page.

**Advantages**:
- Flexible and dynamic, allowing you to pass various optional parameters.
- Suitable for filtering, sorting, and paginating resources without altering the URL structure.

### **Key Differences Between Path Parameters and Query Parameters**

| Feature                      | Path Parameters                         | Query Parameters                             |
|------------------------------|-----------------------------------------|---------------------------------------------|
| **Placement**                 | Part of the URL path                    | Appended to the URL after a `?`             |
| **Usage**                     | Used to specify a specific resource or sub-resource | Used to filter, sort, or modify the data returned by the API |
| **Required or Optional**      | Typically required                      | Typically optional, but can be required in some cases |
| **Example URL Structure**     | `/users/{user_id}`                      | `/users?age=25&status=active`               |
| **How to Access**             | Defined within the URL route            | Passed as key-value pairs at the end of the URL |
| **Use Case**                  | Identifying specific resources (e.g., user ID, order ID) | Filtering, sorting, pagination, search queries |
| **Number of Parameters**      | Usually limited to a few                | Can have multiple query parameters in one request |
| **Example**                   | `/users/123/orders`                     | `/users?age=25&status=active&page=2`        |

### **When to Use Path Parameters vs. Query Parameters**

- **Path Parameters**: Use when you are identifying a specific resource or navigating through a hierarchical structure of data. For example:
  - `/users/123` – to fetch a specific user with ID 123.
  - `/projects/42/tasks` – to retrieve tasks related to project 42.

- **Query Parameters**: Use when you need to filter, sort, or modify the data being requested. For example:
  - `/users?age=25&status=active` – to filter users by age and status.
  - `/articles?sort=date&order=desc` – to sort articles by date in descending order.

### **Combining Path and Query Parameters**
In many cases, you can combine both path and query parameters in the same API call. For example:

```plaintext
GET /users/123/orders?status=shipped&page=2&limit=10
```

- Path parameter (`/users/123/orders`) specifies the user and the resource (orders).
- Query parameters (`status=shipped`, `page=2`, `limit=10`) allow filtering of the orders based on status, and pagination control.

### Conclusion

- **Path Parameters**: Identify resources or sub-resources as part of the URL.
- **Query Parameters**: Provide additional filtering, sorting, or modification of the request.
By understanding the use cases for both, you can design more intuitive, efficient, and RESTful APIs that are easier to maintain and use.