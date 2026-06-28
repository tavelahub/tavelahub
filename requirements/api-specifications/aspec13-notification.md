# Notification API Specification

## Overview

The Notification module manages system notifications for employees and administrators.

The module provides real-time and asynchronous notifications for HRIS activities such as approval requests, announcements, reminders, and employee actions.

Supported notification channels:

- In-app notification
- Email notification
- Push notification (optional)

# Base URL

```
/api/v1/notifications
```

# Authentication

All endpoints require authentication.

```
Authorization: Bearer <access_token>
```

# Authorization

| Role        | Permission                 |
| ----------- | -------------------------- |
| Super Admin | Full Access                |
| HR Admin    | Manage System Notification |
| Manager     | Send Team Notification     |
| Employee    | View Own Notification      |

# Notification Object

```json
{
  "id": "notif_001",
  "userId": "user_001",
  "title": "Leave Request Approved",
  "message": "Your annual leave request has been approved.",
  "type": "LEAVE",
  "priority": "NORMAL",
  "isRead": false,
  "createdAt": "2026-06-28T10:00:00Z"
}
```

# Notification Type

Available values:

```
ANNOUNCEMENT
LEAVE
OVERTIME
ATTENDANCE
REIMBURSEMENT
PAYROLL
DOCUMENT
SYSTEM
```

# Notification Priority

Available values:

```
LOW
NORMAL
HIGH
URGENT
```

# Notification Status

Available values:

```
UNREAD
READ
ARCHIVED
```

# Endpoints

# Get My Notifications

Retrieve current user's notifications.

## Endpoint

```
GET /notifications/me
```

## Query Parameters

| Parameter | Type | Description |
| - | - | |
| page | number | Current page |
| limit | number | Data limit |
| type | string | Filter notification type |
| unreadOnly | boolean | Show unread only |

Example:

```
GET /notifications/me?unreadOnly=true
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
      "id": "notif_001",
      "title": "Leave Approved",
      "message": "Your leave request was approved.",
      "type": "LEAVE",
      "isRead": false
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 20
  }
}
```

# Get Notification Detail

Retrieve notification detail.

## Endpoint

```
GET /notifications/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "notif_001",
    "title": "Payroll Available",
    "message": "Your payslip is available.",
    "type": "PAYROLL",
    "referenceId": "pay_001"
  }
}
```

# Mark Notification as Read

Mark single notification as read.

## Endpoint

```
PATCH /notifications/{id}/read
```

## Response

```json
{
  "success": true,
  "message": "Notification marked as read."
}
```

# Mark All Notifications as Read

Mark all notifications as read.

## Endpoint

```
PATCH /notifications/read-all
```

## Response

```json
{
  "success": true,
  "message": "All notifications marked as read."
}
```

# Archive Notification

Archive notification.

## Endpoint

```
PATCH /notifications/{id}/archive
```

## Response

```json
{
  "success": true,
  "message": "Notification archived."
}
```

# Delete Notification

Delete notification.

## Endpoint

```
DELETE /notifications/{id}
```

## Response

```
204 No Content
```

# Create Notification

Create notification manually.

Used by:

- HR Admin
- System Service
- Background Worker

## Endpoint

```
POST /notifications
```

## Request Body

```json
{
  "userId": "user_001",
  "title": "New Announcement",
  "message": "New company policy has been published.",
  "type": "ANNOUNCEMENT",
  "priority": "HIGH",
  "referenceId": "ann_001"
}
```

## Response

```
201 Created
```

```json
{
  "success": true,
  "message": "Notification created successfully.",
  "data": {
    "id": "notif_001"
  }
}
```

# Broadcast Notification

Send notification to multiple employees.

## Endpoint

```
POST /notifications/broadcast
```

## Request Body

```json
{
  "title": "Company Announcement",
  "message": "Office will be closed tomorrow.",
  "type": "ANNOUNCEMENT",
  "targetType": "ALL"
}
```

## Target Type

Available:

```
ALL
DEPARTMENT
ROLE
EMPLOYEE
```

## Response

```json
{
  "success": true,
  "message": "Notification broadcast successfully.",
  "recipientCount": 150
}
```

# Send Notification To Department

Send notification to specific department.

## Endpoint

```
POST /notifications/department
```

## Request Body

```json
{
  "departmentId": "dept_001",
  "title": "HR Reminder",
  "message": "Please complete your document."
}
```

# Send Email Notification

Send email notification.

## Endpoint

```
POST /notifications/email
```

## Request Body

```json
{
  "recipient": "john@example.com",
  "subject": "Payroll Released",
  "content": "Your payroll is ready."
}
```

## Response

```json
{
  "success": true,
  "message": "Email sent successfully."
}
```

# Notification Preferences

Employee manages notification preferences.

# Get Notification Preferences

## Endpoint

```
GET /notifications/preferences
```

## Response

```json
{
  "success": true,
  "data": {
    "email": true,
    "push": true,
    "announcement": true,
    "payroll": true
  }
}
```

# Update Notification Preferences

## Endpoint

```
PUT /notifications/preferences
```

## Request Body

```json
{
  "email": false,
  "push": true,
  "announcement": true
}
```

## Response

```json
{
  "success": true,
  "message": "Notification preferences updated."
}
```

# Get Unread Count

Retrieve unread notification count.

## Endpoint

```
GET /notifications/unread-count
```

## Response

```json
{
  "success": true,
  "data": {
    "count": 8
  }
}
```

# Notification Statistics

Retrieve notification statistics.

## Endpoint

```
GET /notifications/summary
```

## Response

```json
{
  "success": true,
  "data": {
    "totalSent": 5000,
    "read": 4200,
    "unread": 800
  }
}
```

# Validation Rules

| Field      | Rule                             |
| ---------- | -------------------------------- |
| title      | Required                         |
| message    | Required                         |
| type       | Required                         |
| userId     | Required for single notification |
| targetType | Required for broadcast           |

# Error Responses

## Notification Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Notification not found."
}
```

## Invalid Recipient

```
422 Unprocessable Entity
```

```json
{
  "success": false,
  "message": "Recipient is invalid."
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

- notifications
- notification_preferences
- notification_reads
- users
- employees
- departments
- announcements
- audit_logs

```
users
 |
 |
 └──────── notifications
                |
                |
       ┌────────┴────────┐
       |                 |
notification_reads   notification_preferences


notifications
      |
      |
 referenceId
      |
      |
 ┌────┼───────────┐
 |    |           |
Leave Payroll Announcement
```

```
model Notification {
  id          String @id @default(uuid())

  userId      String

  title       String
  message     String

  type        NotificationType

  priority    NotificationPriority
    @default(NORMAL)

  referenceId String?

  isRead      Boolean
    @default(false)

  createdAt   DateTime @default(now())

  user        User @relation(
    fields:[userId],
    references:[id]
  )
}
```

```
Notification
│
├── Notification Center
├── Notification Detail
├── Unread Notification Badge
├── Notification Preferences
├── Email Settings
└── Admin Notification Management
```

```
Authentication Module
        ↓
User Module
        ↓
Employee Module
        ↓
Announcement Module
        ↓
Notification Module
        ↓
All HR Modules Integration
```
