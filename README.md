# Attendance IQ — Supabase + GitHub Pages

React workforce dashboard. GitHub stores website source; Supabase provides central persistence, authentication and row-level access controls.

## Features
- Auth login with admin, HR and read-only member roles.
- Employee master with the exact supplied 24 columns and ECN ID as unique key.
- Date + ECN ID attendance records; biometric/manual mismatch list; P1/P2/P3 shift counts.
- Department, agency, designation and date filters; dark mode.
- XLSX/CSV import/export and downloadable templates.
- RLS: team members read, HR/admin import and edit, admin alone deletes and manages roles.

## Setup
1. Supabase → SQL Editor: run supabase/schema.sql.
2. Supabase → Authentication → Users: create an initial user and copy their UUID.
3. In SQL Editor, grant that user admin: insert into public.user_roles(user_id, role) values ('YOUR-USER-UUID'::uuid, 'admin');
4. Supabase → Project Settings → API: copy Project URL and publishable/anon key. Never use the service_role key in the browser.
5. Locally copy .env.example to .env.local, fill VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY, then run npm install and npm run dev.
6. GitHub repository Settings → Secrets and variables → Actions: add variable VITE_SUPABASE_URL and secret VITE_SUPABASE_ANON_KEY.
7. GitHub repository Settings → Pages: choose GitHub Actions as build/deploy source. Push the finished branch to main to trigger deployment. This project is configured for the root URL of GitHub Pages.
8. Create team accounts in Supabase Auth. For each user, insert their UUID and role ('hr' or 'member') into public.user_roles. Members are read-only. HR can import/edit; only admins can delete.

## Import formats
Employee master headers in source order: Sr No, taging, ECN ID, Name, Mobile, Allocation ( Local or Migrant ), Department, Sub Dep, Designation, Agency, Weekly OFF, Active, Gender, LOCATION, TRAVEL SOURCE, Shift Time, SHIFT, Birth Date, Aging, Today, DOJ, DOJ AS PER Mars, DOL, Reason For Leaving.
Attendance headers: Date, ECN ID, Biometric, Manual, Shift. Date values should be parseable dates (YYYY-MM-DD recommended). Duplicate ECN IDs update the master row; duplicate attendance date + ECN updates that day.

## Security
- Keep RLS enabled. Do not commit employee spreadsheets or personal data to this repository.
- Use a private repository if company policy requires it.
- Verify retention and access policies before production use.
- The Supabase anon/publishable key is intended for browser use only with correct RLS. Never expose the service_role key.
