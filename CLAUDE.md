# CLAUDE.md - AI Assistant Guide for Breeder Farm ERP

## Repository Overview

**Repository Name:** breeder-farm-erp
**Purpose:** Enterprise Resource Planning system for breeding and farming operations
**Current Status:** Initial setup phase - repository structure needs to be established

This document provides comprehensive guidance for AI assistants working on this codebase. It outlines the intended architecture, development workflows, coding conventions, and key considerations for building a robust ERP system for breeder farm management.

---

## Project Purpose & Domain

### What is a Breeder Farm ERP?

A Breeder Farm ERP is a comprehensive management system designed to handle all aspects of breeding operations, including:

- **Animal Management**: Registration, tracking, identification, and lifecycle management
- **Breeding Records**: Mating schedules, genealogy tracking, breeding outcomes
- **Health Management**: Veterinary records, vaccinations, treatments, health monitoring
- **Inventory Management**: Feed, supplies, equipment, medications
- **Sales & Customers**: Customer relationships, sales transactions, delivery tracking
- **Financial Management**: Expenses, revenue, profitability analysis
- **Reporting & Analytics**: Performance metrics, breeding success rates, financial reports
- **Compliance**: Regulatory requirements, certifications, documentation

### Key Domain Concepts

- **Breeding Lines/Genealogy**: Tracking parentage and genetic lineage
- **Breeding Cycles**: Managing heat cycles, gestation periods, birth records
- **Herd/Flock Management**: Group organization and management
- **Regulatory Compliance**: Health certificates, breeding permits, export documentation

---

## Recommended Technology Stack

Since this is a greenfield project, here are recommended technologies based on modern ERP best practices:

### Backend Options

**Option 1: Node.js + TypeScript**
- Framework: NestJS (enterprise-grade, modular architecture)
- ORM: TypeORM or Prisma
- API: RESTful + GraphQL (for complex queries)
- Validation: class-validator, Zod

**Option 2: Python**
- Framework: Django (batteries-included) or FastAPI (modern, async)
- ORM: Django ORM or SQLAlchemy
- API: Django REST Framework or FastAPI native

**Option 3: Ruby on Rails**
- Full-stack framework with conventions
- ActiveRecord ORM
- Strong ecosystem for business applications

### Frontend Options

**Option 1: Modern React Stack**
- Framework: Next.js (SSR, routing, API routes)
- UI Library: React with TypeScript
- State Management: Zustand, Redux Toolkit, or TanStack Query
- UI Components: shadcn/ui, Material-UI, or Ant Design
- Forms: React Hook Form + Zod validation
- Tables: TanStack Table (for complex data grids)

**Option 2: Vue.js**
- Framework: Nuxt.js
- UI: Vuetify or Element Plus
- State: Pinia

### Database

**Relational Database (Recommended):**
- PostgreSQL (preferred for complex relationships and data integrity)
- MySQL/MariaDB (alternative)

**Document Store (Supplementary):**
- MongoDB for flexible document storage (veterinary notes, reports)

### Additional Infrastructure

- **Authentication**: Auth0, Clerk, or custom JWT implementation
- **File Storage**: AWS S3, MinIO, or Cloudinary (for animal photos, documents)
- **Background Jobs**: Bull/BullMQ (Node.js), Celery (Python), Sidekiq (Ruby)
- **Real-time Updates**: Socket.io or WebSockets
- **PDF Generation**: Puppeteer, PDFKit, or specialized libraries
- **Email**: SendGrid, AWS SES, or Postmark
- **Monitoring**: Sentry (errors), Datadog/New Relic (APM)

---

## Recommended Project Structure

### Full-Stack Monorepo (Recommended)

```
breeder-farm-erp/
├── .github/
│   └── workflows/         # CI/CD pipelines
├── apps/
│   ├── api/              # Backend API
│   │   ├── src/
│   │   │   ├── modules/  # Feature modules
│   │   │   ├── common/   # Shared utilities
│   │   │   ├── config/   # Configuration
│   │   │   └── main.ts
│   │   ├── tests/
│   │   └── package.json
│   └── web/              # Frontend application
│       ├── src/
│       │   ├── components/
│       │   ├── pages/
│       │   ├── hooks/
│       │   ├── lib/
│       │   └── styles/
│       ├── public/
│       └── package.json
├── packages/             # Shared packages
│   ├── types/           # Shared TypeScript types
│   ├── utils/           # Shared utilities
│   └── ui/              # Shared UI components
├── database/
│   ├── migrations/      # Database migrations
│   ├── seeds/          # Seed data
│   └── schema/         # Schema definitions
├── docs/
│   ├── api/            # API documentation
│   ├── architecture/   # Architecture decisions
│   └── user-guide/     # User documentation
├── scripts/            # Automation scripts
├── docker/            # Docker configurations
├── .env.example       # Environment variables template
├── docker-compose.yml
├── package.json       # Root package.json (monorepo)
├── turbo.json        # Turborepo config (if using)
└── README.md
```

### Alternative: Separate Repositories

If using separate repos:
- `breeder-farm-api` - Backend
- `breeder-farm-web` - Frontend
- `breeder-farm-mobile` - Mobile app (future)

---

## Core Domain Models

### Essential Entities

1. **Animals**
   - id, registrationNumber, name, species, breed
   - dateOfBirth, sex, color, markings
   - microchipId, tattooId, tagNumber
   - status (active, sold, deceased, retired)
   - currentLocation, parentage (sire, dam)

2. **Breeding Records**
   - id, animalId (dam), sireId
   - breedingDate, breedingMethod
   - expectedDueDate, actualBirthDate
   - offspringIds, complications, notes

3. **Health Records**
   - id, animalId, recordType
   - date, veterinarianId
   - diagnosis, treatment, medications
   - vaccinations, testResults, nextDueDate

4. **Inventory**
   - id, name, category, quantity
   - unit, reorderLevel, supplier
   - cost, location

5. **Sales**
   - id, animalId, customerId
   - saleDate, price, paymentStatus
   - deliveryStatus, contractTerms

6. **Customers**
   - id, name, contactInfo
   - address, customerType
   - purchaseHistory, preferences

7. **Users**
   - id, email, role
   - permissions, preferences

### Key Relationships

- Animals have many Health Records
- Animals belong to Breeding Records (as parent or offspring)
- Breeding Records produce multiple Animals (offspring)
- Sales connect Animals to Customers
- Inventory tracks consumed items (feed, medications)

---

## Development Workflow

### Initial Setup (When Project Structure Exists)

```bash
# Clone repository
git clone <repository-url>
cd breeder-farm-erp

# Install dependencies
npm install  # or yarn/pnpm

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Set up database
npm run db:migrate
npm run db:seed  # Optional: populate with sample data

# Start development servers
npm run dev  # Starts both API and web
```

### Branch Strategy

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - Feature development
- `bugfix/*` - Bug fixes
- `hotfix/*` - Emergency production fixes
- `claude/*` - AI assistant working branches (temporary)

### Commit Conventions

Follow Conventional Commits specification:

```
type(scope): subject

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Test additions or modifications
- `chore`: Build process, dependencies, tooling

**Examples:**
```
feat(animals): add animal registration form
fix(breeding): correct gestation period calculation
docs(api): update API documentation for health records
```

### Testing Requirements

1. **Unit Tests**: All business logic, utilities, and services
2. **Integration Tests**: API endpoints, database operations
3. **E2E Tests**: Critical user workflows
4. **Minimum Coverage**: 80% for new code

### Code Quality Standards

- **Linting**: ESLint (JavaScript/TypeScript), Pylint/Flake8 (Python)
- **Formatting**: Prettier (JavaScript/TypeScript), Black (Python)
- **Type Safety**: TypeScript strict mode, Python type hints
- **Pre-commit Hooks**: Husky + lint-staged (or pre-commit for Python)

---

## Coding Conventions

### General Principles

1. **SOLID Principles**: Follow single responsibility, open/closed, dependency inversion
2. **DRY**: Don't repeat yourself - extract reusable code
3. **KISS**: Keep it simple - avoid over-engineering
4. **YAGNI**: You aren't gonna need it - build what's needed now
5. **Separation of Concerns**: Clear boundaries between layers

### Naming Conventions

**TypeScript/JavaScript:**
- Files: `kebab-case.ts`, `PascalCase.tsx` (components)
- Classes: `PascalCase`
- Functions/Variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Interfaces: `PascalCase` (no I prefix)
- Types: `PascalCase`

**Python:**
- Files: `snake_case.py`
- Classes: `PascalCase`
- Functions/Variables: `snake_case`
- Constants: `UPPER_SNAKE_CASE`

**Database:**
- Tables: `snake_case` (plural)
- Columns: `snake_case`
- Foreign Keys: `{table}_id`

### File Organization

**Backend (NestJS example):**
```
modules/
├── animals/
│   ├── animals.controller.ts
│   ├── animals.service.ts
│   ├── animals.module.ts
│   ├── dto/
│   │   ├── create-animal.dto.ts
│   │   └── update-animal.dto.ts
│   ├── entities/
│   │   └── animal.entity.ts
│   └── tests/
│       ├── animals.controller.spec.ts
│       └── animals.service.spec.ts
```

**Frontend (React example):**
```
features/
├── animals/
│   ├── components/
│   │   ├── AnimalList.tsx
│   │   ├── AnimalForm.tsx
│   │   └── AnimalCard.tsx
│   ├── hooks/
│   │   ├── useAnimals.ts
│   │   └── useAnimalForm.ts
│   ├── api/
│   │   └── animals.api.ts
│   └── types/
│       └── animal.types.ts
```

### API Design Principles

1. **RESTful Conventions:**
   - `GET /api/animals` - List all animals
   - `GET /api/animals/:id` - Get single animal
   - `POST /api/animals` - Create animal
   - `PUT /api/animals/:id` - Update animal (full)
   - `PATCH /api/animals/:id` - Update animal (partial)
   - `DELETE /api/animals/:id` - Delete animal

2. **Response Format:**
```json
{
  "success": true,
  "data": { /* entity data */ },
  "message": "Optional message",
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 150
  }
}
```

3. **Error Format:**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  }
}
```

4. **Pagination:**
   - Query params: `?page=1&limit=20&sort=createdAt:desc`
   - Include metadata in response

5. **Filtering:**
   - Query params: `?status=active&breed=Holstein`
   - Support complex filters when needed

6. **Versioning:**
   - URL versioning: `/api/v1/animals`
   - Or header versioning: `Accept: application/vnd.breeder.v1+json`

### Security Considerations

1. **Authentication:**
   - Use JWT tokens or session-based auth
   - Implement refresh token rotation
   - Secure password storage (bcrypt, argon2)

2. **Authorization:**
   - Role-Based Access Control (RBAC)
   - Roles: Admin, Manager, Veterinarian, Staff, Viewer
   - Fine-grained permissions for sensitive operations

3. **Input Validation:**
   - Validate all user inputs
   - Sanitize data to prevent XSS
   - Use parameterized queries to prevent SQL injection
   - Implement rate limiting

4. **Data Protection:**
   - Encrypt sensitive data at rest
   - Use HTTPS in production
   - Implement audit logging for sensitive operations
   - GDPR/privacy compliance for customer data

5. **API Security:**
   - CORS configuration
   - CSRF protection
   - API rate limiting
   - Request size limits

---

## AI Assistant Guidelines

### When Working on This Codebase

1. **Always Read Before Modifying:**
   - Never propose changes to files you haven't read
   - Understand existing patterns before adding new code
   - Check for similar implementations elsewhere

2. **Follow Existing Patterns:**
   - Match the established code style
   - Use existing utilities and helpers
   - Follow the module/component structure
   - Maintain consistency with naming conventions

3. **Database Operations:**
   - Always use migrations for schema changes
   - Never modify the database directly
   - Include rollback logic in migrations
   - Test migrations thoroughly

4. **Testing Requirements:**
   - Write tests for new features
   - Update tests when modifying existing code
   - Run the test suite before committing
   - Aim for high coverage on business logic

5. **Documentation:**
   - Update API documentation when changing endpoints
   - Add JSDoc/docstrings for complex functions
   - Update README if changing setup process
   - Document architectural decisions in `/docs/architecture`

6. **Error Handling:**
   - Use proper error types/classes
   - Provide meaningful error messages
   - Log errors appropriately
   - Handle edge cases gracefully

7. **Performance Considerations:**
   - Use database indexes for frequently queried fields
   - Implement pagination for large datasets
   - Cache frequently accessed data when appropriate
   - Optimize N+1 queries
   - Use lazy loading for relationships

8. **Avoid Common Pitfalls:**
   - Don't expose sensitive data in API responses
   - Don't commit environment variables or secrets
   - Don't skip input validation
   - Don't ignore TypeScript errors (if using TS)
   - Don't create circular dependencies

### Domain-Specific Considerations

1. **Animal Management:**
   - Handle parentage tracking carefully (genealogy integrity)
   - Validate breeding compatibility (age, species, health)
   - Track animal status transitions properly

2. **Breeding Records:**
   - Calculate gestation periods based on species
   - Validate breeding dates against animal age/status
   - Handle multiple offspring from single breeding

3. **Health Records:**
   - Maintain complete audit trail
   - Track vaccination schedules and due dates
   - Handle veterinarian certifications

4. **Financial Transactions:**
   - Ensure transaction atomicity
   - Maintain accurate inventory costs
   - Track payment status properly

5. **Regulatory Compliance:**
   - Maintain required documentation
   - Generate compliance reports
   - Track certificate expiration dates

### Questions to Ask Before Implementation

1. **Requirements Clarity:**
   - What is the exact business requirement?
   - Who are the users of this feature?
   - What are the success criteria?

2. **Technical Approach:**
   - Does this fit the existing architecture?
   - Are there reusable components/services?
   - What are the performance implications?

3. **Data Model:**
   - How does this affect the database schema?
   - What are the relationships between entities?
   - Do we need migrations?

4. **Security:**
   - Who should have access to this feature?
   - What data needs protection?
   - Are there any audit requirements?

5. **Testing:**
   - What test cases are needed?
   - Are there edge cases to consider?
   - How can we test this thoroughly?

---

## Common Development Tasks

### Adding a New Entity/Module

1. Create database migration
2. Define entity model
3. Create DTOs (Data Transfer Objects)
4. Implement service layer (business logic)
5. Create controller/resolver (API endpoints)
6. Add validation rules
7. Write unit tests
8. Create frontend types
9. Build UI components
10. Add integration tests
11. Update API documentation

### Adding a New Feature

1. Document requirements in issue/ticket
2. Design database changes (if needed)
3. Create feature branch
4. Implement backend API
5. Add backend tests
6. Implement frontend UI
7. Add frontend tests
8. Manual testing
9. Code review
10. Merge to develop
11. Deploy to staging
12. User acceptance testing
13. Deploy to production

### Fixing a Bug

1. Reproduce the bug
2. Write a failing test that exposes the bug
3. Fix the bug
4. Verify the test passes
5. Check for similar issues elsewhere
6. Update documentation if needed
7. Create bugfix branch
8. Submit for review

### Database Migrations

```bash
# Create new migration
npm run migration:create -- AddAnimalHealthRecords

# Run migrations
npm run migration:run

# Rollback last migration
npm run migration:revert

# Generate migration from entity changes (TypeORM)
npm run migration:generate -- AnimalUpdates
```

---

## Deployment

### Environment Variables

Required environment variables (update `.env`):

```bash
# Application
NODE_ENV=development|staging|production
PORT=3000
API_URL=http://localhost:3000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/breeder_farm
DATABASE_POOL_SIZE=10

# Authentication
JWT_SECRET=your-secret-key-here
JWT_EXPIRATION=1h
REFRESH_TOKEN_EXPIRATION=7d

# File Storage
S3_BUCKET=breeder-farm-files
S3_REGION=us-east-1
S3_ACCESS_KEY=xxx
S3_SECRET_KEY=xxx

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=xxx
SMTP_PASSWORD=xxx

# Monitoring
SENTRY_DSN=xxx
LOG_LEVEL=info
```

### Docker Deployment

```bash
# Build images
docker-compose build

# Start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Production Checklist

- [ ] Environment variables configured
- [ ] Database migrations run
- [ ] SSL certificates installed
- [ ] Backups configured
- [ ] Monitoring and alerting set up
- [ ] Error tracking enabled
- [ ] Rate limiting configured
- [ ] Security headers configured
- [ ] CORS properly configured
- [ ] Health check endpoints working
- [ ] Load balancing configured (if needed)
- [ ] CDN configured for static assets
- [ ] Log aggregation set up

---

## Troubleshooting

### Common Issues

**Database Connection Errors:**
- Check DATABASE_URL format
- Verify database server is running
- Check firewall rules
- Verify credentials

**Build Failures:**
- Clear node_modules and reinstall
- Check Node.js version compatibility
- Verify all dependencies are installed
- Check for TypeScript errors

**Test Failures:**
- Ensure test database is set up
- Check test environment variables
- Clear test data between runs
- Verify mock data is up-to-date

**Performance Issues:**
- Check for N+1 queries
- Review database indexes
- Monitor memory usage
- Profile slow endpoints
- Check for memory leaks

---

## Resources & References

### Documentation

- API Documentation: `/docs/api/README.md`
- Architecture Decisions: `/docs/architecture/ADR.md`
- User Guide: `/docs/user-guide/README.md`

### External Resources

- [ERP Best Practices](https://example.com)
- [Database Design Patterns](https://example.com)
- [RESTful API Guidelines](https://example.com)

### Contact & Support

- Technical Lead: TBD
- Project Manager: TBD
- Repository: https://github.com/titanaifitness/breeder-farm-erp

---

## Changelog

### 2026-01-23 - Initial Creation
- Created comprehensive CLAUDE.md for AI assistant guidance
- Established project structure recommendations
- Defined coding conventions and development workflows
- Documented security and domain-specific considerations

---

## Notes for AI Assistants

This is a **greenfield project** - the repository structure and technology stack are not yet established. When beginning development:

1. **Consult with the user** on their technology preferences
2. **Set up the initial project structure** based on the recommendations in this document
3. **Create necessary configuration files** (package.json, tsconfig.json, etc.)
4. **Initialize the database** with migration system
5. **Set up development tools** (linting, testing, CI/CD)

This document will be updated as architectural decisions are made and the project structure evolves. Always keep this file current with the actual state of the codebase.

**Remember:** This is an ERP system handling critical business data. Prioritize:
- Data integrity and consistency
- Security and access control
- Audit trails and compliance
- Performance and scalability
- User experience and usability

Good luck building an excellent breeding farm management system!
