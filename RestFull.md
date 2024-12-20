# Cross Questions for RESTful API

## General REST Questions

1. **What is a RESTful API, and how is it different from other APIs?**  
   A RESTful API adheres to REST principles, such as client-server architecture, statelessness, a uniform interface, and resource-based design. Unlike SOAP, which is protocol-heavy, REST is lightweight and relies on standard HTTP methods.

2. **What are HTTP methods, and how do they map to CRUD operations?**  
   - **GET**: Read a resource.  
   - **POST**: Create a resource.  
   - **PUT/PATCH**: Update a resource.  
   - **DELETE**: Remove a resource.

---

## Design and Best Practices

3. **How do you design resource URIs in RESTful APIs?**  
   - Use nouns, not verbs, for resources: `/users`, `/products`.  
   - Use hierarchy for related resources: `/users/{id}/orders`.  
   - Avoid actions in URIs: `/createUser` is bad; use `POST /users` instead.

4. **How do you handle versioning in a REST API?**  
   - Use URI versioning: `/api/v1/resource`.  
   - Use query parameters: `/resource?version=1`.  
   - Use headers: `Accept-Version: v1`.

5. **What is the purpose of HTTP status codes, and can you name some commonly used ones?**  
   - **200 OK**: Request successful.  
   - **201 Created**: Resource created.  
   - **204 No Content**: Resource deleted.  
   - **400 Bad Request**: Invalid request.  
   - **401 Unauthorized**: Authentication required.  
   - **404 Not Found**: Resource not found.  
   - **500 Internal Server Error**: Server-side issue.

6. **How do you manage filtering, sorting, and pagination in APIs?**  
   - Filtering: `/products?category=electronics`.  
   - Sorting: `/products?sort=price&order=asc`.  
   - Pagination: `/products?page=2&limit=10`.

---

## Practical Implementation

7. **How do you secure a RESTful API?**  
   - Use HTTPS for encryption.  
   - Implement authentication using tokens (e.g., JWT, OAuth).  
   - Use role-based authorization.  
   - Validate input to prevent injection attacks.  
   - Avoid exposing sensitive data in responses.

8. **What is statelessness in REST, and why is it important?**  
   Statelessness means the server does not store session-related data about the client. Each request must contain all necessary information (e.g., authentication token). This improves scalability and simplifies server design.

9. **How do you handle errors in a RESTful API?**  
   Return appropriate HTTP status codes and descriptive error messages in JSON format:
   ```json
   {
     "error": "Invalid input data",
     "status": 400
   }
   ```

10. **How do you test a RESTful API?**  
    - Use tools like Postman, cURL, or automated testing frameworks (e.g., Jest for Node.js, Pytest for Python).  
    - Write test cases for all endpoints covering positive, negative, and edge cases.

---

## Performance and Scalability

11. **How can you optimize a REST API for better performance?**  
    - Use caching with HTTP headers (`Cache-Control`, `ETag`).  
    - Implement pagination for large datasets.  
    - Optimize database queries and use indexing.  
    - Use a Content Delivery Network (CDN) for static assets.  
    - Compress responses (e.g., Gzip).

12. **What is the role of caching in a REST API?**  
    Caching reduces server load and speeds up responses. HTTP headers like `Cache-Control`, `Expires`, and `ETag` help manage caching behavior.

13. **How would you handle a scenario where multiple clients update the same resource simultaneously?**  
    - Use optimistic concurrency control with versioning.  
    - Example: Include a `version` field in the resource and reject updates if the version has changed.

---

## Debugging and Troubleshooting

14. **What would you do if the API is returning 500 Internal Server Errors?**  
    - Check server logs for detailed error messages.  
    - Debug the code to identify issues like unhandled exceptions.  
    - Validate input data and dependencies (e.g., database connections).

15. **What steps would you take if your API is slow?**  
    - Profile the API to find bottlenecks.  
    - Optimize database queries.  
    - Add caching and load balancing.  
    - Implement asynchronous processing for non-critical tasks.

---

## Advanced Scenarios

16. **What is HATEOAS, and how does it relate to REST?**  
    HATEOAS (Hypermedia as the Engine of Application State) means the API response includes links to related actions:
    ```json
    {
      "id": 1,
      "name": "John Doe",
      "links": [
        { "rel": "self", "href": "/users/1" },
        { "rel": "orders", "href": "/users/1/orders" }
      ]
    }
    ```

17. **What’s the difference between PUT and PATCH?**  
    - **PUT**: Updates the entire resource.  
    - **PATCH**: Partially updates the resource.

18. **How would you implement rate limiting in an API?**  
    - Use tools like **Redis** or libraries like **express-rate-limit** to restrict the number of requests a client can make in a given time frame.

19. **How do you handle long-running processes in a REST API?**  
    - Use asynchronous APIs and return a status URL to the client for checking progress.  
    - Example:  
      - Client starts a process: `POST /process`.  
      - Server responds with a URL: `/process/123/status`.  
      - Client checks progress via: `GET /process/123/status`.

---

## Scenario-Based Questions

20. **Suppose you have an e-commerce API. How would you design endpoints for products and orders?**  
    - Products:  
      - `GET /products`: Retrieve all products.  
      - `GET /products/{id}`: Retrieve a single product by ID.  
      - `POST /products`: Create a new product.  
      - `PUT /products/{id}`: Update a product.  
      - `DELETE /products/{id}`: Delete a product.  
    - Orders:  
      - `GET /orders`: Retrieve all orders.  
      - `POST /orders`: Place a new order.  
      - `GET /orders/{id}`: Retrieve details of a specific order.

21. **How would you implement file uploads in a REST API?**  
    - Use multipart form-data:  
      - Endpoint: `POST /upload`.  
      - Use middleware like `multer` (Node.js) to handle file uploads.

22. **What would you do if clients are complaining about inconsistent data in your API?**  
    - Check for race conditions and implement concurrency control.  
    - Validate data integrity in the database.  
    - Use eventual consistency patterns if applicable.
