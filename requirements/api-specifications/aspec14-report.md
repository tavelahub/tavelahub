# Report API Specification

## Overview

The Report module provides data analytics and reporting capabilities for HR operations.

HR Admins and Managers can generate reports based on employee data, attendance, leave, payroll, reimbursement, and other HR activities.

Supported report formats:

- PDF
- Excel
- CSV

# Base URL

```
/api/v1/reports
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role          | Permission                |
| ------------- | ------------------------- |
| Super Admin   | Full Access               |
| HR Admin      | Generate All Reports      |
| Finance Admin | Payroll & Expense Reports |
| Manager       | Team Reports              |
| Employee      | Own Reports               |

# Report Object

```json
{
  "id": "report_001",
  "name": "Monthly Attendance Report",
  "type": "ATTENDANCE",
  "format": "PDF",
  "status": "COMPLETED",
  "generatedBy": "user_001",
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Report Types

Available values:

```
EMPLOYEE
ATTENDANCE
LEAVE
OVERTIME
PAYROLL
REIMBURSEMENT
DOCUMENT
ANNOUNCEMENT
PERFORMANCE
CUSTOM
```

# Report Status

Available values:

```
PROCESSING
COMPLETED
FAILED
EXPIRED
```

# Endpoints

# Get Available Reports

Retrieve available report templates.

## Endpoint

```
GET /reports
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| type      | string |
| search    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "attendance_monthly",
      "name": "Monthly Attendance Report",
      "type": "ATTENDANCE"
    },
    {
      "id": "payroll_summary",
      "name": "Payroll Summary",
      "type": "PAYROLL"
    }
  ]
}
```

# Generate Report

Generate a report.

## Endpoint

```
POST /reports/generate
```

## Request Body

```json
{
  "reportType": "ATTENDANCE",
  "format": "PDF",
  "filters": {
    "startDate": "2026-06-01",
    "endDate": "2026-06-30",
    "departmentId": "dept_001"
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
  "message": "Report generation started.",
  "data": {
    "reportId": "report_001"
  }
}
```

# Get Generated Report History

Retrieve generated reports.

## Endpoint

```
GET /reports/history
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| page      | number |
| limit     | number |
| type      | string |
| status    | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "report_001",
      "name": "Attendance Report",
      "status": "COMPLETED",
      "downloadUrl": "/reports/report.pdf"
    }
  ]
}
```

# Get Report Detail

Retrieve report information.

## Endpoint

```
GET /reports/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "report_001",
    "type": "PAYROLL",
    "format": "PDF",
    "status": "COMPLETED",
    "createdAt": "2026-06-28"
  }
}
```

# Download Report

Download generated report file.

## Endpoint

```
GET /reports/{id}/download
```

## Response

File download:

```
application/pdf

or

application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
```

# Delete Report

Delete generated report.

## Endpoint

```
DELETE /reports/{id}
```

## Response

```
204 No Content
```

# Employee Report

Generate employee report.

## Endpoint

```
POST /reports/employee
```

## Request Body

```json
{
  "format": "EXCEL",
  "filters": {
    "departmentId": "dept_001",
    "employmentStatus": "ACTIVE"
  }
}
```

## Output Data

Contains:

- Employee profile
- Department
- Position
- Join date
- Employment status

# Attendance Report

Generate attendance report.

## Endpoint

```
POST /reports/attendance
```

## Request Body

```json
{
  "format": "PDF",
  "period": "2026-06",
  "departmentId": "dept_001"
}
```

## Output Data

Contains:

- Total working days
- Present days
- Late count
- Absent count
- Attendance percentage

# Leave Report

Generate leave report.

## Endpoint

```
POST /reports/leave
```

## Request Body

```json
{
  "format": "EXCEL",
  "year": 2026
}
```

## Output Data

Contains:

- Leave usage
- Remaining balance
- Leave type summary
- Employee leave history

# Overtime Report

Generate overtime report.

## Endpoint

```
POST /reports/overtime
```

## Request Body

```json
{
  "format": "PDF",
  "month": "2026-06"
}
```

## Output Data

Contains:

- Employee overtime hours
- Approved overtime
- Overtime payment

# Payroll Report

Generate payroll report.

## Endpoint

```
POST /reports/payroll
```

## Request Body

```json
{
  "format": "EXCEL",
  "period": "2026-06"
}
```

## Output Data

Contains:

- Gross salary
- Allowance
- Deduction
- Tax
- Net salary

# Reimbursement Report

Generate reimbursement report.

## Endpoint

```
POST /reports/reimbursement
```

## Request Body

```json
{
  "format": "PDF",
  "startDate": "2026-06-01",
  "endDate": "2026-06-30"
}
```

## Output Data

Contains:

- Total reimbursement
- Approved requests
- Paid amount
- Expense category

# Dashboard Summary

Retrieve HR dashboard statistics.

## Endpoint

```
GET /reports/dashboard
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
    "attendanceRate": 95,
    "pendingLeave": 15,
    "monthlyPayroll": 3500000000,
    "pendingReimbursement": 10
  }
}
```

# Custom Report

Create custom report query.

## Endpoint

```
POST /reports/custom
```

## Request Body

```json
{
  "name": "Employee Attendance Summary",
  "modules": ["employee", "attendance"],
  "columns": ["employeeName", "attendanceRate"],
  "filters": {
    "departmentId": "dept_001"
  }
}
```

## Response

```json
{
  "success": true,
  "message": "Custom report generated."
}
```

# Scheduled Report

Create automatic report schedule.

## Endpoint

```
POST /reports/schedule
```

## Request Body

```json
{
  "reportType": "PAYROLL",
  "frequency": "MONTHLY",
  "format": "PDF",
  "recipient": ["hr@company.com"]
}
```

## Frequency

Available:

```
DAILY
WEEKLY
MONTHLY
YEARLY
```

# Validation Rules

| Field      | Rule                          |
| ---------- | ----------------------------- |
| reportType | Required                      |
| format     | Required                      |
| date range | Valid period                  |
| recipient  | Required for scheduled report |

# Error Responses

## Report Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Report not found."
}
```

## Report Generation Failed

```
500 Internal Server Error
```

```json
{
  "success": false,
  "message": "Failed to generate report."
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

- reports
- report_templates
- report_schedules
- report_logs
- employees
- attendance_records
- leave_requests
- overtime_requests
- payrolls
- reimbursement_requests
- users
- audit_logs

```
users
 |
 |
 └──────── reports
                |
        ┌───────┼────────┐
        |       |        |
 templates schedules logs


reports
 |
 |
generated from:

Employee
Attendance
Leave
Overtime
Payroll
Reimbursement
Document
```

```
model Report {
  id          String @id @default(uuid())

  name        String

  type        ReportType

  format      ReportFormat

  status      ReportStatus
      @default(PROCESSING)

  fileUrl     String?

  generatedBy String

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

```
Report Management
│
├── Report Dashboard
├── Report Template List
├── Generate Report
├── Report History
├── Report Preview
├── Download Report
├── Scheduled Report
└── Custom Report Builder
```

```
All Core Modules
        ↓
Dashboard Module
        ↓
Report Module
        ↓
Export Service
        ↓
Scheduled Job System
```
