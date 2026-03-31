🎮 Console Collection Tracker
A personal website to manage your video game console collection — with live eBay market prices, repair tracking, and automatic total value calculation.


Features

✅ Track all major consoles (Nintendo, PlayStation, Xbox)
🟢 Mark consoles as owned
🔧 Mark consoles as needing repair
💜 Live eBay market prices (automatically updated daily)
💰 Automatic total collection value
🔍 Search & filter
🔒 Admin login to edit your collection


Requirements
All of the following are completely free:

GitHub account
Firebase account
eBay Developer account
Google account (for Apps Script)


Step 1 – Fork the Repository

Click Fork (top right of this page)
In your forked repo go to Settings → Pages
Source: Deploy from a branch → Branch: main → Folder: / (root)
Save — your site is now live at https://YOURNAME.github.io/REPONAME


Step 2 – Set Up Firebase
2a. Create a project

Go to console.firebase.google.com
Click "Add project" → give it a name → Create
In the project: Realtime Database → Create → Region: europe-west1 → Start in test mode

2b. Set database rules
Under Realtime Database → Rules replace everything with:
json{
  "rules": {
    "counts":       { ".read": true, ".write": "auth != null" },
    "repair":       { ".read": true, ".write": "auth != null" },
    "marketPrices": { ".read": true, ".write": true }
  }
}
Click "Publish".
2c. Create an admin user

In the Firebase project: Authentication → Get started
Enable Email/Password
Under Users → Add user → enter your email + password

2d. Copy your Firebase config

Go to Project Settings (⚙️) → General → Your apps → Add web app
Register the app — you will receive a firebaseConfig object


Step 3 – Update index.html
Open index.html on GitHub (click the ✏️ pencil icon) and replace the firebaseConfig block with your own values:
javascriptconst firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
Click "Commit changes".

Step 4 – Set Up the eBay API
4a. Create a developer account

Go to developer.ebay.com → register for free
Click "Create Application" → give your app a name
Under My Account → Application Access click "Apply for exemption"

Reason: "Personal hobby project, no eBay user data is stored or processed"
Usually approved within minutes


Copy your Production Keys: App ID (Client ID) and Cert ID (Client Secret)


⚠️ Make sure to use the Production Keys, not the Sandbox keys!


Step 5 – Set Up Google Apps Script
5a. Create the script

Go to script.google.com → "New project"
Open apps-script/updatePrices.gs from this repository
Copy the entire content into the Apps Script editor (delete the existing code first)

5b. Enter your credentials
Replace the three placeholders at the very top of the script:
javascriptconst FIREBASE_URL       = "https://YOUR-PROJECT-default-rtdb.europe-west1.firebasedatabase.app";
const EBAY_CLIENT_ID     = "YOUR_PRODUCTION_CLIENT_ID";
const EBAY_CLIENT_SECRET = "YOUR_PRODUCTION_CLIENT_SECRET";
5c. Run a manual test

Select updatePrices from the function dropdown → click ▶ Run
The log should show prices, e.g. gbasp: 75
Check your Firebase Console — you should see a new marketPrices/ node with values

5d. Set up the automatic schedule

In the left sidebar click ⏰ Triggers
Click "+ Add Trigger"
Function: updatePrices → Time-based → Day timer → between 03:00–04:00
Save

Prices will now update automatically every day.

Customizing Consoles
Want to add or remove consoles? Edit the <li data-id="..."> entries in index.html and add the matching IDs to the CONSOLES object in the Apps Script file.

Using the Admin Mode
To edit your collection:

Scroll to the bottom of the website
Enter the email and password you created in Firebase
Click Login
+/− buttons appear to adjust counts, and a 🔧 button to mark consoles for repair


How It Works
index.html              → Complete website (HTML, CSS, JS in one file)
apps-script/
  updatePrices.gs       → Google Apps Script for daily eBay price fetching
ServiceUsageCostGitHub PagesWebsite hostingFreeFirebase Realtime DatabaseStore collection & pricesFreeFirebase AuthenticationAdmin loginFreeeBay Browse APIFetch market pricesFreeGoogle Apps ScriptDaily price updatesFree
Total: $0/month ✅

License
MIT — do whatever you want with it! A ⭐ on the repo is always appreciated 😄
