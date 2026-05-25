# Czechibank API Tests

Automated API testing pipeline for [Czechibank](https://da.czechibank.ostrava.digital/api/v1/) — a learning application for the Czechitas Digital Academy.

## What it does

1. **Registers a new user** via `POST /user/create` with a unique email (`manka_34+{timestamp}@seznam.cz`)
2. **Runs automated tests** to verify:
   - Status code is `201 Created`
   - Response contains `success: true`
   - Email in response matches the sent email (lowercase check)
   - Response contains a user ID and API key
3. **Verifies the user in the database** using a PostgreSQL query with `LOWER()` email matching

## Tech stack

- **Bruno** — API client & collection runner
- **Bruno CLI** — headless collection execution in CI
- **GitHub Actions** — CI/CD pipeline (scheduled + manual trigger)
- **Node.js + pg** — database verification
- **PostgreSQL** — Czechibank database

## Pipeline schedule

| Trigger | When |
|---------|------|
| Automatic | Every day at 7:00 CEST (`cron: '0 5 * * *'`) |
| Manual | Via GitHub Actions → Run workflow |

## Project structure

```
├── .github/workflows/
│   └── api-test.yml          # GitHub Actions workflow
├── environments/
│   └── Czechibank_Register.yml  # Bruno environment (baseUrl)
├── register_user.yml         # Bruno request with pre-script, tests & post-response
├── opencollection.yml        # Bruno collection config
└── .brunoignore              # Excludes .github from Bruno CLI
```

## Setup

1. Clone the repo
2. Add `DATABASE_URL` as a repository secret (Settings → Secrets → Actions)
3. Run the workflow manually or wait for the scheduled run

## Author

Magdalena Roubalová — [Czechitas Digital Academy](https://www.czechitas.cz/) Testing Track 2026
