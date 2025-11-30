Admin panel version (Step 1) - Instructions
------------------------------------------

Files included:
 - index.html     : Public homepage (reads movies from Firestore)
 - movie.html     : Player page (reads single movie doc from Firestore)
 - admin.html     : Admin panel to login and add/edit/delete movies
 - style.css      : Styles
 - README.txt     : This file

Quick setup (Firebase)
----------------------
1. Create a Firebase project at https://console.firebase.google.com/
2. Enable Firestore (Start in test mode for quick testing).
3. Enable Authentication -> Email/Password.
4. In the project settings -> SDK setup -> Add Firebase to web app, and copy the config object.
   Replace the placeholder firebaseConfig in index.html, movie.html, admin.html with your real config.

Security notes (recommended before going production)
--------------------------------------------------
- For admin protection use Firebase custom claims:
  a) Sign in as the admin in the Firebase console (or create the admin using admin.createUser).
  b) Use the Firebase Admin SDK to set custom claim 'admin' for the user:
     Example (Node.js):
       const admin = require('firebase-admin');
       admin.auth().setCustomUserClaims(uid, {admin: true});
  c) Update Firestore security rules to allow writes only if request.auth.token.admin == true

- Example Firestore rules (allow read to everyone, write only for admins):
  rules_version = '2';
  service cloud.firestore {
    match /databases/{database}/documents {
      match /movies/{docId} {
        allow read: if true;
        allow write: if request.auth != null && request.auth.token.admin == true;
      }
    }
  }

- For quick testing you can add your email to allowedAdminEmails array in admin.html, but this is NOT secure for production.

Deploying
---------
- You can host these static files on GitHub Pages, Netlify, Vercel or any static host.
- Make sure Firestore rules and Authentication are configured.

Next steps I can do for you (pick one):
 - Add protected admin via Firebase custom claims + instructions to set them.
 - Add user auth gating and paid access flow (Stripe).
 - Tokenize/proxy video URLs to hide raw links.
