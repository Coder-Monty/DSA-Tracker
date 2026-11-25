# DSA Tracker

A responsive daily DSA progress tracker to help you stay consistent with your problem-solving routine, manage daily study tasks, and review your progress over time.

🌐 **Live website:** [https://dsa-tracker-75ab8.web.app/](https://dsa-tracker-75ab8.web.app/)

## Features

- **Responsive interface** — designed for desktop and mobile screens.
- **Light and dark themes** — switch themes and keep your preferred appearance.
- **Live date and clock** — see the current date and time on the dashboard.
- **Target countdown** — set a target date and track the remaining time.
- **Daily DSA question panel** — keep a daily LeetCode question in view.
- **Daily tasks** — add tasks and track each task's completion.
- **Monthly checkpoint calendar** — review daily progress from the calendar.
- **Date-wise history** — each day has its own task-completion records.
- **Automatic completion status** — a day's checkpoint is completed when all active tasks for that day are checked.
- **Streak tracking** — completed checkpoints contribute to current and best streaks.
- **Guest mode** — use the tracker without signing in; guest data is saved in the current browser.
- **Account login and cloud sync** — sign in with email and password to sync your tasks, checkpoints, theme, countdown, and saved DSA picks across devices.
- **Guest-data migration** — on first login, locally saved guest data is merged into the account rather than being intentionally discarded.

## How the tracker works

- Daily tasks begin appearing on the date they are created.
- Each calendar date keeps its own completion state.
- Past dates can be opened to review progress, but their task checkboxes are read-only.
- Future dates are disabled in the calendar.
- Removing a task archives it from the current date onward; earlier checkpoint history is retained.
- A day counts as completed only when it has at least one active task and every active task is marked complete.
- Today's checkpoint is treated as in progress until all of its active tasks are completed.

## Tech stack

- **HTML5** — page structure
- **CSS3** — responsive layout, themes, and styling
- **Vanilla JavaScript** — application logic and local data handling
- **Firebase Authentication** — email/password sign-in
- **Cloud Firestore** — account data storage and cross-device synchronization
- **Browser localStorage** — guest-mode persistence and local account cache
- **Firebase Hosting** — live deployment

The application is a static web app and does not require a frontend build step.

## Run locally

You can run the app locally using any static HTTP server.

### Option 1: Python

1. Download or clone this repository.
2. Open a terminal in the project folder.
3. Run:

   ```bash
   python -m http.server 8000
   ```

4. Open [http://localhost:8000](http://localhost:8000) in your browser.

### Option 2: VS Code

Use the **Live Server** extension and open the project through its local server URL.

> Firebase Authentication works on `localhost` or an authorized HTTPS domain. Opening the HTML directly using a `file://` URL may prevent authentication from working correctly.

## Firebase setup

The hosted version uses Firebase Authentication and Cloud Firestore for account login and cross-device sync.

To configure your own Firebase project:

1. Create a project in the [Firebase Console](https://console.firebase.google.com/).
2. Register a Web App in the project.
3. Enable **Email/Password** under **Authentication → Sign-in method**.
4. Create a **Cloud Firestore** database.
5. Add your local development domain and deployed website domain under **Authentication → Settings → Authorized domains**, if they are not already listed.
6. Add your Firebase web app configuration to the application code.
7. Publish the Firestore security rules below.

### Firestore security rules

These rules allow an authenticated user to read and write only their own document under `users/{userId}`:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null
                         && request.auth.uid == userId;
    }
  }
}
```

Do not put Firebase Admin SDK credentials or service-account private keys in frontend code. The Firebase web configuration is intended for client-side use; access control must be enforced through Authentication and Firestore Security Rules.

## Data and privacy notes

- Guest data is stored in the browser's localStorage and remains available after normal reloads and browser restarts.
- Guest data is browser/device-specific. Clearing browser site data can remove access to that local guest copy.
- Signing in allows the account's data to sync through Firestore across devices.
- Use the same account on each device to access the same cloud-synced tracker data.
- Firestore rules should be published before enabling account-based use.

## Deployment

The live version is hosted on Firebase Hosting.

If you have Firebase CLI installed and have configured the correct Firebase project for this repository, deploy the static site with:

```bash
firebase deploy --only hosting
```

For a first-time Firebase CLI setup, see the [Firebase Hosting documentation](https://firebase.google.com/docs/hosting).

## Live demo

Visit **[DSA Tracker](https://dsa-tracker-75ab8.web.app/)** to try the application.

---

Made to make daily DSA practice more consistent.
