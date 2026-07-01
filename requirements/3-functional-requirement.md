# Functional Requirement Document (FRD)

## 1. Document Overview

### Project Name

Tavelahub

### Purpose

This document describes the functional requirements of the Tavelahub application. It defines the system features, user interactions, and expected behaviors that must be implemented.

# 2. System Users

The system consists of several user roles:

| Role        | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| Super Admin | Manage system configuration, users, roles, and permissions  |
| HR Admin    | Manage employee data, attendance, payroll, and HR processes |
| Manager     | Review and approve employee requests                        |
| Employee    | Access personal HR information and submit requests          |

# 3. Functional Requirements

## 3.1 Authentication & Authorization

### FR-AUTH-001 User Login

#### Description

The system shall allow users to authenticate using registered credentials.

#### User Flow

1. User enters email and password.
2. System validates credentials.
3. System generates authentication token.
4. User is redirected to the dashboard.

#### Acceptance Criteria

- User can login with valid credentials.
- Invalid credentials display an error message.
- Unauthenticated users cannot access protected pages.

### FR-AUTH-002 User Logout

#### Description

The system shall allow users to securely log out.

#### Acceptance Criteria

- User session is terminated.
- Authentication token is invalidated.
- User is redirected to the login page.

### FR-AUTH-003 Role-Based Access Control

#### Description

The system shall restrict access based on user roles and permissions.

#### Acceptance Criteria

- Users only see authorized menus.
- Unauthorized access attempts are rejected.

## 3.2 User Management

### FR-USER-001 Manage User Accounts

#### Description

Super Admin shall be able to create, update, deactivate, and manage user accounts.

#### Features

- Create user
- Update user information
- Activate/deactivate account
- Assign roles

### FR-USER-002 Manage Roles and Permissions

#### Description

Super Admin shall be able to configure user roles and system permissions.

#### Features

- Create roles
- Assign permissions
- Update permissions

## 3.3 Employee Management

### FR-EMP-001 Create Employee Profile

#### Description

HR Admin shall be able to create employee records.

Employee information includes:

- Employee ID
- Full name
- Email
- Phone number
- Address
- Date of birth
- Gender
- Department
- Position
- Employment status
- Join date

### FR-EMP-002 View Employee Information

#### Description

Authorized users shall be able to view employee profiles.

#### Acceptance Criteria

- Employee data can be searched.
- Employee details can be displayed.
- Access follows user permissions.

### FR-EMP-003 Update Employee Information

#### Description

Authorized users shall be able to update employee information.

### FR-EMP-004 Employee Search and Filter

#### Description

The system shall provide employee searching and filtering capabilities.

Filter options:

- Department
- Position
- Employment status

## 3.4 Organization Management

### FR-ORG-001 Manage Departments

#### Description

HR Admin shall be able to manage company departments.

Features:

- Create department
- Update department
- Delete department
- View department list

### FR-ORG-002 Manage Positions

#### Description

HR Admin shall be able to manage job positions.

Features:

- Create position
- Update position
- Assign position level

## 3.5 Attendance Management

### FR-ATT-001 Employee Check In

#### Description

Employees shall be able to record their start working time.

System shall store:

- Employee ID
- Check-in time
- Date

### FR-ATT-002 Employee Check Out

#### Description

Employees shall be able to record their end working time.

System shall calculate:

- Working duration
- Attendance status

### FR-ATT-003 View Attendance History

#### Description

Employees and HR Admin shall be able to view attendance records.

Features:

- Daily attendance
- Monthly attendance
- Attendance filtering

### FR-ATT-004 Attendance Report

#### Description

HR Admin shall be able to generate attendance reports.

Output:

- PDF
- Excel

## 3.6 Leave Management

### FR-LEAVE-001 Submit Leave Request

#### Description

Employees shall be able to submit leave requests.

Required data:

- Leave type
- Start date
- End date
- Reason
- Attachment

### FR-LEAVE-002 Approve Leave Request

#### Description

Managers shall be able to approve or reject leave requests.

Actions:

- Approve
- Reject

### FR-LEAVE-003 Manage Leave Balance

#### Description

System shall automatically calculate employee leave balance.

## 3.7 Overtime Management

### FR-OT-001 Submit Overtime Request

#### Description

Employees shall be able to submit overtime requests.

Required information:

- Date
- Duration
- Reason

### FR-OT-002 Approve Overtime Request

#### Description

Managers shall review and approve overtime requests.

## 3.8 Payroll Management

### FR-PAY-001 Manage Salary Information

#### Description

HR Admin shall manage employee salary components.

Components:

- Basic salary
- Allowance
- Bonus
- Deduction

### FR-PAY-002 Generate Payroll

#### Description

System shall calculate employee payroll based on configured salary data.

### FR-PAY-003 Generate Payslip

#### Description

System shall generate digital payslips.

Output:

- PDF file

## 3.9 Reimbursement Management

### FR-REIM-001 Submit Reimbursement Request

#### Description

Employees shall be able to submit expense claims.

Required information:

- Category
- Amount
- Description
- Receipt attachment

### FR-REIM-002 Approve Reimbursement

#### Description

Managers shall approve or reject reimbursement requests.

## 3.10 Document Management

### FR-DOC-001 Upload Employee Documents

#### Description

HR Admin shall upload employee documents.

Examples:

- Employment contract
- Certificate
- Identification documents

### FR-DOC-002 Download Documents

#### Description

Authorized users shall download available documents.

## 3.11 Dashboard

### FR-DASH-001 Display HR Summary

#### Description

System shall display important HR information.

Dashboard widgets:

- Total employees
- Attendance summary
- Leave requests
- Payroll summary

## 3.12 Notification Management

### FR-NOTIF-001 Send Notifications

#### Description

System shall notify users about important events.

Examples:

- Leave approval
- Reimbursement approval
- Announcements

## 3.13 Announcement Management

### FR-ANN-001 Create Announcement

#### Description

HR Admin shall create company announcements.

### FR-ANN-002 View Announcement

#### Description

Employees shall view published announcements.

## 3.14 Reporting

### FR-REPORT-001 Generate Reports

#### Description

System shall provide HR reports.

Report types:

- Employee report
- Attendance report
- Leave report
- Payroll report

### FR-REPORT-002 Export Reports

#### Description

Users shall export reports.

Supported formats:

- PDF
- Excel

## 3.15 Audit Log

### FR-AUDIT-001 Record User Activities

#### Description

System shall record important user activities.

Logged activities:

- Login
- Data creation
- Data update
- Data deletion

# 4. System Workflow Summary

## Employee Workflow

```
Login
|
View Dashboard
|
Manage Profile
|
Submit Request
|
Track Approval
```

## Manager Workflow

```
Login
|
View Team
|
Review Requests
|
Approve / Reject
```

## HR Workflow

```
Login
|
Manage Employees
|
Manage Attendance
|
Process Payroll
|
Generate Reports
```

# 5. Future Functional Requirements

The following features may be implemented in future versions:

- Recruitment Management
- Performance Management
- Training Management
- Mobile Application
- Face Recognition Attendance
- AI HR Assistant
- Multi Company Management
