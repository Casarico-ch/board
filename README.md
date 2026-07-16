# Casarico — Board Hub

A single-file board app for Immo-Ressource AG. It combines the light **board hub**
(analytics, meetings, monthly rapports, strategic initiatives) with the full
**board-pack** content from meeting #6 (check-in, financials + narrative, Q2 wins &
learnings, 6-month outlook, automation & AI, decisions → minutes export) and a
**Strategy on a page** reference view with the flywheel.

Everything lives in [`index.html`](index.html) — no build step, no dependencies to install.

---

## Two ways to run it

| | **Local mode** (default) | **Shared mode** (recommended for the board) |
|---|---|---|
| Setup | none | ~5 min one-time Firebase setup |
| Who can edit | whoever opens the file | only you (`daniel@immo-ressource.ch`) |
| Colleagues | send them a copy / a `?view` link | invite as **viewers** — they sign in and see your live board, read-only |
| Data lives | in that browser | in the cloud, shared and live for everyone |

You can start in local mode today and switch on shared mode whenever you're ready — it's
the same file.

---

## Local mode (works immediately)

Open `index.html` in a browser and use it. Your changes save automatically to that
browser. Use **Export data (.json)** in the sidebar to back up or move to another
machine, and **Import data** to restore.

To hand someone a **read-only** copy, add `?view` to the URL (e.g.
`index.html?view=1`) — all editing is hidden and disabled.

> Local mode stores data per-browser, so it can't share one live board between people.
> For that, use shared mode.

---

## Shared mode — invite colleagues as viewers, you as the only editor

This uses **Firebase** (free) so there's one shared board: you edit, everyone else sees
it live and read-only. Permissions are enforced on the server, so a viewer can't edit
even by tampering with the page.

### 1. Create the Firebase project
1. Go to <https://console.firebase.google.com> → **Add project** (any name).
2. **Build → Authentication → Get started → Sign-in method → Google → Enable.**
3. **Build → Firestore Database → Create database** (Production mode, any region).

### 2. Paste your config into `index.html`
In Firebase: **Project settings (gear) → General → Your apps → Web app (`</>`)**. Copy
the `firebaseConfig` values, then edit the CONFIG block near the top of `index.html`:

```js
window.EDITOR_EMAILS = ['daniel@immo-ressource.ch'];   // who may edit
window.FIREBASE_CONFIG = {
  apiKey: '…',
  authDomain: '….firebaseapp.com',
  projectId: '…',
  appId: '…'
};
```

(As long as `apiKey` still contains `PASTE`, the app stays in local mode.)

### 3. Publish the security rules
In Firebase: **Firestore Database → Rules**, paste the contents of
[`firestore.rules`](firestore.rules), and **Publish**. Keep the editor email(s) in that
file in sync with `EDITOR_EMAILS`.

### 4. Host it
Any static host works. **GitHub Pages** is easiest:
- Repo **Settings → Pages → Build from a branch →** pick this branch, folder `/ (root)`.
- After it deploys you get a URL like `https://casarico-ch.github.io/board/`.
- In Firebase **Authentication → Settings → Authorized domains**, add that domain
  (and `localhost` for testing).

### 5. Invite your colleagues
Send them the URL. They click **Sign in with Google**:
- **You** (`daniel@immo-ressource.ch`) → full editing.
- **Everyone else** → viewer (read-only), seeing your changes live.

To add another editor later, add their email to **both** `EDITOR_EMAILS` in `index.html`
and the list in `firestore.rules`, then re-publish the rules.

---

## What's inside

- **Analytics** — KPIs, financial chart and open tasks computed live from meetings & rapports.
- **Board meetings** — each meeting is a full board pack: check-in, agenda (Inform/Discuss/Decide), financials + "reading the numbers", KPI snapshot, wins, fails & learnings (linked to initiatives), 6-month outlook, automation & AI, decisions, actions/to-dos, and **Export minutes (.txt)** / **Print pack**.
- **Monthly rapports** — light monthly entries: figures, highlights, lowlights.
- **Strategic initiatives** — the spine; every meeting and rapport links here and each initiative collects its history and tasks.
- **Strategy on a page** — thesis, flywheel, one promise per persona, how the pieces connect, risks & mitigations, decision rule.

Seeded with the real meeting #6 (21.07.2026, Zurich) content so it's populated on first open.
