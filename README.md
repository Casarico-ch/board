# Casarico — Board Hub

A single-file board app for Immo-Ressource AG. It combines the light **board hub**
(analytics, meetings, monthly rapports, strategic initiatives) with the full
**board-pack** content from meeting #6 (check-in, financials + narrative, Q2 wins &
learnings, 6-month outlook, automation & AI, decisions → minutes export) and a
**Strategy on a page** reference view with the flywheel.

Everything lives in [`index.html`](index.html) — no build step, no accounts, no
dependencies to install.

---

## How sharing works (no accounts, no Firebase)

There is one shared board stored as [`data.json`](data.json) in this repo.

- **You** open the site with `?edit`, make changes, and click **Publish** — that saves a
  fresh `data.json` you commit to the repo.
- **Colleagues** open the plain link and see that published board, **read-only**. No
  sign-in.

Editing is effectively limited to you, because only you can commit `data.json` to the
repo. Anyone opening `?edit` only edits a throwaway copy in their own browser — it can
never reach the published board.

---

## Publish it (one-time, ~3 minutes)

1. **Merge this pull request** so the files land on the `main` branch.
2. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a
   branch → Branch: `main` / `(root)` → Save.**
3. After a minute you get your board link, e.g. **`https://casarico-ch.github.io/board/`**.

That link already shows the seeded meeting #6 board. Send it to your colleagues — they
see it read-only.

---

## Your editing + publishing routine

1. Open **`https://casarico-ch.github.io/board/?edit`** (bookmark this one). The sidebar
   shows **Editing (draft)** and a **Publish snapshot** button.
2. Edit anything — every field is editable; changes are kept in your browser as a draft.
3. Click **Publish snapshot** → it downloads `data.json`.
4. Put that file in the repo to go live. Easiest, no tools:
   - On GitHub open the repo → click **`data.json`** → the **pencil (Edit)** →… or use
     **Add file → Upload files**, drop the downloaded `data.json`, **Commit changes**.
   - Within ~1 minute the live board updates for everyone.

That's the whole loop: **edit → Publish → commit `data.json`.**

> Tip: append `?view` to any link to force read-only (useful if you want a clean
> read-only link to your own `?edit` bookmark).

---

## Optional: live updates instead of publish-per-change (Firebase)

If you later want colleagues to see changes **instantly** (no publish step), the app also
supports a Firebase-backed shared mode with Google sign-in, where only your email can
edit. It needs a one-time free Firebase project. Steps:

1. <https://console.firebase.google.com> → **Add project**. Enable **Authentication →
   Google**, and create a **Firestore database**.
2. Copy your web-app config into the CONFIG block near the top of `index.html`
   (`window.FIREBASE_CONFIG`) and set `window.EDITOR_EMAILS = ['daniel@immo-ressource.ch']`.
3. Publish the rules in [`firestore.rules`](firestore.rules) (Firestore → Rules), and add
   your Pages domain under **Authentication → Settings → Authorized domains**.

As long as `apiKey` still contains `PASTE`, the app ignores Firebase and stays in the
simple snapshot mode above.

---

## What's inside

- **Analytics** — KPIs, financial chart and open tasks computed live from meetings & rapports.
- **Board meetings** — each meeting is a full board pack: check-in, agenda (Inform/Discuss/Decide), financials + "reading the numbers", KPI snapshot, wins, fails & learnings (linked to initiatives), 6-month outlook, automation & AI, decisions, actions/to-dos, and **Export minutes (.txt)** / **Print pack**.
- **Monthly rapports** — light monthly entries: figures, highlights, lowlights.
- **Strategic initiatives** — the spine; every meeting and rapport links here and each initiative collects its history and tasks.
- **Strategy on a page** — thesis, flywheel, one promise per persona, how the pieces connect, risks & mitigations, decision rule.

Seeded with the real meeting #6 (21.07.2026, Zurich) content so it's populated on first open.
