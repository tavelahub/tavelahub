# Dashboard API Specification

## Overview

The Dashboard module provides aggregated HR analytics and operational insights.

The dashboard collects data from multiple HRIS modules and displays summarized information for different user roles.

Dashboard data sources:

- Employee Module
- Attendance Module
- Leave Module
- Overtime Module
- Payroll Module
- Reimbursement Module
- Document Module
- Announcement Module

# Base URL

```
/api/v1/dashboard
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role          | Permission                       |
| ------------- | -------------------------------- |
| Super Admin   | View All Dashboard Data          |
| HR Admin      | View HR Dashboard                |
| Finance Admin | View Payroll & Expense Dashboard |
| Manager       | View Team Dashboard              |
| Employee      | View Personal Dashboard          |

# Dashboard Response Object

```json
{
  "period": "2026-06",
  "generatedAt": "2026-06-28T10:00:00Z",
  "data": {}
}
```

# Endpoints

# Get Dashboard Overview

Retrieve main dashboard summary.

## Endpoint

```
GET /dashboard/overview
```

## Query Parameters

| Parameter | Type | Description |
| | | -- |
| period | string | YYYY-MM |
| departmentId | UUID | Filter department |

Example:

```
GET /dashboard/overview?period=2026-06
```

## Response

```json
{
  "success": true,
  "data": {
    "employee": {
      "total": 250,
      "active": 230,
      "inactive": 20
    },

    "attendance": {
      "attendanceRate": 95,
      "lateToday": 12,
      "absentToday": 3
    },

    "leave": {
      "pending": 15,
      "approved": 120
    },

    "overtime": {
      "pending": 8,
      "totalHours": 450
    },

    "payroll": {
      "monthlyExpense": 3500000000
    },

    "reimbursement": {
      "pendingAmount": 25000000
    }
  }
}
```

# Employee Dashboard

Employee statistics overview.

## Endpoint

```
GET /dashboard/employees
```

## Response

```json
{
  "success": true,
  "data": {
    "totalEmployees": 250,
    "newEmployees": 10,
    "terminatedEmployees": 3,
    "departmentDistribution": [
      {
        "department": "Engineering",
        "total": 50
      }
    ]
  }
}
```

# Employee Growth Trend

Retrieve employee growth data.

## Endpoint

```
GET /dashboard/employees/trend
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| year      | number |

## Response

```json
{
  "success": true,
  "data": [
    {
      "month": "January",
      "total": 220
    },
    {
      "month": "February",
      "total": 230
    }
  ]
}
```

# Attendance Dashboard

Attendance statistics.

## Endpoint

```
GET /dashboard/attendance
```

## Query Parameters

| Parameter    | Type   |
| ------------ | ------ |
| period       | string |
| departmentId | UUID   |

## Response

```json
{
  "success": true,
  "data": {
    "attendanceRate": 96,
    "present": 5000,
    "late": 120,
    "absent": 50,
    "leave": 80
  }
}
```

# Attendance Trend

Retrieve attendance trend.

## Endpoint

```
GET /dashboard/attendance/trend
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "date": "2026-06-01",
      "attendanceRate": 97
    }
  ]
}
```

# Leave Dashboard

Leave statistics.

## Endpoint

```
GET /dashboard/leave
```

## Response

```json
{
  "success": true,
  "data": {
    "totalRequests": 150,
    "pending": 20,
    "approved": 120,
    "rejected": 10,

    "topLeaveType": [
      {
        "type": "Annual Leave",
        "days": 300
      }
    ]
  }
}
```

# Overtime Dashboard

Overtime statistics.

## Endpoint

```
GET /dashboard/overtime
```

## Response

```json
{
  "success": true,
  "data": {
    "totalHours": 500,
    "pendingApproval": 15,
    "approvedHours": 450,
    "estimatedCost": 15000000
  }
}
```

# Payroll Dashboard

Payroll overview.

## Endpoint

```
GET /dashboard/payroll
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| period    | string |

## Response

```json
{
  "success": true,
  "data": {
    "totalEmployees": 250,
    "grossSalary": 4000000000,
    "totalDeduction": 100000000,
    "netSalary": 3900000000
  }
}
```

# Reimbursement Dashboard

Expense overview.

## Endpoint

```
GET /dashboard/reimbursement
```

## Response

```json
{
  "success": true,
  "data": {
    "totalRequests": 100,
    "pending": 20,
    "approved": 70,
    "paidAmount": 50000000
  }
}
```

# Document Dashboard

Document management overview.

## Endpoint

```
GET /dashboard/document
```

## Response

```json
{
  "success": true,
  "data": {
    "totalDocuments": 1000,
    "verified": 900,
    "pending": 50,
    "expired": 50
  }
}
```

# Approval Dashboard

Retrieve pending approval tasks.

Used by:

- Manager
- HR Admin

## Endpoint

```
GET /dashboard/approvals
```

## Response

```json
{
  "success": true,
  "data": {
    "leave": 10,
    "overtime": 5,
    "reimbursement": 8,
    "document": 3
  }
}
```

# Personal Dashboard

Employee personal overview.

## Endpoint

```
GET /dashboard/me
```

## Response

```json
{
  "success": true,
  "data": {
    "attendance": {
      "present": 20,
      "late": 1
    },

    "leaveBalance": {
      "annual": 7
    },

    "pendingRequest": {
      "leave": 1,
      "reimbursement": 2
    },

    "notifications": {
      "unread": 5
    }
  }
}
```

# Department Dashboard

Manager dashboard.

## Endpoint

```
GET /dashboard/team
```

## Response

```json
{
  "success": true,
  "data": {
    "totalMembers": 20,

    "attendanceRate": 95,

    "pendingLeave": 3,

    "pendingOvertime": 2
  }
}
```

# Real-time Dashboard Update

Optional endpoint for realtime data refresh.

## Endpoint

```
GET /dashboard/stream
```

## Protocol

```
Server Sent Events (SSE)
```

Example:

```
event:update

{
 "attendanceRate":96
}
```

# Cache Refresh

Refresh dashboard cache.

## Endpoint

```
POST /dashboard/refresh
```

## Permission

```
Super Admin
HR Admin
```

## Response

```json
{
  "success": true,
  "message": "Dashboard cache refreshed."
}
```

# Validation Rules

| Field        | Rule           |
| ------------ | -------------- |
| period       | Format YYYY-MM |
| departmentId | Valid UUID     |
| year         | Valid year     |

# Error Responses

## Unauthorized

```
401 Unauthorized
```

```json
{
  "success": false,
  "message": "Unauthorized access."
}
```

## Invalid Period

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Invalid period format."
}
```

# HTTP Status Codes

| Code | Description           |
| ---- | --------------------- |
| 200  | Success               |
| 201  | Created               |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 422  | Validation Error      |
| 500  | Internal Server Error |

# Related Modules

Dashboard does not own transactional data.

Data sources:

- employees
- attendance_records
- leave_requests
- overtime_requests
- payrolls
- reimbursement_requests
- documents
- announcements
- notifications

# Database Tables

Optional cache tables:

- dashboard_cache
- dashboard_logs

```
                Dashboard Module

                       |
                       |

 ┌─────────┬───────────┬──────────┬──────────┐

Employee Attendance  Leave   Payroll  Expense

                       |
                       |

              PostgreSQL Database
```

```
model DashboardCache {

  id        String @id @default(uuid())

  key       String

  data      Json

  expiresAt DateTime

  createdAt DateTime @default(now())

}
```

```
Dashboard
│
├── HR Dashboard
│   ├── Employee Overview
│   ├── Attendance Chart
│   ├── Leave Summary
│   ├── Payroll Summary
│   └── Expense Summary
│
├── Manager Dashboard
│   ├── Team Attendance
│   ├── Approval Queue
│   └── Team Leave
│
└── Employee Dashboard
    ├── Personal Attendance
    ├── Leave Balance
    ├── Payroll Summary
    └── Notifications
```
