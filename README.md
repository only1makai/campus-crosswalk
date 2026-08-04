# Campus Crosswalk

Cross-campus student networking for the UC system — find your people and your classes across all 9 UC schools.

## The problem

UC students take equivalent courses at sister campuses constantly (summer sessions, transfers, cross-enrollment) with no easy way to know which classes actually match up, or to find other students in them. Campus Crosswalk solves both: AI-powered course equivalency matching plus peer discovery, gated behind verified .edu identity.

## Features

- Consent-based registration with verified-.edu email gating
- Identity split: a verification-only .edu email vs. a personal contact email, so students aren't forced to expose their real inbox
- CSRF-protected authentication with hashed passwords
- Peer discovery with granular visibility filtering
- AI-driven course equivalency matching across UC campuses

## Tech stack

- **Backend:** Flask, SQLAlchemy, SQLite
- **Auth / Security:** CSRF protection, password hashing, custom email verification flow
- **Testing:** pytest, committed test suite
- **Dev tooling:** `seed.py` for local seed data, `.env.example` for config

## Architecture

The app follows a layered structure rather than a flat Flask app:

```
routes/       ->  HTTP layer, request/response handling only
services/     ->  business logic, orchestrates repository calls
repository/   ->  data access layer, all queries live here
models.py     ->  SQLAlchemy models
```

Routes never touch the database directly — they call services, which call the repository layer. This keeps business logic testable independent of Flask, and keeps the data layer swappable if the storage backend ever changes.

## Security decisions

- CSRF protection on all state-changing routes
- Password hashing (never stored in plaintext)
- Verified-email gating — accounts can't act on the platform until .edu ownership is confirmed
- Split identity model — verification email and contact email are stored separately
- Visibility filtering on peer discovery

## Local setup

```bash
git clone https://github.com/only1makai/campus-crosswalk.git
cd campus-crosswalk
pip install -r requirements.txt
cp .env.example .env
python seed.py
python app.py
```

## Running tests

```bash
pytest
```

## Roadmap

- Expand course equivalency matching to more UC campuses
- GitHub Actions CI running the test suite on every push
- Public deployment
