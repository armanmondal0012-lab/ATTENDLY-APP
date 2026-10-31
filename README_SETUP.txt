ATTENDLY — CLEAN CONNECTED ADMIN + STUDENT APP

WHAT THIS BUILD DOES
- Keeps the original Attendly interface and existing app features.
- Admin account (UID configured in index.html) can manage the full student list, all attendance, schedules, pages, app logo and push notifications.
- Public app settings (schedule, page text, logo and alarm setting) sync to signed-in and guest devices.
- Student accounts sign in with Firebase Authentication email/password.
- A student can read ONLY their own profile and attendance history. Other students' names, IDs, phone numbers and attendance are not included in the public app document.
- Admin student profiles include a Firebase Auth UID field. Admin maps each student account to its profile by entering that UID.
- Attendance history is retained; duplicate attendance is blocked for 15 hours, rather than deleting older history.
- Removes the insecure hard-coded student edit authorization code from the browser.
- Includes Firebase Cloud Messaging service worker and GitHub Actions push sender.

REQUIRED ONE-TIME FIREBASE SETUP (cannot be completed from a ZIP file)
1. In Firebase Console > Authentication > Sign-in method, enable Email/Password and Anonymous.
2. Create each student account in Authentication > Users (Add user). Give the student their own email and password securely.
3. Copy that student's Firebase Authentication UID.
4. Sign in to Attendly as the authorized Admin. Open Admin Panel > Students, edit/add the student, paste the UID in FIREBASE STUDENT UID, and save.
5. Publish firestore.rules from this package in Firebase Console > Firestore Database > Rules.
6. Upload index.html, firebase-messaging-sw.js, firestore.rules (publish rules in console; do not serve it as a page), package.json, scripts/, and .github/ to the correct hosting/repository locations.
7. Host the app on HTTPS. For push notifications, configure GitHub Actions secret FIREBASE_SERVICE_ACCOUNT_JSON as described below.

ADMIN ACCOUNT
- Authorized admin UID: 7AOEs1d8hGQGYxU2GAyKCjdhkTG3
- The admin must sign in with the Firebase Authentication account matching that UID.
- Admin Panel opens by clicking the Notifications icon five times, then using the Cloud Sync sign-in panel.

PUSH NOTIFICATIONS
- In GitHub repository Settings > Secrets and variables > Actions, create:
  Name: FIREBASE_SERVICE_ACCOUNT_JSON
  Value: the complete Firebase service-account JSON contents.
- Never commit the service-account JSON/private key to the website or repository.
- Enable GitHub Actions and run the Attendly Push Notifications workflow.

IMPORTANT SECURITY NOTES
- Student account creation is intentionally done in Firebase Console; a public browser app must not contain Firebase Admin credentials or create privileged accounts.
- Only map a student's own Firebase UID to their profile. Do not reuse one UID for multiple students.
- Existing Firestore data may remain in old documents; after publishing these rules, old /attendly/shared data is admin-only. The public app reads only /attendly/public.
- If you previously deployed broader Firestore rules, replace them with this complete ruleset.

LIMITATIONS / VERIFICATION
- JavaScript syntax and package structure are checked locally. A live end-to-end test requires access to your Firebase Console, Hosting and GitHub repository.
- GitHub scheduled push delivery is approximate and may be delayed.


MOBILE CAMERA TROUBLESHOOTING
- Open the site using its HTTPS GitHub Pages URL in Chrome (not a downloaded HTML file).
- Tap "Open phone camera" to request the rear camera.
- If Android says the site cannot ask for permission, close floating bubbles/chat heads,
  screen filters, or other apps drawing over the screen, then retry.
- In Chrome: Site settings > Camera > Allow for this website.
- If live camera is unavailable, tap "Scan QR from photo" and choose a clear QR image.
