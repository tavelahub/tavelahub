# Department API Specification

## Overview

The Department module manages company organizational departments.

A department represents a functional division within the company, such as Engineering, Human Resources, Finance, or Marketing.

This module allows authorized users to create, update, view, search, and manage departments.

# Base URL

```
/api/v1/departments
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

# Department Object

```json
{
  "id": "dept_001",
  "name": "Engineering",
  "code": "ENG",
  "description": "Software development department",
  "managerId": "emp_001",
  "employeeCount": 25,
  "status": "ACTIVE",
  "createdAt": "2026-01-10T08:00:00Z"
}
```

# Endpoints

# Get Departments

Retrieve a paginated list of departments.

## Endpoint

```
GET /departments
```

## Query Parameters

| Parameter | Type   | Description                 |
| --------- | ------ | --------------------------- |
| page      | number | Current page                |
| limit     | number | Number of records           |
| search    | string | Search department name/code |
| status    | string | Filter by status            |
| sort      | string | Sorting field               |

Example:

```
GET /departments?page=1&limit=10&status=ACTIVE
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Departments retrieved successfully.",
  "data": [
    {
      "id": "dept_001",
      "name": "Engineering",
      "code": "ENG",
      "employeeCount": 25,
      "status": "ACTIVE"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 5,
    "totalPages": 1
  }
}
```

# Get Department Detail

Retrieve department information by ID.

## Endpoint

```
GET /departments/{id}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Department retrieved successfully.",
  "data": {
    "id": "dept_001",
    "name": "Engineering",
    "code": "ENG",
    "description": "Software development department",
    "manager": {
      "id": "emp_001",
      "name": "John Doe"
    },
    "employeeCount": 25
  }
}
```

# Create Department

Create a new department.

## Endpoint

```
POST /departments
```

## Request Body

```json
{
  "name": "Engineering",
  "code": "ENG",
  "description": "Software development department",
  "managerId": "emp_001"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Department created successfully.",
  "data": {
    "id": "dept_001"
  }
}
```

# Validation Rules

| Field       | Rule             |
| ----------- | ---------------- |
| name        | Required         |
| code        | Required, unique |
| description | Optional         |
| managerId   | Optional         |

# Update Department

Update department information.

## Endpoint

```
PUT /departments/{id}
```

## Request Body

```json
{
  "name": "Product Engineering",
  "description": "Product development team"
}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Department updated successfully."
}
```

# Delete Department

Remove department.

## Endpoint

```
DELETE /departments/{id}
```

## Response

```
204 No Content
```

## Business Rules

- Department uses soft delete.
- Department cannot be deleted if active employees are assigned.
- Deleted departments cannot be assigned to new employees.

# Activate Department

Activate a department.

## Endpoint

```
PATCH /departments/{id}/activate
```

## Response

```json
{
  "success": true,
  "message": "Department activated successfully."
}
```

# Deactivate Department

Deactivate a department.

## Endpoint

```
PATCH /departments/{id}/deactivate
```

## Response

```json
{
  "success": true,
  "message": "Department deactivated successfully."
}
```

# Assign Department Manager

Assign an employee as department manager.

## Endpoint

```
PATCH /departments/{id}/manager
```

## Request Body

```json
{
  "managerId": "emp_001"
}
```

## Response

```json
{
  "success": true,
  "message": "Department manager assigned successfully."
}
```

# Remove Department Manager

Remove current department manager.

## Endpoint

```
DELETE /departments/{id}/manager
```

## Response

```json
{
  "success": true,
  "message": "Department manager removed successfully."
}
```

# Get Department Employees

Retrieve employees assigned to a department.

## Endpoint

```
GET /departments/{id}/employees
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
      "position": "Software Engineer"
    }
  ]
}
```

# Department Statistics

Retrieve department statistics.

## Endpoint

```
GET /departments/{id}/statistics
```

## Response

```json
{
  "success": true,
  "data": {
    "totalEmployees": 25,
    "activeEmployees": 23,
    "inactiveEmployees": 2
  }
}
```

# Search Department

Search departments.

## Endpoint

```
GET /departments/search
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| keyword   | string |

Example:

```
GET /departments/search?keyword=engineering
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "dept_001",
      "name": "Engineering"
    }
  ]
}
```

# Department Status

Available values:

```
ACTIVE
INACTIVE
```

# Error Responses

## Department Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Department not found."
}
```

## Duplicate Department

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Department code already exists."
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

- departments
- employees
- positions
- audit_logs

```
Organization
│
└── Departments
    │
    ├── Department List
    ├── Department Detail
    ├── Create Department
    ├── Edit Department
    ├── Assign Manager
    └── Employee List by Department
```

## Prisma

```
model Department {
  id          String   @id @default(uuid())
  name        String
  code        String   @unique
  description String?

  managerId   String?
  manager     Employee? @relation(
    fields: [managerId],
    references: [id]
  )

  employees   Employee[]
  positions   Position[]

  status      DepartmentStatus @default(ACTIVE)

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```
