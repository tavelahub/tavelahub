# Payroll Module

```
| Column               | Type          | Constraint        | Description                         |
| -------------------- | ------------- | ----------------- | ----------------------------------- |
| id                   | UUID          | PK                | Payroll ID                          |
| employee_id          | UUID          | FK → employees.id | Employee                            |
| payroll_period       | DATE          | NOT NULL          | Payroll period (first day of month) |
| basic_salary         | DECIMAL(15,2) | NOT NULL          | Basic salary                        |
| allowance            | DECIMAL(15,2) | DEFAULT 0         | Total allowance                     |
| overtime_pay         | DECIMAL(15,2) | DEFAULT 0         | Overtime compensation               |
| reimbursement_amount | DECIMAL(15,2) | DEFAULT 0         | Approved reimbursement              |
| deduction            | DECIMAL(15,2) | DEFAULT 0         | Salary deduction                    |
| tax                  | DECIMAL(15,2) | DEFAULT 0         | Tax deduction                       |
| gross_salary         | DECIMAL(15,2) | NOT NULL          | Gross salary                        |
| net_salary           | DECIMAL(15,2) | NOT NULL          | Final salary                        |
| status               | ENUM          | NOT NULL          | Payroll status                      |
| paid_at              | TIMESTAMP     | NULL              | Payment timestamp                   |
| generated_by         | UUID          | FK → users.id     |                                     |
| created_at           | TIMESTAMP     | DEFAULT now()     | Created time                        |
| updated_at           | TIMESTAMP     | DEFAULT now()     | Updated time                        |
```

Payroll Status

- DRAFT
- GENERATED
- PAID
- CANCELLED

relationship

```
employees

1 -------- N payrolls

users

1 -------- N payrolls
```

# Reimbursement Module

### reimbursement_categories

```
| Column      | Type         | Constraint |
| ----------- | ------------ | ---------- |
| id          | UUID         | PK         |
| code        | VARCHAR(20)  | UNIQUE     |
| name        | VARCHAR(100) |            |
| description | TEXT         | NULL       |
| created_at  | TIMESTAMP    |            |
| updated_at  | TIMESTAMP    |            |
```

example category

- TRAVEL
- MEAL
- MEDICAL
- TRAINING
- OFFICE
- OTHER

### reimbursement_requests

```
| Column             | Type          | Constraint                       | Description        |
| ------------------ | ------------- | -------------------------------- | ------------------ |
| id                 | UUID          | PK                               | Request ID         |
| employee_id        | UUID          | FK → employees.id                | Employee           |
| category_id        | UUID          | FK → reimbursement_categories.id | Category           |
| approver_id        | UUID          | FK → users.id NULL               | Approver           |
| reimbursement_date | DATE          | NOT NULL                         | Expense date       |
| amount             | DECIMAL(15,2) | NOT NULL                         | Requested amount   |
| description        | TEXT          | NULL                             | Description        |
| receipt_url        | TEXT          | NULL                             | Receipt attachment |
| status             | ENUM          | NOT NULL                         | Request status     |
| approved_amount    | DECIMAL(15,2) | DEFAULT 0                        | Approved amount    |
| approved_at        | TIMESTAMP     | NULL                             | Approval time      |
| rejected_reason    | TEXT          | NULL                             | Rejection reason   |
| created_at         | TIMESTAMP     | DEFAULT now()                    | Created time       |
| updated_at         | TIMESTAMP     | DEFAULT now()                    | Updated time       |


```

Reimbursement Status

- PENDING
- APPROVED
- REJECTED
- PAID
- CANCELLED

Relationship

```
employees

1 -------- N reimbursement_requests

reimbursement_categories

1 -------- N reimbursement_requests

users

1 -------- N reimbursement_requests
```

# Document Module

### documents

```
| Column        | Type         | Constraint         | Description         |
| ------------- | ------------ | ------------------ | ------------------- |
| id            | UUID         | PK                 | Document ID         |
| employee_id   | UUID         | FK → employees.id  | Employee            |
| uploaded_by   | UUID         | FK → users.id      | Uploaded by         |
| document_type | ENUM         | NOT NULL           | Document type       |
| title         | VARCHAR(255) | NOT NULL           | Document title      |
| file_name     | VARCHAR(255) | NOT NULL           | Original filename   |
| file_url      | TEXT         | NOT NULL           | Storage URL         |
| file_size     | BIGINT       | NOT NULL           | File size (bytes)   |
| mime_type     | VARCHAR(100) | NOT NULL           | MIME type           |
| expires_at    | DATE         | NULL               | Expiration date     |
| is_verified   | BOOLEAN      | DEFAULT FALSE      | Verification status |
| verified_by   | UUID         | FK → users.id NULL | Verified by         |
| verified_at   | TIMESTAMP    | NULL               | Verification time   |
| created_at    | TIMESTAMP    | DEFAULT now()      | Created time        |
| updated_at    | TIMESTAMP    | DEFAULT now()      | Updated time        |

```

Document Type

- IDENTITY_CARD
- FAMILY_CARD
- TAX_DOCUMENT
- BANK_ACCOUNT
- EMPLOYMENT_CONTRACT
- CERTIFICATE
- MEDICAL
- PERFORMANCE
- OTHER

Relationship

```
employees

1 -------- N documents

users

1 -------- N documents (uploaded_by)

users

1 -------- N documents (verified_by)
```

# Relationship Diagram

```

                         Employees
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
        ▼                    ▼                    ▼

    Payrolls        Reimbursement Requests     Documents
        │                    │                    │
        │                    │                    │
        ▼                    ▼                    ▼

 Generated By        Reimbursement Category   Uploaded By
      Users                 │                    │
                             ▼                   ▼
                          Approved By       Verified By
                              Users             Users
```

# Cardinality

```
Employees

1 -------- N Payrolls

Employees

1 -------- N Reimbursement Requests

Employees

1 -------- N Documents

Users

1 -------- N Payrolls

Users

1 -------- N Reimbursement Requests

Users

1 -------- N Documents (Uploader)

Users

1 -------- N Documents (Verifier)

Reimbursement Categories

1 -------- N Reimbursement Requests
```
