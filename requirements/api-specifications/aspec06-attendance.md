# Attendance API Specification

## Overview

The Attendance module manages employee attendance records, including check-in, check-out, attendance history, attendance correction requests, and attendance reports.

The module helps HR and managers monitor employee working hours, punctuality, and attendance performance.

# Base URL

```
/api/v1/attendance
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission                      |
| ----------- | ------------------------------- |
| Super Admin | Full Access                     |
| HR Admin    | Full Access                     |
| Manager     | View Team Attendance & Approval |
| Employee    | Manage Own Attendance           |

# Attendance Object

```json
{
  "id": "att_001",
  "employeeId": "emp_001",
  "date": "2026-06-28",
  "checkIn": "2026-06-28T08:01:00Z",
  "checkOut": "2026-06-28T17:05:00Z",
  "workingHours": 8.5,
  "status": "PRESENT",
  "lateMinutes": 1
}
```

# Attendance Status

Available values:

```
PRESENT
LATE
ABSENT
LEAVE
SICK
HOLIDAY
HALF_DAY
```

# Endpoints

# Check In

Employee starts working session.

## Endpoint

```
POST /attendance/check-in
```

## Authentication

Required

Employee can only check-in for themselves.

## Request Body

```json
{
  "latitude": -6.2,
  "longitude": 106.816666,
  "deviceInfo": "Chrome Windows"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Check-in successful.",
  "data": {
    "id": "att_001",
    "checkIn": "2026-06-28T08:01:00Z",
    "status": "PRESENT"
  }
}
```

## Business Rules

- Employee cannot check-in twice on the same day.
- Check-in time determines late status.
- Location validation is optional.

# Check Out

Employee ends working session.

## Endpoint

```
POST /attendance/check-out
```

## Request Body

```json
{
  "latitude": -6.2,
  "longitude": 106.816666
}
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Check-out successful.",
  "data": {
    "checkOut": "2026-06-28T17:05:00Z",
    "workingHours": 8.5
  }
}
```

## Business Rules

- Employee must check-in before check-out.
- Cannot check-out without active attendance record.

# Get My Attendance History

Retrieve current user's attendance records.

## Endpoint

```
GET /attendance/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| startDate | date   |
| endDate   | date   |
| month     | number |
| year      | number |

Example:

```
GET /attendance/me?month=6&year=2026
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "data": [
    {
      "date": "2026-06-28",
      "checkIn": "08:01",
      "checkOut": "17:05",
      "status": "PRESENT"
    }
  ]
}
```

# Get Employee Attendance

Retrieve attendance records of an employee.

## Endpoint

```
GET /attendance/employees/{employeeId}
```

## Permission

- HR Admin
- Manager (team only)

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| startDate | date   |
| endDate   | date   |
| status    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "date": "2026-06-28",
      "status": "PRESENT",
      "workingHours": 8
    }
  ]
}
```

# Get Attendance List

Retrieve all employee attendance.

## Endpoint

```
GET /attendance
```

## Query Parameters

| Parameter    | Type   |
| ------------ | ------ |
| page         | number |
| limit        | number |
| employeeId   | UUID   |
| departmentId | UUID   |
| date         | date   |
| status       | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "employee": "John Doe",
      "date": "2026-06-28",
      "status": "PRESENT"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

# Attendance Detail

Retrieve attendance detail.

## Endpoint

```
GET /attendance/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "att_001",
    "employee": "John Doe",
    "date": "2026-06-28",
    "checkIn": "08:01",
    "checkOut": "17:05"
  }
}
```

# Update Attendance

Update attendance record manually.

Used by HR Admin for corrections.

## Endpoint

```
PUT /attendance/{id}
```

## Request Body

```json
{
  "checkIn": "08:00",
  "checkOut": "17:00",
  "status": "PRESENT",
  "notes": "Manual correction"
}
```

## Response

```json
{
  "success": true,
  "message": "Attendance updated successfully."
}
```

# Delete Attendance

Remove attendance record.

## Endpoint

```
DELETE /attendance/{id}
```

## Response

```
204 No Content
```

# Attendance Correction Request

Employee requests attendance correction.

## Endpoint

```
POST /attendance/correction-request
```

## Request Body

```json
{
  "attendanceId": "att_001",
  "reason": "Forgot to check-in"
}
```

## Response

```json
{
  "success": true,
  "message": "Correction request submitted."
}
```

# Get Correction Requests

Retrieve attendance correction requests.

## Endpoint

```
GET /attendance/correction-requests
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "req_001",
      "employee": "John Doe",
      "reason": "Forgot check-in",
      "status": "PENDING"
    }
  ]
}
```

# Approve Correction Request

Manager or HR approves correction.

## Endpoint

```
PATCH /attendance/correction-requests/{id}/approve
```

## Response

```json
{
  "success": true,
  "message": "Correction request approved."
}
```

# Reject Correction Request

Reject correction request.

## Endpoint

```
PATCH /attendance/correction-requests/{id}/reject
```

## Request Body

```json
{
  "reason": "Invalid request"
}
```

# Attendance Summary

Retrieve attendance statistics.

## Endpoint

```
GET /attendance/summary
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
    "totalEmployees": 150,
    "present": 130,
    "late": 10,
    "absent": 5,
    "leave": 5
  }
}
```

# Attendance Report

Generate attendance report.

## Endpoint

```
GET /attendance/report
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

# Validation Rules

## Check In

| Field     | Rule     |
| --------- | -------- |
| latitude  | Optional |
| longitude | Optional |

## Attendance Correction

| Field        | Rule     |
| ------------ | -------- |
| attendanceId | Required |
| reason       | Required |

# Error Responses

## Already Checked In

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Employee already checked in today."
}
```

## Attendance Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Attendance record not found."
}
```

## Invalid Action

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Employee must check-in first."
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

- attendance_records
- attendance_corrections
- employees
- departments
- users
- audit_logs
