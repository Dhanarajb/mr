# JSON Web Tokens (JWT)

## **What is JWT?**

**JSON Web Token (JWT)** is a compact, URL-safe, and self-contained token format used to securely transmit information between two parties (typically a client and a server). JWT is widely used for **authentication** and **authorization** in modern web applications.

A JWT consists of three parts, separated by dots (`.`):

1. **Header**: Contains metadata about the token, such as the signing algorithm (`HS256`, `RS256`).
   ```json
   {
     "alg": "HS256",
     "typ": "JWT"
   }
   ```

2. **Payload**: Contains claims, which are statements about the user (e.g., `id`, `email`, `role`) and additional metadata.
   ```json
   {
     "id": "12345",
     "email": "user@example.com",
     "role": "admin"
   }
   ```

3. **Signature**: A cryptographic signature generated using the **header**, **payload**, and a **secret key** to ensure the token's integrity.

The final token looks like this:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEyMzQ1IiwiZW1haWwiOiJ1c2VyQGV4YW1wbGUuY29tIiwicm9sZSI6ImFkbWluIn0.s5TbytnPhnTIn3G4uwThPKXI-jWOrMN9zlwJfhv7AXw
```

---

## **How JWT is Used for Authentication**

### **Step-by-Step Process:**

1. **User Login**:
   - A user logs in with their credentials (e.g., email and password).
   - The server verifies the credentials and generates a **JWT**, including user details (e.g., `id`, `role`).

2. **Token Issuance**:
   - The server sends the JWT to the client (typically in the response body or as a cookie).

3. **Token Storage**:
   - The client stores the token in a secure location, such as:
     - **LocalStorage** (not recommended for highly sensitive data).
     - **HttpOnly Cookies** (preferred for added security).

4. **Accessing Protected Resources**:
   - For subsequent requests, the client includes the JWT in the `Authorization` header:
     ```http
     Authorization: Bearer <JWT>
     ```
   - The server validates the JWT:
     - Verifies the signature to ensure the token hasn’t been tampered with.
     - Checks the token's expiration time.

5. **Grant or Deny Access**:
   - If the JWT is valid, the server allows access to the requested resource.
   - If the token is invalid (e.g., expired or tampered), the server rejects the request.

---

## **Advantages of JWT**

1. **Stateless Authentication**:
   - JWT eliminates the need for server-side session storage, as all necessary information is embedded in the token.

2. **Compact and Portable**:
   - The token is URL-safe and can easily be transmitted via HTTP headers, cookies, or query parameters.

3. **Self-Contained**:
   - All information required to validate and authorize a user is present within the token itself.

4. **Cross-Platform Compatibility**:
   - JWT works seamlessly across different platforms (e.g., web, mobile, IoT).

---

## **Challenges of JWT**

1. **Token Revocation**:
   - Once issued, JWTs cannot be revoked without additional mechanisms like a **token blacklist**.

2. **Expiration Handling**:
   - If the token has a long expiration time, it poses security risks. If it expires too quickly, frequent reauthentication is required.

3. **Storage Vulnerabilities**:
   - Improper storage (e.g., in localStorage) can expose the token to XSS attacks.

---

## **Example Code (Node.js + Express)**

### **Token Generation**:
```javascript
const jwt = require('jsonwebtoken');

const user = { id: '12345', email: 'user@example.com', role: 'admin' };
const secretKey = 'your_secret_key';
const token = jwt.sign(user, secretKey, { expiresIn: '1h' });

console.log('JWT:', token);
```

### **Token Verification**:
```javascript
const express = require('express');
const jwt = require('jsonwebtoken');

const app = express();
const secretKey = 'your_secret_key';

app.get('/protected', (req, res) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).send('Token missing');

  try {
    const decoded = jwt.verify(token, secretKey);
    res.send(`Welcome ${decoded.email}`);
  } catch (err) {
    res.status(401).send('Invalid token');
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

## **Conclusion**
JWT is a powerful tool for stateless authentication and authorization in modern web applications. Its compact and self-contained nature makes it ideal for securing APIs and managing user sessions, but it requires careful handling to mitigate risks such as token revocation and storage vulnerabilities.
