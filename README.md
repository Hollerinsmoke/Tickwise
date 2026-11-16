# Tickwise — GitHub Pages Edition

**No Python server required.** This version is entirely HTML, CSS and JavaScript. It keeps the dark mode, six timer backgrounds, whole-number time display, flexible visible units, and editable Travel / School / CIE checklist templates.

## Publish with GitHub Pages (no coding needed)

1. Download and unzip `tickwise-github-pages.zip` on your computer.
2. Log in to [github.com](https://github.com) and click **New repository**. Name it `tickwise` (or anything you like) and create it as **Public**.
3. Open the new repository. Choose **Add file → Upload files**.
4. **Upload the CONTENTS of the extracted `tickwise-github-pages` folder**, *not* the ZIP and not a second folder. You should see `index.html`, `public.html`, `firebase-config.js`, `static/`, etc. at the repository root.
5. Click **Commit changes**.
6. In your repository, go to **Settings → Pages**. Under **Build and deployment**, select **Deploy from a branch**, choose **main** and **/(root)**, then click **Save**.
7. GitHub will display your public link. It usually looks like `https://YOUR-USERNAME.github.io/tickwise/`.

**IMPORTANT:** `index.html` must be directly at the root of the published source. If you see a 404, confirm this and check the GitHub Pages deployment status in the repository's **Actions** tab.

The site uses **relative paths** so it works at both `username.github.io/tickwise/` and a custom domain.

## How checklists work with just GitHub Pages (no database)

- Create a checklist or start with Travel, School or CIE.
- Click **Save & get share link**. Your checklist is stored **in your browser** (localStorage). Its public share link contains a **copy of the checklist** and can be opened by other people on any device.
- People tick their boxes and click **Create completed response link**. They must **send the returned link to you** (WhatsApp, message, email, etc.).
- **Open that return link on the same browser where you created the checklist.** The response will be imported into your results dashboard.
- Editing your original after sharing does not change older links (the links contain snapshots). Make and send a new link after edits.
- Your checklist creator/editor link only opens on the browser where you saved the checklist. Clearing browser data loses local checklists and locally imported responses.
- Because the checklist data is embedded in the URL, long checklists result in longer links. Some messaging platforms may shorten very long URLs.

This link-based mode is usable **immediately** after GitHub Pages deployment with **no extra accounts or configuration**. There is no automatic submission inbox in this mode, because GitHub Pages cannot store visitor data itself.

## OPTIONAL: Make it automatically collect submissions (Google Forms–style)

To get stable public links, automatic response submissions, and cloud storage, you can connect **Firebase Firestore**. You do NOT need a separate Python server. The repository includes a secure starter rules file.

1. Go to [Firebase Console](https://console.firebase.google.com/). Create a project.
2. Under the project, register a **Web app**. Firebase will show a `firebaseConfig` object containing `apiKey`, `authDomain`, `projectId`, and `appId`.
3. In Firebase Console, open **Build → Authentication → Sign-in method**, and enable **Anonymous** sign-in. In **Authentication → Settings → Authorized domains**, add `YOUR-USERNAME.github.io` if required (without `https://`).
4. Open **Build → Firestore Database → Create database**, choose an appropriate region and start in **production mode**.
5. Open Firestore's **Rules** tab. Copy the contents of this project's `firestore.rules` file into the Firebase rules editor and click **Publish**. **Do not use “allow read, write: if true”.**
6. On GitHub, edit `firebase-config.js` in your repo and fill in the four values from your Firebase web app config. Commit the change. GitHub Pages will redeploy.
7. Open Tickwise and make a **new** checklist. It will now be saved to Firestore. Every recipient's **Submit** button saves answers to the cloud, and **View submissions** retrieves them.

**Security and ownership:** Firebase anonymous login identifies the owner **on that browser**. For this simple project you cannot transfer ownership to a new browser just by copying an editor link. If you clear browser authentication/storage, you may lose owner access. Back up your important forms and consider adding Google sign-in before production use. Anonymous submissions can be spammed; for a public/high-traffic launch add abuse controls, App Check, and monitoring. Firestore usage may incur charges above free quotas.

Firebase app configuration is not treated as a secret. **Firestore Security Rules are essential**; they control who can edit lists or read responses. Anyone with a public checklist URL can view its questions and submit a response.

## Features

- 2 tabs: **Countdown** and **Checklists**.
- Flexible timers: duration or target date, optional time units (years, months, days, hours, minutes, seconds).
- Unselected time units are converted to the remaining selected units; whole numbers are always rounded **down** (e.g. 20.787 hours → 20).
- Six timer backgrounds: Nebula, Midnight, Aurora, Ocean, Sunset and Rose.
- Full site dark mode.
- Custom checklist sections; optional required items.
- Ready-made Travel, School and CIE Exam templates.
- Local share-and-return workflow; **optional** automatic Firebase cloud responses.

## File layout

```text
index.html                The main website
public.html               The recipient/checklist page
static/style.css          Dark mode styling
static/app.js             Countdown, builder, share links, local inbox
static/public.js          Checklist answering and returned links
static/cloud.js           Optional Firebase connection
firebase-config.js        Empty by default (site works without Firebase)
firestore.rules           Optional Firebase security rules
.nojekyll                 Makes GitHub Pages serve files as-is
README.md                 Deployment guide
```

## Testing on your own computer

With Python installed (for preview only; NOT needed on GitHub), open a terminal inside the folder and run:

```bash
python -m http.server 8000
```

Then visit `http://localhost:8000/`.

**Do not open `index.html` directly using `file://`**: JavaScript modules need an HTTP server or GitHub Pages.

## More info

Official guide: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
