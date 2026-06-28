# Settings API Specification

## Overview

The Settings module manages global application configurations for the HRIS system.

Only authorized administrators can modify system settings.

The settings module controls:

- Company information
- HR policies
- Attendance rules
- Leave configuration
- Payroll configuration
- Notification preferences
- Security policies
- Application preferences

# Base URL

```
/api/v1/settings
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role          | Permission              |
| ------------- | ----------------------- |
| Super Admin   | Full Access             |
| HR Admin      | Manage HR Settings      |
| Finance Admin | Manage Payroll Settings |
| Employee      | View Personal Settings  |

# Settings Object

```json
{
  "id": "setting_001",
  "key": "attendance.work_hours",
  "value": "8",
  "category": "ATTENDANCE",
  "updatedAt": "2026-06-28T10:00:00Z"
}
```

# Setting Categories

Available values:

```
COMPANY
GENERAL
ATTENDANCE
LEAVE
PAYROLL
NOTIFICATION
SECURITY
STORAGE
LOCALIZATION
WORKFLOW
```

# Endpoints

# Get All Settings

Retrieve system settings.

## Endpoint

```
GET /settings
```

## Query Parameters

| Parameter | Type   |
| --------- | ------ |
| category  | string |
| search    | string |

Example:

```
GET /settings?category=ATTENDANCE
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "key": "attendance.work_hours",
      "value": "8",
      "category": "ATTENDANCE"
    }
  ]
}
```

# Get Setting Detail

Retrieve specific setting.

## Endpoint

```
GET /settings/{key}
```

Example:

```
GET /settings/company.name
```

## Response

```json
{
  "success": true,
  "data": {
    "key": "company.name",
    "value": "ABC Company",
    "category": "COMPANY"
  }
}
```

# Update Setting

Update system configuration.

## Endpoint

```
PUT /settings/{key}
```

## Request Body

```json
{
  "value": "09:00"
}
```

## Response

```json
{
  "success": true,
  "message": "Setting updated successfully."
}
```

# Reset Setting

Restore setting to default value.

## Endpoint

```
PATCH /settings/{key}/reset
```

## Response

```json
{
  "success": true,
  "message": "Setting reset successfully."
}
```

# Company Settings

Manage company information.

# Get Company Profile

## Endpoint

```
GET /settings/company
```

## Response

```json
{
  "success": true,
  "data": {
    "name": "ABC Company",
    "email": "info@company.com",
    "phone": "+62123456789",
    "address": "Jakarta",
    "logoUrl": "/uploads/logo.png"
  }
}
```

# Update Company Profile

## Endpoint

```
PUT /settings/company
```

## Request Body

```json
{
  "name": "ABC Company",
  "email": "hr@company.com",
  "phone": "+62123456789",
  "address": "Jakarta"
}
```

# Upload Company Logo

## Endpoint

```
POST /settings/company/logo
```

## Content-Type

```
multipart/form-data
```

## Request

```
file: company_logo.png
```

## Response

```json
{
  "success": true,
  "message": "Logo uploaded successfully.",
  "logoUrl": "/uploads/company/logo.png"
}
```

# Attendance Settings

Manage attendance rules.

## Endpoint

```
GET /settings/attendance
```

## Response

```json
{
  "success": true,
  "data": {
    "workStart": "09:00",
    "workEnd": "17:00",
    "lateTolerance": 15,
    "workingDays": ["MONDAY", "TUESDAY", "WEDNESDAY", "THURSDAY", "FRIDAY"]
  }
}
```

# Update Attendance Settings

## Endpoint

```
PUT /settings/attendance
```

## Request Body

```json
{
  "workStart": "08:30",
  "workEnd": "17:30",
  "lateTolerance": 10
}
```

# Leave Settings

Manage leave policy.

## Endpoint

```
GET /settings/leave
```

## Response

```json
{
  "success": true,
  "data": {
    "annualLeaveQuota": 12,
    "allowCarryForward": true,
    "maxCarryForwardDays": 5
  }
}
```

# Update Leave Settings

## Endpoint

```
PUT /settings/leave
```

## Request Body

```json
{
  "annualLeaveQuota": 14,
  "allowCarryForward": true
}
```

# Payroll Settings

Manage payroll configuration.

## Endpoint

```
GET /settings/payroll
```

## Response

```json
{
  "success": true,
  "data": {
    "currency": "IDR",
    "payday": "25",
    "taxEnabled": true
  }
}
```

# Update Payroll Settings

## Endpoint

```
PUT /settings/payroll
```

## Request Body

```json
{
  "currency": "IDR",
  "payday": "28"
}
```

# Notification Settings

Manage notification configuration.

## Endpoint

```
GET /settings/notification
```

## Response

```json
{
  "success": true,
  "data": {
    "emailEnabled": true,
    "pushEnabled": true,
    "announcementNotification": true
  }
}
```

# Update Notification Settings

## Endpoint

```
PUT /settings/notification
```

## Request Body

```json
{
  "emailEnabled": true,
  "pushEnabled": false
}
```

# Security Settings

Manage security policy.

## Endpoint

```
GET /settings/security
```

## Response

```json
{
  "success": true,
  "data": {
    "passwordMinLength": 8,
    "sessionTimeout": 60,
    "maxLoginAttempt": 5,
    "twoFactorEnabled": false
  }
}
```

# Update Security Settings

## Endpoint

```
PUT /settings/security
```

## Request Body

```json
{
  "passwordMinLength": 10,
  "twoFactorEnabled": true
}
```

# Storage Settings

Manage file storage configuration.

## Endpoint

```
GET /settings/storage
```

## Response

```json
{
  "success": true,
  "data": {
    "provider": "SUPABASE",
    "maxFileSize": 10,
    "allowedTypes": ["PDF", "PNG", "JPG"]
  }
}
```

# Workflow Settings

Manage approval workflow.

## Endpoint

```
GET /settings/workflow
```

## Response

```json
{
  "success": true,
  "data": {
    "leaveApproval": ["MANAGER", "HR"],
    "overtimeApproval": ["MANAGER"]
  }
}
```

# Update Workflow Settings

## Endpoint

```
PUT /settings/workflow
```

## Request Body

```json
{
  "leaveApproval": ["MANAGER", "HR"]
}
```

# Localization Settings

Manage language and timezone.

## Endpoint

```
GET /settings/localization
```

## Response

```json
{
  "success": true,
  "data": {
    "language": "en",
    "timezone": "Asia/Jakarta",
    "dateFormat": "DD-MM-YYYY"
  }
}
```

# Update Localization Settings

## Endpoint

```
PUT /settings/localization
```

## Request Body

```json
{
  "language": "id",
  "timezone": "Asia/Jakarta"
}
```

# Settings History

Retrieve settings changes.

## Endpoint

```
GET /settings/history
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "key": "payroll.payday",
      "oldValue": "25",
      "newValue": "28",
      "updatedBy": "admin",
      "updatedAt": "2026-06-28"
    }
  ]
}
```

# Validation Rules

| Field    | Rule           |
| -------- | -------------- |
| key      | Required       |
| value    | Required       |
| email    | Valid email    |
| timezone | Valid timezone |

# Error Responses

## Setting Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Setting not found."
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

- settings
- setting_histories
- company_profiles
- users
- audit_logs
