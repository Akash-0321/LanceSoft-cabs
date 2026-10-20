# LanceSoft Cabs — Web + Android starter

## What is included
- `public/index.html`: the attendance starter UI and Firebase integration placeholders.
- `public/manifest.webmanifest`, `public/sw.js`, `public/icons/`: PWA install metadata and basic offline shell caching.
- `package.json`, `capacitor.config.json`: Capacitor wrapper configuration for Android.
- `.github/workflows/build-apk.yml`: cloud workflow to build a debug APK from GitHub Actions.

## A. Connect Firebase (required)
1. Open `public/index.html` in an editor.
2. Replace the placeholder `firebaseConfig` values with your project's Web app config from Firebase Console → Project settings → Your apps.
3. Enable Email/Password and Google sign-in in Firebase Authentication.
4. Create Firestore and publish the supplied `firestore.rules` file (included in this ZIP if copied from the starter). If it is absent, use the rules file from the original `LanceSoft_Cabs_Firebase_Starter.zip`.
5. Securely provision the first admin using Firebase Admin SDK on a trusted server. Never place service-account keys in the web app.

## B. Publish the Web App from a phone
1. Create/sign in to a GitHub account and create a new repository named `lansesoft-cabs`.
2. Upload all files/folders from this ZIP into the repository (including the hidden `.github` folder).
3. For a simple static web preview, enable GitHub Pages from repository Settings → Pages → Deploy from branch → `main` / root. Note: Firebase Authentication requires the deployed domain to be added in Firebase Authentication → Settings → Authorized domains.
4. For production, prefer Firebase Hosting: use Firebase Console/CLI from a computer or cloud shell, or a hosting service connected to the repository. Do not treat a preview deployment as production until rules/auth have been tested.

## C. Build APK using GitHub Actions
1. Upload the project to GitHub as above.
2. Open repository → Actions → Build Android APK → Run workflow (or push a commit to `main`).
3. Wait for the workflow to finish successfully.
4. Open the completed workflow run, scroll to Artifacts, and download `LanceSoft-Cabs-debug-apk`.
5. Extract the artifact ZIP and install `app-debug.apk` on your Android phone. Android may ask you to allow installation from that browser/files app.
6. A debug APK is for testing. A signed release APK/AAB requires signing configuration and secure storage of signing keys.

## Important limitations and security
- This is a starter project, not a production-certified system. Test on a separate Firebase project first.
- User approval relies on a trusted Firebase custom claim (`admin: true`); the web page must never grant this claim to itself.
- The starter's location feature is user-triggered location sharing, not continuous/background tracking. Background GPS requires a native Android foreground service, runtime permissions, privacy disclosure, and testing.
- In-app calling without showing personal numbers requires a calling/VoIP provider and backend; not included.
- Push notifications, trip assignment, scheduled shifts, driver/vendor management workflows, odometer/fuel sheets, audit logs, and server-side validations remain additional work.
- Do not use real employee location data until security rules, privacy notices, access roles, and retention policy have been reviewed.
