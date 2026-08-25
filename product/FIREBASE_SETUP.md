# Firebase setup — do this before testing the new app

`app.html` is now wired to the new **powerup-kids-app** project (separate from `koda-routine`, which stays untouched running your son's actual daily app). Same three steps as before, just in the new project this time.

## 1. Enable Firestore Database

1. Go to https://console.firebase.google.com and open the **powerup-kids-app** project.
2. In the left sidebar, under **Build**, click **Firestore Database**.
3. Click **Create database**.
4. Choose **Production mode** (we're pasting in our own rules below, so this is safe — this is the one you already confirmed).
5. Pick a location — **australia-southeast1 (Sydney)** or **australia-southeast2 (Melbourne)** will be fastest for you.
6. Click **Enable**.

## 2. Enable sign-in methods

1. Click **Authentication** in the left sidebar.
2. Click **Sign-in method** (or **Get started** if this is your first time here).
3. Click **Email/Password**, toggle it **on**, click **Save**. (You've likely already done this one.)
4. Click **Add new provider** (or find it in the list) and enable **Anonymous** too — this is new, needed for the kid link-code sign-in. It's a one-click toggle, no extra info required.

## 3. Paste in the security rules (updated)

This is the important one — it's what keeps each family's data private, and now also makes sure a linked kid device can only ever check things off for its own kid, never edit the routine, see other kids, or touch settings.

1. In **Firestore Database**, click the **Rules** tab at the top.
2. Replace whatever is there with this, then click **Publish** (this replaces the simpler version from before — it's grown to cover the kid link-code system):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /families/{familyId} {
      allow write: if request.auth != null && request.auth.uid == familyId;
      allow read: if request.auth != null && (
        request.auth.uid == familyId ||
        (exists(/databases/$(database)/documents/linkedDevices/$(request.auth.uid)) &&
         get(/databases/$(database)/documents/linkedDevices/$(request.auth.uid)).data.familyId == familyId)
      );

      match /childLogs/{childId} {
        allow read, write: if request.auth != null && (
          request.auth.uid == familyId ||
          (exists(/databases/$(database)/documents/linkedDevices/$(request.auth.uid)) &&
           get(/databases/$(database)/documents/linkedDevices/$(request.auth.uid)).data.familyId == familyId &&
           get(/databases/$(database)/documents/linkedDevices/$(request.auth.uid)).data.childId == childId)
        );
      }
    }

    match /linkCodes/{code} {
      allow read: if request.auth != null;
      allow create, update: if request.auth != null && request.auth.uid == request.resource.data.familyId;
      allow delete: if request.auth != null && request.auth.uid == resource.data.familyId;
    }

    match /linkedDevices/{deviceId} {
      allow read: if request.auth != null && request.auth.uid == deviceId;
      allow create: if request.auth != null && request.auth.uid == deviceId &&
        exists(/databases/$(database)/documents/linkCodes/$(request.resource.data.code)) &&
        get(/databases/$(database)/documents/linkCodes/$(request.resource.data.code)).data.familyId == request.resource.data.familyId &&
        get(/databases/$(database)/documents/linkCodes/$(request.resource.data.code)).data.childId == request.resource.data.childId;
      allow delete: if request.auth != null && (
        request.auth.uid == deviceId || request.auth.uid == resource.data.familyId
      );
      allow update: if false;
    }

  }
}
```

That's it — once all of this is done, `app.html` will work: sign up as the parent, generate a link code for your kid from the "🔗 Get Kid's Link Code" button, and enter that code on the kid's own device via "Kid? Enter your code instead" on the sign-in screen.

## A note on what I couldn't test myself

I tested all of this app's logic thoroughly against a stand-in backend (signup, linking, task/feat toggling, celebrations, and confirming a kid device truly can't see or reach any admin controls). What I can't test from here is whether the security rules above, once actually live on your project, behave exactly as written — that only happens against the real Firestore. If you want to double check, Firestore's **Rules** tab has a **Playground** button that lets you simulate a request (e.g. "can a random signed-in user read someone else's family doc?") without writing any code — worth trying once, otherwise the real test is simply: does a kid's linked device work, and does a stranger with no code get rejected (it should, and did in my testing).

## A note on cost

Both of these are on Firebase's free "Spark" plan, which is generous enough for testing and even a real early launch (50K reads / 20K writes per day on Firestore, free). We won't need to touch billing until much later, if push notifications (Phase 5) or higher usage push us onto the paid "Blaze" plan — I'll flag that clearly if/when we get there.
