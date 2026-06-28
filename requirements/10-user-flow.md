# User Flow

## Overview

This document describes the user navigation and interaction flow within the Human Resource Information System (HRIS).

The purpose of this document is to provide a clear understanding of how users interact with the application based on their assigned roles.

# User Roles

The system supports four user roles:

- Super Admin
- HR Admin
- Manager
- Employee

# General Application Flow

```text
Landing Page
      │
      ▼
Login
      │
      ▼
Authentication
      │
      ▼
Dashboard
      │
      ▼
Role-Based Navigation
      │
      ▼
Feature Module
      │
      ▼
CRUD / Action
      │
      ▼
Success Notification
```

# Authentication Flow

```text
Login Page
      │
      ▼
Enter Email & Password
      │
      ▼
Click Login
      │
      ▼
Validate Credentials
      │
      ├──────────────┐
      │              │
      ▼              ▼
Success          Failed
      │              │
      ▼              ▼
Dashboard     Error Message
```

# Employee Management Flow

```text
Dashboard
      │
      ▼
Employee List
      │
      ▼
Search / Filter
      │
      ▼
Select Employee
      │
      ▼
Employee Detail
      │
 ┌────┼─────┐
 │    │     │
 ▼    ▼     ▼
Edit Delete View
 │
 ▼
Save Changes
 │
 ▼
Success Notification
```

# Attendance Flow

```text
Dashboard
      │
      ▼
Attendance
      │
 ┌────┴────┐
 ▼         ▼
Check In  History
 │
 ▼
Working
 │
 ▼
Check Out
 │
 ▼
Attendance Recorded
```

# Leave Request Flow

```text
Dashboard
      │
      ▼
Leave Management
      │
      ▼
Create Request
      │
      ▼
Fill Leave Form
      │
      ▼
Submit Request
      │
      ▼
Manager Review
      │
 ┌────┴─────┐
 ▼          ▼
Approved  Rejected
 │          │
 ▼          ▼
Update Leave Balance
```

# Overtime Flow

```text
Dashboard
      │
      ▼
Overtime
      │
      ▼
Submit Request
      │
      ▼
Manager Approval
      │
 ┌────┴─────┐
 ▼          ▼
Approved  Rejected
```

# Reimbursement Flow

```text
Dashboard
      │
      ▼
Reimbursement
      │
      ▼
Create Request
      │
      ▼
Upload Receipt
      │
      ▼
Submit
      │
      ▼
Manager Review
      │
 ┌────┴─────┐
 ▼          ▼
Approved  Rejected
```

# Payroll Flow

```text
HR Admin
      │
      ▼
Payroll Module
      │
      ▼
Generate Payroll
      │
      ▼
Review Payroll
      │
      ▼
Publish Payslip
      │
      ▼
Employee Downloads Payslip
```

# Document Management Flow

```text
Employee Profile
      │
      ▼
Documents
      │
 ┌────┼─────┐
 ▼    ▼     ▼
Upload View Download
```

# Announcement Flow

```text
HR Admin
      │
      ▼
Create Announcement
      │
      ▼
Publish
      │
      ▼
Employees Receive Notification
      │
      ▼
Read Announcement
```

# Notification Flow

```text
System Event
      │
      ▼
Create Notification
      │
      ▼
Store Notification
      │
      ▼
User Opens Notification Center
      │
      ▼
Mark as Read
```

# Report Flow

```text
Dashboard
      │
      ▼
Reports
      │
      ▼
Choose Report Type
      │
      ▼
Apply Filter
      │
      ▼
Generate Report
      │
 ┌────┴────┐
 ▼         ▼
PDF      Excel
```

# User Navigation

## Super Admin

```text
Dashboard
│
├── User Management
├── Role Management
├── Employee Management
├── Departments
├── Positions
├── Reports
├── Settings
└── Audit Logs
```

## HR Admin

```text
Dashboard
│
├── Employees
├── Attendance
├── Leave
├── Overtime
├── Payroll
├── Reimbursement
├── Documents
├── Reports
└── Announcements
```

## Manager

```text
Dashboard
│
├── Team Members
├── Attendance
├── Leave Approval
├── Overtime Approval
├── Reimbursement Approval
└── Reports
```

## Employee

```text
Dashboard
│
├── My Profile
├── Attendance
├── Leave
├── Overtime
├── Reimbursement
├── Documents
├── Payslips
└── Notifications
```

# Error Flow

```text
User Action
      │
      ▼
Validation
      │
 ┌────┴─────┐
 ▼          ▼
Valid     Invalid
 │          │
 ▼          ▼
Continue  Show Error
```

# Session Flow

```text
Login
 │
 ▼
JWT Token
 │
 ▼
Access Application
 │
 ▼
Token Expired
 │
 ▼
Refresh Token
 │
 ├─────────────┐
 ▼             ▼
Success      Failed
 │             │
 ▼             ▼
Continue     Login Again
```

# Logout Flow

```text
User Menu
      │
      ▼
Logout
      │
      ▼
Clear Session
      │
      ▼
Redirect to Login
```
