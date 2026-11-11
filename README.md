# Comfort Board

A live, shared champion comfort tier list for the whole team.

- **Anyone with the link** can view it.
- **The team passcode** lets people add players and move champions.
- **The owner passcode** also unlocks owner settings: tier letters and subtitles, clearing history, and resetting boards.

- `index.html` is the whole app. It runs on GitHub Pages.
- `firestore.rules` holds the security rules that enforce both passcodes. Paste them into Firebase.

## Setup

### 1. Create the Firebase project
1. Go to https://console.firebase.google.com and click **Create a project**. Analytics isn't needed.
2. On the project overview, click the **Web** icon (`</>`) to add a web app. Give it any nickname and leave Firebase Hosting unchecked.
3. Copy the `firebaseConfig` values it shows. Paste them into `FIREBASE_CONFIG` near the top of the script in `index.html`, replacing the `PASTE_...` placeholders.

### 2. Turn on anonymous sign-in
1. Go to **Build → Authentication → Get started**.
2. On **Sign-in method**, enable **Anonymous** and save.

### 3. Create the database and lock it down
1. Go to **Build → Firestore Database → Create database**.
2. Pick a location (`us-central1` is closest to Kansas City), then choose **production mode**.
3. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.

### 4. Set the passcodes
1. In Firestore's **Data** tab, click **Start collection**.
2. Set the collection ID to `config` and the document ID to `team`.
3. Add two string fields, then save:
   - `passcode`: the team passcode you'll share with teammates.
   - `ownerPasscode`: a different passcode that only you know.

   Use something longer than a few characters for both.

### 5. Put it on GitHub Pages
1. Create a new **public** repository on GitHub, e.g. `comfort-board`.
2. Upload `index.html`, `firestore.rules` and this README to the main branch.
3. In the repo, go to **Settings → Pages**. Under Source choose **Deploy from a branch**, then branch `main` and folder `/ (root)`. Save.
4. After a minute the site is live at `https://<your-username>.github.io/comfort-board/`.

### 6. Allow your GitHub domain in Firebase
1. Go to **Authentication → Settings → Authorized domains → Add domain**.
2. Enter `<your-username>.github.io`. Without this step, sign-in fails on the live site.

### 7. Use it
1. Open the site and click **Owner**. Enter your name and the owner passcode. This also lets you edit.
2. Add your players. Rename the tiers under **Owner settings** if you like.
3. Send teammates the link plus the passcode. Each person unlocks once per device.

## Day-to-day
- **Change a passcode:** edit `passcode` or `ownerPasscode` in `config/team` in the Firestore console. Every device using the old one is locked out until it enters the new one.
- **Owner settings** (header button, owner devices only):
  - **Tier labels:** rename each tier's letter (up to 4 characters) and subtitle. Champions stay in the same rows.
  - **Reset boards:** empty one player's tiers or everyone's, with an option to delete their history too. Players stay on the roster.
  - **History:** clear one player's log or everything.
- **Lock a device:** use **Lock** in the header, for example on a shared PC.
- **Back up:** use **Backup** in the header to download every player and the full history as JSON.
- **Viewing without the passcode:** anyone with the link can view. To hide the board from people without the passcode, change the two `allow read: if signedIn();` lines in the rules to `allow read: if isEditor();`.
- **Is the config in a public repo safe?** Yes. Firebase web keys are meant to be public; the security rules are what protect the data.
