# LanceSoft Cabs — Firebase web starter

## Included
- `index.html`: responsive starter interface for sign-in, access request, user approval, duty IN/OUT, trip notes, location sharing, attendance history, and CSV export.
- `firestore.rules`: starter Firestore rules that require an admin custom claim and approval status.

## 1. Connect Firebase
1. Open Firebase Console → Project settings → Your apps → Web app.
2. Copy the `firebaseConfig` object values into `index.html`. Do not add a service-account key.
3. Authentication → Sign-in method: enable Email/Password and Google.
4. Authentication → Settings → Authorized domains: add the domain where you host this app.
5. Firestore Database → Create database. Start in production/locked mode.
6. Firestore → Rules: paste `firestore.rules` and publish.

## 2. IMPORTANT: provision the first admin securely
The interface intentionally does not allow users to make themselves admin. A trusted project owner must assign the Firebase custom claim `admin: true` to the first admin user's Firebase Auth UID using a trusted server environment and Firebase Admin SDK. Never put Admin SDK credentials in this HTML or in a browser. Until this is done, approval management and admin reports will be unavailable.

Example trusted server-side Node.js code (run only in a secured admin environment):
```js
import { initializeApp, applicationDefault } from "firebase-admin/app";
import { getAuth } from "firebase-admin/auth";
initializeApp({ credential: applicationDefault() });
await getAuth().setCustomUserClaims("REPLACE_WITH_ADMIN_AUTH_UID", { admin: true });
```
Then sign out and sign back in as that admin so the new token claim is refreshed.

## 3. Run and host
Because Firebase modules and browser geolocation need a secure context, use Firebase Hosting or another HTTPS host. For a quick test, use Firebase Hosting:
- Install Node.js and Firebase CLI on a computer.
- `npm install -g firebase-tools`
- `firebase login`
- In a folder containing `index.html`, run `firebase init hosting`, choose the existing LanceSoft Cabs project, set the public directory to `.` and configure as a single-page app: No is fine for this single HTML page.
- `firebase deploy --only hosting`
Use a separate project for testing before real deployment.

## 4. Current limitations / production work still required
This is a starter, not a fully production-certified app:
- Location sharing is user-triggered current-location sharing; continuous background GPS tracking is not implemented. Reliable background tracking on Android requires a native app/foreground service and explicit permission.
- Calling without exposing personal numbers requires a telephony/VoIP provider and backend; this starter does not implement calls.
- Push notifications, employee roster imports, shift scheduling, trip assignment, cab-reading/odometer sheets, payroll, audit logs, and server-side validation need to be added for production.
- Use Firestore indexes if Firebase asks for them.
- Test rules with the Firebase Rules Playground or Emulator before using real employee data.
- Keep location collection transparent, limited to active duty, and governed by company policy and applicable law.
