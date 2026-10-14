# Architecture

## Request flow

1. React/Vite calls the FastAPI REST API.
2. FastAPI validates requests with Pydantic schemas.
3. SQLAlchemy reads/writes framework outcomes, assessments, results, and tasks.
4. Assessment summaries are calculated from stored outcome statuses and risk ratings.
5. The UI renders coverage, open gaps, priority actions, and remediation tasks.

## Core entities

- `FrameworkOutcome`: versioned framework reference item.
- `Assessment`: named review scoped to an organization.
- `AssessmentResult`: one status and evidence note per assessment/outcome pair.
- `RemediationTask`: action linked to an assessment and outcome.

## Scoring notes

Coverage is `implemented / assessed applicable × 100`, where not-assessed and not-applicable results are excluded. Risk triage uses likelihood × impact, each 1–5. These are application-defined indicators and must be reviewed before operational use.

## Production design recommendations

- Add identity provider integration and role-based access control.
- Enforce tenant boundaries at every API endpoint.
- Use PostgreSQL migrations (Alembic) and managed backups.
- Store evidence files in private object storage; scan uploads and log checksums.
- Version framework datasets and assessments.
- Separate audit events from editable business records.
- Add rate limiting, security headers, monitoring, secrets management, and disaster recovery.
