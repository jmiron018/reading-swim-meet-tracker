# Reading Swim Meet Tracker

Phone-friendly dual-meet scorer. Tap the places your team finished in each race; the scoreboard totals both teams. Progress is saved on the device, so a refresh won't lose the meet.

Live: https://reading-swim-meet-tracker.web.app

## Hosting

Static site in `public/`, served by Firebase Hosting. Every push to `main` deploys via `.github/workflows/firebase-deploy.yml`.

One-time setup:
1. Create Firebase project `reading-swim-meet-tracker` at https://console.firebase.google.com (if the ID is taken, change it in `.firebaserc` and the workflow).
2. Locally: `npx firebase-tools login` then `npx firebase-tools init hosting:github` and pick this repo. It creates the `FIREBASE_SERVICE_ACCOUNT_READING_SWIM_MEET_TRACKER` secret for you. Keep the existing `firebase.json` and workflow when asked.
3. Push to `main` and the site goes live.

## Results tab

A **Results** tab (open to everyone) reads `meets` and shows the season record (W–L, points for/against), each meet with a tap-to-open race-by-race breakdown, and average points per event. Seasons run Aug–Jul; a season picker appears once there is more than one.

## Final results

Each phone keeps its own scores (saved on that device only), labeled with the scorer's name. **Submit final results** asks for the opponent, meet date and the scorekeeper password, then saves that phone's sheet to Firestore at `meets/{date}_{opponent}`. Re-submitting the same date + opponent replaces the record. After a successful submit the form closes and that phone's sheet is cleared for the next meet.

Sign-in is a Firebase Auth email/password account. `firestore.rules` only accepts records from accounts listed in the `Scorekeepers` collection (capital S) (document ID = the account's lowercase email, or its UID; added in the Firebase console), and stamps each record with `submittedBy`. Rules deploy with every push (the deploy service account needs the **Firebase Rules Admin** role).
