# Employee API Specification

## Overview

The Employee module manages employee information, employment records, organizational assignments, and employee profiles within the HRIS system.

This module allows authorized users to create, view, update, search, and manage employee records.

# Base URL

```
/api/v1/employees
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission            |
| ----------- | --------------------- |
| Super Admin | Full Access           |
| HR Admin    | Full Access           |
| Manager     | View Team Employees   |
| Employee    | View Own Profile Only |

# Employee Object

```json
{
  "id": "emp_001",
  "employeeNumber": "EMP-0001",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+628123456789",
  "department": {
    "id": "dept_001",
    "name": "Engineering"
  },
  "position": {
    "id": "pos_001",
    "name": "Software Engineer"
  },
  "employmentStatus": "ACTIVE",
  "hireDate": "2026-01-10"
}
```

# Endpoints

# Get Employees

Retrieve a paginated list of employees.

## Endpoint

```
GET /employees
```

## Query Parameters

| Parameter    | Type   | Description                |
| ------------ | ------ | -------------------------- |
| page         | number | Current page               |
| limit        | number | Number of records          |
| search       | string | Search employee name/email |
| departmentId | UUID   | Filter by department       |
| positionId   | UUID   | Filter by position         |
| status       | string | Employment status          |
| sort         | string | Sorting field              |

Example:

```
GET /employees?page=1&limit=10&status=ACTIVE
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Employees retrieved successfully.",
  "data": [
    {
      "id": "emp_001",
      "employeeNumber": "EMP-0001",
      "fullName": "John Doe",
      "department": "Engineering",
      "position": "Software Engineer",
      "status": "ACTIVE"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "totalPages": 10
  }
}
```

# Get Employee Detail

Retrieve employee information by ID.

## Endpoint

```
GET /employees/{id}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Employee retrieved successfully.",
  "data": {
    "id": "emp_001",
    "employeeNumber": "EMP-0001",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "phone": "+628123456789",
    "gender": "MALE",
    "birthDate": "1998-05-10",
    "address": "Jakarta",
    "hireDate": "2026-01-10",
    "employmentStatus": "ACTIVE"
  }
}
```

# Get My Profile

Retrieve currently authenticated employee profile.

## Endpoint

```
GET /employees/me
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "data": {
    "id": "emp_001",
    "fullName": "John Doe",
    "email": "john@example.com",
    "department": "Engineering",
    "position": "Software Engineer"
  }
}
```

# Create Employee

Create a new employee record.

## Endpoint

```
POST /employees
```

## Request Body

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+628123456789",
  "gender": "MALE",
  "birthDate": "1998-05-10",
  "address": "Jakarta",
  "departmentId": "dept_001",
  "positionId": "pos_001",
  "hireDate": "2026-01-10",
  "employmentStatus": "ACTIVE"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Employee created successfully.",
  "data": {
    "id": "emp_001"
  }
}
```

# Update Employee

Update employee information.

## Endpoint

```
PUT /employees/{id}
```

## Request Body

```json
{
  "phone": "+628987654321",
  "address": "Bandung",
  "departmentId": "dept_002",
  "positionId": "pos_002"
}
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Employee updated successfully."
}
```

# Delete Employee

Remove employee record.

## Endpoint

```
DELETE /employees/{id}
```

## Response

```
204 No Content
```

## Business Rule

Employee deletion uses soft delete.

The system updates:

```
deletedAt = current_timestamp
```

# Update Employment Status

Change employee employment status.

## Endpoint

```
PATCH /employees/{id}/status
```

## Request Body

```json
{
  "status": "INACTIVE"
}
```

## Status Values

```
ACTIVE
INACTIVE
ON_LEAVE
RESIGNED
TERMINATED
```

## Response

```json
{
  "success": true,
  "message": "Employment status updated successfully."
}
```

# Assign Department

Assign employee to a department.

## Endpoint

```
PATCH /employees/{id}/department
```

## Request Body

```json
{
  "departmentId": "dept_001"
}
```

## Response

```json
{
  "success": true,
  "message": "Department assigned successfully."
}
```

# Assign Position

Assign employee position.

## Endpoint

```
PATCH /employees/{id}/position
```

## Request Body

```json
{
  "positionId": "pos_001"
}
```

# Upload Employee Photo

Upload employee profile picture.

## Endpoint

```
POST /employees/{id}/photo
```

## Content-Type

```
multipart/form-data
```

## Request

```
photo: image_file
```

## Response

```json
{
  "success": true,
  "message": "Profile photo uploaded successfully.",
  "data": {
    "photoUrl": "/uploads/employees/photo.jpg"
  }
}
```

# Employee Search

Search employees.

## Endpoint

```
GET /employees/search
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| keyword   | string |

Example:

```
GET /employees/search?keyword=john
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "emp_001",
      "name": "John Doe"
    }
  ]
}
```

# Employee Statistics

Retrieve employee statistics.

## Endpoint

```
GET /employees/statistics
```

## Response

```json
{
  "success": true,
  "data": {
    "totalEmployees": 150,
    "activeEmployees": 140,
    "inactiveEmployees": 10
  }
}
```

# Validation Rules

## Create Employee

| Field            | Rule             |
| ---------------- | ---------------- |
| firstName        | Required         |
| lastName         | Required         |
| email            | Required, unique |
| departmentId     | Required         |
| positionId       | Required         |
| hireDate         | Required         |
| employmentStatus | Required         |

# Employment Status

Available values:

```
ACTIVE
INACTIVE
ON_LEAVE
RESIGNED
TERMINATED
```

# Gender

Available values:

```
MALE
FEMALE
OTHER
```

# Error Responses

## Employee Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Employee not found."
}
```

## Duplicate Employee

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Employee email already exists."
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

- employees
- departments
- positions
- users
- attendance_records
- leave_requests
- overtime_requests
- payrolls
- documents
- reimbursements
- audit_logs
