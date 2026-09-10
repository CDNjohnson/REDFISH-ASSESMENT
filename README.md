# Redfish / AeroBarrier — Type Preferences Index

A login-free assessment page for candidates (`index.html`) and a login-gated
dashboard for you to view every submission (`admin.html`). No build step —
plain HTML/CSS/JS, using Firebase for storage and admin login.

## Files

- `index.html` — the 60-question assessment candidates take. Saves each
  completed submission to a Firestore collection called `submissions`.
- `admin.html` — sign in with an admin email/password to see every
  candidate's name, role, result type, and a full breakdown; export to CSV.
- `firebase-config.js` — your Firebase project's connection details (not a
  secret — safe to be public; access is controlled by `firestore.rules`).
- `firestore.rules` — security rules: anyone can submit an assessment,
  only a signed-in admin can read/list/export results.

## One-time setup (about 10 minutes)

### 1. Create a Firebase project
1. Go to https://console.firebase.google.com and click **Add project**.
2. Name it (e.g. `redfish-tpi`), disable Google Analytics if you don't need
   it, and click **Create project**.

### 2. Register a web app and get your config
1. In the project overview, click the **</>** (web) icon to add a web app.
2. Give it a nickname (e.g. "assessment site") and click **Register app**.
3. You'll see a `firebaseConfig` object — copy those values into
   `firebase-config.js` in this project (replace the `PASTE_...` placeholders).

### 3. Turn on Firestore (the database)
1. In the left sidebar: **Build → Firestore Database → Create database**.
2. Choose a location close to you, start in **production mode**.
3. Go to the **Rules** tab and paste in the contents of `firestore.rules`
   from this project, then click **Publish**.

### 4. Turn on Authentication (so you can log in to `admin.html`)
1. Left sidebar: **Build → Authentication → Get started**.
2. Under **Sign-in method**, enable **Email/Password**.
3. Go to the **Users** tab → **Add user** → enter your own email and a
   password. This is the account you'll use to sign in at `admin.html`.
   (Add more admin users the same way if others need access.)

### 5. Try it locally
Just open `index.html` in a browser to test the assessment, and
`admin.html` to test logging in and seeing results. (Some browsers block
Firebase's requests from a bare `file://` page — if that happens, run a
quick local server: `python3 -m http.server 8080` from this folder, then
visit `http://localhost:8080/index.html`.)

## Hosting it for real

Two easy free options once the code is on GitHub:

**Option A — Firebase Hosting** (matches the Firebase backend you're already using)
```
npm install -g firebase-tools
firebase login
firebase init hosting   # pick this project, set public dir to "." , single-page app: No
firebase deploy
```
This gives you a live URL like `https://redfish-tpi.web.app`.

**Option B — GitHub Pages**
Push this folder to your GitHub repo, then in the repo's Settings → Pages,
set the source to your main branch. You'll get a URL like
`https://cdnjohnson.github.io/REDFISH-ASSESMENT/`.

Either way, send candidates the link to `index.html` (or set it as the
repo/site's homepage) and keep `admin.html`'s link to yourself.

## Notes

- Candidate name, optional role/email, their answer-by-answer responses,
  the resulting 4-letter type, and axis percentages are all saved per
  submission — that's what shows up in the admin dashboard and CSV export.
- This is an internal, informal instrument, not a licensed psychometric
  test — that disclaimer is shown to candidates on the intro screen.
- To add more admins later, add their email in Firebase Authentication →
  Users; no code changes needed.
