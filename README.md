🛰️ Firebase Relay Chat Hub
A secure, real-time, single-page application (SPA) built entirely within a single index.html file using React, Babel, Tailwind CSS, and Google Firestore for data persistence and real-time communication.
This application is designed as a centralized communication platform featuring:
 * Real-time group chat for a company/business.
 * User Authentication (Login/Registration with Relay ID and PIN).
 * User Profile Management and Theming (Light/Dark mode, Accent Color).
 * Friends Manager for peer-to-peer private messaging.
 * Admin Dashboards for Owner/Manager roles (Staff/Team Management, Company Notifications).
 * Global Admin Dashboard (for system-wide notices).
 * Local Data Backup Utility.
🚀 Setup and Usage
1. Configure Firebase
This application requires a Firebase project with Firestore enabled.
 * Create a Firebase Project: If you don't have one, create a project and enable Firestore Database.
 * Get Configuration: In your Firebase project settings, find your app's configuration object (the apiKey, projectId, etc.).
 * Update index.html: Open the index.html file and look for the STANDALONE_FIREBASE_CONFIG block around line 35. You must replace the placeholder values with your actual Firebase credentials.
<!-- end list -->
const STANDALONE_FIREBASE_CONFIG = {
    apiKey: "YOUR_FIREBASE_API_KEY", // <-- REPLACE THIS
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com", // <-- REPLACE THIS
    projectId: "YOUR-PROJECT-ID", // <-- REPLACE THIS
    // ... rest of config
};

2. Run the Application
Since this is a single, self-contained HTML file, you can run it in two ways:
 * Locally: Simply open the index.html file directly in your web browser.
 * Deployment: Upload the index.html file to any static hosting service (e.g., GitHub Pages, Firebase Hosting, Netlify, Vercel).
3. Initial Registration
The first users should follow this flow:
 * Click Register.
 * When selecting the role, choose Business Owner (First Registration).
 * After registration, you will be taken to the Business Dashboard, where you can manage your company and notifications.
 * Subsequent users should register as Staff (Employee) using the same company name.
🔒 Firebase Security Rules
For the chat and user data to function correctly, your Firestore Security Rules must be configured to allow public reads and writes to the correct paths.
Minimum Required Rules:
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // --- Global and Public Data ---
    // Public chats, banned lists, global notices
    match /artifacts/{appId}/public/data/{collection}/{document} {
      allow read, write: if request.auth != null;
    }
    
    // User Profiles must be publicly accessible for friends search/display
    match /artifacts/{appId}/public/data/all_profiles/{userId} {
      allow read, write: if request.auth != null;
    }
    
    // --- Private User Data ---
    // Private chats, friends lists, personal profiles, business teams
    match /artifacts/{appId}/users/{userId}/{path=**} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
