# Deployment Architecture

## Overview

This document describes the production deployment architecture for the Human Resource Information System (HRIS).

The application follows a modern three-tier architecture consisting of:

- Presentation Layer
- Application Layer
- Data Layer

# Architecture Diagram

```text
                     ┌──────────────────────┐
                     │        Users         │
                     └──────────┬───────────┘
                                │
                                │ HTTPS
                                │
                ┌───────────────▼────────────────┐
                │          Netlify               │
                │      React Frontend (Vite)     │
                └───────────────┬────────────────┘
                                │
                                │ HTTPS REST API
                                │
                ┌───────────────▼────────────────┐
                │          Railway               │
                │     Hono API + Bun Runtime     │
                └───────────────┬────────────────┘
                                │
                  Prisma ORM    │
                                │
                ┌───────────────▼────────────────┐
                │          Supabase              │
                │       PostgreSQL Database      │
                └────────────────────────────────┘
```

# Deployment Components

## Frontend

Platform

- Netlify

Technology

- React
- TypeScript
- Vite
- Tailwind CSS
- shadcn/ui

Responsibilities

- Render user interface
- Handle client-side routing
- Manage application state
- Call backend REST API

## Backend

Platform

- Railway

Technology

- Bun
- Hono
- Prisma ORM
- TypeScript

Responsibilities

- Business logic
- Authentication
- Authorization
- Data validation
- REST API
- Database access

## Database

Platform

- Supabase

Technology

- PostgreSQL

Responsibilities

- Store application data
- Maintain data integrity
- Backup and recovery
- Secure data access

# Request Flow

```
User

↓

React Application

↓

REST API Request

↓

Hono Backend

↓

Prisma ORM

↓

PostgreSQL

↓

Response

↓

React UI
```

# Source Code Management

Platform

- GitHub

Repositories

```
hris-docs
```

Project documentation.

```
hris-web
```

Frontend application.

```
hris-api
```

Backend REST API.

# CI/CD Flow

```
Developer

↓

Git Commit

↓

Git Push

↓

GitHub Repository

↓

GitHub Actions

↓

Frontend Build

↓

Deploy to Netlify

↓

Backend Build

↓

Deploy to Railway
```

# Environment Configuration

## Frontend

Example

```
VITE_API_URL=https://api.example.com/api/v1
```

## Backend

Example

```
DATABASE_URL=

DIRECT_URL=

JWT_SECRET=

JWT_EXPIRES_IN=

APP_URL=

FRONTEND_URL=
```

# Production Environment

## Frontend

Hosting

Netlify

Example URL

```
https://hris.example.com
```

## Backend

Hosting

Railway

Example URL

```
https://api.hris.example.com
```

## Database

Hosting

Supabase PostgreSQL

# API Documentation

Platform

Scalar OpenAPI

Development

```
http://localhost:3000/docs
```

Production

```
https://api.example.com/docs
```

# Security

HTTPS

All production traffic must use HTTPS.

Authentication

JWT Bearer Token

Authorization

Role-Based Access Control (RBAC)

Password Storage

Passwords must be hashed using bcrypt or Argon2.

Secrets

Secrets must be stored as environment variables.

# Monitoring

Production monitoring should include:

- Application Logs
- API Logs
- Error Logs
- Database Logs

# Backup

Database

Supabase automated backups.

Recommended backup schedule:

- Daily backup
- Weekly backup
- Monthly backup

# Disaster Recovery

If deployment fails:

1. Roll back to previous deployment.
2. Verify environment variables.
3. Verify database connection.
4. Redeploy application.

# Deployment Checklist

Before deploying:

- All tests passed
- ESLint passed
- Build successful
- Database migrations executed
- Environment variables configured
- Documentation updated

# Infrastructure Summary

| Layer          | Technology     | Platform |
| -------------- | -------------- | -------- |
| Frontend       | React + Vite   | Netlify  |
| Backend        | Hono + Bun     | Railway  |
| ORM            | Prisma         | Railway  |
| Database       | PostgreSQL     | Supabase |
| Documentation  | Scalar OpenAPI | Railway  |
| Source Control | Git            | GitHub   |

# Future Improvements

Potential infrastructure enhancements:

- Docker containerization
- GitHub Actions CI/CD automation
- Redis caching
- CDN integration
- Object Storage (Supabase Storage)
- Email service integration
- Monitoring with Grafana and Prometheus
