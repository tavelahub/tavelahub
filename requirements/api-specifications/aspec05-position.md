# Position API Specification

## Overview

The Position module manages employee job positions within the company organization structure.

A position represents a job title or role assigned to employees, such as Software Engineer, HR Manager, Finance Staff, or Product Manager.

This module allows authorized users to create, update, view, search, and manage company positions.

# Base URL

```
/api/v1/positions
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission  |
| ----------- | ----------- |
| Super Admin | Full Access |
| HR Admin    | Full Access |
| Manager     | View Access |
| Employee    | No Access   |

# Position Object

```json
{
  "id": "pos_001",
  "name": "Software Engineer",
  "code": "SE",
  "description": "Responsible for software development",
  "departmentId": "dept_001",
  "department": {
    "id": "dept_001",
    "name": "Engineering"
  },
  "level": "MID",
  "employeeCount": 15,
  "status": "ACTIVE",
  "createdAt": "2026-01-10T08:00:00Z"
}
```

# Endpoints

# Get Positions

Retrieve a paginated list of positions.

## Endpoint

```
GET /positions
```

## Query Parameters

| Parameter    | Type   | Description               |
| ------------ | ------ | ------------------------- |
| page         | number | Current page              |
| limit        | number | Number of records         |
| search       | string | Search position name/code |
| departmentId | UUID   | Filter by department      |
| level        | string | Filter by level           |
| status       | string | Filter by status          |
| sort         | string | Sorting field             |

Example:

```
GET /positions?page=1&limit=10&departmentId=dept_001
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Positions retrieved successfully.",
  "data": [
    {
      "id": "pos_001",
      "name": "Software Engineer",
      "code": "SE",
      "department": "Engineering",
      "level": "MID",
      "employeeCount": 15,
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

# Get Position Detail

Retrieve position information by ID.

## Endpoint

```
GET /positions/{id}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Position retrieved successfully.",
  "data": {
    "id": "pos_001",
    "name": "Software Engineer",
    "code": "SE",
    "description": "Responsible for software development",
    "department": {
      "id": "dept_001",
      "name": "Engineering"
    },
    "level": "MID",
    "employeeCount": 15
  }
}
```

# Create Position

Create a new position.

## Endpoint

```
POST /positions
```

## Request Body

```json
{
  "name": "Software Engineer",
  "code": "SE",
  "description": "Responsible for software development",
  "departmentId": "dept_001",
  "level": "MID"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Position created successfully.",
  "data": {
    "id": "pos_001"
  }
}
```

# Validation Rules

| Field        | Rule             |
| ------------ | ---------------- |
| name         | Required         |
| code         | Required, unique |
| departmentId | Required         |
| level        | Required         |
| description  | Optional         |

# Update Position

Update position information.

## Endpoint

```
PUT /positions/{id}
```

## Request Body

```json
{
  "name": "Senior Software Engineer",
  "description": "Senior engineering role",
  "level": "SENIOR"
}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Position updated successfully."
}
```

# Delete Position

Delete a position.

## Endpoint

```
DELETE /positions/{id}
```

## Response

```
204 No Content
```

## Business Rules

- Position uses soft delete.
- Position cannot be deleted if active employees are assigned.
- Deleted positions cannot be assigned to employees.

# Activate Position

Activate a position.

## Endpoint

```
PATCH /positions/{id}/activate
```

## Response

```json
{
  "success": true,
  "message": "Position activated successfully."
}
```

# Deactivate Position

Deactivate a position.

## Endpoint

```
PATCH /positions/{id}/deactivate
```

## Response

```json
{
  "success": true,
  "message": "Position deactivated successfully."
}
```

# Assign Department

Move position to another department.

## Endpoint

```
PATCH /positions/{id}/department
```

## Request Body

```json
{
  "departmentId": "dept_002"
}
```

## Response

```json
{
  "success": true,
  "message": "Position department updated successfully."
}
```

# Get Position Employees

Retrieve employees assigned to a position.

## Endpoint

```
GET /positions/{id}/employees
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| page      | number |
| limit     | number |
| status    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "emp_001",
      "name": "John Doe",
      "email": "john@example.com"
    }
  ]
}
```

# Position Statistics

Retrieve position statistics.

## Endpoint

```
GET /positions/{id}/statistics
```

## Response

```json
{
  "success": true,
  "data": {
    "totalEmployees": 15,
    "activeEmployees": 14,
    "inactiveEmployees": 1
  }
}
```

# Search Position

Search positions.

## Endpoint

```
GET /positions/search
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| keyword   | string |

Example:

```
GET /positions/search?keyword=engineer
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "pos_001",
      "name": "Software Engineer"
    }
  ]
}
```

# Position Level

Available values:

```
INTERN
JUNIOR
MID
SENIOR
LEAD
MANAGER
DIRECTOR
```

# Position Status

Available values:

```
ACTIVE
INACTIVE
```

# Error Responses

## Position Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Position not found."
}
```

## Duplicate Position

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Position code already exists."
}
```

## Validation Error

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {}
}
```

# HTTP Status Codes

| Code | Description           |
| ---- | --------------------- |
| 200  | Success               |
| 201  | Created               |
| 204  | Deleted               |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 409  | Conflict              |
| 422  | Validation Error      |
| 500  | Internal Server Error |

# Related Database Tables

- positions
- departments
- employees
- audit_logs
