# Document API Specification

## Overview

The Document module manages employee and company documents.

Employees can upload and access their own documents, while HR Admins can manage, verify, categorize, and monitor employee documents.

The module supports document upload, document versioning, expiration tracking, and document verification.

# Base URL

```
/api/v1/documents
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission           |
| ----------- | -------------------- |
| Super Admin | Full Access          |
| HR Admin    | Full Access          |
| Manager     | View Team Documents  |
| Employee    | Manage Own Documents |

# Document Object

```json
{
  "id": "doc_001",
  "employeeId": "emp_001",
  "categoryId": "cat_contract",
  "name": "Employment Contract",
  "fileUrl": "/uploads/contracts/contract.pdf",
  "fileType": "PDF",
  "fileSize": 1024000,
  "issueDate": "2026-01-01",
  "expiryDate": "2027-01-01",
  "status": "VERIFIED",
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Document Status

Available values:

```
PENDING
VERIFIED
REJECTED
EXPIRED
ARCHIVED
```

# Endpoints

# Get Documents

Retrieve document list.

## Endpoint

```
GET /documents
```

## Query Parameters

| Parameter  | Type   | Description          |
| ---------- | ------ | -------------------- |
| page       | number | Current page         |
| limit      | number | Data limit           |
| employeeId | UUID   | Filter employee      |
| categoryId | UUID   | Filter category      |
| status     | string | Filter status        |
| search     | string | Search document name |

Example:

```
GET /documents?page=1&status=VERIFIED
```

## Response

```json
{
  "success": true,
  "message": "Documents retrieved successfully.",
  "data": [
    {
      "id": "doc_001",
      "employee": "John Doe",
      "name": "Employment Contract",
      "status": "VERIFIED"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 50
  }
}
```

# Get My Documents

Employee retrieves own documents.

## Endpoint

```
GET /documents/me
```

## Query Parameters

| Parameter  | Type   |
| ---------- | ------ |
| categoryId | UUID   |
| status     | string |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "doc_001",
      "name": "Identity Card",
      "status": "VERIFIED"
    }
  ]
}
```

# Get Document Detail

Retrieve document detail.

## Endpoint

```
GET /documents/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "doc_001",
    "employee": "John Doe",
    "category": "Contract",
    "name": "Employment Contract",
    "fileUrl": "/uploads/document.pdf",
    "status": "VERIFIED"
  }
}
```

# Upload Document

Upload employee document.

## Endpoint

```
POST /documents
```

## Content-Type

```
multipart/form-data
```

## Request

```
file: document_file

employeeId: emp_001

categoryId: cat_contract

name: Employment Contract

issueDate: 2026-01-01

expiryDate: 2027-01-01
```

## Success Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Document uploaded successfully.",
  "data": {
    "id": "doc_001"
  }
}
```

# Upload Document Version

Upload new document version.

## Endpoint

```
POST /documents/{id}/version
```

## Request

```
file: updated_document.pdf
```

## Response

```json
{
  "success": true,
  "message": "New document version uploaded."
}
```

# Download Document

Download document file.

## Endpoint

```
GET /documents/{id}/download
```

## Response

File download.

# Update Document Information

Update document metadata.

## Endpoint

```
PUT /documents/{id}
```

## Request Body

```json
{
  "name": "Updated Employment Contract",
  "expiryDate": "2028-01-01"
}
```

## Response

```json
{
  "success": true,
  "message": "Document updated successfully."
}
```

# Delete Document

Delete document.

## Endpoint

```
DELETE /documents/{id}
```

## Response

```
204 No Content
```

# Verify Document

HR verifies uploaded document.

## Endpoint

```
PATCH /documents/{id}/verify
```

## Request Body

```json
{
  "note": "Document verified successfully."
}
```

## Response

```json
{
  "success": true,
  "message": "Document verified."
}
```

# Reject Document

Reject invalid document.

## Endpoint

```
PATCH /documents/{id}/reject
```

## Request Body

```json
{
  "reason": "Document is unclear."
}
```

## Response

```json
{
  "success": true,
  "message": "Document rejected."
}
```

# Archive Document

Archive old document.

## Endpoint

```
PATCH /documents/{id}/archive
```

## Response

```json
{
  "success": true,
  "message": "Document archived."
}
```

# Document Categories

Manage document categories.

Examples:

```
CONTRACT
IDENTITY
CERTIFICATE
POLICY
MEDICAL
TAX
OTHER
```

# Get Document Categories

## Endpoint

```
GET /documents/categories
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "cat_contract",
      "name": "Contract"
    }
  ]
}
```

# Create Document Category

## Endpoint

```
POST /documents/categories
```

## Request Body

```json
{
  "name": "Certificate",
  "description": "Employee certificate documents"
}
```

# Update Document Category

## Endpoint

```
PUT /documents/categories/{id}
```

# Delete Document Category

## Endpoint

```
DELETE /documents/categories/{id}
```

# Expiring Documents

Retrieve documents that will expire.

## Endpoint

```
GET /documents/expiring
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| days      | number |

Example:

```
GET /documents/expiring?days=30
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "employee": "John Doe",
      "document": "Contract",
      "expiryDate": "2026-07-15"
    }
  ]
}
```

# Document Statistics

Retrieve document statistics.

## Endpoint

```
GET /documents/summary
```

## Response

```json
{
  "success": true,
  "data": {
    "totalDocuments": 500,
    "verified": 450,
    "pending": 30,
    "expired": 20
  }
}
```

# Document Report

Generate document report.

## Endpoint

```
GET /documents/report
```

## Query Parameters

| Parameter  | Type   |
| ---------- | ------ |
| categoryId | UUID   |
| format     | string |

Available:

```
PDF
EXCEL
```

# Validation Rules

| Field      | Rule          |
| ---------- | ------------- |
| file       | Required      |
| fileSize   | Maximum 10MB  |
| fileType   | PDF, JPG, PNG |
| categoryId | Required      |
| employeeId | Required      |

# Error Responses

## Document Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Document not found."
}
```

## Invalid File

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Invalid file format."
}
```

## Unauthorized Access

```
403 Forbidden
```

```json
{
  "success": false,
  "message": "You do not have permission to access this document."
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

- documents
- document_categories
- document_versions
- employees
- users
- audit_logs
