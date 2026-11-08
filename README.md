# 📥 IFFCO Egypt · Incoming PM — PWA

Incoming Selection → Ready-to-send Approval Email.
Offline-capable PWA (installable on Android / iPhone) with an **encrypted cloud register**
on our own Firebase — on its **own path**, separate from the Soap Bar Daily Log document.

---

## What it does

| Section | Purpose |
|---|---|
| 1 · Incoming Selection | Tick the materials received today. Wrapper / Stiffener get GSM tables. |
| 2 · GSM Tables | One calculator per pallet: width × repeat × weight → GSM, with LSL/USL check. |
| 3 · Generator | Type a GSM value, get tables of 6 samples around it (±0.1 … ±1). |
| Email | Builds the approval email (HTML for Teams + plain-text fallback) and copies it. |
| Incoming Log | One row per material: date, shift, qty, truck, trader = Manufacturer, decision. |
| Excel Log | The register row (Date · shift · RM Name · Quantity · UNIT · 6 samples · AVERG) for paste into Excel. |
| ☁️ Cloud Register | The Incoming Log rows are stored in our encrypted cloud so every phone sees the same register. |

## Install on the phone

Tap the **⬇️ Install app** button in the top bar (it appears as soon as the app is installable).

**Android (Chrome)** — the button opens the native install dialog → *Install*.
**iPhone (Safari)** — the button shows the steps: Share ⬆️ → *Add to Home Screen* → *Add*.
**Desktop Chrome** — the button explains it, or use the install icon in the address bar.

After the first open the app works **offline** (the service worker caches the shell).
The cloud buttons need a network.

## The cloud

- Same database as the other plant apps:
  `https://soap-lab-shift-sync-default-rtdb.europe-west1.firebasedatabase.app`
- **Its own document path** — the Soap Bar Daily Log is never touched:
  `lab/incoming-pm-865b658190c198039dc1e173.json`
- Encryption: **AES-GCM 256 + PBKDF2 120k**, PIN `2780` (same scheme as the other apps).
- The `salt` is created once and never changes, so every phone can decrypt the document.
- **Everything is stored there**: the draft (ticks, quantities, truck, trader, pallet weights,
  generator, added items) *and* the register rows.
- Register rows are keyed `date | shift | material` — on a merge the **newest row wins**.
- The draft is taken from the cloud only when the cloud copy is the newer one; a phone with
  unsaved work of its own keeps it (and `⟳ Sync both` pushes it instead of overwriting it).
- **🗑 Clear all data** (top of the page) wipes the draft, the register, the added items —
  on the phone **and** in the cloud — so you can start fresh on every phone.

## Repo layout

```
index.html              the whole app (HTML + CSS + JS) + the cloud module
manifest.webmanifest    PWA manifest (name, icons, standalone display)
sw.js                   service worker — offline shell, never caches the cloud
icons/                  192 / 512 / maskable / apple-touch icons
.nojekyll               keep GitHub Pages from processing the files
```

## Changing the cloud path

`index.html` → search for `CLOUD_PATH` and put the new path there, then re-push.
Keep the PIN and the encryption format — the other apps read their own documents the same way.
