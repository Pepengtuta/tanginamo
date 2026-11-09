# always use the ponytail

# Utang List: MVP

Context file for any agent or developer picking up this project. Read this first.

## What this is

A tiny app for **one sari-sari store owner** to record **utang** (goods and food taken on credit) and see who owes how much. It is for the owner's own store only. It is not a public product, so there are no accounts, no server and no multi-user features.

The owner currently tracks utang by memory or notebook. The app should feel like the notebook: type a line, see a running total.

## Who is building it

- A student and solo developer who also owns the store.
- Preference: **simple, direct, plain-language code. School-project level. No over-engineering.**
- Do not add frameworks, build tools or libraries unless asked.

## Decisions already made

| Topic | Decision | Why |
|---|---|---|
| Format | One plain HTML file (HTML, CSS and JS together) | Smallest possible scope |
| Flutter, Xamarin, MAUI, Expo | Rejected | Too big for this project |
| Storage | Browser `localStorage` | No server needed |
| Packaging | Wrap the HTML with **Capacitor** into an Android APK later | Owner has Android Studio; no rewrite needed |
| Research | Existing apps cover utang tracking (Lista PH, Peddlr, SukliPH, others). A custom app is justified only because the owner wants a very simple notebook-style flow. | Checked before building |

## The core idea (entry format)

Each line is typed as **price, then name, then items**:

```
23 claude coke
23 gemini sprite

46 total
```

The total is added automatically below the list.

## Features in the MVP (all built in `utang.html`)

1. **Add entry with quantity multiplier**: fields: price, qty (defaults to 1), name, items. If qty is greater than 1 (e.g. price 23, qty 3, gemini, coke), it records 3 separate entries (`23 gemini coke`) sharing the locked timestamp. This allows partial payments per item later. Press Add or Enter.
2. **Locked timestamp**: the date and time is captured at the **first keystroke** and kept, even if saving happens a minute later. If the form is emptied, the next typing starts a new time. Format: `11/9/2026 8:46 AM`.
3. **Notebook-style list**: one line per entry, with its time, and Paid and Delete buttons.
4. **Totals**: a total per person, plus `N total` at the bottom. Only **unpaid** entries count.
5. **Filter**: "Show" dropdown to view one customer or everyone.
6. **Customer list**: names are suggested while typing. A new name is added automatically on save. Customers can be renamed with **cascade update**, which updates all their historical records and running totals automatically. Matching ignores upper and lower case.
7. **Item list with prices**: if the typed item matches the list and the price is empty, the price fills in. Unit prices can be edited for future entries without altering past recorded utang.
8. **Paid**: marks an entry paid (crossed out, removed from totals). Can be undone.
9. **Backup**: Download backup (JSON file) and Load backup (replaces the current list after confirmation).

## Data model

Saved in `localStorage` under the key `utangApp.v1`:

```json
{
  "customers": ["claude", "gemini"],
  "items": [{ "name": "coke", "price": 23 }],
  "entries": [
    { "id": "string", "price": 23, "name": "claude", "items": "coke", "time": 1762649160000, "paid": false }
  ]
}
```

- `time` is milliseconds since 1970 (the locked first-keystroke time).
- **Keep this shape and the key backward-compatible.** The owner's real data lives here. If the shape must change, bump the key (for example `utangApp.v2`) and migrate the old data.

## Rules for agents working on this

- Keep it **one file**, plain JavaScript, no CDN or external requests.
- Keep the code readable for a student. Prefer simple loops and plain functions.
- Do not add features that are not listed here without asking.
- Never delete or reset stored data without an explicit confirmation step in the UI.

## Goal now: make the HTML "ready to wrap"

The next step is to get the page ready for Capacitor. Checklist:

- [x] Fix **Download backup** for the Android WebView. Detects `window.Capacitor.isNativePlatform()` at runtime; uses `Filesystem.writeFile` (CACHE dir, no extra permissions) + `Share.share` on-device, falls back to the blob link in a plain browser.
- [x] Check the layout on a phone-sized screen — Bootstrap 5 (local) applied with mobile-first grid, `form-control` tap targets, and `safe-area-inset-bottom` padding for notched phones.
- [x] Add an app icon and name (named "Utangina mo", package `com.mystore.utang`).
- [x] Make sure there is a `<head>` tag (Capacitor requires it; the file has one).
- [ ] Test saving, reloading and the Paid, Delete and filter buttons on the real phone.

## How to wrap it (Capacitor v8, from the official docs)

Requirements: Node.js 22 or higher, Android Studio 2025.2.1 or newer, and an Android SDK with API 24 or higher (installed from Tools > SDK Manager). The JDK comes with Android Studio.

```
mkdir utang-app && cd utang-app
mkdir www            # put utang.html here, renamed to index.html
npm init -y
npm i @capacitor/core
npm i -D @capacitor/cli
npx cap init         # app name, package ID (e.g. com.mystore.utang), web dir: www
npm i @capacitor/android
npx cap add android
npx cap sync
npx cap open android
```

After editing the page, run `npx cap sync` again, then run the app from Android Studio.
To enforce portrait mode in the Android APK, open `android/app/src/main/AndroidManifest.xml` and ensure the main `<activity>` includes:
```xml
android:screenOrientation="portrait"
```
Building an APK in Android Studio: Build > Build Bundle(s) / APK(s) > Build APK(s).

## Known limits (accepted for the MVP)

- Data is stored on **one device only**. Clearing app or browser data, or uninstalling the app, deletes the list. The Backup button is the only protection, so back up often.
- Entries cannot be edited, only marked paid or deleted. Delete cannot be undone.
- Partial payments are not supported (an entry is either paid or unpaid).

## Ideas for later (not MVP, do not build unless asked)

- Edit an entry.
- Partial payments.
- Per-customer credit limit warning (for example when someone passes a set amount).
- Search by item or date.
- Reminder text for collecting.
- Sync between devices (this would need a server and is a big step).

## Files

- `utang.html`: the whole app (rename to `www/index.html` when wrapping).
- `mvp.md`: this file.