# Kazuramimba Parish Management System

This is an early bilingual-ready (currently primarily Kiswahili) Progressive Web App prototype for Parokia ya Mt. Anthony wa Padua – Kazuramimba.

## Current limitations — read before use
- Records are stored only in the browser's local storage on the current device.
- There is no shared online database, staff login, role-based access, or multi-device synchronization yet.
- Do not enter real parishioner, sacramental, or financial data. Use fictional test data only until secure authentication, database rules, backups, and audit controls are implemented and tested.
- A public GitHub repository is not a secure place for private parish records.

## Included prototype functions
Parishioner, family, sacrament, contribution, and Jumuiya registers; dashboard counts; JSON backup and restore; installable PWA shell.

## Deploy
The repository can be hosted as a static site over HTTPS. For GitHub Pages: repository Settings → Pages → Deploy from a branch → main → / (root) → Save. After Pages publishes, open the provided HTTPS address in Chrome and use Install app / Add to Home screen.

This static prototype does not provide shared cloud storage. Firebase or another backend must be configured separately before multiple staff can share records.
