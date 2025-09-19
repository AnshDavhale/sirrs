# SIRRS — System Design & Architecture

## Overview
SIRRS is a MERN web application:
- Frontend: React (Vite)
- Backend: Node.js + Express
- Database: MongoDB (Atlas for production)
- File storage: Dev: local `uploads/`; Prod: Cloudinary or AWS S3
- Real-time (optional): Socket.IO
- Email: Nodemailer (SMTP) or transactional provider (SendGrid)

## High-level components
- **Client (React)**: Login/Signup, ReportForm, MyReports, Authority Dashboard, IncidentDetail.
- **API Server (Express)**: Auth, Incident CRUD, File upload, Notifications, AI categorizer endpoint.
- **Database (MongoDB)**: Stores users, incidents, notifications, comments, audit logs.
- **File Storage**: images for incident/report/resolution.
- **Email Service**: Send status-change emails.
- **Optional**: Socket server for realtime dashboard updates.

## Component diagram (see `docs/diagrams/component-diagram.png`)

## Data model (ER) summary
- **User**
  - `_id`: ObjectId
  - `name`: string
  - `email`: string (unique)
  - `passwordHash`: string
  - `role`: enum ['citizen','authority','admin']
  - `createdAt`, `updatedAt`

- **Incident**
  - `_id`
  - `title`
  - `description`
  - `category` (string)
  - `reporterId` -> User._id
  - `photos`: [string] (URLs)
  - `location`: { type: "Point", coordinates: [lng, lat] }
  - `status`: enum ['Pending','Acknowledged','In Progress','Resolved','Rejected']
  - `deadline`: Date
  - `resolvedAt`: Date
  - `timeline`: [{action, byUserId, note, date}]
  - `createdAt`, `updatedAt`

- **Notification**
  - `_id`, `userId`, `incidentId`, `type`, `message`, `isRead`, `createdAt`

- **Comment / Audit** (optional)
  - `_id`, `incidentId`, `userId`, `text`, `createdAt`

(ER diagram PNG: `docs/diagrams/er-diagram.png`)

## API Endpoints (summary)
See `docs/api_spec.md` for full request/response examples.

Important endpoints:
- `POST /api/auth/signup` — create user
- `POST /api/auth/login` — return JWT
- `GET /api/users/me` — get profile
- `POST /api/incidents` — create incident (multipart for images)
- `GET /api/incidents` — list with filters (`?status=&category=&near=lat,lng&radius=km`)
- `GET /api/incidents/:id`
- `PATCH /api/incidents/:id/status` — authority only
- `POST /api/incidents/:id/photos`
- `GET /api/users/:id/incidents` — user's incidents
- `GET /api/admin/stats` — counts by status/category

## Sequence diagram (see `docs/diagrams/sequence-report-create.png`)
Flow for report creation:
1. Client sends `POST /api/incidents` (+ images, lat/lng)
2. Server authenticates JWT
3. Server persists images to storage and saves Incident in DB
4. Server triggers AI categorizer (sync or async) to set `category`
5. Server sends confirmation email to reporter and creates an in-app Notification
6. Authority dashboard receives update (poll or via Socket.IO)

## Security considerations
- Passwords hashed with bcrypt.
- JWT with reasonable expiry; refresh tokens optional for long sessions.
- Validate and sanitize all inputs (express-validator).
- Limit file upload size and file types (images only).
- CORS configured to allow frontend origins only.
- Rate-limiting on auth endpoints.
- Store sensitive env vars (DB URI, JWT secret, SMTP creds) in `.env` (not committed).

## Deployment plan (dev → prod)
- Dev: run frontend locally (`npm run dev`) and backend with `nodemon`.
- Prod:
  - Frontend: Vercel / Netlify (build output).
  - Backend: Render / Heroku / DigitalOcean App (Node 18/20).
  - DB: MongoDB Atlas.
  - File storage: Cloudinary or S3 (use environment variables).
  - Env variables to set: `MONGO_URI`, `JWT_SECRET`, `SMTP_HOST`, `SMTP_USER`, `SMTP_PASS`, `CLOUDINARY_URL` (if used).
- Health checks and logging: use simple console logs / morgan; in production use structured logs.

## Scaling notes (brief)
- Read-heavy: use indexes on `createdAt`, `status`, and `location` (2dsphere).
- Use pagination & limit for incident lists.
- Move image storage to CDN-backed storage (S3 + Cloudfront) for performance.

## Acceptance criteria for Week 1
- `docs/architecture.md` added to repo
- ER diagram PNG in `docs/diagrams/`
- Component diagram PNG in `docs/diagrams/`
- `docs/api_spec.md` stub exists listing endpoints & request/response examples
- Clear deployment plan and env var list

