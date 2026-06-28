# API Specification

## Overview

This document defines the REST API specification for the Travel Hub (HRIS) system.

### Base URL

Development

```
http://localhost:3000/v1
```

Production

```
https://api.example.com/v1
```

# Authentication

Authentication uses **JWT (JSON Web Token)**.

Protected endpoints require:

```
Authorization: Bearer <access_token>
```

# API Response Format

## Success Response

```json
{
  "success": true,
  "message": "Request completed successfully.",
  "data": {}
}
```

## Error Response

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {}
}
```

# Authentication Module

### Login

POST /auth/login

##### Request

```json
{
  "email": "admin@example.com",
  "password": "password"
}
```

##### Response

```json
{
  "accessToken": "",
  "refreshToken": "",
  "user": {}
}
```

### Logout

POST /auth/logout

### Refresh Token

POST /auth/refresh

### Current User

GET /auth/me

# User Module

### Get Users

GET /users

### Get User Detail

GET /users/{id}

### Create User

POST /users

### Update User

PUT /users/{id}

### Delete User

DELETE /users/{id}

### Reset Password

PATCH /users/{id}/reset-password

# Employee Module

### Get Employees

GET /employees

Query Parameters

- page
- limit
- search
- department
- position

### Employee Detail

GET /employees/{id}

### Create Employee

POST /employees

### Update Employee

PUT /employees/{id}

### Delete Employee

DELETE /employees/{id}

# Department Module

### Get Departments

GET /departments

### Department Detail

GET /departments/{id}

### Create Department

POST /departments

### Update Department

PUT /departments/{id}

### Delete Department

DELETE /departments/{id}

# Position Module

### Get Positions

GET /positions

### Position Detail

GET /positions/{id}

### Create Position

POST /positions

### Update Position

PUT /positions/{id}

### Delete Position

DELETE /positions/{id}

# Attendance Module

### Check In

POST /attendance/check-in

### Check Out

POST /attendance/check-out

### Attendance History

GET /attendance

### Attendance Detail

GET /attendance/{id}

### Attendance Report

GET /attendance/report

# Leave Module

### Get Leave Requests

GET /leave-requests

### Submit Leave Request

POST /leave-requests

### Leave Detail

GET /leave-requests/{id}

### Update Leave Request

PUT /leave-requests/{id}

### Cancel Leave Request

DELETE /leave-requests/{id}

### Approve Leave

PATCH /leave-requests/{id}/approve

### Reject Leave

PATCH /leave-requests/{id}/reject

# Overtime Module

### Get Overtime Requests

GET /overtime-requests

### Submit Overtime

POST /overtime-requests

### Overtime Detail

GET /overtime-requests/{id}

### Approve Overtime

PATCH /overtime-requests/{id}/approve

### Reject Overtime

PATCH /overtime-requests/{id}/reject

# Payroll Module

### Payroll List

GET /payrolls

### Payroll Detail

GET /payrolls/{id}

### Generate Payroll

POST /payrolls/generate

### Generate Payslip

POST /payrolls/{id}/generate-payslip

### Download Payslip

GET /payrolls/{id}/download

# Reimbursement Module

### Reimbursement List

GET /reimbursements

### Submit Reimbursement

POST /reimbursements

### Reimbursement Detail

GET /reimbursements/{id}

### Approve Reimbursement

PATCH /reimbursements/{id}/approve

### Reject Reimbursement

PATCH /reimbursements/{id}/reject

# Document Module

### Upload Document

POST /documents

### Document List

GET /documents

### Download Document

GET /documents/{id}

### Delete Document

DELETE /documents/{id}

# Announcement Module

### Get Announcements

GET /announcements

### Announcement Detail

GET /announcements/{id}

### Create Announcement

POST /announcements

### Update Announcement

PUT /announcements/{id}

### Delete Announcement

DELETE /announcements/{id}

# Notification Module

### Notification List

GET /notifications

### Mark as Read

PATCH /notifications/{id}/read

### Mark All as Read

PATCH /notifications/read-all

# Report Module

### Employee Report

GET /reports/employees

### Attendance Report

GET /reports/attendance

### Leave Report

GET /reports/leave

### Payroll Report

GET /reports/payroll

### Reimbursement Report

GET /reports/reimbursements

# Dashboard Module

### Dashboard Summary

GET /dashboard

### Dashboard Statistics

GET /dashboard/statistics

### Dashboard Recent Activities

GET /dashboard/activities

# Settings Module

### Company Settings

GET /settings/company

PUT /settings/company

### Working Hours

GET /settings/working-hours

PUT /settings/working-hours

### Leave Types

GET /settings/leave-types

POST /settings/leave-types

PUT /settings/leave-types/{id}

DELETE /settings/leave-types/{id}

# Audit Log Module

### Activity Logs

GET /audit-logs

### Activity Detail

### GET /audit-logs/{id}

---

---

---

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

# Pagination

Request

```
?page=1&limit=10
```

Response

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "totalPages": 10
  }
}
```

# Filtering

Example

```
GET /employees?department=IT
```

```
GET /employees?status=ACTIVE
```

```
GET /employees?search=john
```

# Sorting

```
GET /employees?sort=name
```

```
GET /employees?sort=-createdAt
```

# API Versioning

Current Version

```
/api/v1
```

Future Versions

```
/api/v2
```

# OpenAPI Documentation

The backend automatically generates API documentation using **Scalar OpenAPI**.

Development

```
http://localhost:3000/docs
```

Production

```
https://api.example.com/docs
```

## File Sitemap

```
src/
├── modules/
│ ├── auth/
│ ├── users/
│ ├── employees/
│ ├── departments/
│ ├── positions/
│ ├── attendance/
│ ├── leave/
│ ├── overtime/
│ ├── payroll/
│ ├── reimbursements/
│ ├── documents/
│ ├── announcements/
│ ├── notifications/
│ ├── reports/
│ ├── dashboard/
│ ├── settings/
│ └── audit-log/
│
├── middleware/
├── lib/
├── prisma/
└── app.ts
```
