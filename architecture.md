KAWUO Connect — Architecture

Overview

KAWUO Connect is designed as an offline-first mobile client with a secure backend and an admin dashboard. The architecture prioritizes privacy, data minimization, secure storage, and the ability to operate in low-connectivity environments.

Components

- Mobile App (Flutter)
  - Encrypted local SQLite for draft reports and queued media
  - Sync engine with client-generated UUIDs for idempotency
  - Media compression & EXIF stripping
  - Accessibility features (audio guidance, large targets)

- API Gateway & Backend (Django + DRF)
  - Authentication (OTP + JWT + refresh tokens)
  - Sync endpoints (/sync/reports) for batched idempotent uploads
  - Signed upload initiation for attachments
  - Role-based access control and category-level sensitivity flags

- Database (PostgreSQL)
  - Normalized schema for users, reports, cases, attachments, audit logs
  - Column-level encryption for sensitive fields if required

- Object Storage (S3-compatible)
  - Encrypted buckets, signed upload/download URLs, lifecycle policies

- Workers (Celery)
  - Media processing (thumbnailing, compression), virus scanning, exports

- Notifications
  - Push via FCM (mobile)
  - SMS via gateway (AfricasTalking/Twilio) for fallback
  - Templates enforced to avoid sensitive content

Security & Privacy

- TLS for all endpoints
- Strong input validation & rate limiting
- Passwords hashed with bcrypt/Argon2
- Access control per role and per-case sensitivity
- Audit logging for all important actions
- Local data encryption on mobile and automatic purge policies

Offline Sync Strategy

- Client stores reports and attachments locally with client_id UUIDs.
- On reconnect, client POSTs to /sync/reports with batched reports and attachment metadata.
- Server returns mapping of client_id -> server_id and signed upload URLs for attachments.
- Client uploads attachments directly to S3 using signed URLs and confirms completion.
- Server processes attachments asynchronously, associates them with reports, and returns final status.
- Client polls /sync/updates?since=TOKEN to receive case updates, messages, and configuration changes.

Idempotency & Duplicate Prevention

- Reports use client-generated UUIDs; server de-duplicates by client_id and checksum.
- Attachments include checksums and client_attachment_id, allowing server-side dedup and resumption.

Data Retention & Lifecycle

- Media retention policy configurable by admins (e.g., retain for X years)
- Automated purging for old/unauthorized attachments

Monitoring & Observability

- Centralized logs (ELK/Loki)
- Metrics with Prometheus and Grafana
- Alerts for failed sync rates, worker queue growth, error spikes

Deployment Recommendations

- Containerized services (Docker)
- Managed DB (Postgres) with daily backups
- Use a secrets manager (cloud KMS or Vault)
- Multi-AZ deployment for availability

