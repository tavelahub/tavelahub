# UI Sitemap

## Overview

This document defines the page hierarchy and navigation structure of the Human Resource Information System (HRIS).

The application uses Role-Based Access Control (RBAC), meaning each user role has access to different menus and pages.

# Public Pages

```text
/
├── Landing Page
├── Login
├── Forgot Password
├── Reset Password
└── 404 Not Found
```

# Authenticated Area

```text
Dashboard
│
├── Dashboard Home
│
├── Employee Management
│   ├── Employee List
│   ├── Employee Detail
│   ├── Create Employee
│   └── Edit Employee
│
├── Organization
│   ├── Departments
│   ├── Create Department
│   ├── Edit Department
│   ├── Positions
│   ├── Create Position
│   └── Edit Position
│
├── Attendance
│   ├── Attendance Dashboard
│   ├── Attendance History
│   ├── Attendance Detail
│   └── Attendance Report
│
├── Leave Management
│   ├── Leave Requests
│   ├── Leave Detail
│   ├── Create Leave Request
│   └── Leave Calendar
│
├── Overtime
│   ├── Overtime Requests
│   ├── Overtime Detail
│   └── Create Overtime Request
│
├── Payroll
│   ├── Payroll List
│   ├── Payroll Detail
│   ├── Payslip
│   └── Payroll Report
│
├── Reimbursement
│   ├── Reimbursement List
│   ├── Reimbursement Detail
│   └── Create Reimbursement
│
├── Documents
│   ├── Document List
│   ├── Upload Document
│   └── Document Detail
│
├── Announcements
│   ├── Announcement List
│   ├── Announcement Detail
│   ├── Create Announcement
│   └── Edit Announcement
│
├── Reports
│   ├── Employee Report
│   ├── Attendance Report
│   ├── Leave Report
│   ├── Payroll Report
│   └── Reimbursement Report
│
├── Notifications
│   ├── Notification List
│   └── Notification Detail
│
├── Settings
│   ├── Company Settings
│   ├── Working Hours
│   ├── Leave Types
│   └── General Settings
│
├── Audit Logs
│   ├── Activity Logs
│   └── Activity Detail
│
├── Profile
│   ├── My Profile
│   ├── Change Password
│   └── Account Settings
│
└── Logout
```

# Navigation by User Role

## Super Admin

```text
Dashboard
│
├── Dashboard
├── User Management
├── Employee Management
├── Organization
├── Reports
├── Audit Logs
├── Settings
├── Profile
└── Logout
```

## HR Admin

```text
Dashboard
│
├── Dashboard
├── Employee Management
├── Organization
├── Attendance
├── Leave Management
├── Overtime
├── Payroll
├── Reimbursement
├── Documents
├── Announcements
├── Reports
├── Profile
└── Logout
```

## Manager

```text
Dashboard
│
├── Dashboard
├── Team Members
├── Attendance
├── Leave Approval
├── Overtime Approval
├── Reimbursement Approval
├── Reports
├── Profile
└── Logout
```

## Employee

```text
Dashboard
│
├── Dashboard
├── My Profile
├── Attendance
├── Leave
├── Overtime
├── Reimbursement
├── Documents
├── Payslips
├── Notifications
├── Profile
└── Logout
```

# Page Hierarchy

```text
Public
│
├── Login
├── Forgot Password
└── Reset Password

Authenticated
│
├── Dashboard
│
├── Employee
│
├── Organization
│
├── Attendance
│
├── Leave
│
├── Overtime
│
├── Payroll
│
├── Reimbursement
│
├── Documents
│
├── Reports
│
├── Notifications
│
├── Settings
│
├── Audit Logs
│
└── Profile
```

# Page Access Matrix

| Page                | Super Admin |  HR Admin  |   Manager   |  Employee  |
| ------------------- | :---------: | :--------: | :---------: | :--------: |
| Dashboard           |     ✅      |     ✅     |     ✅      |     ✅     |
| User Management     |     ✅      |     ❌     |     ❌      |     ❌     |
| Employee Management |     ✅      |     ✅     |   👁️ Team   |  👁️ Self   |
| Organization        |     ✅      |     ✅     |     👁️      |     ❌     |
| Attendance          |     ✅      |     ✅     |   👁️ Team   |     ✅     |
| Leave               |     ✅      |     ✅     | ✅ Approval | ✅ Request |
| Overtime            |     ✅      |     ✅     | ✅ Approval | ✅ Request |
| Payroll             |     ✅      |     ✅     |     ❌      | 👁️ Payslip |
| Reimbursement       |     ✅      |     ✅     | ✅ Approval | ✅ Request |
| Documents           |     ✅      |     ✅     |   👁️ Team   |  👁️ Self   |
| Announcements       |     ✅      |     ✅     |     👁️      |     👁️     |
| Reports             |     ✅      |     ✅     |   👁️ Team   |     ❌     |
| Audit Logs          |     ✅      | 👁️ Limited |     ❌      |     ❌     |
| Settings            |     ✅      | 👁️ Limited |     ❌      |     ❌     |
| Profile             |     ✅      |     ✅     |     ✅      |     ✅     |

Legend:

- ✅ Full Access
- 👁️ View Only
- ❌ No Access

# Suggested URL Structure

```text
/
├── login
├── forgot-password
├── reset-password
│
└── dashboard
    ├── overview
    ├── employees
    ├── employees/new
    ├── employees/:id
    ├── employees/:id/edit
    │
    ├── departments
    ├── positions
    │
    ├── attendance
    ├── leave
    ├── overtime
    ├── payroll
    ├── reimbursements
    ├── documents
    ├── announcements
    ├── reports
    ├── notifications
    ├── settings
    ├── audit-logs
    └── profile
```

## File Sitemap

```
src/
├── routes/
│
├── public/
│   ├── login.tsx
│   ├── forgot-password.tsx
│   └── reset-password.tsx
│
├── protected/
│   ├── dashboard.tsx
│   ├── employees/
│   ├── departments/
│   ├── positions/
│   ├── attendance/
│   ├── leave/
│   ├── overtime/
│   ├── payroll/
│   ├── reimbursements/
│   ├── documents/
│   ├── announcements/
│   ├── reports/
│   ├── notifications/
│   ├── settings/
│   ├── audit-logs/
│   └── profile/
│
├── root.tsx
└── index.ts
```
