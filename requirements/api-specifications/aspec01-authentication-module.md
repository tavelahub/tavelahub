# Authentication API Specification

## Overview

The Authentication module provides secure access to the HRIS application through JWT-based authentication. It supports login, token refresh, logout, password management, and retrieval of the authenticated user's profile.

# Base URL

```
/v1/auth
```

# Authentication Flow

```text
User
  │
  ▼
Login
  │
  ▼
Validate Credentials
  │
  ▼
Generate Access Token
Generate Refresh Token
  │
  ▼
Return Tokens
  │
  ▼
Access Protected APIs
  │
  ▼
Access Token Expired
  │
  ▼
Refresh Token
  │
  ▼
New Access Token
```

# Authentication Method

JWT Bearer Token

```
Authorization: Bearer <access_token>
```

# Endpoints

## Login

Authenticate a user.

### Endpoint

```
POST /auth/login
```

### Authentication

Not Required

### Request Body

| Field    | Type   | Required | Description              |
| -------- | ------ | -------- | ------------------------ |
| email    | string | Yes      | Registered email address |
| password | string | Yes      | User password            |

Example

```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

### Success Response

Status

```
200 OK
```

```json
{
  "success": true,
  "message": "Login successful.",
  "data": {
    "accessToken": "jwt_access_token",
    "refreshToken": "jwt_refresh_token",
    "expiresIn": 3600,
    "user": {
      "id": "usr_001",
      "employeeId": "emp_001",
      "fullName": "John Doe",
      "email": "john@example.com",
      "role": "HR_ADMIN"
    }
  }
}
```

### Error Responses

Invalid Credentials

```
401 Unauthorized
```

```json
{
  "success": false,
  "message": "Invalid email or password."
}
```

Validation Error

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {
    "email": ["Email is required."]
  }
}
```

## Logout

Invalidate the current session.

### Endpoint

```
POST /auth/logout
```

### Authentication

Required

### Headers

```
Authorization: Bearer <access_token>
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Logout successful."
}
```

## Refresh Access Token

Generate a new access token using a valid refresh token.

### Endpoint

```
POST /auth/refresh
```

### Authentication

Not Required

### Request Body

```json
{
  "refreshToken": "jwt_refresh_token"
}
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Token refreshed successfully.",
  "data": {
    "accessToken": "new_access_token",
    "expiresIn": 3600
  }
}
```

### Error Response

```
401 Unauthorized
```

```json
{
  "success": false,
  "message": "Invalid refresh token."
}
```

## Get Current User

Retrieve information about the authenticated user.

### Endpoint

```
GET /auth/me
```

### Authentication

Required

### Headers

```
Authorization: Bearer <access_token>
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "User retrieved successfully.",
  "data": {
    "id": "usr_001",
    "employeeId": "emp_001",
    "fullName": "John Doe",
    "email": "john@example.com",
    "role": "HR_ADMIN",
    "department": "Human Resources",
    "position": "HR Manager"
  }
}
```

## Forgot Password

Request a password reset link.

### Endpoint

```
POST /auth/forgot-password
```

### Authentication

Not Required

### Request Body

```json
{
  "email": "john@example.com"
}
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Password reset instructions have been sent if the email exists."
}
```

## Reset Password

Reset a user's password using a valid reset token.

### Endpoint

```
POST /auth/reset-password
```

### Authentication

Not Required

### Request Body

```json
{
  "token": "reset_token",
  "password": "newPassword123",
  "confirmPassword": "newPassword123"
}
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Password reset successfully."
}
```

## Change Password

Allow an authenticated user to change their password.

### Endpoint

```
PATCH /auth/change-password
```

### Authentication

Required

### Headers

```
Authorization: Bearer <access_token>
```

### Request Body

```json
{
  "currentPassword": "oldPassword123",
  "newPassword": "newPassword123",
  "confirmPassword": "newPassword123"
}
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Password changed successfully."
}
```

### Error Response

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Current password is incorrect."
}
```

# Authentication Middleware

Protected endpoints require:

```
Authorization: Bearer <access_token>
```

The middleware validates:

- JWT signature
- Token expiration
- User existence
- User status (Active/Inactive)

# Validation Rules

## Login

| Field    | Rule                           |
| -------- | ------------------------------ |
| email    | Required, valid email          |
| password | Required, minimum 8 characters |

## Forgot Password

| Field | Rule                  |
| ----- | --------------------- |
| email | Required, valid email |

## Reset Password

| Field           | Rule                           |
| --------------- | ------------------------------ |
| token           | Required                       |
| password        | Required, minimum 8 characters |
| confirmPassword | Must match password            |

## Change Password

| Field           | Rule                           |
| --------------- | ------------------------------ |
| currentPassword | Required                       |
| newPassword     | Required, minimum 8 characters |
| confirmPassword | Must match newPassword         |

# HTTP Status Codes

| Status | Description           |
| ------ | --------------------- |
| 200    | Success               |
| 400    | Bad Request           |
| 401    | Unauthorized          |
| 403    | Forbidden             |
| 404    | User Not Found        |
| 422    | Validation Error      |
| 500    | Internal Server Error |

# Security Notes

- Passwords must be hashed using Argon2 or bcrypt.
- Access tokens should have a short expiration time (e.g. 1 hour).
- Refresh tokens should be securely stored and revocable.
- All authentication endpoints must be served over HTTPS in production.
- Rate limiting should be applied to login and password reset endpoints to reduce brute-force attacks.

# Related Database Tables

- users
- roles
- refresh_tokens (optional, if implementing refresh token persistence)
- password_reset_tokens (optional, if implementing password reset via email)
