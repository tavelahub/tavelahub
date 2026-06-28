# Coding Standard

## 1. Overview

This document defines the coding standards and development conventions for the Human Resource Information System (HRIS).

The purpose of this standard is to ensure code consistency, readability, maintainability, and collaboration across the development team.

# 2. General Principles

Developers should follow these principles:

- Write clean and readable code.
- Keep functions and components small and focused.
- Avoid code duplication (DRY).
- Prefer simplicity over unnecessary complexity (KISS).
- Separate business logic from presentation.
- Follow SOLID principles where applicable.
- Write self-documenting code with meaningful names.

# 3. Technology Stack

## Frontend

- React
- TypeScript
- React Router
- Tailwind CSS
- shadcn/ui
- TanStack Query
- Zustand
- React Hook Form
- Zod

## Backend

- Bun
- Hono
- Prisma ORM
- PostgreSQL
- TypeScript
- Zod
- Scalar OpenAPI

# 4. Code Formatting

The project uses:

- ESLint
- Prettier

Rules:

- Indentation: 2 spaces
- UTF-8 encoding
- LF line ending
- Remove trailing spaces
- Use semicolons
- Use single quotes
- Maximum line length: 100 characters

# 5. Naming Convention

## Variables

Use camelCase.

```ts
const employeeName = "";
const totalSalary = 0;
```

## Functions

Use camelCase.

```ts
calculateSalary();
createEmployee();
getAttendanceHistory();
```

## Constants

Use UPPER_SNAKE_CASE.

```ts
const MAX_UPLOAD_SIZE = 5000000;
const DEFAULT_PAGE_SIZE = 10;
```

## Interfaces

Use PascalCase.

```ts
interface Employee {}
interface CreateEmployeeRequest {}
```

## Types

Use PascalCase.

```ts
type EmployeeStatus = "ACTIVE" | "INACTIVE";
```

## Enums

Use PascalCase.

```ts
enum UserRole {
  SUPER_ADMIN,
  HR_ADMIN,
  MANAGER,
  EMPLOYEE,
}
```

## React Components

Use PascalCase.

```text
EmployeeTable.tsx
EmployeeCard.tsx
LeaveRequestForm.tsx
```

## Files

Use kebab-case unless exporting a React component.

Examples:

```text
employee.service.ts
employee.repository.ts
employee.validation.ts
employee.routes.ts
employee.schema.ts
```

React components:

```text
EmployeeTable.tsx
EmployeeForm.tsx
DashboardCard.tsx
```

## Folder Names

Use kebab-case.

```text
employee-management
attendance-management
shared-components
```

# 6. Project Structure

## Frontend

```text
src/
├── app/
├── components/
├── features/
├── hooks/
├── layouts/
├── lib/
├── pages/
├── routes/
├── services/
├── stores/
├── styles/
├── types/
├── utils/
└── constants/
```

## Backend

```text
src/
├── modules/
├── middleware/
├── lib/
├── database/
├── routes/
├── utils/
├── types/
├── config/
└── app.ts
```

# 7. Import Order

Imports should be ordered as follows:

1. External packages
2. Internal libraries
3. Components
4. Hooks
5. Types
6. Styles

Example:

```ts
import { useState } from "react";

import { Button } from "@/components/ui/button";

import { useEmployees } from "@/hooks/use-employees";

import type { Employee } from "@/types/employee";

import "./employee.css";
```

# 8. API Naming

Use RESTful naming.

Good:

GET /employees

POST /employees

PUT /employees/{id}

DELETE /employees/{id}

Avoid:

GET /getEmployees

POST /createEmployee

# 9. Database Naming

Tables

Use plural nouns.

```text
employees
departments
attendance_records
leave_requests
```

Columns

Use snake_case.

```text
employee_id
created_at
updated_at
deleted_at
```

# 10. Git Branch Naming

Use the following prefixes:

```text
main
develop

feature/employee-management
feature/attendance

bugfix/login-error

hotfix/token-expired

refactor/user-service

docs/update-readme

test/auth-module

chore/update-eslint
```

# 11. Commit Convention

Use Conventional Commits.

Examples:

```text
feat: add employee management

fix: resolve login validation

docs: update API specification

refactor: simplify attendance service

test: add payroll service tests

style: format source code

chore: update dependencies
```

# 12. Pull Request Rules

Each Pull Request must:

- Target the `develop` branch.
- Include a clear description.
- Reference related issue(s).
- Pass linting and tests.
- Receive at least one approval before merging.

# 13. Error Handling

Never expose internal errors.

Good:

```json
{
  "success": false,
  "message": "Employee not found."
}
```

Bad:

```text
PrismaClientKnownRequestError...
```

# 14. Logging

Log only important events.

Examples:

- Login
- Logout
- Failed authentication
- Data creation
- Data update
- Data deletion
- Unexpected server errors

Never log:

- Passwords
- Access tokens
- Refresh tokens
- Sensitive personal data

# 15. Validation

All incoming requests must be validated using Zod.

Example:

```ts
const CreateEmployeeSchema = z.object({
  fullName: z.string().min(3),
  email: z.string().email(),
});
```

# 16. Environment Variables

Do not hardcode sensitive values.

Good:

```ts
process.env.DATABASE_URL;
```

Bad:

```ts
const DATABASE_URL = "postgres://...";
```

# 17. Code Review Checklist

Before approving a Pull Request, reviewers should verify:

- Code follows project conventions.
- No duplicated logic.
- No unused variables.
- Proper error handling.
- Validation is implemented.
- Naming is consistent.
- Documentation updated if needed.
- Tests added or updated.

# 18. Documentation

Each module should include:

- Purpose
- API endpoints
- Database model
- Validation rules
- Business logic (if applicable)

# 19. Testing

Recommended coverage:

- Unit Test
- Integration Test

Critical modules:

- Authentication
- Payroll
- Attendance
- Leave Approval

# 20. Security

Developers must:

- Hash passwords.
- Validate all inputs.
- Use parameterized database queries (handled by Prisma).
- Protect routes with authentication middleware.
- Enforce Role-Based Access Control (RBAC).
- Never commit secrets or credentials.

# 21. Definition of Done

A task is considered complete when:

- Feature is implemented.
- Code passes linting.
- Tests pass.
- Documentation is updated.
- Pull Request is approved.
- Merged into the `develop` branch.
