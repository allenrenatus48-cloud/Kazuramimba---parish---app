# Kazuramimba Parish Management System

**Parokia ya Mt. Anthony wa Padua – Kazuramimba · Jimbo la Kigoma · Tanzania**

Live website: https://allenrenatus48-cloud.github.io/Kazuramimba---parish---app/

## Current application

This is a browser-based Progressive Web App (PWA) connected to Firebase Authentication and Cloud Firestore.

Available modules:
- Staff sign-in using Firebase email/password authentication
- Parishioner and family registers
- Sacramental register (Baptism, First Communion, Confirmation, Marriage)
- Contributions in TZS, including Zaka, Ujenzi, Sadaka, Mavuno and additional categories
- Jumuiya and transfer registers
- Dashboard, contribution reports, CSV export and print/PDF
- Record editing, audit history and manual JSON backup
- Role-based access for parish leadership, finance staff and read-only staff

## Access and installation

The HTTPS website is the main distribution channel and works in current browsers on Android, iPhone/iPad and Windows/macOS computers.

- **Android:** Open the live website in Chrome, open the browser menu, then choose **Install app** or **Add to Home screen** if offered.
- **iPhone/iPad:** Open the live website in Safari, tap **Share**, then **Add to Home Screen**.
- **Computer:** Open the website in Chrome or Edge. Use the install icon in the address bar or the browser menu if offered.

The PWA website is not the same as a Play Store or Apple App Store listing. Store publication requires separate developer accounts, store-specific packaging, review and approval. No store listing is currently claimed.

## Firebase setup and security

Firebase project: `parish-administration-88aa6`.

The application expects a Firestore document at `staff/{Firebase Authentication UID}` for each staff member. Each staff document must contain:
- `name`: staff display name (string)
- `role`: one of `parish_priest`, `assistant_priest`, `secretary`, `finance_officer`, `read_only`
- `active`: Boolean `true`

The document ID must exactly match that user's Firebase Authentication UID. Do not use a display label such as "Parish Priest" as the role value; use `parish_priest`.

Firestore security rules are in `firestore.rules`. They must be deployed to the same Firebase project before staff use the system. Confirm the rules in Firebase Console after deployment. Never enable public reads on private collections such as `members`, `families`, `sacraments`, `contributions`, `staff` or `auditLogs`.

## Important operational limitations

- This repository currently represents **one parish deployment**. It is not yet a complete multi-parish diocesan tenant system. Do not add other parishes' records to this Firebase project until tenant separation and diocesan permissions have been designed and tested.
- There is no public parishioner directory in this release. Do not make private records public.
- Manual JSON backup is available to authorised parish administrators. It is not an automatic scheduled backup. Download backups regularly and store them securely outside the device.
- The application has not been independently security-audited or penetration-tested. Test with fictional records and separate test accounts before entering real personal, sacramental or financial data.
- Keep Firebase rules restrictive. Never share staff passwords, authentication UIDs unnecessarily, backup files or private parish records.
- The Firebase web configuration in a browser app is not a password. Firestore security rules and Firebase Authentication are the controls that protect data.

## Deploying Firestore rules

From the repository folder on a computer with Firebase CLI installed and authenticated, deploy rules to the intended project:

```bash
firebase use parish-administration-88aa6
firebase deploy --only firestore:rules
```

Then open Firebase Console → Firestore Database → Rules and verify that the published rules match `firestore.rules`.

## GitHub Pages

The live website is hosted from this repository's GitHub Pages deployment. Changes committed to the published branch become available after GitHub Pages finishes publishing.

Repository: https://github.com/allenrenatus48-cloud/Kazuramimba---parish---app
