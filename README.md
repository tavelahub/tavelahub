# tavelahub
TavelaHub primary documentation


# HRIS - Human Resource Information System

![HRIS Banner](./assets/banner.png)

## Overview

HRIS (Human Resource Information System) is a web-based application designed to simplify and automate human resource management processes within an organization.

The system provides centralized management for employee information, attendance, leave requests, payroll, documents, and HR-related workflows.

This project is developed as a portfolio project using modern web technologies with a focus on scalability, maintainability, and real-world software development practices.

---

# Project Goals

The main objectives of this project are:

- Build a modern HR management platform
- Implement scalable frontend and backend architecture
- Apply clean code and software engineering practices
- Create realistic business workflows
- Implement role-based access control
- Provide comprehensive project documentation

---

# Project Scope

## Included Features

### Authentication & Authorization
- User login
- Logout
- JWT authentication
- Role-based access control
- Permission management

### Employee Management
- Employee profiles
- Personal information
- Employment information
- Department management
- Position management

### Attendance Management
- Employee attendance tracking
- Check-in
- Check-out
- Attendance history

### Leave Management
- Leave request
- Leave approval
- Leave balance tracking

### Overtime Management
- Overtime request
- Overtime approval
- Overtime history

### Payroll Management
- Salary management
- Allowances
- Deductions
- Payslip generation

### Reimbursement Management
- Expense submission
- Approval workflow
- Reimbursement history

### Reporting
- HR reports
- Attendance reports
- Payroll reports
- Export reports

### Administration
- User management
- Role management
- System settings
- Audit logs

                User
                  |
                  |
          React Frontend
                  |
                  |
          REST API Backend
                  |
                  |
          PostgreSQL Database


---

# Repository Structure

This project consists of three repositories:

## 1. Documentation Repository
hris-docs

Contains:

- Project documentation
- Requirements
- Architecture diagrams
- Database design
- API documentation
- Deployment guide


## 2. Frontend Repository

hris-web


Contains:

- User interface
- Client-side logic
- API integration
- Application pages


## 3. Backend Repository


hris-api


Contains:

- REST API
- Business logic
- Database management
- Authentication
- Authorization

---

# Technology Stack

## Frontend

| Technology | Purpose |
|------------|---------|
| React | Frontend library |
| TypeScript | Programming language |
| Vite | Build tool |
| React Router | Client-side routing |
| Tailwind CSS | Styling framework |
| shadcn/ui | UI component library |
| Radix UI | Accessible components |
| TanStack Query | Server state management |
| Zustand | Client state management |
| React Hook Form | Form management |
| Zod | Data validation |
| Axios | HTTP client |

---

## Backend

| Technology | Purpose |
|------------|---------|
| Bun | JavaScript runtime |
| Hono | Backend framework |
| TypeScript | Programming language |
| Prisma ORM | Database ORM |
| PostgreSQL | Database |
| Zod | Schema validation |
| JWT | Authentication |
| Scalar OpenAPI | API documentation |
| Pino | Logging |

---

## Infrastructure

| Technology | Purpose |
|------------|---------|
| GitHub | Source control |
| GitHub Actions | CI/CD |
| Supabase | PostgreSQL hosting |
| Netlify | Frontend deployment |
| Railway | Backend deployment |

---

# User Roles

The system supports several user roles:

## Super Admin

Responsibilities:

- Manage users
- Manage permissions
- Configure system settings


## HR Admin

Responsibilities:

- Manage employees
- Manage attendance
- Manage payroll
- Manage HR documents


## Manager

Responsibilities:

- Review employee requests
- Approve leave
- Monitor team performance


## Employee

Responsibilities:

- Manage personal profile
- Submit leave requests
- View attendance
- View payslips

---

# Development Workflow

## Git Branch Strategy
main
|
develop
|
feature/*


Branch naming:


feature/add-authentication
feature/create-employee-api
bugfix/fix-login-error
docs/update-readme


---

# Commit Convention

This project follows Conventional Commits.

Examples:


feat: add employee management module

fix: resolve attendance calculation issue

docs: update API documentation

refactor: improve authentication service

test: add employee service tests


---

# Documentation

Project documentation includes:

## Requirements

- Business Requirements
- Functional Requirements
- Non Functional Requirements


## Architecture

- System Architecture
- Component Diagram
- Deployment Diagram


## Database

- ERD
- Database Schema
- Table Dictionary


## API

- API Specification
- Authentication Flow
- Error Handling


## Development

- Setup Guide
- Coding Standard
- Git Workflow


## Deployment

- Environment Configuration
- Production Deployment Guide

---

# Development Timeline

## Phase 1 - Planning

- Requirement analysis
- Feature planning
- System design


## Phase 2 - Development Setup

- Repository setup
- Development environment
- CI/CD configuration


## Phase 3 - Core Development

- Authentication
- Employee Management
- Attendance
- Leave
- Payroll


## Phase 4 - Testing

- Unit testing
- Integration testing
- Bug fixing


## Phase 5 - Production

- Production deployment
- Final testing
- Documentation completion

---

# Deployment Architecture

Developer
|
|
GitHub Repository
|
|
| |
Frontend Backend
Netlify Railway
| |
      |
   Supabase
  PostgreSQL

---

# Team

| Role | Responsibility |
|-|-|
| Tech Lead | Architecture, Code Review, Technical Direction |
| Frontend Developer | React Application Development |
| Backend Developer | API, Database, Infrastructure |

---

# License

This project is created for learning and portfolio purposes.

---

# Author

HRIS Development Team

---

# System Architecture

The project follows a separated repository architecture.
