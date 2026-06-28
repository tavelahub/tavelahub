# Audit Log API Specification

## Overview

The Audit Log module records and tracks all important activities performed within the HRIS system.

Audit logs provide transparency, security monitoring, and compliance tracking by storing information about:

- Who performed an action
- What action was performed
- Which data was affected
- When the action occurred
- Previous and updated values

The Audit Log module is read-only for normal users.

# Base URL

```
/api/v1/audit-logs
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role           | Permission           |
| -------------- | -------------------- |
| Super Admin    | Full Access          |
| HR Admin       | View HR Activities   |
| Security Admin | Full Audit Access    |
| Manager        | View Team Activities |
| Employee       | View Own Activities  |

# Audit Log Object

```json
{
  "id": "log_001",
  "userId": "user_001",
  "action": "UPDATE",
  "module": "EMPLOYEE",
  "entityId": "emp_001",
  "description": "Updated employee information",
  "ipAddress": "192.168.1.10",
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Action Type

Available values:

```
CREATE
UPDATE
DELETE
LOGIN
LOGOUT
APPROVE
REJECT
EXPORT
UPLOAD
DOWNLOAD
PASSWORD_CHANGE
PERMISSION_CHANGE
```

# Module Type

Available values:

```
AUTHENTICATION
USER
ROLE
EMPLOYEE
DEPARTMENT
POSITION
ATTENDANCE
LEAVE
OVERTIME
PAYROLL
REIMBURSEMENT
DOCUMENT
ANNOUNCEMENT
NOTIFICATION
REPORT
SETTINGS
```

# Endpoints

# Get Audit Logs

Retrieve audit log list.

## Endpoint

```
GET /audit-logs
```

## Query Parameters

| Parameter | Type   | Description   |
| --------- | ------ | ------------- |
| page      | number | Current page  |
| limit     | number | Data limit    |
| userId    | UUID   | Filter user   |
| module    | string | Filter module |
| action    | string | Filter action |
| entityId  | UUID   | Filter entity |
| startDate | date   | Start date    |
| endDate   | date   | End date      |

Example:

```
GET /audit-logs?module=PAYROLL&action=UPDATE
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "log_001",
      "user": "Admin",
      "module": "PAYROLL",
      "action": "UPDATE",
      "description": "Updated salary"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

# Get Audit Log Detail

Retrieve audit log detail.

## Endpoint

```
GET /audit-logs/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "log_001",
    "module": "EMPLOYEE",
    "action": "UPDATE",
    "entityId": "emp_001",
    "before": {
      "position": "Staff"
    },
    "after": {
      "position": "Senior Staff"
    }
  }
}
```

# Get My Activity Logs

Retrieve current user's activities.

## Endpoint

```
GET /audit-logs/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| page      | number |
| limit     | number |
| action    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "action": "LOGIN",
      "createdAt": "2026-06-28"
    }
  ]
}
```

# Get Entity History

Retrieve history of specific data.

Example:

Employee history:

```
GET /audit-logs/entity/employee/emp_001
```

## Endpoint

```
GET /audit-logs/entity/{module}/{entityId}
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "action": "UPDATE",
      "user": "HR Admin",
      "changes": {
        "salary": {
          "old": 8000000,
          "new": 10000000
        }
      }
    }
  ]
}
```

# Create Audit Log

Internal system endpoint.

Used by backend services.

## Endpoint

```
POST /audit-logs
```

## Request Body

```json
{
  "action": "UPDATE",
  "module": "EMPLOYEE",
  "entityId": "emp_001",
  "description": "Updated employee profile",
  "before": {
    "status": "ACTIVE"
  },
  "after": {
    "status": "INACTIVE"
  }
}
```

## Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Audit log created."
}
```

# Login Activity

Retrieve authentication activities.

## Endpoint

```
GET /audit-logs/login-history
```

## Query Parameters

| Parameter | Type |
| --------- | ---- |
| userId    | UUID |
| startDate | date |
| endDate   | date |

## Response

```json
{
  "success": true,
  "data": [
    {
      "user": "John Doe",
      "action": "LOGIN",
      "ipAddress": "192.168.1.20",
      "device": "Chrome Windows"
    }
  ]
}
```

# Security Activity

Retrieve security-related events.

## Endpoint

```
GET /audit-logs/security
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "action": "PASSWORD_CHANGE",
      "user": "Admin",
      "createdAt": "2026-06-28"
    }
  ]
}
```

# Export Audit Log

Export audit history.

## Endpoint

```
GET /audit-logs/export
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| format    | string |
| startDate | date   |
| endDate   | date   |
| module    | string |

Available:

```
PDF
EXCEL
CSV
```

## Response

File download.

# Audit Log Statistics

Retrieve audit statistics.

## Endpoint

```
GET /audit-logs/summary
```

## Response

```json
{
  "success": true,
  "data": {
    "totalLogs": 50000,
    "todayActivities": 300,
    "loginCount": 200,
    "updateCount": 80,
    "deleteCount": 20
  }
}
```

# Delete Audit Logs

Delete old audit logs.

Only:

```
Super Admin
```

allowed.

## Endpoint

```
DELETE /audit-logs/archive
```

## Request Body

```json
{
  "beforeDate": "2025-01-01"
}
```

## Response

```json
{
  "success": true,
  "message": "Old audit logs archived."
}
```

# Validation Rules

| Field       | Rule                      |
| ----------- | ------------------------- |
| action      | Required                  |
| module      | Required                  |
| entityId    | Required for data changes |
| description | Required                  |

# Error Responses

## Audit Log Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Audit log not found."
}
```

## Forbidden Access

```
403 Forbidden
```

```json
{
  "success": false,
  "message": "You do not have permission."
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
| 422  | Validation Error      |
| 500  | Internal Server Error |

# Related Database Tables

- audit_logs
- users
- roles
- permissions
- employees
- settings
