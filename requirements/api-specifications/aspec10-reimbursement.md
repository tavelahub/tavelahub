# Reimbursement API Specification

## Overview

The Reimbursement module manages employee expense reimbursement requests.

Employees can submit expense claims with supporting documents, managers can review requests, HR/Admin can approve and process payments, and employees can track reimbursement status.

Examples of reimbursable expenses:

- Transportation
- Medical expenses
- Business travel
- Meals
- Equipment purchase
- Other company-approved expenses

# Base URL

```
/api/v1/reimbursements
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role          | Permission            |
| ------------- | --------------------- |
| Super Admin   | Full Access           |
| HR Admin      | Full Access           |
| Finance Admin | Process Payment       |
| Manager       | Approve Team Requests |
| Employee      | Create Own Request    |

# Reimbursement Object

```json
{
  "id": "req_001",
  "employeeId": "emp_001",
  "categoryId": "cat_001",
  "amount": 500000,
  "description": "Client meeting transportation",
  "expenseDate": "2026-06-20",
  "receiptUrl": "/uploads/receipt.jpg",
  "status": "PENDING",
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Reimbursement Status

Available values:

```
DRAFT
PENDING
APPROVED
REJECTED
PAID
CANCELLED
```

# Endpoints

# Get Reimbursement Requests

Retrieve reimbursement request list.

## Endpoint

```
GET /reimbursements
```

## Query Parameters

| Parameter    | Type   | Description       |
| ------------ | ------ | ----------------- |
| page         | number | Current page      |
| limit        | number | Data limit        |
| employeeId   | UUID   | Filter employee   |
| categoryId   | UUID   | Filter category   |
| departmentId | UUID   | Filter department |
| status       | string | Filter status     |
| startDate    | date   | Start period      |
| endDate      | date   | End period        |

Example:

```
GET /reimbursements?page=1&status=PENDING
```

## Success Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Reimbursement requests retrieved successfully.",
  "data": [
    {
      "id": "req_001",
      "employee": "John Doe",
      "category": "Transportation",
      "amount": 500000,
      "status": "PENDING"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 30,
    "totalPages": 3
  }
}
```

# Get My Reimbursements

Retrieve current employee reimbursement history.

## Endpoint

```
GET /reimbursements/me
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| status    | string |
| month     | number |
| year      | number |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "req_001",
      "category": "Medical",
      "amount": 300000,
      "status": "APPROVED"
    }
  ]
}
```

# Get Reimbursement Detail

Retrieve reimbursement detail.

## Endpoint

```
GET /reimbursements/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "req_001",
    "employee": "John Doe",
    "category": "Transportation",
    "amount": 500000,
    "description": "Client meeting",
    "receiptUrl": "/uploads/receipt.jpg",
    "status": "PENDING"
  }
}
```

# Create Reimbursement Request

Employee creates reimbursement request.

## Endpoint

```
POST /reimbursements
```

## Request Body

```json
{
  "categoryId": "cat_001",
  "amount": 500000,
  "expenseDate": "2026-06-20",
  "description": "Transportation for client meeting",
  "receiptUrl": "/uploads/receipt.jpg"
}
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Reimbursement request submitted successfully.",
  "data": {
    "id": "req_001"
  }
}
```

# Validation Rules

| Field       | Rule                     |
| ----------- | ------------------------ |
| categoryId  | Required                 |
| amount      | Required, greater than 0 |
| expenseDate | Required                 |
| description | Required                 |
| receiptUrl  | Required                 |

# Update Reimbursement Request

Update reimbursement request.

Only requests with status:

```
DRAFT
PENDING
```

can be updated.

## Endpoint

```
PUT /reimbursements/{id}
```

## Request Body

```json
{
  "amount": 600000,
  "description": "Updated transportation expense",
  "receiptUrl": "/uploads/new-receipt.jpg"
}
```

## Response

```json
{
  "success": true,
  "message": "Reimbursement updated successfully."
}
```

# Delete Reimbursement Request

Delete reimbursement request.

Only requests with status:

```
DRAFT
PENDING
```

can be deleted.

## Endpoint

```
DELETE /reimbursements/{id}
```

## Response

```
204 No Content
```

# Cancel Reimbursement Request

Employee cancels reimbursement request.

## Endpoint

```
PATCH /reimbursements/{id}/cancel
```

## Response

```json
{
  "success": true,
  "message": "Reimbursement cancelled successfully."
}
```

# Approve Reimbursement

Manager or HR approves reimbursement request.

## Endpoint

```
PATCH /reimbursements/{id}/approve
```

## Request Body

```json
{
  "note": "Approved based on company policy"
}
```

## Response

```json
{
  "success": true,
  "message": "Reimbursement approved successfully."
}
```

# Reject Reimbursement

Reject reimbursement request.

## Endpoint

```
PATCH /reimbursements/{id}/reject
```

## Request Body

```json
{
  "reason": "Receipt is invalid"
}
```

## Response

```json
{
  "success": true,
  "message": "Reimbursement rejected successfully."
}
```

# Mark Reimbursement as Paid

Finance/Admin marks reimbursement as paid.

## Endpoint

```
PATCH /reimbursements/{id}/paid
```

## Request Body

```json
{
  "paymentReference": "TRX-20260628-001",
  "paymentDate": "2026-06-30"
}
```

## Response

```json
{
  "success": true,
  "message": "Reimbursement marked as paid."
}
```

# Upload Receipt

Upload reimbursement receipt.

## Endpoint

```
POST /reimbursements/{id}/receipt
```

## Content-Type

```
multipart/form-data
```

## Request

```
file: receipt_image_or_pdf
```

## Response

```json
{
  "success": true,
  "message": "Receipt uploaded successfully.",
  "data": {
    "receiptUrl": "/uploads/reimbursements/receipt.pdf"
  }
}
```

# Get Reimbursement Categories

Retrieve available reimbursement categories.

## Endpoint

```
GET /reimbursements/categories
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "cat_001",
      "name": "Transportation",
      "limit": 1000000
    },
    {
      "id": "cat_002",
      "name": "Medical",
      "limit": 5000000
    }
  ]
}
```

# Create Reimbursement Category

Create expense category.

## Endpoint

```
POST /reimbursements/categories
```

## Request Body

```json
{
  "name": "Business Travel",
  "description": "Travel expenses for company activities",
  "limit": 5000000
}
```

## Response

```json
{
  "success": true,
  "message": "Category created successfully."
}
```

# Update Reimbursement Category

Update reimbursement category.

## Endpoint

```
PUT /reimbursements/categories/{id}
```

## Request Body

```json
{
  "limit": 7500000
}
```

# Delete Reimbursement Category

Delete reimbursement category.

## Endpoint

```
DELETE /reimbursements/categories/{id}
```

## Response

```
204 No Content
```

# Reimbursement Summary

Retrieve reimbursement statistics.

## Endpoint

```
GET /reimbursements/summary
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
    "totalRequests": 100,
    "pendingRequests": 15,
    "approvedRequests": 70,
    "paidAmount": 25000000
  }
}
```

# Reimbursement Report

Generate reimbursement report.

## Endpoint

```
GET /reimbursements/report
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

## Reimbursement Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Reimbursement request not found."
}
```

## Invalid Status

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "This reimbursement cannot be modified."
}
```

## Exceeded Limit

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Reimbursement amount exceeds category limit."
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

- reimbursement_requests
- reimbursement_categories
- reimbursement_attachments
- employees
- departments
- payrolls
- users
- audit_logs

```
employees
    |
    |
    └──────── reimbursement_requests
                    |
                    |
          ┌─────────┴─────────┐
          |                   |
 reimbursement_categories   attachments
          |
          |
       payrolls
```

```
model ReimbursementRequest {
  id          String @id @default(uuid())

  employeeId  String
  employee    Employee @relation(
    fields: [employeeId],
    references: [id]
  )

  categoryId  String
  category    ReimbursementCategory @relation(
    fields: [categoryId],
    references: [id]
  )

  amount      Decimal
  expenseDate DateTime

  description String

  receiptUrl  String?

  status      ReimbursementStatus @default(PENDING)

  approvedBy  String?
  paidAt      DateTime?

  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

```
Reimbursement
│
├── Reimbursement Dashboard
├── My Requests
├── Create Request
├── Request Detail
├── Upload Receipt
├── Approval Queue
├── Payment Processing
├── Categories Management
└── Reports
```

```
Employee Module
        ↓
User & Authentication
        ↓
Reimbursement Module
        ↓
Payroll Module
```
