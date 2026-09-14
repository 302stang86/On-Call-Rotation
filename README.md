[README.md](https://github.com/user-attachments/files/32178718/README.md)
# FDEC Time Off

Static single-page tool for the Fire Defense Equipment Company (FDEC) sprinkler
department. Techs request vacation, personal, and sick time and watch their own
balances. The manager approves, tracks coverage, and sets allotments.

Hosted on GitHub Pages. Sign-in and data live in Firebase.

## Files

- `index.html` — the whole app, no build step
- `firestore.rules` — security rules to publish in the Firebase console

## Set it up

### 1. Create the Firebase project

1. Go to console.firebase.google.com and add a project, for example `fdec-timeoff`.
2. Build → Authentication → Get started → enable Email/Password.
3. Build → Firestore Database → Create database → production mode.
4. Firestore → Rules → paste `firestore.rules` → Publish.

### 2. Wire up the web app

1. Project settings → General → Your apps → Web (the `</>` icon) → register the app.
2. Copy the `firebaseConfig` values.
3. Open `index.html`, find the `firebaseConfig` block near the bottom, paste the values over the `PASTE_...` placeholders.

### 3. Lock the domain

Authentication → Settings → Authorized domains → add your Pages host,
for example `yourname.github.io`. Remove any domain you do not use.

### 4. Publish

1. Push `index.html` to the repo.
2. Repo → Settings → Pages → deploy from branch `main`, folder `/ (root)`.
3. The app loads at `https://yourname.github.io/<repo>/`.

### 5. Make yourself the manager

1. Authentication → Users → Add user → your work email and a password.
2. Sign in to the app once. That creates your profile with the tech role.
3. Firestore → `users` → your document → change `role` to `manager`.
4. Reload. The Approvals, Coverage, and Roster tabs appear.

### 6. Add the techs

1. Authentication → Users → Add user for each tech.
2. Send each one the link and their temporary password.
3. After each tech signs in once, they land on the Roster tab.
4. Enter granted hours and carryover hours per tech, then save.

## How the numbers work

- One (1) day equals eight (8) hours. Change `HOURS_PER_DAY` in `index.html` to adjust.
- Remaining equals granted plus carryover, minus approved, minus pending.
- Pending hours stay reserved until you approve, deny, or the tech cancels.
- Weekends default to zero (0) hours inside a date range. A tech can override any day.
- Unpaid time draws no balance but still shows on the coverage calendar.

## Notes

- The Firebase web key is public by design. The rules decide who reads and writes.
- Techs cannot grant themselves hours, change their role, or see another tech's requests.
- The coverage calendar flags any day with two (2) or more techs off. Change
  `COVERAGE_WARN_AT` in `index.html` to adjust that threshold.
- Export approved time to CSV from the Approvals tab for payroll.
