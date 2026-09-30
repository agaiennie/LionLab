Lion Lab
Lion Lab is the field-and-home toolkit for Red Lion Football (built for junior high; works grades 7–12). Good players are smart students. Athletes train and keep a study block. Parents see their own kid. Coaches run the field.
The mark is the words Lion Lab / Red Lion Football in red `#C8102E`. There is no lion-head artwork.
Estimates are coaching tools. They are not medical advice, injury screens, or recruiting scores. The glossary is linked from every section.
Roles
Role	What they see
Public (no sign-in)	`/` program page, public board, FAST, team-wide totals only, `/join` interest form, `/signup`, glossary
Athlete	Own data only: Today, Train, Measure (my numbers + camera self-test), Study, Exams, Fuel, My path (position fit)
Parent	Linked kid only: Today, Home, Progress (numbers, fit, exams), Fuel, Notes, academic updates
Coach	Roster, Test day, Camera, Positions (fit + depth board), Recruit, Exams, Field, Skills, Approvals, Standards
Admin	Everything a coach has + Users, join code, approving coaches
Sign-up is required and approval-gated: families and coaches create an account at `/signup` with the team join code, and a coach approves it (admins approve coaches). Coaches can still create accounts directly. See `README-DEPLOY.md` to go live.
What's in the box (Sep 29, 2026)
Test day (`/coach/measure`): 14-test battery from `lib/content/metrics.ts`: 40, 10-yd burst, 5-10-5, standing long jump, vertical, ball toss, push-ups, plank, sit & reach, ankle knee-to-wall (L/R), deep squat check, single-leg balance (L/R), camera sway, QB throw. Whole-roster entry grid, phone stopwatch with a 10-yd split, personal-best flags, and team flexibility/balance gaps.
Camera (`/camera`): single-leg balance timer + sway, deep-squat score, sit & reach guide. MediaPipe runs on the phone and video never leaves the device. Lift and stance form lives in `/tracker`, which now includes Feed-Bucket Squat and Barn-Floor Hinge.
Positions (`/coach/fit`): weighted fit engine (`lib/position-fit.ts`) across 11 roles. Uses height, weight, test numbers, and QB/MLB exam scores. Team percentiles by grade band, coaching anchors when the pool is small, and a confidence score. Depth board with open and thin spots. Coach-tunable weights and players-needed.
Recruit (`/coach/recruit`): gaps become "look for" profiles in your roster's own numbers. Covers which sports feed each role, girls' teams, what a non-athlete who fits looks like, a 3-station tryout, and the prospect list (public form + coach list). PIAA recruiting guardrails are on the page.
Exams (`/exams`, `/coach/exams`): QB Reads, MLB Fits, and Captain's Standard (59 starter questions). Graded on the server, with review and explanations. Coaches edit and add their own plays.
Fuel (`/fuel`): age-safe fueling guide (no calorie targets, no weight-cutting, no supplements), cheap staples, red flags, a flexibility & balance program picked from each kid's weakest numbers, and a weekly bodyweight plan by position.
Academics first: GOOD / WATCH / HOLD status (coach sets, parent can report), weekly study minutes requirement (WATCH = 1.5×), chips on the roster and in the CSV.
Demo accounts
Every demo account uses the password `LionLab2026!`. One-tap demo buttons show only when `DEMO_MODE=true`. The demo seed also adds 22 fictional junior-high athletes (`jh.*@lionlab.local`) with full test numbers so Positions, Recruit, and team gaps have data.
Role	Name	Email
Coach	Dana Ruiz	`coach@lionlab.local`
Parent	Pat Hale	`parent@lionlab.local`
Athlete	Marcus Hale	`marcus@lionlab.local`
Athlete	Avery Chen	`avery@lionlab.local`
Athlete	Jordan Brooks	`jordan@lionlab.local`
Admin	Sam Okonkwo	`admin@lionlab.local`
Pat Hale is linked to Marcus. Marcus has a study-and-mobility day logged for yesterday, so the streak is 1 and today still shows study not done. Re-running the seed does not reset a password you already changed, and it does not duplicate tests, subjects, or that parent note.
Run locally
Requires Node.js 20+.
```bash
cp .env.example .env
npm install
npm run db:setup
npm run dev -- --port 43123 --hostname 0.0.0.0
```
Open http://127.0.0.1:43123.
`npm run db:setup` creates the SQLite file at `prisma/dev.db` and loads the demo roster.
```bash
npm test          # scoring, study gate, position fit, pose math, exams, academics (52 tests)
npm run fit-report   # print every athlete's top roles + the depth board
npm run lint
npm run build
npm start
```
Navigation
Athlete: Today → Train → Measure → Study → My path. Glossary stays in the bar. Password is in the header.
Parent: Today → Home → Progress → Notes.
Coach: Today → Roster → Field → Skills → Log tests → Tracker → Standards.
Admin: Today → Roster → Users → Field → Tracker → Standards.
Old athlete result links under `/athlete` open Measure. Profile edit stays at `/athlete/profile`.
How to invite a parent
Sign in as a coach or admin.
Open the athlete (Roster → name) or open Users as an admin.
Enter the parent's name, their own email, a temporary password, and the athlete to link. One parent can be linked to more than one athlete from Users.
Share the email and password in person. The app does not send mail.
The parent signs in and sees only the linked athlete. Today shows study done or not done.
If that email is already a parent, leave the password blank and submit again to add another athlete. An email that already belongs to a player or coach is rejected.
Field tablet
Sign in as the coach on the tablet browser.
Open Field (`/field`) for FAST, declared play, the mobility coach card, and measurement how-to. Use the browser's full screen. Landscape fits the FAST card. The sideline line is: First step. Attack. Shoulder level. Through — wrap if you’re tackling, drive if you’re blocking. Through on a tackle is wrap, cheek to the ball, and drive the legs. Through on a block is hands inside, feet moving, drive, and whistle plus one.
Open Tracker (`/tracker`), pick the athlete, and save a checklist or a camera session. Field saves are not blocked by the study gate.
Open Log tests to enter a 40, med ball, or stance. Those logs are not gated either.
Add the site to the home screen if the tablet will live on the sideline.
Camera scoring uses the phone or tablet camera through the browser. It needs `localhost` or `https`. A plain `http` address on the field Wi-Fi usually blocks the camera. If the camera is denied, the checklist still saves a session. Stop for pain, numbness, or a stinger and see the athletic trainer.
Skill library
Coaches and admins open Skills (`/skills`). Search, or filter by phase (fundamentals, position, team) and by position chip. `/skills/positions` lists every position. `/skills/positions/OL` (and the other codes) is the short list: must teach first, then next. Every row opens the same page, `/skills/[skillId]`.
Each guide is Demo, Break down, Speed up, plus any FAST letter, the Lion Lab metric it belongs with, and a 5-minute station card. S is shoulder level. T is Through: the tackle page is the wrap finish, and Through the block is the drive finish for OL, WR, TE, and RB. Square up stays in the library as its own landmark skill. Athletes and parents can read the position lists and the overview from My path or Progress. The step-by-step coach script and the station card stay on the coach view.
On an athlete’s roster page, mark Taught this week. That checkbox is a reminder for the current week. It is not a grade.
The library is version `v1` in `lib/content/skills.ts`. It is not a playbook.
Study gate
Home mobility, extra home training, and a home body-tracker save need today's study block or a parent note. The line on the screen is: Train the body. Train the mind.
Coaches turn the gate on or off, and set the minute goal and the 25/5 timer, under Standards. Field tests a coach logs stay open either way. The roster shows each athlete's study streak, last study date, and whether today is done.
What the battery still means
40: `adjusted = raw × (1 + 0.10 × (1 − form index))`. Labeled ESTIMATE.
Med ball (10 lb): `adjusted = raw × (0.90 + 0.10 × form index)`. Labeled ESTIMATE.
Stance: 80%+ Game-ready, 60–79% Developing, under 60% Rebuild.
Worked examples stay on the glossary page. Formula version `v1`.
Environment variables
Name	Purpose
`DATABASE_URL`	Local default is `file:./dev.db` (SQLite, path relative to `prisma/schema.prisma`). Production should be a Postgres URL.
`AUTH_SECRET`	Signs the session cookie. The example file has a local-only value. Generate a new one before deploying: `openssl rand -base64 32`.
`AUTH_TRUST_HOST`	Set to `true` so Auth.js accepts the host on Vercel or Origin.
Deploy on Vercel or Origin
SQLite in `prisma/dev.db` is for local development. A hosted deploy needs Postgres so data survives new servers. Neon works with Vercel and Origin.
Create a Neon project and copy the direct connection string.
In `prisma/schema.prisma`, change the datasource to:
```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   ```
In the host's environment settings, set:
`DATABASE_URL` to the Neon URL
`AUTH_SECRET` to a new random string (do not reuse the local example)
`AUTH_TRUST_HOST` to `true`
From your machine, against that database:
```bash
   DATABASE_URL="postgres://..." npx prisma db push
   DATABASE_URL="postgres://..." npm run db:seed
   ```
The committed schema starts as SQLite, so switch the provider before `db push` to Postgres. Do not point production at the SQLite file.
Deploy the Next.js app. The build runs `prisma generate` and `next build`.
After deploy, sign in as the demo coach only if you seeded production. For a real roster, change those passwords or skip the seed and create the first admin locally before deploy.
Project notes
Next.js App Router, TypeScript, Tailwind, shadcn/ui
Auth.js credentials (email and password) with a JWT session
Prisma, SQLite locally and Postgres when you deploy
Pose scoring is the MediaPipe prototype under `public/pose`. Checklist mode does not need that network.
