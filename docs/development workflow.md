# IDINEX Development Workflow

## Purpose

This document defines the engineering workflow, Git strategy, review
process and development standards for all contributors.

## Engineering Principles

-   Build features incrementally.
-   Keep the backend stateless.
-   One responsibility per file.
-   Keep SQL inside repositories.
-   Keep business logic inside services.
-   One Pull Request equals one feature.
-   Prefer readable code over clever code.

## Team Roles

### Founder / Technical Lead

-   Product vision
-   Architecture
-   Sprint planning
-   Final PR approval
-   Releases

### Backend Developer

-   Go API
-   Database
-   Authentication
-   Tests

### Frontend Developer

-   UI
-   API integration
-   Responsive design
-   Client-side validation

Everyone participates in code reviews.

## Git Workflow

``` text
main
│
develop
│
├── feature/authentication
├── feature/create-idea
├── feature/messages
└── feature/profile
```

Never commit directly to `main` or `develop`.

## Branch Naming

``` text
feature/login
bugfix/auth
docs/api
refactor/users
test/auth
```

## Daily Workflow

1.  Pull latest changes from develop.
2.  Create a feature branch.
3.  Implement one feature.
4.  Test locally.
5.  Commit with meaningful messages.
6.  Push branch.
7.  Open Pull Request.
8.  Request review.
9.  Merge after approval.

## Commit Examples

``` text
feat(auth): implement login
feat(ideas): create publish endpoint
fix(users): validate email uniqueness
docs(api): update auth documentation
test(messages): add repository tests
```

## Pull Request Checklist

-   Clear description
-   Reason for change
-   Testing completed
-   Screenshots for UI
-   Documentation updated

## Code Review Checklist

-   Architecture respected
-   Clean code
-   Error handling
-   Security
-   Naming consistency
-   Tests included
-   Documentation updated

Never review the developer. Review the code.

## Definition of Done

A task is complete when:

-   Feature works
-   Backend complete
-   Frontend complete
-   Tests passing
-   Documentation updated
-   Code reviewed
-   Merged into develop

## Coding Standards

### Backend

-   Stateless
-   DTOs for API
-   Services contain business logic
-   Repositories contain SQL
-   Handlers remain thin

### Frontend

-   Reusable components
-   Feature-based JavaScript
-   Responsive layouts
-   Consistent styling

## Documentation

Every completed feature should update:

-   README (when required)
-   API documentation
-   Architecture documentation
-   Database documentation

## Long-Term Goal

Build a clean modular codebase that can evolve into a production-grade
platform without a complete rewrite.
