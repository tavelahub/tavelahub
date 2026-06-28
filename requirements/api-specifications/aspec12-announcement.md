# Announcement API Specification

## Overview

The Announcement module manages company announcements and internal communication.

HR Admins can create and publish announcements, assign target audiences, attach files, and monitor employee read status.

Employees can view announcements, mark announcements as read, and receive important company information.

Examples:

- Company policy updates
- Holiday announcements
- HR notifications
- Internal events
- Emergency announcements
- Employee reminders

# Base URL

```
/api/v1/announcements
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
| HR Admin    | Create, Update, Delete, Publish |
| Manager     | View Department Announcement    |
| Employee    | View Announcement               |

# Announcement Object

```json
{
  "id": "ann_001",
  "title": "Company Holiday Announcement",
  "content": "The company will be closed on July 1st.",
  "category": "GENERAL",
  "priority": "HIGH",
  "status": "PUBLISHED",
  "publishedAt": "2026-06-28T10:00:00Z",
  "createdBy": "user_001",
  "createdAt": "2026-06-28T09:00:00Z"
}
```

# Announcement Status

Available values:

```
DRAFT
SCHEDULED
PUBLISHED
ARCHIVED
```

# Announcement Priority

Available values:

```
LOW
NORMAL
HIGH
URGENT
```

# Announcement Category

Available values:

```
GENERAL
HR
POLICY
EVENT
REMINDER
EMERGENCY
```

# Endpoints

# Get Announcements

Retrieve announcement list.

## Endpoint

```
GET /announcements
```

## Query Parameters

| Parameter | Type   | Description     |
| --------- | ------ | --------------- |
| page      | number | Current page    |
| limit     | number | Data limit      |
| category  | string | Filter category |
| priority  | string | Filter priority |
| status    | string | Filter status   |
| search    | string | Search title    |

Example:

```
GET /announcements?page=1&category=HR
```

## Response

```
200 OK
```

```json
{
  "success": true,
  "message": "Announcements retrieved successfully.",
  "data": [
    {
      "id": "ann_001",
      "title": "Company Holiday Announcement",
      "priority": "HIGH",
      "status": "PUBLISHED",
      "publishedAt": "2026-06-28"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 25
  }
}
```

# Get My Announcements

Retrieve announcements visible to current user.

## Endpoint

```
GET /announcements/me
```

## Query Parameters

| Parameter  | Type    |
| ---------- | ------- |
| unreadOnly | boolean |
| category   | string  |

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "ann_001",
      "title": "New Company Policy",
      "isRead": false
    }
  ]
}
```

# Get Announcement Detail

Retrieve announcement detail.

## Endpoint

```
GET /announcements/{id}
```

## Response

```json
{
  "success": true,
  "data": {
    "id": "ann_001",
    "title": "Company Policy Update",
    "content": "Updated employee policy...",
    "priority": "HIGH",
    "attachments": []
  }
}
```

# Create Announcement

Create new announcement.

## Endpoint

```
POST /announcements
```

## Request Body

```json
{
  "title": "New Company Policy",
  "content": "Please read the updated policy.",
  "category": "POLICY",
  "priority": "HIGH",
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

```
201 Created
```

```json
{
  "success": true,
  "message": "Announcement created successfully.",
  "data": {
    "id": "ann_001"
  }
}
```

# Update Announcement

Update announcement information.

Only available for:

```
DRAFT
SCHEDULED
```

status.

## Endpoint

```
PUT /announcements/{id}
```

## Request Body

```json
{
  "title": "Updated Announcement Title",
  "content": "Updated content",
  "priority": "NORMAL"
}
```

## Response

```json
{
  "success": true,
  "message": "Announcement updated successfully."
}
```

# Delete Announcement

Delete announcement.

## Endpoint

```
DELETE /announcements/{id}
```

## Response

```
204 No Content
```

# Publish Announcement

Publish announcement immediately.

## Endpoint

```
PATCH /announcements/{id}/publish
```

## Response

```json
{
  "success": true,
  "message": "Announcement published successfully."
}
```

# Schedule Announcement

Schedule announcement publication.

## Endpoint

```
PATCH /announcements/{id}/schedule
```

## Request Body

```json
{
  "publishAt": "2026-07-01T08:00:00Z"
}
```

## Response

```json
{
  "success": true,
  "message": "Announcement scheduled successfully."
}
```

# Archive Announcement

Archive old announcement.

## Endpoint

```
PATCH /announcements/{id}/archive
```

## Response

```json
{
  "success": true,
  "message": "Announcement archived successfully."
}
```

# Mark Announcement as Read

Employee marks announcement as read.

## Endpoint

```
PATCH /announcements/{id}/read
```

## Response

```json
{
  "success": true,
  "message": "Announcement marked as read."
}
```

# Get Read Status

Retrieve employee read status.

## Endpoint

```
GET /announcements/{id}/read-status
```

## Permission

```
HR Admin
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "employee": "John Doe",
      "readAt": "2026-06-28T12:00:00Z"
    }
  ]
}
```

# Upload Attachment

Upload announcement attachment.

## Endpoint

```
POST /announcements/{id}/attachments
```

## Content-Type

```
multipart/form-data
```

## Request

```
file: announcement_document.pdf
```

## Response

```json
{
  "success": true,
  "message": "Attachment uploaded successfully.",
  "data": {
    "url": "/uploads/announcement/file.pdf"
  }
}
```

# Delete Attachment

Remove attachment.

## Endpoint

```
DELETE /announcements/{id}/attachments/{attachmentId}
```

## Response

```
204 No Content
```

# Get Announcement Statistics

Retrieve announcement statistics.

## Endpoint

```
GET /announcements/summary
```

## Response

```json
{
  "success": true,
  "data": {
    "totalAnnouncements": 120,
    "published": 100,
    "draft": 10,
    "scheduled": 5,
    "archived": 5
  }
}
```

# Validation Rules

| Field      | Rule     |
| ---------- | -------- |
| title      | Required |
| content    | Required |
| category   | Required |
| priority   | Required |
| targetType | Required |

# Error Responses

## Announcement Not Found

```
404 Not Found
```

```json
{
  "success": false,
  "message": "Announcement not found."
}
```

## Invalid Status

```
400 Bad Request
```

```json
{
  "success": false,
  "message": "Announcement cannot be modified."
}
```

## Unauthorized Access

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
| 409  | Conflict              |
| 422  | Validation Error      |
| 500  | Internal Server Error |

# Related Database Tables

- announcements
- announcement_targets
- announcement_attachments
- announcement_reads
- users
- employees
- departments
- audit_logs
