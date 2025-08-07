# Bug Fixes Documentation – Scheduling App (v1.3.0)
🛠️## Overview
This document outlines the major bugs that were discovered and resolved in the
Scheduling App
---
## Critical Fixes Implemented
### 1. Timezone Display Errors in Appointments
**File**: `src/utils/dateFormatter.ts`
**Severity**: Critical
**Status**: Fixed
#### Problem
Appointment times displayed inconsistently across users in different timezones. A
booking at 4:00 PM EST appeared as 1:00 PM PST, leading to:
- Missed meetings
- Confusion in customer support
- Frustration and loss of trust
#### Root Cause
Timestamps were rendered using `new Date().toLocaleString()` without setting a
consistent server-side timezone.
#### Fix
Replaced local formatting with a UTC-standardized formatter using `date-fns-tz`:
```typescript
format(utcToZonedTime(appointmentTimeUTC, userTimeZone), 'hh:mm a zzz')
```
✅
✅
✅
#### Impact
-
Accurate appointment display in all user timezones
-
Fewer missed appointments
-
Time consistency across platforms
---
### 2. Duplicate Bookings on Retry
**File**: `src/hooks/useBookAppointment.ts`
**Severity**: High
**Status**:
Fixed
✅#### Problem
Users experiencing network issues and retrying caused **duplicate bookings**, which
cluttered the database and overbooked slots.
#### Root Cause
No idempotency token was implemented to recognize retries of the same booking.
#### Fix
Introduced a unique `x-request-id` in each booking attempt and deduplicated on the
server:
```typescript
// Frontend
axios.post('/api/book', payload, {
headers: { 'x-request-id': uuidv4() }
});
```
```ts
// Backend
if (hasAlreadyProcessed(requestId)) return;
```
# Welcome to your Lovable project

## Project info

**URL**: https://lovable.dev/projects/94b52f1d-10a5-4e88-9a9c-5c12cf45d83a

## How can I edit this code?

There are several ways of editing your application.

**Use Lovable**

Simply visit the [Lovable Project](https://lovable.dev/projects/94b52f1d-10a5-4e88-9a9c-5c12cf45d83a) and start prompting.

Changes made via Lovable will be committed automatically to this repo.

**Use your preferred IDE**

If you want to work locally using your own IDE, you can clone this repo and push changes. Pushed changes will also be reflected in Lovable.

The only requirement is having Node.js & npm installed - [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating)

Follow these steps:

```sh
# Step 1: Clone the repository using the project's Git URL.
git clone <YOUR_GIT_URL>

# Step 2: Navigate to the project directory.
cd <YOUR_PROJECT_NAME>

# Step 3: Install the necessary dependencies.
npm i

# Step 4: Start the development server with auto-reloading and an instant preview.
npm run dev
```

**Edit a file directly in GitHub**

- Navigate to the desired file(s).
- Click the "Edit" button (pencil icon) at the top right of the file view.
- Make your changes and commit the changes.

**Use GitHub Codespaces**

- Navigate to the main page of your repository.
- Click on the "Code" button (green button) near the top right.
- Select the "Codespaces" tab.
- Click on "New codespace" to launch a new Codespace environment.
- Edit files directly within the Codespace and commit and push your changes once you're done.

## What technologies are used for this project?

This project is built with:

- Vite
- TypeScript
- React
- shadcn-ui
- Tailwind CSS

## How can I deploy this project?

Simply open [Lovable](https://lovable.dev/projects/94b52f1d-10a5-4e88-9a9c-5c12cf45d83a) and click on Share -> Publish.

## Can I connect a custom domain to my Lovable project?

Yes, you can!

To connect a domain, navigate to Project > Settings > Domains and click Connect Domain.

Read more here: [Setting up a custom domain](https://docs.lovable.dev/tips-tricks/custom-domain#step-by-step-guide)
