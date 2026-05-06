# CriminalAlert – Phase 4 (Assignment 04)

## What Was Added

| Requirement | Implementation |
|---|---|
| **F1 – Firebase Auth (Email/Password)** | `AuthRepository.kt` → `signInWithEmail()`, `registerWithEmail()` |
| **F1 – Firebase Auth (Google Sign-In)** | `AuthRepository.kt` → `signInWithGoogle()`, `LoginActivity.kt` |
| **F1 – Session Persistence** | `SplashActivity` checks `FirebaseAuth.currentUser` on startup |
| **F2 – Firestore Reports Collection** | `FirestoreHelper.kt` → `addReport()`, `syncActiveReports()` |
| **F2 – Firestore Users Collection** | `FirestoreHelper.kt` → `saveUserProfile()`, `syncUserData()` |
| **F2 – Real-Time Sync** | `DashboardFragment` & `ProfileFragment` collect Firestore Flows |
| **FCM Push Notifications** | `CriminalAlertFCMService.kt` – foreground notifications + token refresh |
| **Jetpack Compose Screen** | `ComposeStatsActivity.kt` – live crime statistics dashboard |
| **Logout** | `ProfileFragment` → `AuthRepository.signOut()` |

## Setup Steps

### 1. Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create a project or use existing
3. Add Android app → package: `com.example.criminalalertprototype`
4. Download `google-services.json` → place in `app/` folder

### 2. Enable Authentication
- Firebase Console → Authentication → Sign-in method
- Enable **Email/Password**
- Enable **Google** → copy the **Web Client ID**
- Paste it in `app/src/main/res/values/strings.xml` → `default_web_client_id`

### 3. Add SHA-1
- Run: `./gradlew signingReport`
- Copy the debug SHA-1
- Firebase Console → Project Settings → Your apps → Add fingerprint

### 4. Firestore Database
- Firebase Console → Firestore Database → Create in Native mode
- For development, use these rules:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 5. Sync & Build
- Sync Gradle → Build → Run

## New Files

```
app/src/main/java/com/example/criminalalertprototype/
├── auth/
│   └── AuthRepository.kt          ← F1: Firebase Auth
├── firebase/
│   └── FirestoreHelper.kt         ← F2: Firestore real-time
├── notifications/
│   └── CriminalAlertFCMService.kt ← FCM push notifications
├── activities/
│   ├── ComposeStatsActivity.kt    ← Jetpack Compose screen
│   ├── LoginActivity.kt           ← Updated (Firebase auth)
│   ├── MainActivity.kt            ← Updated (auth guard)
│   └── SplashActivity.kt          ← Updated (session check)
├── fragments/
│   ├── DashboardFragment.kt       ← Updated (real-time Firestore)
│   ├── ProfileFragment.kt         ← Updated (logout + profile sync)
│   └── ReportFragment.kt          ← Updated (Firestore + notification)
└── models/
    ├── UserProfileModel.kt        ← New
    ├── ReportModel.kt             ← Updated (firestoreId)
    └── AlertModel.kt              ← Updated (firestoreId)
app/google-services.json           ← REPLACE with yours!
logic_map.pdf                      ← Required documentation
```
