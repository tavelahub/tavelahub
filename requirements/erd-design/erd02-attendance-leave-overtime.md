# Attendance Module

### attendance_records

```
| Column              | Type         | Constraint        | Description          |
| ------------------- | ------------ | ----------------- | -------------------- |
| id                  | UUID         | PK                | Attendance ID        |
| employee_id         | UUID         | FK → employees.id | Employee             |
| attendance_date     | DATE         | NOT NULL          | Attendance date      |
| check_in            | TIMESTAMP    | NULL              | Check-in time        |
| check_out           | TIMESTAMP    | NULL              | Check-out time       |
| break_start         | TIMESTAMP    | NULL              | Break start          |
| break_end           | TIMESTAMP    | NULL              | Break end            |
| working_hours       | DECIMAL(5,2) | DEFAULT 0         | Total working hours  |
| overtime_hours      | DECIMAL(5,2) | DEFAULT 0         | Total overtime hours |
| late_minutes        | INTEGER      | DEFAULT 0         | Late duration        |
| early_leave_minutes | INTEGER      | DEFAULT 0         | Early leave duration |
| attendance_status   | ENUM         | NOT NULL          | Attendance status    |
| notes               | TEXT         | NULL              | Remarks              |
| created_at          | TIMESTAMP    | DEFAULT now()     | Created time         |
| updated_at          | TIMESTAMP    | DEFAULT now()     | Updated time         |

```

Attendance Status

- PRESENT
- LATE
- ABSENT
- LEAVE
- SICK
- HOLIDAY
- REMOTE
- BUSINESS_TRIP

Relationship

```
employees
1 -------- N attendance_records
```

# Leave Module

### leave_types

```
| Column            | Type         | Constraint   |
| ----------------- | ------------ | ------------ |
| id                | UUID         | PK           |
| code              | VARCHAR(20)  | UNIQUE       |
| name              | VARCHAR(100) | NOT NULL     |
| annual_quota      | INTEGER      | DEFAULT 0    |
| description       | TEXT         | NULL         |
| is_paid           | BOOLEAN      | DEFAULT TRUE |
| requires_approval | BOOLEAN      | DEFAULT TRUE |
| created_at        | TIMESTAMP    |              |
| updated_at        | TIMESTAMP    |              |

```

example

- AL : Annual Leave
- SL : Sick Leave
- ML : Maternity Leave
- PL : Paternity Leave
- UL : Unpaid Leave

### leave_requests

```
| Column          | Type         | Constraint          |
| --------------- | ------------ | ------------------- |
| id              | UUID         | PK                  |
| employee_id     | UUID         | FK → employees.id   |
| leave_type_id   | UUID         | FK → leave_types.id |
| approver_id     | UUID         | FK → users.id NULL  |
| start_date      | DATE         |                     |
| end_date        | DATE         |                     |
| total_days      | DECIMAL(4,1) |                     |
| reason          | TEXT         |                     |
| attachment_url  | TEXT         | NULL                |
| status          | ENUM         |                     |
| approved_at     | TIMESTAMP    | NULL                |
| rejected_reason | TEXT         | NULL                |
| created_at      | TIMESTAMP    |                     |
| updated_at      | TIMESTAMP    |                     |

```

Leave Status

- PENDING
- APPROVED
- REJECTED
- CANCELLED

Relationship

```
employees

1 -------- N leave_requests

leave_types

1 -------- N leave_requests

users

1 -------- N leave_requests
```

# Overtime Module

```

| Column          | Type         | Constraint         |
| --------------- | ------------ | ------------------ |
| id              | UUID         | PK                 |
| employee_id     | UUID         | FK → employees.id  |
| approver_id     | UUID         | FK → users.id NULL |
| overtime_date   | DATE         |                    |
| start_time      | TIMESTAMP    |                    |
| end_time        | TIMESTAMP    |                    |
| total_hours     | DECIMAL(4,2) |                    |
| reason          | TEXT         |                    |
| status          | ENUM         |                    |
| approved_at     | TIMESTAMP    | NULL               |
| rejected_reason | TEXT         | NULL               |
| created_at      | TIMESTAMP    |                    |
| updated_at      | TIMESTAMP    |                    |

```

Overtime Status

- PENDING
- APPROVED
- REJECTED
- CANCELLED

Relationship

```
employees

1 -------- N overtime_requests

users

1 -------- N overtime_requests
```

```
                  Employees
                      │
      ┌───────────────┼─────────────────┐
      │               │                 │
      ▼               ▼                 ▼

Attendance      Leave Request      Overtime Request
      │               │                 │
      │               │                 │
      │               ▼                 ▼
      │          Leave Type         Approved By
      │
      ▼
Attendance Status
```

Cardinality

```
Employees

1 -------- N Attendance Records

Employees

1 -------- N Leave Requests

Employees

1 -------- N Overtime Requests

Leave Types

1 -------- N Leave Requests

Users

1 -------- N Leave Requests (Approver)

Users

1 -------- N Overtime Requests (Approver)
```
