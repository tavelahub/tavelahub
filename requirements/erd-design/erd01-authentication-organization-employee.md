# Authentication Module

### users

```
| Column            | Type         | Constraint              | Description                  |
| ----------------- | ------------ | ----------------------- | ---------------------------- |
| id                | UUID         | PK                      | User ID                      |
| employee_id       | UUID         | FK → employees.id, NULL | Linked employee              |
| role_id           | UUID         | FK → roles.id           | User role                    |
| email             | VARCHAR(255) | UNIQUE, NOT NULL        | Login email                  |
| password          | TEXT         | NOT NULL                | Hashed password              |
| email_verified_at | TIMESTAMP    | NULL                    | Email verification timestamp |
| is_active         | BOOLEAN      | DEFAULT TRUE            | User status                  |
| last_login_at     | TIMESTAMP    | NULL                    | Last login                   |
| created_at        | TIMESTAMP    | DEFAULT now()           | Created time                 |
| updated_at        | TIMESTAMP    | DEFAULT now()           | Updated time                 |
| deleted_at        | TIMESTAMP    | NULL                    | Soft delete                  |
```

### roles

```
| Column      | Type         | Constraint |
| ----------- | ------------ | ---------- |
| id          | UUID         | PK         |
| name        | VARCHAR(100) | UNIQUE     |
| description | TEXT         | NULL       |
| created_at  | TIMESTAMP    |            |
| updated_at  | TIMESTAMP    |            |

```

Example:

- Super Admin
- HR Admin
- Manager
- Employee

### permissions

```
| Column      | Type         | Constraint |
| ----------- | ------------ | ---------- |
| id          | UUID         | PK         |
| module      | VARCHAR(100) |            |
| action      | VARCHAR(100) |            |
| description | TEXT         | NULL       |
| created_at  | TIMESTAMP    |            |
```

Example:

| module   | action  |
| -------- | ------- |
| Employee | create  |
| Employee | update  |
| Payroll  | approve |

### role_permissions

Many-to-many table

```
| Column        | Type | Constraint |
| ------------- | ---- | ---------- |
| role_id       | UUID | FK         |
| permission_id | UUID | FK         |

```

### refresh_tokens

```
| Column     | Type      | Constraint  |
| ---------- | --------- | ----------- |
| id         | UUID      | PK          |
| user_id    | UUID      | FK users.id |
| token      | TEXT      | UNIQUE      |
| expires_at | TIMESTAMP |             |
| revoked_at | TIMESTAMP | NULL        |
| created_at | TIMESTAMP |             |

```

### user_sessions

```
| Column           | Type         | Constraint           |
| ---------------- | ------------ | -------------------- |
| id               | UUID         | PK                   |
| user_id          | UUID         | FK users.id          |
| refresh_token_id | UUID         | FK refresh_tokens.id |
| ip_address       | VARCHAR(100) |                      |
| user_agent       | TEXT         |                      |
| device_name      | VARCHAR(255) |                      |
| platform         | VARCHAR(100) |                      |
| browser          | VARCHAR(100) |                      |
| login_at         | TIMESTAMP    |                      |
| expired_at       | TIMESTAMP    |                      |
| logout_at        | TIMESTAMP    | NULL                 |

```

# Organization Module

### departments

```
| Column      | Type         | Constraint           |
| ----------- | ------------ | -------------------- |
| id          | UUID         | PK                   |
| code        | VARCHAR(20)  | UNIQUE               |
| name        | VARCHAR(150) |                      |
| manager_id  | UUID         | FK employees.id NULL |
| description | TEXT         | NULL                 |
| created_at  | TIMESTAMP    |                      |
| updated_at  | TIMESTAMP    |                      |
```

Example:

- IT
- HR
- Finance
- Marketing

### positions

```
| Column        | Type         | Constraint        |
| ------------- | ------------ | ----------------- |
| id            | UUID         | PK                |
| department_id | UUID         | FK departments.id |
| title         | VARCHAR(150) |                   |
| level         | VARCHAR(50)  |                   |
| description   | TEXT         | NULL              |
| created_at    | TIMESTAMP    |                   |
| updated_at    | TIMESTAMP    |                   |

```

Example

- Junior Backend
- Senior Backend
- HR Staff
- Finance Manager

# Employee Module

### employees

```
| Column                  | Type         | Constraint        |
| ----------------------- | ------------ | ----------------- |
| id                      | UUID         | PK                |
| employee_number         | VARCHAR(30)  | UNIQUE            |
| department_id           | UUID         | FK departments.id |
| position_id             | UUID         | FK positions.id   |
| first_name              | VARCHAR(100) |                   |
| last_name               | VARCHAR(100) | NULL              |
| gender                  | ENUM         |                   |
| birth_place             | VARCHAR(100) |                   |
| birth_date              | DATE         |                   |
| marital_status          | ENUM         |                   |
| religion                | VARCHAR(50)  | NULL              |
| nationality             | VARCHAR(50)  | DEFAULT Indonesia |
| phone                   | VARCHAR(30)  |                   |
| address                 | TEXT         |                   |
| hire_date               | DATE         |                   |
| employment_status       | ENUM         |                   |
| profile_photo           | TEXT         | NULL              |
| emergency_contact_name  | VARCHAR(100) | NULL              |
| emergency_contact_phone | VARCHAR(30)  | NULL              |
| created_at              | TIMESTAMP    |                   |
| updated_at              | TIMESTAMP    |                   |
| deleted_at              | TIMESTAMP    | NULL              |

```

### Enum

Gender

- MALE
- FEMALE

Employment Status

- PROBATION
- CONTRACT
- PERMANENT
- RESIGNED
- TERMINATED

Marital Status

- SINGLE
- MARRIED
- DIVORCED
- WIDOWED

```

Role
 │
 │1
 ▼
Users
 │
 │N
 ▼
Refresh Tokens
 │
 │1
 ▼
User Sessions

Users
 │
 │1
 ▼
Employees

Departments
 │
 ├──────────────┐
 │              │
 ▼              ▼
Positions    Employees
      │
      │1
      ▼
Employees
```

## Cardinality

```
Role

1 -------- N Users

Users

1 -------- N Refresh Tokens

Refresh Tokens

1 -------- N User Sessions

Departments

1 -------- N Positions

Departments

1 -------- N Employees

Positions

1 -------- N Employees

Employees

1 -------- 1 Users (optional)

Employees

1 -------- N Departments (manager)
```
