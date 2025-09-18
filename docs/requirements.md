# SIRRS — Requirements & Scope

## 1. Project summary
**Project name:** SIRRS (Smart Incident Reporting & Response System)  
**Short description:** Web app for citizens to report civil incidents (text/photo/location). Authorities view incidents on a dashboard, update status, and upload resolution evidence. The system includes basic AI-assisted categorization.

## 2. Stakeholders
- Citizens / Reporters
- Municipal Authorities (inspectors, field workers)
- Admin (system manager)
- Instructor / Evaluator (course)

## 3. Goals & Problem Statement
Many civic problems (potholes, waste, lighting) are reported inefficiently. SIRRS aims to provide a structured report → acknowledgement → resolution workflow, with geolocation and images, making issue tracking transparent.

## 4. Scope
**In scope (MVP):**
- User signup/login (roles: citizen, authority, admin)
- Create incident: title, description, photos, geolocation, optional deadline
- Authorities: live dashboard (list + map), update status, upload resolution images
- In-app & email notifications for status changes
- Rule-based AI to suggest category

**Out of scope (for MVP):**
- Mobile native app (PWA optional)
- Full ML image classification
- Complex assignment/worker scheduling
- Multi-language support (stretch)

## 5. Functional Requirements (high-level)
1. FR-01: Users can create an account and login.
2. FR-02: Authenticated user can submit an incident with text, photos, and location.
3. FR-03: System stores incidents with timestamps and reporter info.
4. FR-04: Authorities can view incidents, filter by status/category/date, and update status.
5. FR-05: On status change, reporter receives an in-app notification and email.
6. FR-06: Authorities can upload resolution photos after marking resolved.
7. FR-07: System suggests a category from report text via a rule-based classifier.

## 6. Non-Functional Requirements
- NFR-01: System must be accessible via standard browsers (Chrome/Firefox).
- NFR-02: Image uploads limited to 5MB per image.
- NFR-03: Authentication via JWT; password stored hashed (bcrypt).
- NFR-04: Response time for main endpoints < 1.5s under normal load (dev target).
- NFR-05: Data retention: store images & incidents until admin deletes them.

## 7. User Stories (examples)
- As a **Citizen**, I want to submit an incident with a photo and my location so that authorities can find and fix it.
  - Acceptance criteria: Create incident form accepts text, at least one photo or location; POST returns 201 with incident id; incident shows in My Reports.
- As an **Authority**, I want to filter incidents by status so I can prioritize pending tasks.
  - Acceptance criteria: Filter UI returns matching incidents and the backend supports `?status=` query.

## 8. Use case / flow (brief)
1. Citizen logs in → opens Report Form → attaches photo + location → submits.
2. Backend stores incident → sends confirmation email / notification.
3. Authority dashboard receives incident (live or via refresh) → authority updates status → reporter notified.

## 9. Data entities (core)
- User { id, name, email, role, passwordHash }
- Incident { id, title, description, category, photos[], location{lat,lng}, status, reporterId, deadline, createdAt, updatedAt }
- Notification { id, userId, incidentId, type, message, read }

## 10. Constraints & Risks
- Constraint: Limited development hours (3–4 days/week × 2–3 hrs/day).
- Risk: Image storage and email config cause delays — mitigation: initially use local storage and nodemailer dev SMTP.
- Risk: AI classification may be inaccurate — mitigation: mark as suggestion and allow manual override.

## 11. Acceptance criteria for this phase (what will be shown)
- A `requirements.md` file in repo with the contents above.
- 4–5 low-fidelity wireframes covering login, report form, my reports, authority dashboard, and incident detail (images or markdown).
- Gantt chart / project plan showing timeline and tasks.
- Short demo script of how you will show the MVP working.

## 12. Next steps
- Week1: scaffold backend and JWT auth
- Week2: implement report creation & image upload
- Week3: dashboard + notifications
- Week4: AI rule-based classifier + deploy

