# KAWUO Connect

"Your Voice. Your Safety. Your Community."

KAWUO Connect is a secure, privacy-first mobile + web case reporting and management system for Karamoja Women Umbrella Organisation (KAWUO). It enables women and girls to safely report incidents, request support, and track case status, and enables KAWUO staff to receive, triage, manage, refer, and report on cases.

This repository contains the project architecture and initial specifications for the MVP.

Key components

- Mobile app: Flutter (offline-first, encrypted local storage, sync engine)
- Backend API: Django + Django REST Framework (authentication, case workflows, audit logs)
- Database: PostgreSQL
- Storage: S3-compatible object storage for attachments (encrypted)
- Admin dashboard: React or Django admin (role-based access control)

Initial artifacts in this repo

- openapi.yaml — initial API contract for MVP
- plantuml/use-case.puml — use-case diagram (PlantUML)
- plantuml/erd.puml — ERD (PlantUML)
- architecture.md — detailed architecture & sync strategy

Getting started

1. Review the OpenAPI spec in openapi.yaml to align backend and mobile teams.
2. Review architecture.md for deployment and security considerations.
3. Use the PlantUML files to render diagrams during design reviews.

Next steps

- Approve architecture/docs
- Generate OpenAPI client & server stubs
- Start Phase 1 (API + Data models)

Contact

Project lead: lomerfp
