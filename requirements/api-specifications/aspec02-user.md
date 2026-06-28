# User API Specification

## Overview

The User module manages application user accounts, including account creation, profile management, role assignment, activation, and password administration.

This module is intended for **Super Admin** and authorized administrators only.

# Base URL

```
/api/v1/users
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission           |
| ----------- | -------------------- |
| Super Admin | Full Access          |
| HR Admin    | Read Only (Optional) |
| Manager     | No Access            |
| Employee    | No Access            |

# User Object

```json
{
  "id": "usr_001",
  "employeeId": "emp_001",
  "email": "john@example.com",
  "role": "HR_ADMIN",
  "status": "ACTIVE",
  "lastLoginAt": "2026-06-28T08:30:00Z",
  "createdAt": "2026-01-15T10:00:00Z",
  "updatedAt": "2026-06-28T09:15:00Z"
}
```

# Endpoints

# Get Users

Retrieve a paginated list of users.

## Endpoint

```
GET /users
```

### Query Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| page      | number | Current page                     |
| limit     | number | Items per page                   |
| search    | string | Search by email or employee name |
| role      | string | Filter by role                   |
| status    | string | Filter by account status         |
| sort      | string | Sort field                       |

Example

```
GET /users?page=1&limit=10&role=HR_ADMIN
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Users retrieved successfully.",
  "data": [
    {
      "id": "usr_001",
      "email": "john@example.com",
      "role": "HR_ADMIN",
      "status": "ACTIVE"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 20,
    "totalPages": 2
  }
}
```

# Get User Detail

Retrieve a single user by ID.

## Endpoint

```
GET /users/{id}
```

### Path Parameters

| Name | Type |
| ---- | ---- |
| id   | UUID |

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
    "email": "john@example.com",
    "role": "HR_ADMIN",
    "status": "ACTIVE"
  }
}
```

# Create User

Create a new user account.

## Endpoint

```
POST /users
```

### Request Body

```json
{
  "employeeId": "emp_001",
  "email": "john@example.com",
  "password": "Password123!",
  "roleId": "role_hr_admin"
}
```

### Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "User created successfully.",
  "data": {
    "id": "usr_001"
  }
}
```

### Validation Rules

| Field      | Rule                           |
| ---------- | ------------------------------ |
| employeeId | Required                       |
| email      | Required, unique, valid email  |
| password   | Required, minimum 8 characters |
| roleId     | Required                       |

# Update User

Update user information.

## Endpoint

```
PUT /users/{id}
```

### Request Body

```json
{
  "email": "john.updated@example.com",
  "roleId": "role_manager",
  "status": "ACTIVE"
}
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "User updated successfully."
}
```

# Delete User

Soft delete a user account.

## Endpoint

```
DELETE /users/{id}
```

### Success Response

```
204 No Content
```

### Notes

- The record is not permanently removed.
- `deletedAt` is populated.
- The user can no longer log in.

# Activate User

Activate a user account.

## Endpoint

```
PATCH /users/{id}/activate
```

### Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "User activated successfully."
}
```

# Deactivate User

Deactivate a user account.

## Endpoint

```
PATCH /users/{id}/deactivate
```

### Success Response

```json
{
  "success": true,
  "message": "User deactivated successfully."
}
```

# Reset User Password

Reset a user's password.

## Endpoint

```
PATCH /users/{id}/reset-password
```

### Request Body

```json
{
  "password": "NewPassword123!"
}
```

### Success Response

```json
{
  "success": true,
  "message": "Password reset successfully."
}
```

# Assign Role

Assign or change a user's role.

## Endpoint

```
PATCH /users/{id}/role
```

### Request Body

```json
{
  "roleId": "role_manager"
}
```

### Success Response

```json
{
  "success": true,
  "message": "Role updated successfully."
}
```

# Unlock User

Unlock a previously locked account.

## Endpoint

```
PATCH /users/{id}/unlock
```

### Success Response

```json
{
  "success": true,
  "message": "User unlocked successfully."
}
```

# User Activity

Retrieve login history and account activity.

## Endpoint

```
GET /users/{id}/activities
```

### Success Response

```json
{
  "success": true,
  "data": [
    {
      "action": "LOGIN",
      "ipAddress": "192.168.1.1",
      "createdAt": "2026-06-28T08:00:00Z"
    }
  ]
}
```

# Validation Rules

## Create User

| Field      | Validation                     |
| ---------- | ------------------------------ |
| employeeId | Required                       |
| email      | Required, unique, valid email  |
| password   | Required, minimum 8 characters |
| roleId     | Required                       |

## Update User

| Field  | Validation            |
| ------ | --------------------- |
| email  | Optional, valid email |
| roleId | Optional              |
| status | ACTIVE or INACTIVE    |

# Status Values

```
ACTIVE
INACTIVE
LOCKED
```

# Error Responses

## Validation Error

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {
    "email": ["Email already exists."]
  }
}
```

## User Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "User not found."
}
```

## Unauthorized

```
403 Forbidden
```

```json
{
  "success": false,
  "message": "You do not have permission to access this resource."
}
```

# HTTP Status Codes

| Code | Description           |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 204  | No Content            |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 409  | Conflict              |
| 422  | Validation Error      |
| 500  | Internal Server Error |

# Business Rules

- Each user account must be linked to exactly one employee.
- One employee can have only one active user account.
- Email addresses must be unique across all users.
- A user cannot delete their own account.
- The last active Super Admin account cannot be deactivated or deleted.
- Deactivated users cannot log in.
- Soft delete should be used instead of permanent deletion.
- All user management actions must be recorded in the audit log.

# Related Database Tables

- users
- employees
- roles
- permissions
- role_permissions
- audit_logs
