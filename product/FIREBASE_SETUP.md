# Firebase setup — do this before testing the new app

The new app (`app.html`) needs two things turned on in your existing Firebase project that the old single-family version didn't use. This only takes a few minutes.

## 1. Enable Firestore Database

1. Go to https://console.firebase.google.com and open the **koda-routine** project.
2. In the left sidebar, under **Build**, click **Firestore Database**.
3. Click **Create database**.
4. Choose **Start in production mode** (we're pasting in our own rules below, so this is safe).
5. Pick a location — **australia-southeast1 (Sydney)** or **australia-southeast2 (Melbourne)** will be fastest for you.
6. Click **Enable**.

## 2. Enable Email/Password sign-in

1. Still in the left sidebar under **Build**, click **Authentication**.
2. Click **Get started**.
3. Click **Email/Password** in the provider list, toggle it **on**, and click **Save**.

## 3. Paste in the security rules

This is the important one — without it, anyone with the app could read or edit anyone else's family data. With it, a family can only ever see their own.

1. In **Firestore Database**, click the **Rules** tab at the top.
2. Replace whatever is there with this, then click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /families/{familyId} {
      allow read, write: if request.auth != null && request.auth.uid == familyId;
    }
  }
}
```

That's it — once those three things are done, `app.html` will work: sign up with any email/password, and it'll create your family's data automatically.

## A note on cost

Both of these are on Firebase's free "Spark" plan, which is generous enough for testing and even a real early launch (50K reads / 20K writes per day on Firestore, free). We won't need to touch billing until much later, if push notifications (Phase 5) or higher usage push us onto the paid "Blaze" plan — I'll flag that clearly if/when we get there.
