# User Roles Document

## 1. Overview

This document defines the user roles, responsibilities, access levels, and permissions within the Human Resource Information System (HRIS).

The purpose of defining user roles is to ensure that each user can only access features and information based on their responsibilities within the organization.

# 2. Role Hierarchy

The HRIS system consists of four main user roles:

```
Super Admin
|
|
HR Admin
|
|
Manager
|
|
Employee
```

# 3. User Roles

## 3.1 Super Admin

### Description

Super Admin is the highest-level system administrator responsible for managing system configuration, security, and user access.

### Responsibilities

- Manage system users
- Manage roles and permissions
- Configure system settings
- Monitor system activities
- Manage organization settings

### Access Level

Full system access.

### Permissions

#### User Management

✅ Create users  
✅ Update users  
✅ Deactivate users  
✅ Reset passwords

#### Role Management

✅ Create roles  
✅ Update roles  
✅ Assign permissions

#### System Settings

✅ Manage company settings  
✅ Configure application settings  
✅ Configure system preferences

#### Audit Log

✅ View all system activities

## 3.2 HR Admin

### Description

HR Admin is responsible for managing human resource operations and employee-related processes.

### Responsibilities

- Manage employee information
- Manage attendance records
- Manage leave policies
- Process payroll
- Generate HR reports

### Access Level

Administrative access to HR modules.

### Permissions

#### Employee Management

✅ Create employee profiles  
✅ Update employee information  
✅ View employee data  
✅ Manage employment information

#### Organization Management

✅ Manage departments  
✅ Manage positions  
✅ Manage employee assignments

#### Attendance Management

✅ View attendance records  
✅ Correct attendance data  
✅ Generate attendance reports

#### Leave Management

✅ Manage leave types  
✅ View leave requests  
✅ Manage leave balance

#### Payroll Management

✅ Manage salary information  
✅ Process payroll  
✅ Generate payslips

#### Document Management

✅ Upload employee documents  
✅ Manage HR documents

#### Reporting

✅ Generate HR reports  
✅ Export reports

## 3.3 Manager

### Description

Manager is responsible for managing team members and approving employee requests.

### Responsibilities

- Monitor team activities
- Approve employee requests
- Review team attendance
- Evaluate employee performance

### Access Level

Limited access to assigned team data.

### Permissions

#### Team Management

✅ View team members  
✅ View employee information within team

#### Approval Management

✅ Approve leave requests  
✅ Reject leave requests  
✅ Approve overtime requests  
✅ Approve reimbursement requests

#### Attendance

✅ View team attendance records

#### Performance

✅ Review employee performance

## 3.4 Employee

### Description

Employee is a regular user who can access personal HR services through the Employee Self-Service (ESS) platform.

### Responsibilities

- Manage personal information
- Submit HR requests
- View personal records

### Access Level

Personal data access only.

### Permissions

#### Personal Profile

✅ View personal profile  
✅ Update allowed personal information

#### Attendance

✅ Check in  
✅ Check out  
✅ View attendance history

#### Leave Management

✅ Submit leave request  
✅ View leave balance  
✅ View leave history

#### Overtime Management

✅ Submit overtime request  
✅ View overtime status

#### Reimbursement

✅ Submit reimbursement request  
✅ View reimbursement status

#### Payroll

✅ View payslip  
✅ Download payslip

#### Document

✅ View personal documents  
✅ Download personal documents

# 4. Permission Matrix

| Module              | Super Admin | HR Admin | Manager     | Employee      |
| ------------------- | ----------- | -------- | ----------- | ------------- |
| Dashboard           | Full        | Full     | Team Only   | Personal      |
| User Management     | Full        | No       | No          | No            |
| Role Management     | Full        | No       | No          | No            |
| Employee Management | Full        | Full     | View Team   | Self Only     |
| Department          | Full        | Full     | View        | No            |
| Position            | Full        | Full     | View        | No            |
| Attendance          | Full        | Full     | Team View   | Self View     |
| Leave               | Full        | Full     | Approve     | Request       |
| Overtime            | Full        | Full     | Approve     | Request       |
| Payroll             | Full        | Full     | No          | Own Payslip   |
| Reimbursement       | Full        | Full     | Approve     | Request       |
| Documents           | Full        | Full     | Team View   | Own Documents |
| Reports             | Full        | Full     | Team Report | No            |
| Settings            | Full        | Limited  | No          | No            |
| Audit Log           | Full        | Limited  | No          | No            |

# 5. Access Control Rules

### Rule 1 - Authentication Required

All users must authenticate before accessing the system.

### Rule 2 - Role-Based Access

Users can only access features assigned to their role.

Example:

Employee cannot access payroll management.

### Rule 3 - Data Visibility

Users can only view data based on their access level.

Examples:

- Employee → Own data only
- Manager → Team data
- HR Admin → All employee data
- Super Admin → Entire system

### Rule 4 - Approval Authority

Only authorized roles can approve requests.

Example:

Employee submits leave request:

```
Employee
|
|
Manager Approval
|
|
HR Verification
```

# 6. Future Roles

Additional roles that may be added in future versions:

### Finance Admin

Responsibilities:

- Manage payroll processing
- Verify financial transactions
- Manage reimbursement payments

### Recruiter

Responsibilities:

- Manage job vacancies
- Manage candidates
- Schedule interviews

### Trainer

Responsibilities:

- Manage employee training
- Manage certifications

# 7. Role Implementation

The system implements Role-Based Access Control (RBAC).

Example:

```
User
|
|
Role
|
|
Permission
|
|
Feature Access
```

Database relationship:

```
users
|
|
roles
|
|
permissions
```

User → Role → Permission → Module

```
Request
  |
JWT Middleware
  |
Role Middleware
  |
Permission Check
  |
Controller
  |
Service
  |
Database
```
