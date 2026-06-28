# Business Requirement Document (BRD)

## 1. Project Overview

### Project Name

TavelaHub - Human Resource Information System (HRIS)

### Document Purpose

This document defines the business requirements for the development of a Human Resource Information System (HRIS). The purpose of this system is to improve and automate human resource management processes by providing a centralized platform for managing employee information, attendance, leave, payroll, and HR operations.

# 2. Business Background

Many organizations still manage employee data and HR processes using manual methods such as spreadsheets, paper documents, and separate systems. These approaches create several challenges:

- Data duplication and inconsistency
- Difficult employee information management
- Slow approval processes
- Limited visibility into workforce data
- Time-consuming reporting processes
- Increased risk of human errors

The HRIS application is developed to provide an integrated solution that simplifies HR operations and improves data accuracy.

# 3. Business Objectives

The objectives of this project are:

## 3.1 Centralize Employee Information

Provide a single platform to store and manage employee-related information, including personal data, employment details, documents, and organizational information.

## 3.2 Improve HR Operational Efficiency

Reduce manual HR processes by automating employee management, attendance tracking, leave requests, and approval workflows.

## 3.3 Improve Data Accuracy

Ensure HR data consistency by maintaining structured records and reducing manual data entry errors.

## 3.4 Provide Better Workforce Visibility

Provide dashboards and reports that help HR teams and managers monitor workforce information and make informed decisions.

## 3.5 Improve Employee Experience

Allow employees to access HR services independently through an Employee Self-Service platform.

# 4. Target Users

## 4.1 Super Administrator

Responsible for managing the entire system.

Responsibilities:

- Manage user accounts
- Manage roles and permissions
- Configure system settings
- Monitor system activities

## 4.2 HR Administrator

Responsible for managing human resource operations.

Responsibilities:

- Manage employee data
- Manage attendance
- Manage leave policies
- Manage payroll information
- Generate HR reports

## 4.3 Manager

Responsible for managing team activities.

Responsibilities:

- Review employee requests
- Approve leave requests
- Approve overtime requests
- Monitor team information

## 4.4 Employee

Responsible for managing personal HR activities.

Responsibilities:

- View personal information
- Submit leave requests
- View attendance records
- View payroll information
- Download documents

# 5. Business Scope

## 5.1 In Scope

The system will include:

### Employee Management

- Employee profile management
- Employment information management
- Department management
- Position management

### Attendance Management

- Employee attendance recording
- Check-in and check-out
- Attendance history
- Attendance reports

### Leave Management

- Leave request submission
- Leave approval workflow
- Leave balance tracking

### Payroll Management

- Salary information management
- Salary calculation
- Payslip generation -->

### Reimbursement Management

- Expense claim submission
- Approval process
- Reimbursement tracking

### Reporting

- Employee reports
- Attendance reports
- Payroll reports
- Export reports

### User Management

- User accounts
- Roles
- Permissions

# 5.2 Out of Scope

The following features are not included in the initial version:

- Biometric fingerprint integration
- Full recruitment management
- AI resume screening
- Mobile native application
- External payroll provider integration
- Advanced performance management system

# 6. Business Processes

## 6.1 Employee Registration Process

Current Process:

1. HR collects employee information manually.
2. Data is stored in spreadsheets or documents.
3. Updates require manual changes.

Problem:

- Data inconsistency
- Difficult tracking

Expected Process:

1. HR creates employee profile.
2. System stores employee information.
3. Employee can access their personal information.

## 6.2 Leave Request Process

Current Process:

1. Employee submits leave request manually.
2. Manager reviews request.
3. HR updates records.

Problem:

- Slow approval process
- Difficult tracking

Expected Process:

1. Employee submits leave request through the system.
2. Manager receives approval notification.
3. Manager approves or rejects request.
4. System updates leave balance automatically.

## 6.3 Attendance Process

Current Process:

1. Employee attendance is recorded separately.
2. HR manually calculates attendance.

Problem:

- Time-consuming
- Error-prone

Expected Process:

1. Employee checks in and checks out.
2. System records attendance data.
3. HR can generate attendance reports.

## 6.4 Payroll Process

Current Process:

1. HR calculates salary manually.
2. Data is collected from multiple sources.

Problem:

- High risk of calculation errors
- Difficult verification

Expected Process:

1. System collects attendance and salary data.
2. Payroll calculation is generated.
3. Employee receives digital payslip.

# 7. Business Rules

## Employee Rules

- Each employee must have a unique employee ID.
- Employee information must be managed by authorized users.
- Employee records cannot be permanently deleted without permission.

## Attendance Rules

- Employees can only check in once per working day.
- Check-out is required to complete attendance.
- Attendance records must be stored permanently.

## Leave Rules

- Employees can only request leave if they have sufficient leave balance.
- Leave requests require manager approval.
- Approved leave reduces employee leave balance.

## Payroll Rules

- Payroll calculation uses employee salary information.
- Attendance data may affect salary calculation.
- Payslips must be generated for each payroll period.

# 8. Expected Business Benefits

## For HR Department

- Faster employee data management
- Reduced administrative workload
- Easier reporting process

## For Managers

- Faster approval workflow
- Better team visibility
- Easier decision making

## For Employees

- Self-service HR access
- Faster request processing
- Transparent HR information

# 9. Success Metrics

The project is considered successful when:

| Metric                             | Target                      |
| ---------------------------------- | --------------------------- |
| Employee data centralized          | 100% managed through system |
| Manual HR processes reduced        | Significant reduction       |
| Approval workflow available        | Yes                         |
| HR reports generated automatically | Yes                         |
| Employee self-service available    | Yes                         |
| System availability                | Reliable production access  |

# 10. Future Improvements

Potential future enhancements:

- Mobile application
- Face recognition attendance
- AI HR assistant
- Recruitment management
- Performance management
- Training management
- Multi-company support
