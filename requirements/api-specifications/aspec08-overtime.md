# Overtime API Specification

## Overview

The Overtime module manages employee overtime requests, approval workflows, overtime records, and overtime reports.

Employees can submit overtime requests, managers can review requests, and HR Admins can manage approved overtime records for payroll calculation.

# Base URL

```
/api/v1/overtime
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission                     |
| ----------- | ------------------------------ |
| Super Admin | Full Access                    |
| HR Admin    | Full Access                    |
| Manager     | Approve Team Overtime Requests |
| Employee    | Create Own Overtime Request    |

# Overtime Object

```json
{
  "id": "ot_001",
  "employeeId": "emp_001",
  "date": "2026-06-28",
  "startTime": "18:00",
  "endTime": "21:00",
  "duration": 3,
  "reason": "Complete project deadline",
  "status": "PENDING",
  "approvedBy": null,
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Overtime Status

Available values:

```
PENDING
APPROVED
REJECTED
CANCELLED
```

# Endpoints

# Get Overtime Requests

Retrieve overtime request list.

## Endpoint

```
GET /overtime
```

## Query Parameters

| Parameter    | Type   | Description       |
| ------------ | ------ | ----------------- |
| page         | number | Current page      |
| limit        | number | Data limit        |
| employeeId   | UUID   | Filter employee   |
| departmentId | UUID   | Filter department |
| status       | string | Filter status     |
| startDate    | date   | Start period      |
| endDate      | date   | End period        |

Example:

```
GET /overtime?page=1&limit=10&status=PENDING
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Overtime requests retrieved successfully.",
  "data": [
    {
      "id": "ot_001",
      "employee": "John Doe",
      "date": "2026-06-28",
      "duration": 3,
      "status": "PENDING"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 50,
    "totalPages": 5
  }
}
```

# Get My Overtime History

Retrieve current employee overtime history.

## Endpoint

```
GET /overtime/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| month     | number |
| year      | number |
| status    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "date": "2026-06-28",
      "duration": 3,
      "status": "APPROVED"
    }
  ]
}
```

# Get Overtime Detail

Retrieve overtime request detail.

## Endpoint

```
GET /overtime/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "ot_001",
    "employee": "John Doe",
    "date": "2026-06-28",
    "startTime": "18:00",
    "endTime": "21:00",
    "duration": 3,
    "reason": "Project deadline",
    "status": "APPROVED"
  }
}
```

# Create Overtime Request

Employee submits overtime request.

## Endpoint

```
POST /overtime
```

## Request Body

```json
{
  "date": "2026-06-28",
  "startTime": "18:00",
  "endTime": "21:00",
  "reason": "Complete project deadline"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Overtime request submitted successfully.",
  "data": {
    "id": "ot_001"
  }
}
```

# Validation Rules

| Field     | Rule                   |
| --------- | ---------------------- |
| date      | Required               |
| startTime | Required               |
| endTime   | Required               |
| reason    | Required               |
| duration  | Must be greater than 0 |

# Update Overtime Request

Update pending overtime request.

## Endpoint

```
PUT /overtime/{id}
```

## Request Body

```json
{
  "startTime": "19:00",
  "endTime": "22:00",
  "reason": "Extended work requirement"
}
```

## Response

```json
{
  "success": true,
  "message": "Overtime request updated successfully."
}
```

# Cancel Overtime Request

Employee cancels their overtime request.

## Endpoint

```
PATCH /overtime/{id}/cancel
```

## Response

```json
{
  "success": true,
  "message": "Overtime request cancelled successfully."
}
```

# Approve Overtime Request

Manager or HR approves overtime.

## Endpoint

```
PATCH /overtime/{id}/approve
```

## Request Body

```json
{
  "note": "Approved based on project requirement"
}
```

## Response

```json
{
  "success": true,
  "message": "Overtime request approved."
}
```

# Reject Overtime Request

Reject overtime request.

## Endpoint

```
PATCH /overtime/{id}/reject
```

## Request Body

```json
{
  "reason": "Overtime was not authorized"
}
```

## Response

```json
{
  "success": true,
  "message": "Overtime request rejected."
}
```

# Calculate Overtime Duration

Calculate overtime duration.

## Endpoint

```
POST /overtime/calculate
```

## Request Body

```json
{
  "startTime": "18:00",
  "endTime": "21:30"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "duration": 3.5
  }
}
```

# Get Team Overtime

Manager retrieves team overtime requests.

## Endpoint

```
GET /overtime/team
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "employee": "John Doe",
      "duration": 4,
      "status": "PENDING"
    }
  ]
}
```

# Overtime Summary

Retrieve overtime statistics.

## Endpoint

```
GET /overtime/summary
```

## Query Parameters

| Parameter    | Type   |
| ------------ | ------ |
| month        | number |
| year         | number |
| departmentId | UUID   |

## Response

```json
{
  "success": true,
  "data": {
    "totalOvertimeRequests": 120,
    "approvedHours": 350,
    "pendingRequests": 10,
    "rejectedRequests": 5
  }
}
```

# Overtime Report

Generate overtime report.

## Endpoint

```
GET /overtime/report
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| startDate | date   |
| endDate   | date   |
| format    | string |

Available format:

```
PDF
EXCEL
```

## Response

File download.

# Error Responses

## Overtime Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Overtime request not found."
}
```

## Invalid Status

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Only pending overtime can be updated."
}
```

## Duplicate Request

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Overtime request already exists for this date."
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

- overtime_requests
- employees
- attendance_records
- departments
- payrolls
- users
- audit_logs

```
employees
     |
     |
     └──────── overtime_requests
                    |
                    |
             approved_by (users)

```

```
model OvertimeRequest {
  id          String @id @default(uuid())

  employeeId  String
  employee    Employee @relation(
    fields: [employeeId],
    references: [id]
  )

  date        DateTime
  startTime   DateTime
  endTime     DateTime

  duration    Float

  reason      String

  status      OvertimeStatus @default(PENDING)

  approvedBy  String?

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

```
Overtime
│
├── Overtime Dashboard
├── My Overtime History
├── Create Overtime Request
├── Overtime Detail
├── Team Overtime Approval
├── Approval Detail
└── Overtime Report
```

```
Employee Module
        ↓
Attendance Module
        ↓
Overtime Module
        ↓
Payroll Module
```
