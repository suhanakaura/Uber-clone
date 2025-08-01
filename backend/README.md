# API Endpoint Documentation

---

## /users/register Endpoint

**Description:**  
Registers a new user by accepting their details, hashing the password, and creating the user in the database. On success, the endpoint returns an authentication token along with the created user object.

**URL:**  
`POST /users/register`

**Request Body:**  
The request body should be in JSON format and must include the following fields:

- **fullname** (object):  
  - **firstname** (string): User's first name (minimum 3 characters).  
  - **lastname** (string): User's last name (optional, minimum 3 characters if provided).
- **email** (string): A valid email address.
- **password** (string): Password (minimum 6 characters).

Example:
```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "secret123"
}
```

**Responses:**

- **201 Created:**  
  On successful registration, returns a JSON object containing:
  - `token`: A JWT token for authentication.
  - `user`: The created user object.

  Example:
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "60d0fe4f5311236168a109ca",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

- **400 Bad Request:**  
  If the input validation fails, returns an error detailing the validation issues.

  Example:
  ```json
  {
    "errors": [
      {
        "msg": "firstname must be 3 characters long or more",
        "param": "fullname.firstname",
        "location": "body"
      }
    ]
  }
  ```

- **500 Internal Server Error:**  
  For unexpected server errors.

---

## /users/login Endpoint

**Description:**  
Logs in an existing user. The endpoint verifies the provided email and password. If the credentials are valid, it returns an authentication token and the user object; otherwise, it returns an error message.

**URL:**  
`POST /users/login`

**Request Body:**  
The request should be in JSON format with the following fields:

- **email** (string): A valid email address.
- **password** (string): User's password (minimum 6 characters).

Example:
```json
{
  "email": "john.doe@example.com",
  "password": "secret123"
}
```

**Responses:**

- **200 OK:**  
  On successful login, returns a JSON object containing:
  - `token`: A JWT token for authentication.
  - `user`: The user object.

  Example:
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "60d0fe4f5311236168a109ca",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

- **400 Bad Request:**  
  If input validation fails, returns details about the validation errors.

  Example:
  ```json
  {
    "errors": [
      {
        "msg": "Password must be atleast 6 characters long",
        "param": "password",
        "location": "body"
      }
    ]
  }
  ```

- **401 Unauthorized:**  
  Returned when the email does not exist or the password is incorrect.

  Example:
  ```json
  {
    "message": "Invalid email or password"
  }
  ```

- **500 Internal Server Error:**  
  For unexpected server errors.

---

## /users/profile Endpoint

**Description:**  
Returns the profile of the authenticated user.

**URL:**  
`GET /users/profile`

**Headers:**  
- A valid JWT token must be provided either in the `token` cookie or in the Authorization header as `Bearer <token>`.

**Responses:**  

- **200 OK:**  
  Returns the authenticated user's data in JSON format.

  Example:
  ```json
  {
    "_id": "60d0fe4f5311236168a109ca",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com",
    "socketId": "optional_socket_id"
  }
  ```

- **401 Unauthorized:**  
  If the token is missing, invalid, or blacklisted.
  ```json
  {
    "message": "unauthorized user"
  }
  ```

---

## /users/logout Endpoint

**Description:**  
Logs out the authenticated user by clearing the authentication token cookie and adding the token to a blacklist.

**URL:**  
`GET /users/logout`

**Headers:**  
- A valid JWT token must be provided either in the `token` cookie or in the Authorization header as `Bearer <token>`.

**Responses:**  

- **200 OK:**  
  Returns a confirmation message once the user is logged out successfully.

  Example:
  ```json
  {
    "message": "logged out"
  }
  ```

---

**Note:**  
Ensure that your server has the required environment variables configured, such as `JWT_SECRET` for token generation.

# Captain Endpoint Documentation

---

## /captains/register Endpoint

**Description:**  
Registers a new captain by accepting their personal details, vehicle details, and password. The endpoint validates the request, hashes the password, and creates a new captain entry in the database. On success, it returns an authentication token along with the captain's data.

**URL:**  
`POST /captains/register`

**Request Body:**  
The request body should be in JSON format and must include the following fields:

- **fullname** (object):  
  - **firstname** (string): The captain's first name (minimum 3 characters required).  
  - **lastname** (string): The captain's last name (optional).
- **email** (string): A valid email address.
- **password** (string): The captain's password (minimum 6 characters required).
- **vehicle** (object):  
  - **color** (string): The color of the vehicle (minimum 3 characters required).
  - **plate** (string): The vehicle's plate number (minimum 3 characters required).
  - **capacity** (number): The seating or load capacity of the vehicle (minimum value: 1).
  - **vehicleType** (string): The type of the vehicle. Allowed values: `"car"`, `"motorcycle"`, `"auto"`.

Example:
```json
{
  "fullname": {
    "firstname": "Alice",
    "lastname": "Smith"
  },
  "email": "alice.smith@example.com",
  "password": "securepass",
  "vehicle": {
    "color": "Red",
    "plate": "XYZ123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

**Responses:**

- **201 Created:**  
  On successful registration, returns a JSON object containing:
  - `token`: A JWT token used for authentication.
  - `captain`: The created captain object.

Example:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "captain": {
    "_id": "60d0fe4f5311236168a109ca",
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

- **400 Bad Request:**  
  If the input validation fails, the endpoint returns details about the validation errors.

Example:
```json
{
  "errors": [
    {
      "msg": "firstname must be at least 3 characters long",
      "param": "fullname.firstname",
      "location": "body"
    }
  ]
}
```

- **401 Unauthorized:**  
  If a captain with the provided email already exists, the endpoint returns an error message.

Example:
```json
{
  "message": "captain already exists"
}
```

- **500 Internal Server Error:**  
  Returned for any unexpected server errors.

**Note:**  
Ensure that your server is correctly configured with the necessary environment variables, such as `JWT_SECRET`, for token generation

## /captains/login Endpoint

**Description:**  
Logs in an existing captain. The endpoint verifies the provided email and password. If the credentials are valid, it returns an authentication token along with the captain's data; otherwise, it returns an error message.

**URL:**  
`POST /captains/login`

**Request Body:**  
The request should be in JSON format with the following fields:

- **email** (string): A valid email address.
- **password** (string): The captain's password (minimum 6 characters).

Example:
```json
{
  "email": "alice.smith@example.com",
  "password": "securepass"
}
```

**Responses:**

- **201 Created:**  
  On successful login, returns a JSON object containing:
  - `token`: A JWT token used for authentication.
  - `captain`: The captain object.

Example:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "captain": {
    "_id": "60d0fe4f5311236168a109ca",
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

- **400 Bad Request:**  
  If input validation fails or the credentials are invalid, returns an error message.

Example:
```json
{
  "message": "invalid email or password"
}
```

- **500 Internal Server Error:**  
  For unexpected server errors.

---

## /captains/profile Endpoint

**Description:**  
Returns the profile of the authenticated captain.

**URL:**  
`GET /captains/profile`

**Headers:**  
- A valid JWT token must be provided either in the `token` cookie or in the Authorization header as `Bearer <token>`.

**Responses:**

- **201 Created:**  
  On successful authentication, returns a JSON object containing the captain's data.

Example:
```json
{
  "captain": {
    "_id": "60d0fe4f5311236168a109ca",
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ123",
      "capacity": 4,
      "vehicleType": "car"
    },
    "status": "inactive"
  }
}
```

- **401 Unauthorized:**  
  If the token is missing, invalid, or blacklisted.

Example:
```json
{
  "message": "unauthorized"
}
```

---

## /captains/logout Endpoint

**Description:**  
Logs out the authenticated captain by clearing the authentication token cookie and adding the token to a blacklist.

**URL:**  
`GET /captains/logout`

**Headers:**  
- A valid JWT token must be provided either in the `token` cookie or in the Authorization header as `Bearer <token>`.

**Responses:**

- **201 Created:**  
  Upon successful logout, returns a confirmation message.

Example:
```json
{
  "message": "logged out successfully"
}
```

- **401 Unauthorized:**  
  If the token is missing, invalid, or already blacklisted.

Example:
```json
{
  "message": "unauthorized"
}
```

- **500 Internal Server Error:**  
  For unexpected server