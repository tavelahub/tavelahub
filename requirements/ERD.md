```
                                +----------------+
                                |     roles      |
                                +----------------+
                                | id             |
                                | name           |
                                | description    |
                                +--------+-------+
                                         |
                                         |
                                         |
                                +--------v-------+
                                |     users      |
                                +----------------+
                                | id             |
                                | employee_id FK |
                                | role_id FK     |
                                | email          |
                                | password       |
                                | status         |
                                +--------+-------+
                                         |
                                         |
                                         |
                           +-------------v--------------+
                           |        employees           |
                           +----------------------------+
                           | id                         |
                           | employee_number            |
                           | department_id FK           |
                           | position_id FK             |
                           | first_name                 |
                           | last_name                  |
                           | gender                     |
                           | birth_date                 |
                           | phone                      |
                           | address                    |
                           | hire_date                  |
                           | employment_status          |
                           +------+---------------------+
                                  |
          ------------------------------------------------------------
          |            |             |           |          |         |
          |            |             |           |          |         |
+---------v--+ +-------v-----+ +-----v-----+ +---v-----+ +--v-----+ +-v---------+
|attendance  | | leave_req   | | overtime  | | payroll | | docs   | |reimburse  |
+------------+ +-------------+ +-----------+ +---------+ +---------+ +-----------+
| id         | | id          | | id        | | id      | | id      | | id        |
| employeeFK | | employee FK | | employee  | | emp FK  | | emp FK  | | emp FK    |
| check_in   | | leave_type  | | date      | | period  | | type    | | category  |
| check_out  | | start_date  | | hours     | | salary  | | file    | | amount    |
| status     | | end_date    | | reason    | | bonus   | | url     | | receipt   |
+------------+ | status      | | status    | | deduct  | +---------+ | status    |
               +-------------+ +-----------+ +---------+ +-----------+

       +-------------------+
       | departments       |
       +-------------------+
       | id                |
       | name              |
       | description       |
       +--------+----------+
                |
                |
        +-------v-------+
        | positions     |
        +---------------+
        | id            |
        | department FK |
        | name          |
        | level         |
        +---------------+

                 +----------------------+
                 | announcements        |
                 +----------------------+
                 | id                   |
                 | title                |
                 | content              |
                 | published_at         |
                 +----------------------+

                 +----------------------+
                 | notifications        |
                 +----------------------+
                 | id                   |
                 | user_id FK           |
                 | title                |
                 | message              |
                 | is_read              |
                 +----------------------+

                 +----------------------+
                 | audit_logs           |
                 +----------------------+
                 | id                   |
                 | user_id FK           |
                 | action               |
                 | module               |
                 | created_at           |
                 +----------------------+
```

## Relationship

| Table       | Relationship                          |
| ----------- | ------------------------------------- |
| Roles       | One Role → Many Users                 |
| Users       | One User → One Employee               |
| Departments | One Department → Many Employees       |
| Departments | One Department → Many Positions       |
| Positions   | One Position → Many Employees         |
| Employees   | One Employee → Many Attendance        |
| Employees   | One Employee → Many Leave Requests    |
| Employees   | One Employee → Many Overtime Requests |
| Employees   | One Employee → Many Payroll Records   |
| Employees   | One Employee → Many Documents         |
| Employees   | One Employee → Many Reimbursements    |
| Users       | One User → Many Notifications         |
| Users       | One User → Many Audit Logs            |

## Database Modules

| Database Modules | modules           |
| ---------------- | ----------------- | ----------- | --------- |
| Authentication   | roles             | users       |
| Employee         | employees         | departments | positions |
| Attendance       | attendance        |
| Leave            | leave_requests    |
| Overtime         | overtime_requests |
| Payroll          | payrolls          |
| Reimbursement    | reimbursements    |
| Documents        | documents         |
| Notification     | notifications     |
| Announcement     | announcements     |
| Audit            | audit_logs        |

| Module         |        Tables |
| -------------- | ------------: |
| Authentication |             2 |
| Employee       |             3 |
| Attendance     |             1 |
| Leave          |             1 |
| Overtime       |             1 |
| Payroll        |             1 |
| Reimbursement  |             1 |
| Documents      |             1 |
| Notification   |             1 |
| Announcement   |             1 |
| Audit          |             1 |
| **Total**      | **14 Tables** |
