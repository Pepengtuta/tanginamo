# Utangina mo (Utang List App) 📓

A lightweight, mobile-first notebook tracker designed for sari-sari store owners to track **utang** (credit purchases) simply and quickly. Works as a standalone single-file web app or as an Android APK wrapped via Capacitor.

---

## 🌟 Key Features

- **Store-Speed Entry**: Enter `[ ₱ Price ] [ × Qty ] [ Name ] [ Items ]` and hit Enter.
- **Quantity Multiplier**: Entering e.g. ₱23, Qty: 3 for `coke` splits into 3 individual records so customers can make partial payments per item later.
- **Notebook Aesthetic**: Visual notebook-line styling with red margin line and timestamps captured at first keystroke.
- **Customer & Item Management**: 
  - Auto-suggests known customer names and item prices.
  - **Cascading Name Edit**: Renaming a customer updates their entire utang history so totals remain unified.
  - Price edits update future suggestions without modifying historical records.
- **Offline-First**: All data is stored locally in `localStorage` (`utangApp.v1`). Zero external databases or servers required.
- **Backup & Restore**:
  - Export backup as a `.json` file (uses native Android Share Sheet via `@capacitor/share` and `@capacitor/filesystem`).
  - Import / Restore backup file directly from your device file manager.
- **Mobile-Optimized & Capacitor Ready**:
  - Offcanvas drawer for settings, customer management, item price cheat-sheet, and backups.
  - Locked to portrait orientation.
  - Soft-keyboard friendly with auto-scrolling focused inputs.
  - Safe-area insets supported for notched screens.

---

## 🛠️ Tech Stack

- **Frontend**: HTML5, Vanilla JavaScript (zero frameworks).
- **Styling**: Bootstrap 5.3 (bundled locally, no CDN required).
- **Mobile Packaging**: Capacitor v8 (Android).

---

## 📂 Project Structure

```
debt-chain/
├── bootstrap/               # Local Bootstrap 5 CSS & JS bundle
├── www/                     # Web assets root for Capacitor
│   ├── index.html           # Main web application
│   └── bootstrap/           # Bundled Bootstrap assets
├── android/                 # Native Android project (Capacitor)
├── utang.html               # Standalone single-file version
├── mvp.md                   # MVP specification and design notes
├── Utangina-mo-debug.apk     # Pre-built debug APK ready to install
└── README.md
```

---

## 🚀 Getting Started

### Running in the Browser
Simply open `utang.html` or `www/index.html` in any modern web browser. No server or build step needed.

### Building the Android APK with Capacitor

1. Install dependencies:
   ```bash
   npm install
   ```

2. Sync web assets with native Android:
   ```bash
   npx cap sync
   ```

3. Build the Debug APK using Gradle:
   ```bash
   cd android
   .\gradlew assembleDebug
   ```
   The built APK will be located at:
   `android/app/build/outputs/apk/debug/app-debug.apk`

4. Or open in Android Studio:
   ```bash
   npx cap open android
   ```

---

## 📄 License
MIT License
