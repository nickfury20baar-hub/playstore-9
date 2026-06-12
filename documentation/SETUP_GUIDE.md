# AppVerse Setup Guide

## Complete Setup Instructions

### Prerequisites

Before starting, ensure you have installed:
- **Flutter SDK** (latest stable) - https://flutter.dev/docs/get-started/install
- **Node.js** 14+ - https://nodejs.org/
- **Firebase CLI** - `npm install -g firebase-tools`
- **Android Studio** or **VS Code** with Flutter extension
- **Git** - https://git-scm.com/
- **Google Account** - for Firebase and Google Sign-In

### Step 1: Clone Repository

```bash
git clone https://github.com/nickfury20baar-hub/playstore-9.git
cd playstore-9
```

### Step 2: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click "Create a new project"
3. Name it `AppVerse` or similar
4. Enable Google Analytics (optional)
5. Click "Create project"
6. Wait for project creation to complete

### Step 3: Enable Firebase Services

In your Firebase project:

1. **Firestore Database**
   - Navigate to "Build" > "Firestore Database"
   - Click "Create database"
   - Start in **Production mode**
   - Choose region closest to your users
   - Click "Create"

2. **Authentication**
   - Go to "Build" > "Authentication"
   - Click "Get started"
   - Enable "Email/Password" provider
   - Enable "Google" provider
     - Go to Settings gear icon
     - Add your OAuth consent screen details
     - Save

3. **Storage**
   - Go to "Build" > "Storage"
   - Click "Get started"
   - Start in **Production mode**
   - Choose same region as Firestore
   - Click "Done"

4. **Cloud Functions**
   - Go to "Build" > "Functions"
   - Click "Get started"
   - Select your region
   - Wait for setup to complete

### Step 4: Configure Flutter App

1. **Download google-services.json**
   - In Firebase Console, go to Project Settings (⚙️)
   - Go to "General" tab
   - Scroll to "Your apps" section
   - Click on Android icon if not present, then "Add app"
   - Package name: `com.appverse.marketplace`
   - Download `google-services.json`
   - Place in: `flutter_app/android/app/google-services.json`

2. **Setup Flutter Project**
   ```bash
   cd flutter_app
   flutter pub get
   ```

3. **Configure Firebase in Flutter**
   - Edit `lib/config/firebase_config.dart`
   - Update with your Firebase project details

### Step 5: Deploy Firebase Security Rules

1. **Firestore Rules**
   ```bash
   firebase deploy --only firestore:rules
   ```

2. **Storage Rules**
   ```bash
   firebase deploy --only storage
   ```

### Step 6: Deploy Cloud Functions

```bash
cd ../cloud_functions
npm install
firebase deploy --only functions
```

### Step 7: Run Flutter App

```bash
cd ../flutter_app
flutter run
```

### Step 8: (Optional) Deploy Admin & Developer Panels

```bash
cd ../web_admin
npm install
npm run build
firebase deploy --only hosting:admin

cd ../web_developer
npm install
npm run build
firebase deploy --only hosting:developer
```

## File Organization

Create these directories in `flutter_app/lib/`:

```
lib/
├── config/
│   ├── firebase_config.dart
│   ├── app_theme.dart
│   └── app_constants.dart
├── models/
│   ├── user_model.dart
│   ├── app_model.dart
│   ├── review_model.dart
│   └── download_model.dart
├── services/
│   ├── auth_service.dart
│   ├── firestore_service.dart
│   ├── storage_service.dart
│   └── notification_service.dart
├── providers/
│   ├── auth_provider.dart
│   ├── app_provider.dart
│   ├── search_provider.dart
│   └── user_provider.dart
├── screens/
│   ├── auth/
│   ├── home/
│   ├── search/
│   ├── app_detail/
│   ├── downloads/
│   ├── profile/
│   ├── developer/
│   └── admin/
├── widgets/
│   ├── app_card.dart
│   ├── app_rating_bar.dart
│   ├── review_item.dart
│   └── loading_skeleton.dart
├── utils/
│   ├── validators.dart
│   ├── format_utils.dart
│   └── extensions.dart
├── l10n/
│   ├── en.json
│   ├── es.json
│   └── fr.json
└── main.dart
```

## Environment Variables

Create `.env` file in `flutter_app/`:

```
FIREBASE_API_KEY=YOUR_API_KEY
FIREBASE_APP_ID=YOUR_APP_ID
FIREBASE_PROJECT_ID=YOUR_PROJECT_ID
FIREBASE_STORAGE_BUCKET=YOUR_BUCKET
```

## Firestore Initialization

After deploying rules, initialize collections:

1. Go to Firestore Console
2. Create the following collections (empty, will be populated by app):
   - `users`
   - `developers`
   - `apps`
   - `app_versions`
   - `reviews`
   - `downloads`
   - `wishlists`
   - `categories`
   - `notifications`
   - `admin_logs`
   - `reports`

## Testing Setup

```bash
cd flutter_app
flutter test
```

## Troubleshooting

### Firebase Connection Issues
- Verify google-services.json is in correct location
- Check Firebase project ID matches in config
- Ensure Firestore is in Production mode

### Flutter Build Issues
- Run `flutter clean`
- Run `flutter pub get`
- Ensure Android SDK is updated

### Google Sign-In Issues
- Add your app's SHA-1 fingerprint to Firebase
- Get SHA-1: `keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android`

## Next Steps

1. Review [DATABASE_SCHEMA.md](./DATABASE_SCHEMA.md) for data structure
2. Read [SECURITY.md](./SECURITY.md) for security implementation
3. Check [ARCHITECTURE.md](./ARCHITECTURE.md) for system design
4. See [DEPLOYMENT.md](./DEPLOYMENT.md) for production deployment

## Support

For detailed component implementation, see individual file documentation in the codebase.