# Reading Swim Meet Tracker

Phone-friendly dual-meet scorer. Tap the places your team finished in each race; the scoreboard totals both teams. Progress is saved on the device, so a refresh won't lose the meet.

Live: https://reading-swim-meet-tracker.web.app

## Hosting

Static site in `public/`, served by Firebase Hosting. Every push to `main` deploys via `.github/workflows/firebase-deploy.yml`.

One-time setup:
1. Create Firebase project `reading-swim-meet-tracker` at https://console.firebase.google.com (if the ID is taken, change it in `.firebaserc` and the workflow).
2. Locally: `npx firebase-tools login` then `npx firebase-tools init hosting:github` and pick this repo. It creates the `FIREBASE_SERVICE_ACCOUNT_READING_SWIM_MEET_TRACKER` secret for you. Keep the existing `firebase.json` and workflow when asked.
3. Push to `main` and the site goes live.

## Final results

Each phone keeps its own scores (saved on that device only), labeled with the scorer's name. **Submit final results** asks for the opponent, meet date and the scorekeeper password, then saves that phone's sheet to Firestore at `meets/{date}_{opponent}`. Re-submitting the same date + opponent replaces the record.

The password is the Firebase Auth account `scorekeeper@reading-swim-meet-tracker.web.app`; `firestore.rules` only lets that account write. Rules deploy with every push (the deploy service account needs the **Firebase Rules Admin** role).
