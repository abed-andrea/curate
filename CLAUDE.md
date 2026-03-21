# Curate

## What it is
A distraction-free web app where users save content from social media 
(screenshots or links), organize it into personal categories, and track 
what they've acted on. Solves the problem of saving content inside the 
same apps designed to keep you scrolling.

## Current focus
Backend only. Build it API-first so it works seamlessly with any 
frontend — Next.js web app first, React Native mobile app later.

## Stack
- Python 3.11, FastAPI, SQLAlchemy, Pydantic v2
- PostgreSQL (Docker locally, Railway in production)
- JWT authentication (PyJWT, passlib, bcrypt)
- uvicorn
- pytest for testing
- GitHub Actions for CI
- Docker for local DB and backend containerization
- Railway for backend deployment, Vercel for frontend later

## Data model
Three tables:

Users: id, email, hashed_password, created_at
Categories: id, name, user_id (FK → Users)
Items: id, title, link, image_path, is_done, created_at, category_id (FK → Categories), user_id (FK → Users)

Relationships:
- User has many Categories (one to many)
- User has many Items (one to many)
- Category has many Items (one to many)
- user_id on Items is intentional — fast security checks without joining through Category
- Deleting a Category cascades and deletes all its Items

## API endpoints
Auth: POST /auth/register, POST /auth/login
Categories: GET /categories, POST /categories, DELETE /categories/{id}
Items: GET /items, GET /items/{id}, GET /items?category_id=, POST /items, PATCH /items/{id}, DELETE /items/{id}

## Folder structure
backend/app/core/ — config, database, security
backend/app/models/ — SQLAlchemy models
backend/app/schemas/ — Pydantic schemas
backend/app/api/v1/ — route handlers (thin, no business logic)
backend/app/services/ — business logic (tested heavily)
backend/tests/ — mirrors services, one test file per service

## Engineering practices
- API-first design — endpoints are frontend agnostic (web or mobile)
- TDD — write failing test first, then implementation, then verify
- One small change at a time — verify before moving on
- Never commit broken code
- Services contain all business logic — routes just call services
- Tests run against a separate test database
- Environment variables for all secrets — never hardcoded
- Consistent error responses: { detail: "message" }
- All protected endpoints require JWT Bearer token
- CORS configured for frontend domains

## Current phase
Setting up project foundation — folder structure, Docker, FastAPI 
skeleton, database connection, GitHub Actions CI.