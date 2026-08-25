EduLearn Platform - FREE Version (No Storage Costs)
=====================================================

FIREBASE SETUP (Required before first use):
-------------------------------------------
1. Go to https://console.firebase.google.com/
2. Select your project: osama-100fc
3. Enable Authentication:
   - Go to Authentication > Sign-in method
   - Enable "Email/Password"

4. Create Admin Account:
   - Go to Authentication > Users > Add User
   - Email: osama@edulearn.admin
   - Password: OS1234

5. Enable Firestore Database:
   - Go to Firestore Database > Create Database
   - Start in "Test mode" (allows reads/writes)

6. Set Firestore Rules:
   Go to Firestore Database > Rules, paste:

   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read, write: if request.auth != null;
       }
       match /content/{contentId} {
         allow read: if request.auth != null;
         allow write: if request.auth != null && 
           get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
       }
     }
   }

HOW TO USE:
-----------
- Open index.html in any modern browser
- Login as Admin:
  Username: Osama
  Password: OS1234

- Admin can:
  * View Dashboard with stats
  * Add students (creates login accounts)
  * Approve/Block/Delete students
  * Add content: Images, PDFs (stored as Base64 in Firestore)
  * Add YouTube videos (by link)
  * Add external links
  * Filter content by Grade, Year, Subject, Term
  * Upload files up to 800KB (Firestore document limit)

- Students can:
  * Login with username/password given by admin
  * View learning materials
  * Filter by Grade, Year, Subject, Term
  * Update profile and password

IMPORTANT NOTES:
----------------
- This version uses NO Firebase Storage (saves money!)
- Files are stored as Base64 directly in Firestore
- Maximum file size: 800KB per file
- For larger files or videos, use "YouTube Link" or "External Link" options
- Firestore free tier: 50K reads/day, 20K writes/day, 1GB storage

FILE STRUCTURE:
---------------
edulearn-platform/
  index.html          - Main HTML file
  css/
    style.css         - Glassmorphism styles
  js/
    firebase-config.js - Firebase config (your credentials)
    app.js            - All app logic (no storage dependency)
