# Leave API Specification

## Overview

The Leave module manages employee leave requests, leave balances, leave policies, and approval workflows.

Employees can submit leave requests, managers can approve or reject requests, and HR Admins can manage leave types, balances, and leave reports.

Supported leave examples:

- Annual Leave
- Sick Leave
- Maternity Leave
- Emergency Leave
- Unpaid Leave
- Personal Leave

# Base URL

```
/api/v1/leaves
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission               |
| ----------- | ------------------------ |
| Super Admin | Full Access              |
| HR Admin    | Full Access              |
| Manager     | Approve Team Leave       |
| Employee    | Create Own Leave Request |

# Leave Object

```json
{
  "id": "leave_001",
  "employeeId": "emp_001",
  "leaveTypeId": "type_annual",
  "startDate": "2026-07-01",
  "endDate": "2026-07-03",
  "totalDays": 3,
  "reason": "Family event",
  "status": "PENDING",
  "approvedBy": null,
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Leave Status

Available values:

```
PENDING
APPROVED
REJECTED
CANCELLED
```

# Endpoints

# Get Leave Requests

Retrieve leave request list.

## Endpoint

```
GET /leaves
```

## Query Parameters

| Parameter    | Type   | Description       |
| ------------ | ------ | ----------------- |
| page         | number | Current page      |
| limit        | number | Data limit        |
| employeeId   | UUID   | Filter employee   |
| departmentId | UUID   | Filter department |
| leaveTypeId  | UUID   | Filter leave type |
| status       | string | Filter status     |
| startDate    | date   | Start date        |
| endDate      | date   | End date          |

Example:

```
GET /leaves?page=1&status=PENDING
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Leave requests retrieved successfully.",
  "data": [
    {
      "id": "leave_001",
      "employee": "John Doe",
      "type": "Annual Leave",
      "totalDays": 3,
      "status": "PENDING"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 50
  }
}
```

# Get My Leave Requests

Retrieve current employee leave history.

## Endpoint

```
GET /leaves/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| year      | number |
| status    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "leave_001",
      "type": "Annual Leave",
      "startDate": "2026-07-01",
      "endDate": "2026-07-03",
      "status": "APPROVED"
    }
  ]
}
```

# Get Leave Detail

Retrieve leave request detail.

## Endpoint

```
GET /leaves/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "leave_001",
    "employee": "John Doe",
    "leaveType": "Annual Leave",
    "startDate": "2026-07-01",
    "endDate": "2026-07-03",
    "totalDays": 3,
    "reason": "Family event",
    "status": "APPROVED"
  }
}
```

# Create Leave Request

Employee submits leave request.

## Endpoint

```
POST /leaves
```

## Request Body

```json
{
  "leaveTypeId": "type_annual",
  "startDate": "2026-07-01",
  "endDate": "2026-07-03",
  "reason": "Family event"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Leave request submitted successfully.",
  "data": {
    "id": "leave_001"
  }
}
```

# Validation Rules

| Field       | Rule                   |
| ----------- | ---------------------- |
| leaveTypeId | Required               |
| startDate   | Required               |
| endDate     | Required               |
| reason      | Required               |
| totalDays   | Must be greater than 0 |

# Update Leave Request

Update pending leave request.

Only available for:

```
PENDING
```

status.

## Endpoint

```
PUT /leaves/{id}
```

## Request Body

```json
{
  "startDate": "2026-07-02",
  "endDate": "2026-07-04",
  "reason": "Updated schedule"
}
```

## Response

```json
{
  "success": true,
  "message": "Leave request updated successfully."
}
```

# Cancel Leave Request

Employee cancels leave request.

## Endpoint

```
PATCH /leaves/{id}/cancel
```

## Response

```json
{
  "success": true,
  "message": "Leave request cancelled successfully."
}
```

# Approve Leave Request

Manager or HR approves leave.

## Endpoint

```
PATCH /leaves/{id}/approve
```

## Request Body

```json
{
  "note": "Approved"
}
```

## Response

```json
{
  "success": true,
  "message": "Leave request approved successfully."
}
```

# Reject Leave Request

Reject leave request.

## Endpoint

```
PATCH /leaves/{id}/reject
```

## Request Body

```json
{
  "reason": "Insufficient leave balance"
}
```

## Response

```json
{
  "success": true,
  "message": "Leave request rejected successfully."
}
```

# Get Leave Balance

Retrieve employee leave balance.

## Endpoint

```
GET /leaves/balance
```

## Query Parameters

| Parameter  | Type   |
| ---------- | ------ |
| employeeId | UUID   |
| year       | number |

## Response

```json
{
  "success": true,
  "data": [
    {
      "leaveType": "Annual Leave",
      "allocated": 12,
      "used": 5,
      "remaining": 7
    }
  ]
}
```

# Update Leave Balance

Update employee leave quota.

## Endpoint

```
PUT /leaves/balance/{employeeId}
```

## Request Body

```json
{
  "leaveTypeId": "type_annual",
  "allocated": 14
}
```

## Response

```json
{
  "success": true,
  "message": "Leave balance updated successfully."
}
```

# Get Leave Calendar

Retrieve company leave calendar.

## Endpoint

```
GET /leaves/calendar
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
  "data": [
    {
      "employee": "John Doe",
      "date": "2026-07-01",
      "type": "Annual Leave"
    }
  ]
}
```

# Leave Type Management

Manage available leave categories.

# Get Leave Types

## Endpoint

```
GET /leaves/types
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "type_annual",
      "name": "Annual Leave",
      "defaultQuota": 12
    }
  ]
}
```

# Create Leave Type

## Endpoint

```
POST /leaves/types
```

## Request Body

```json
{
  "name": "Annual Leave",
  "defaultQuota": 12,
  "description": "Yearly employee leave"
}
```

# Update Leave Type

## Endpoint

```
PUT /leaves/types/{id}
```

# Delete Leave Type

## Endpoint

```
DELETE /leaves/types/{id}
```

# Leave Summary

Retrieve leave statistics.

## Endpoint

```
GET /leaves/summary
```

## Query Parameters

| Parameter    | Type   |
| ------------ | ------ |
| year         | number |
| departmentId | UUID   |

## Response

```json
{
  "success": true,
  "data": {
    "totalRequests": 150,
    "approved": 120,
    "pending": 20,
    "rejected": 10,
    "totalDaysUsed": 350
  }
}
```

# Leave Report

Generate leave report.

## Endpoint

```
GET /leaves/report
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| startDate | date   |
| endDate   | date   |
| format    | string |

Available:

```
PDF
EXCEL
```

# Error Responses

## Leave Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Leave request not found."
}
```

## Insufficient Balance

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Insufficient leave balance."
}
```

## Invalid Status

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Leave request cannot be modified."
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

- leave_requests
- leave_types
- leave_balances
- employees
- departments
- attendance_records
- payrolls
- users
- audit_logs

```
employees
    |
    |
    └──────── leave_requests
                    |
                    |
             leave_types
                    |
                    |
             leave_balances
```

```
model LeaveRequest {
  id          String @id @default(uuid())

  employeeId  String
  employee    Employee @relation(
    fields: [employeeId],
    references: [id]
  )

  leaveTypeId String
  leaveType   LeaveType @relation(
    fields: [leaveTypeId],
    references: [id]
  )

  startDate   DateTime
  endDate     DateTime

  totalDays   Float

  reason      String

  status      LeaveStatus @default(PENDING)

  approvedBy  String?

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

```

Leave Management
│
├── Leave Dashboard
├── My Leave Requests
├── Create Leave Request
├── Leave Detail
├── Leave Balance
├── Approval Queue
├── Leave Calendar
├── Leave Type Management
└── Leave Report

```
