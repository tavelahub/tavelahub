# ERD

```
Authentication

users
├── id
├── employee_id (FK)
├── role_id (FK)
├── email
├── password
├── status
├── last_login
├── created_at
└── updated_at

roles
├── id
├── name
├── description
└── created_at

permissions
├── id
├── name
├── module
└── description

role_permissions
├── role_id (FK)
├── permission_id (FK)

────────────────────────────────────────────

Organization

departments
├── id
├── name
├── code
├── manager_id (FK -> employees)
└── created_at

positions
├── id
├── department_id (FK)
├── title
├── level
└── created_at

employees
├── id
├── employee_number
├── department_id (FK)
├── position_id (FK)
├── first_name
├── last_name
├── gender
├── birth_date
├── phone
├── address
├── hire_date
├── employment_status
├── profile_photo
└── created_at

────────────────────────────────────────────

Attendance

attendance_records
├── id
├── employee_id (FK)
├── check_in
├── check_out
├── working_hours
├── late_minutes
├── overtime_hours
├── status
└── created_at

────────────────────────────────────────────

Leave

leave_types
├── id
├── name
├── annual_quota
└── description

leave_requests
├── id
├── employee_id (FK)
├── leave_type_id (FK)
├── start_date
├── end_date
├── total_days
├── reason
├── status
├── approved_by (FK users)
└── created_at

────────────────────────────────────────────

Overtime

overtime_requests
├── id
├── employee_id (FK)
├── date
├── start_time
├── end_time
├── total_hours
├── reason
├── status
├── approved_by
└── created_at

────────────────────────────────────────────

Payroll

payrolls
├── id
├── employee_id (FK)
├── period
├── basic_salary
├── allowance
├── overtime_pay
├── deduction
├── tax
├── net_salary
├── status
└── created_at

────────────────────────────────────────────

Reimbursement


reimbursement_categories
├── id
├── name
└── description

reimbursement_requests
├── id
├── employee_id (FK)
├── category_id (FK)
├── amount
├── description
├── receipt_url
├── status
├── approved_by
└── created_at

────────────────────────────────────────────

Document


documents
├── id
├── employee_id (FK)
├── category
├── file_name
├── file_url
├── expired_at
├── verified
└── created_at

────────────────────────────────────────────

Announcement

announcements
├── id
├── title
├── content
├── category
├── priority
├── status
├── published_at
├── created_by (FK users)
└── created_at

announcement_targets
├── id
├── announcement_id (FK)
├── target_type
├── target_id

announcement_reads
├── id
├── announcement_id (FK)
├── employee_id (FK)
├── read_at

announcement_attachments
├── id
├── announcement_id (FK)
├── file_name
├── file_url

────────────────────────────────────────────

Notification

notifications
├── id
├── user_id (FK)
├── title
├── message
├── type
├── priority
├── reference_type
├── reference_id
├── is_read
└── created_at

notification_preferences
├── id
├── user_id (FK)
├── email_enabled
├── push_enabled
├── announcement_enabled

────────────────────────────────────────────

Report

reports
├── id
├── generated_by (FK users)
├── type
├── format
├── file_url
├── status
└── created_at

report_schedules
├── id
├── report_id (FK)
├── frequency
├── next_run
└── recipients

────────────────────────────────────────────

Settings

settings
├── id
├── key
├── value (JSON)
├── category
├── updated_by
└── updated_at

setting_histories
├── id
├── setting_id (FK)
├── old_value
├── new_value
├── updated_by
└── created_at

────────────────────────────────────────────

Audit

audit_logs
├── id
├── user_id (FK)
├── module
├── action
├── entity_id
├── before_data
├── after_data
├── ip_address
├── user_agent
└── created_at
```

### Relationship

```
Role
 │
 │ 1
 ▼
Users
 │
 │1
 ▼
Employees
 ├───────────────┐
 │               │
 │               │
 ▼               ▼
Departments   Positions
 │               │
 └──────┬────────┘
        │
        ▼
Attendance

Employees
 ├──────── Leave Requests
 ├──────── Overtime Requests
 ├──────── Payrolls
 ├──────── Documents
 ├──────── Reimbursements
 ├──────── Announcement Reads

Announcements
 ├──────── Announcement Targets
 └──────── Announcement Attachments

Users
 ├──────── Notifications
 ├──────── Reports
 ├──────── Audit Logs
 └──────── Settings History

Roles
 └──────── Permissions
```

## Database Modules

| Database Modules | modules 1         | module 2    | module 3  |
| ---------------- | ----------------- | ----------- | --------- |
| Authentication   | roles             | users       | ---       |
| Employee         | employees         | departments | positions |
| Attendance       | attendance        | ---         | ---       |
| Leave            | leave_requests    | ---         | ---       |
| Overtime         | overtime_requests | ---         | ---       |
| Payroll          | payrolls          | ---         | ---       |
| Reimbursement    | reimbursements    | ---         | ---       |
| Documents        | documents         | ---         | ---       |
| Notification     | notifications     | ---         | ---       |
| Announcement     | announcements     | ---         | ---       |
| Audit            | audit_logs        | ---         | ---       |

## Tables

| Module         |       Tables |
| -------------- | -----------: |
| Authentication |            4 |
| Organization   |            3 |
| Employee       |            1 |
| Attendance     |            1 |
| Leave          |            2 |
| Overtime       |            1 |
| Payroll        |            1 |
| Reimbursement  |            2 |
| Document       |            1 |
| Announcement   |            4 |
| Notification   |            2 |
| Report         |            2 |
| Settings       |            2 |
| Audit Log      |            1 |
| **Total**      | **27 tabel** |

## ERD part 1

```
1. Authentication
   ├── users
   ├── roles
   ├── permissions
   ├── role_permissions
   ├── refresh_tokens
   └── user_sessions

2. Organization
   ├── departments
   └── positions

3. Employee
   └── employees

4. Attendance
   └── attendance_records

5. Leave
   ├── leave_types
   └── leave_requests

6. Overtime
   └── overtime_requests

7. Payroll
   └── payrolls

8. Reimbursement
   ├── reimbursement_categories
   └── reimbursement_requests

9. Document
   └── documents

10. Announcement
    ├── announcements
    ├── announcement_targets
    ├── announcement_reads
    └── announcement_attachments

11. Notification
    ├── notifications
    └── notification_preferences

12. Report
    ├── reports
    └── report_schedules

13. Settings
    ├── settings
    └── setting_histories

14. Audit Log
    └── audit_logs
```

Part 1: Authentication + Organization + Employee
Part 2: Attendance + Leave + Overtime
Part 3: Payroll + Reimbursement + Document
Part 4: Announcement + Notification
Part 5: Report + Settings + Audit Log
Part 6: ERD Final (Mermaid) + schema.prisma lengkap
