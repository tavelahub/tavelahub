# Payroll API Specification

## Overview

The Payroll module manages employee salary processing, payroll periods, salary calculation, allowances, deductions, payslips, and payroll reports.

The module allows HR Admins to generate payroll, review salary calculations, approve payroll, and provide payslips to employees.

# Base URL

```
/api/v1/payroll
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission                |
| ----------- | ------------------------- |
| Super Admin | Full Access               |
| HR Admin    | Full Access               |
| Manager     | View Team Payroll Summary |
| Employee    | View Own Payslip          |

# Payroll Object

```json
{
  "id": "pay_001",
  "employeeId": "emp_001",
  "period": "2026-06",
  "basicSalary": 10000000,
  "allowance": 1500000,
  "overtimePay": 500000,
  "deduction": 300000,
  "tax": 200000,
  "netSalary": 11500000,
  "status": "DRAFT"
}
```

# Payroll Status

Available values:

```
DRAFT
PROCESSING
COMPLETED
APPROVED
PAID
CANCELLED
```

# Endpoints

# Get Payroll List

Retrieve payroll records.

## Endpoint

```
GET /payroll
```

## Query Parameters

| Parameter    | Type   | Description            |
| ------------ | ------ | ---------------------- |
| page         | number | Current page           |
| limit        | number | Data limit             |
| employeeId   | UUID   | Filter employee        |
| departmentId | UUID   | Filter department      |
| period       | string | Payroll period YYYY-MM |
| status       | string | Payroll status         |

Example:

```
GET /payroll?period=2026-06&status=COMPLETED
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Payroll retrieved successfully.",
  "data": [
    {
      "id": "pay_001",
      "employee": "John Doe",
      "period": "2026-06",
      "netSalary": 11500000,
      "status": "COMPLETED"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

# Get My Payslip

Employee retrieves their own payslip.

## Endpoint

```
GET /payroll/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| period    | string |

Example:

```
GET /payroll/me?period=2026-06
```

## Response

```json
{
  "success": true,
  "data": {
    "period": "2026-06",
    "basicSalary": 10000000,
    "allowance": 1500000,
    "deduction": 300000,
    "netSalary": 11200000
  }
}
```

# Get Payroll Detail

Retrieve payroll detail.

## Endpoint

```
GET /payroll/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "employee": "John Doe",
    "period": "2026-06",
    "basicSalary": 10000000,
    "allowances": [
      {
        "name": "Transport",
        "amount": 500000
      }
    ],
    "deductions": [
      {
        "name": "Late Deduction",
        "amount": 100000
      }
    ],
    "netSalary": 11000000
  }
}
```

# Create Payroll Period

Create a payroll period.

## Endpoint

```
POST /payroll/period
```

## Request Body

```json
{
  "period": "2026-06",
  "paymentDate": "2026-06-30"
}
```

## Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Payroll period created successfully."
}
```

# Generate Payroll

Generate payroll calculation for all employees.

## Endpoint

```
POST /payroll/generate
```

## Request Body

```json
{
  "period": "2026-06"
}
```

## Response

```json
{
  "success": true,
  "message": "Payroll generation started.",
  "data": {
    "period": "2026-06",
    "totalEmployees": 150
  }
}
```

# Payroll Calculation

Calculation formula:

```
Net Salary =
Basic Salary
+ Allowances
+ Overtime Payment
- Deductions
- Tax
```

# Recalculate Payroll

Recalculate payroll data.

## Endpoint

```
POST /payroll/{id}/recalculate
```

## Response

```json
{
  "success": true,
  "message": "Payroll recalculated successfully."
}
```

# Update Payroll

Update payroll manually.

## Endpoint

```
PUT /payroll/{id}
```

## Request Body

```json
{
  "allowance": 2000000,
  "deduction": 500000
}
```

## Response

```json
{
  "success": true,
  "message": "Payroll updated successfully."
}
```

# Approve Payroll

Approve payroll before payment.

## Endpoint

```
PATCH /payroll/{id}/approve
```

## Response

```json
{
  "success": true,
  "message": "Payroll approved successfully."
}
```

# Mark Payroll as Paid

Mark salary as paid.

## Endpoint

```
PATCH /payroll/{id}/paid
```

## Request Body

```json
{
  "paymentReference": "TRX-123456"
}
```

## Response

```json
{
  "success": true,
  "message": "Payroll marked as paid."
}
```

# Cancel Payroll

Cancel payroll process.

## Endpoint

```
PATCH /payroll/{id}/cancel
```

## Response

```json
{
  "success": true,
  "message": "Payroll cancelled."
}
```

# Salary Structure

Manage employee salary components.

# Get Salary Structure

## Endpoint

```
GET /payroll/salary-structure/{employeeId}
```

## Response

```json
{
  "success": true,
  "data": {
    "basicSalary": 10000000,
    "allowances": [
      {
        "name": "Transport",
        "amount": 500000
      }
    ]
  }
}
```

# Update Salary Structure

## Endpoint

```
PUT /payroll/salary-structure/{employeeId}
```

## Request Body

```json
{
  "basicSalary": 12000000,
  "allowances": [
    {
      "name": "Meal",
      "amount": 500000
    }
  ]
}
```

# Payroll Summary

Retrieve payroll statistics.

## Endpoint

```
GET /payroll/summary
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
    "totalEmployees": 150,
    "totalGrossSalary": 2000000000,
    "totalDeductions": 50000000,
    "totalNetSalary": 1950000000
  }
}
```

# Payroll Report

Generate payroll report.

## Endpoint

```
GET /payroll/report
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| period    | string |
| format    | string |

Available format:

```
PDF
EXCEL
```

## Response

File download.

# Payslip Download

Download employee payslip.

## Endpoint

```
GET /payroll/{id}/payslip
```

## Response

PDF File

# Validation Rules

## Payroll Generation

| Field  | Rule           |
| ------ | -------------- |
| period | Required       |
| period | Format YYYY-MM |

# Error Responses

## Payroll Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Payroll record not found."
}
```

## Invalid Payroll Status

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Payroll cannot be modified after approval."
}
```

## Duplicate Payroll

```
409 Conflict
```

```json
{
  "success": false,
  "message": "Payroll already exists for this period."
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

- payrolls
- payroll_periods
- salary_structures
- salary_components
- employees
- attendance_records
- overtime_requests
- leave_requests
- users
- audit_logs

```
employees
    |
    |
    └──────── payrolls
                  |
                  |
        ┌─────────┼─────────┐
        |         |         |
 salary_structure allowances deductions
        |
        |
 attendance + overtime + leave
```

```
model Payroll {
  id          String @id @default(uuid())

  employeeId  String
  employee    Employee @relation(
    fields: [employeeId],
    references: [id]
  )

  period      String

  basicSalary Decimal
  allowance   Decimal
  overtimePay Decimal
  deduction   Decimal
  tax         Decimal
  netSalary   Decimal

  status      PayrollStatus @default(DRAFT)

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

```
Payroll
│
├── Payroll Dashboard
├── Generate Payroll
├── My Payslip
├── Team Payroll
├── Payroll Detail
├── Approve Payroll
├── Mark as Paid
├── Cancel Payroll
└── Payroll Report
```

```
Employee Module
        ↓
Attendance Module
        ↓
Leave Module
        ↓
Overtime Module
        ↓
Payroll Module
```
