# Firebase Hosting and Realtime Database Deployment Instructions

## Firebase Hosting
1. Make sure you have Firebase CLI installed. You can install it using npm:
   ```bash
   npm install -g firebase-tools
   ```

2. Login to your Firebase account:
   ```bash
   firebase login
   ```

3. Initialize your Firebase project:
   ```bash
   firebase init
   ```

4. Deploy to Firebase Hosting:
   ```bash
   firebase deploy
   ```

## Firebase Configuration in SawitPro.html
1. Open the `SawitPro.html` file.
2. Add the following Firebase configuration snippet:
   ```javascript
   // Your web app's Firebase configuration
   var firebaseConfig = {
       apiKey: "YOUR_API_KEY",
       authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
       databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
       projectId: "YOUR_PROJECT_ID",
       storageBucket: "YOUR_PROJECT_ID.appspot.com",
       messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
       appId: "YOUR_APP_ID"
   };
   // Initialize Firebase
   firebase.initializeApp(firebaseConfig);
   ```
   Replace placeholders with your actual Firebase project settings.

## Update .firebaserc
1. In your project root, edit the `.firebaserc` file to include your Firebase project ID:
   ```json
   {
     "projects": {
       "default": "YOUR_PROJECT_ID"
     }
   }
   ```

## Create FIREBASE_TOKEN Secret
1. Run the following command to create a Firebase token:
   ```bash
   firebase login:ci
   ```
2. Save the generated token as a secret in your CI/CD environment, so it’s not exposed in your codebase.

## Enable Realtime Database
1. Go to the Firebase Console.
2. Select your project and navigate to "Realtime Database."
3. Click "Create Database" and set the rules:
   ```json
   {
     "rules": {
       ".read": "auth != null",
       ".write": "auth != null"
     }
   }
   ```
4. Click "Publish."

## Security Recommendations
- Always keep your Firebase API key secure; don't hardcode sensitive credentials within your codebase. Consider using environment variables for secrets.
- Regularly review your database security rules to ensure only authenticated users have access to read/write data.
- Enable 2FA for Firebase accounts to enhance security.

## Final Notes
- Make sure to test your deployment thoroughly.
- Document any changes made to configurations.