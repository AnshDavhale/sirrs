# SIRRS — Smart Incident Reporting & Response System

**Short:** A MERN-stack web app for citizens to report civil incidents (text, photo, location), and for authorities to track and resolve them.

## Features (MVP)
- User signup/login (roles: citizen, authority, admin)
- Report incidents with text, images, and geolocation
- Authority dashboard to view/filter incidents and update status
- Notifications on status changes
- Basic AI-assisted categorization (rule-based)

## Tech Stack
- **Backend:** Node.js, Express, MongoDB, Mongoose
- **Frontend:** React (Vite)
- **Other:** JWT for auth, Multer for image uploads, Nodemailer for emails

## Local setup
1. Clone the repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/sirrs.git
   cd sirrs
