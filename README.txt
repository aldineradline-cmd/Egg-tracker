EGG TRACKER - PRIVATE SETUP FOR TWO PARTNERS (free)

HOW PRIVACY WORKS
The web page itself is public but empty. Your egg and order data lives in Firebase and can only be read by
the two partner accounts you approve. Nobody else can see it, even if they open the page.

PART 1 - Firebase (database + sign-in)
1. console.firebase.google.com > Add project (Analytics off).
2. Build > Authentication > Get started > Sign-in method > Email/Password > Enable > Save.
3. Build > Firestore Database > Create database > pick a region > Start in production mode.
4. Firestore > Rules tab. STEP A (temporary, only for setup). Replace everything with this and Publish:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /teams/{team}/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}

5. Project settings (gear) > General > Your apps > </> Web > register (no hosting).
   Copy the firebaseConfig block.

PART 2 - Host the page (GitHub Pages) - see earlier steps. Upload all the loose files.

PART 3 - Connect and approve
1. Open the app > Settings > type your name, paste the Firebase config as JSON, Save. It reloads.
2. Press Create account (email + password). Open Settings, press "Copy my ID".
3. In Settings press "Copy partner setup link" and send it to your partner. They open it, press Create account,
   then send you their ID.
4. LOCK IT (STEP B). In Firestore > Rules, replace with this (put the two IDs in), Publish:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /teams/{team}/{document=**} {
      allow read, write: if request.auth.uid in ['YOUR_ID_HERE', 'PARTNER_ID_HERE'];
    }
  }
}

From now on only your two accounts can read or write the data. Anyone else who creates an account sees
"Not approved yet" and no data.

INSTALL ON PHONE: Android Chrome > menu > Install app. iPhone Safari > Share > Add to Home Screen.
ONE DEVICE ONLY (no sharing): double-click index.html; leave the Firebase box empty.
