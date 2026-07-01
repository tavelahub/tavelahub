# Announcement Module

### announcements

```
| Column     | Type         | Constraint     | Description           |
| ---------- | ------------ | -------------- | --------------------- |
| id         | UUID         | PK             | Announcement ID       |
| title      | VARCHAR(255) | NOT NULL       | Announcement title    |
| content    | TEXT         | NOT NULL       | Announcement content  |
| category   | ENUM         | NOT NULL       | Announcement category |
| priority   | ENUM         | DEFAULT NORMAL | Priority level        |
| status     | ENUM         | DEFAULT DRAFT  | Publication status    |
| publish_at | TIMESTAMP    | NULL           | Publish schedule      |
| expire_at  | TIMESTAMP    | NULL           | Expiration time       |
| created_by | UUID         | FK → users.id  | Creator               |
| created_at | TIMESTAMP    | DEFAULT now()  | Created time          |
| updated_at | TIMESTAMP    | DEFAULT now()  | Updated time          |

```

Announcement Category

- GENERAL
- HR
- EVENT
- POLICY
- SYSTEM
- PAYROLL
  Announcement Priority
- LOW
- NORMAL
- HIGH
- URGENT

### announcement_targets

```
| Column          | Type      | Constraint            |
| --------------- | --------- | --------------------- |
| id              | UUID      | PK                    |
| announcement_id | UUID      | FK → announcements.id |
| target_type     | ENUM      |                       |
| target_id       | UUID      | NULL                  |
| created_at      | TIMESTAMP |                       |
```

Target Type

- ALL
- DEPARTMENT
- POSITION
- EMPLOYEE

Example

- ALL
- DEPARTMENT → IT
- POSITION → HR Manager
- EMPLOYEE → John Doe

### announcement_reads

| Column          | Type      | Constraint            |
| --------------- | --------- | --------------------- |
| id              | UUID      | PK                    |
| announcement_id | UUID      | FK → announcements.id |
| employee_id     | UUID      | FK → employees.id     |
| read_at         | TIMESTAMP |                       |
| created_at      | TIMESTAMP |                       |

### announcement_attachments

```
| Column          | Type         | Constraint            |
| --------------- | ------------ | --------------------- |
| id              | UUID         | PK                    |
| announcement_id | UUID         | FK → announcements.id |
| file_name       | VARCHAR(255) |                       |
| file_url        | TEXT         |                       |
| file_size       | BIGINT       |                       |
| mime_type       | VARCHAR(100) |                       |
| created_at      | TIMESTAMP    |                       |

```

Relationship

```
announcements

1 -------- N announcement_targets

announcements

1 -------- N announcement_reads

announcements

1 -------- N announcement_attachments

employees

1 -------- N announcement_reads

users

1 -------- N announcements
```

# Notification Module

### notifications

```
| Column         | Type         | Constraint     | Description       |
| -------------- | ------------ | -------------- | ----------------- |
| id             | UUID         | PK             | Notification ID   |
| user_id        | UUID         | FK → users.id  | Recipient         |
| type           | ENUM         | NOT NULL       | Notification type |
| title          | VARCHAR(255) | NOT NULL       | Title             |
| message        | TEXT         | NOT NULL       | Message           |
| reference_type | VARCHAR(100) | NULL           | Related module    |
| reference_id   | UUID         | NULL           | Related entity    |
| priority       | ENUM         | DEFAULT NORMAL | Priority          |
| is_read        | BOOLEAN      | DEFAULT FALSE  | Read status       |
| read_at        | TIMESTAMP    | NULL           | Read time         |
| created_at     | TIMESTAMP    | DEFAULT now()  | Created time      |
```

Notification Type

- ATTENDANCE
- LEAVE
- PAYROLL
- REIMBURSEMENT
- ANNOUNCEMENT
- SYSTEM

Notification Priority

- LOW
- NORMAL
- HIGH
- URGENT

### notification_preferences

```
| Column                | Type      | Constraint           |
| --------------------- | --------- | -------------------- |
| id                    | UUID      | PK                   |
| user_id               | UUID      | FK → users.id UNIQUE |
| email_enabled         | BOOLEAN   | DEFAULT TRUE         |
| push_enabled          | BOOLEAN   | DEFAULT TRUE         |
| announcement_enabled  | BOOLEAN   | DEFAULT TRUE         |
| payroll_enabled       | BOOLEAN   | DEFAULT TRUE         |
| attendance_enabled    | BOOLEAN   | DEFAULT TRUE         |
| leave_enabled         | BOOLEAN   | DEFAULT TRUE         |
| reimbursement_enabled | BOOLEAN   | DEFAULT TRUE         |
| created_at            | TIMESTAMP |                      |
| updated_at            | TIMESTAMP |                      |

```

Relationship

```
users

1 -------- N notifications

users

1 -------- 1 notification_preferences
```

## Relationship Diagram

```
                          Users
                            │
            ┌───────────────┼────────────────┐
            │               │                │
            ▼               ▼                ▼

     Announcements    Notifications   Notification Preferences
            │
            │
      ┌─────┼───────────────┐
      │     │               │
      ▼     ▼               ▼

 Targets   Reads      Attachments
            │
            ▼
       Employees
```

## Cardinality

```
Users

1 -------- N Announcements (created_by)

Announcements

1 -------- N Announcement Targets

Announcements

1 -------- N Announcement Reads

Announcements

1 -------- N Announcement Attachments

Employees

1 -------- N Announcement Reads

Users

1 -------- N Notifications

Users

1 -------- 1 Notification Preferences
```
