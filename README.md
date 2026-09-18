# Kearah — setup guide

This version has real accounts, groups, live shared notes, and synced music,
backed by your Firebase project. It **will not run** by double-clicking the
file — it needs to be hosted online (see Step 4). It also won't work as a
Claude.ai artifact link, since Claude's published pages block outside network
calls, and this app needs to talk to Firebase.

## Step 1 — Turn on Email/Password sign-in
1. Go to https://console.firebase.google.com and open your **kearah** project.
2. Left sidebar → **Build → Authentication** → **Get started**.
3. Click **Email/Password** → toggle it **Enabled** → **Save**.

## Step 2 — Turn on Firestore (the database)
1. Left sidebar → **Build → Firestore Database** → **Create database**.
2. Choose **Start in production mode** → pick any location → **Enable**.
3. Once it's created, click the **Rules** tab.
4. Delete everything there and paste in the entire contents of `firestore.rules`
   (included in this folder) — but first, open that file and replace
   `PUT-YOUR-SIGNUP-EMAIL-HERE@example.com` with the **exact email address**
   you're going to sign up with in the app.
5. Click **Publish**.

## Step 3 — Set yourself as founder in the app file
1. Open `index.html` in a text editor.
2. Find this line near the top of the `<script type="module">` section:
   `const FOUNDER_EMAIL = "PUT-YOUR-SIGNUP-EMAIL-HERE@example.com";`
3. Replace it with the same email you used in Step 2.4. It must match exactly.

## Step 4 — Put it online (pick one)
This app must be served over `https://` — Firebase sign-in will not work on a
file opened directly from your computer.

**Netlify (fastest):**
1. Go to https://app.netlify.com/drop
2. Drag the whole `kearah-live` folder onto the page.
3. You'll get a live link like `https://random-name-123.netlify.app`.

**GitHub Pages (more permanent):**
1. Create a free account at https://github.com if you don't have one.
2. New repository → upload `index.html` and `firestore.rules` into it.
3. Settings → Pages → Branch: `main` → Save. Your link appears in ~1 minute.

## Step 5 — Authorize your new domain in Firebase
1. Back in the Firebase console → **Authentication → Settings → Authorized domains**.
2. Click **Add domain** and paste in your new Netlify or GitHub Pages domain
   (just the domain, e.g. `random-name-123.netlify.app`).
3. Without this step, sign-in will fail with a "domain not authorized" error.

## Step 6 — Try it
1. Open your live link.
2. Sign up with the email you set as founder.
3. Pick a nickname, then **Create a group** — you'll get an invite code.
4. Send that code to Kearah (or anyone else) — they sign up on the same link,
   enter the code, and you're both in the same live space.
5. Your account will show a **"Founder view"** link in the top-right that
   nobody else sees — it shows signups, groups, and ratings, not the content
   of anyone's private notes.

## What's shared live vs. what's simple
- **Notes** and **music playback** are live and shared instantly within a group.
- **Music now supports four kinds of links**, auto-detected when you paste one:
  - **YouTube** (watch link, youtu.be, or Shorts) — free, plays in full.
  - **SoundCloud** — free, plays in full for public tracks.
  - **Spotify** — paste a link to a single **track** (not an album or
    playlist). Whoever is listening needs to be logged into Spotify with a
    **Premium** account in that same browser tab, or only a short preview
    plays. The app tells you this with a small note whenever a Spotify link
    is added.
  - **Direct audio file link** — free, plays in full.
- **Multiple groups**: anyone can create or join more than one group (e.g.
  a couple's space and a separate family space). Use **"Switch group"** in
  the top bar to move between the groups you belong to.
- **Memory wall** photos and **countdown dates** are shared per group too.
- The founder dashboard shows **activity only** (who joined, when, group
  sizes, ratings) — never private note text or messages, by design.

## Account security
- **Forgot password** — the "Forgot password?" link on the login screen
  emails a reset link via Firebase (works automatically, no extra setup).
- **Change password** — while signed in, click **"Account"** in the top bar
  to change your password (it asks for your current one first).
- **Two-factor authentication (2FA) was not added.** Firebase's phone-based
  2FA requires upgrading your Firebase project to the pay-as-you-go
  ("Blaze") plan and attaching a billing card — it stays free at this scale,
  but Google requires the card on file, plus extra setup for phone
  verification. Since that's a real account/billing decision on your end
  rather than just code, I held off — tell me if you want to go ahead
  knowing that, and I'll build it in.

## About Spotify, Apple Music, Deezer, Tidal
- **Spotify**: added, with the Premium/login caveat above — Spotify's rules
  simply don't allow full-track playback otherwise.
- **Apple Music**: not added. It requires a paid Apple Developer account
  ($99/year) to get a token, on top of each listener needing an Apple Music
  subscription — a much bigger lift than the others.
- **Deezer / Tidal**: not added. Neither offers a simple, free, no-login
  embeddable player the way YouTube and SoundCloud do — full playback on
  either would need a similar OAuth + subscription setup to Spotify's.

## Good to know
- Photos are compressed and stored directly in the database, so very large
  images may take a moment to upload.
- The music player needs a **direct link to an audio file** (ending in
  something like `.mp3`), not a Spotify or YouTube link.
- This is a first version — video/calling between members isn't built yet,
  but the account and group structure here is what that would build on top of.
