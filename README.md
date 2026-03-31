# 🎮 Console Collection Tracker

A personal website to manage your video game console collection — with live eBay market prices, repair tracking, and automatic total value calculation.

**Live Demo:** [Your GitHub Pages URL here]

---

## Features

- ✅ Track all major consoles (Nintendo, PlayStation, Xbox)
- 🟢 Mark consoles as owned
- 🔧 Mark consoles as needing repair
- 💜 Live eBay market prices (automatically updated daily)
- 💰 Automatic total collection value
- 🔍 Search & filter
- 🔒 Admin login to edit your collection

---

## Requirements

All of the following are **completely free**:

- [GitHub](https://github.com) account
- [Firebase](https://firebase.google.com) account
- [eBay Developer](https://developer.ebay.com) account
- [Google](https://google.com) account (for Apps Script)

---

## Step 1 – Fork the Repository

1. Click **Fork** (top right of this page)
2. In your forked repo go to **Settings → Pages**
3. Source: **Deploy from a branch** → Branch: `main` → Folder: `/ (root)`
4. Save — your site is now live at `https://YOURNAME.github.io/REPONAME`

---

## Step 2 – Set Up Firebase

### 2a. Create a project
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add project"** → give it a name → Create
3. In the project: **Realtime Database → Create** → Region: `europe-west1` → **Start in test mode**

### 2b. Set database rules
Under **Realtime Database → Rules** replace everything with:

```json
{
  "rules": {
    "counts":       { ".read": true, ".write": "auth != null" },
    "repair":       { ".read": true, ".write": "auth != null" },
    "marketPrices": { ".read": true, ".write": true }
  }
}
```

Click **"Publish"**.

### 2c. Create an admin user
1. In the Firebase project: **Authentication → Get started**
2. Enable **Email/Password**
3. Under **Users → Add user** → enter your email + password

### 2d. Copy your Firebase config
1. Go to **Project Settings** (⚙️) → **General → Your apps → Add web app**
2. Register the app — you will receive a `firebaseConfig` object

---

## Step 3 – Update index.html

Open `index.html` on GitHub (click the ✏️ pencil icon) and replace the `firebaseConfig` block with your own values:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Click **"Commit changes"**.

---

## Step 4 – Set Up the eBay API

### 4a. Create a developer account
1. Go to [developer.ebay.com](https://developer.ebay.com) → register for free
2. Click **"Create Application"** → give your app a name
3. Under **My Account → Application Access** click **"Apply for exemption"**
   - Reason: *"Personal hobby project, no eBay user data is stored or processed"*
   - Usually approved within minutes
4. Copy your **Production Keys**: **App ID (Client ID)** and **Cert ID (Client Secret)**

> ⚠️ Make sure to use the **Production Keys**, not the Sandbox keys!

---

## Step 5 – Set Up Google Apps Script

### 5a. Create the script
1. Go to [script.google.com](https://script.google.com) → **"New project"**
2. Open `apps-script/updatePrices.gs` from this repository
3. Copy the entire content into the Apps Script editor (delete the existing code first)

### 5b. Enter your credentials
Replace the three placeholders at the very top of the script:

```javascript
const FIREBASE_URL       = "https://YOUR-PROJECT-default-rtdb.europe-west1.firebasedatabase.app";
const EBAY_CLIENT_ID     = "YOUR_PRODUCTION_CLIENT_ID";
const EBAY_CLIENT_SECRET = "YOUR_PRODUCTION_CLIENT_SECRET";
```

### 5c. Run a manual test
- Select `updatePrices` from the function dropdown → click **▶ Run**
- The log should show prices, e.g. `gbasp: 75`
- Check your Firebase Console — you should see a new `marketPrices/` node with values

### 5d. Set up the automatic schedule
1. In the left sidebar click **⏰ Triggers**
2. Click **"+ Add Trigger"**
3. Function: `updatePrices` → Time-based → **Day timer** → between 03:00–04:00
4. Save

Prices will now update automatically every day.

---

## Customizing Consoles

Want to add or remove consoles? Edit the `<li data-id="...">` entries in `index.html` and add the matching IDs to the `CONSOLES` object in the Apps Script file.

---

## Using the Admin Mode

To edit your collection:

1. Scroll to the bottom of the website
2. Enter the email and password you created in Firebase
3. Click **Login**
4. **+/−** buttons appear to adjust counts, and a **🔧** button to mark consoles for repair

---

## How It Works

```
index.html              → Complete website (HTML, CSS, JS in one file)
apps-script/
  updatePrices.gs       → Google Apps Script for daily eBay price fetching
```

| Service | Usage | Cost |
|---|---|---|
| GitHub Pages | Website hosting | Free |
| Firebase Realtime Database | Store collection & prices | Free |
| Firebase Authentication | Admin login | Free |
| eBay Browse API | Fetch market prices | Free |
| Google Apps Script | Daily price updates | Free |

**Total: $0/month** ✅

---

## License

MIT — do whatever you want with it! A ⭐ on the repo is always appreciated 😄
