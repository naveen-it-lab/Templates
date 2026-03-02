<div align="center">

<img src="public/logo_icon.svg" alt="KokoMate Icon" width="100" height="100" />

<img src="public/logo_full.svg" alt="KokoMate Full Logo" width="260" />

**The exact installment & merchant fee calculator for Sri Lankan Koko shoppers.**

<p>
  <a href="#-features">Features</a> •
  <a href="#%EF%B8%8F-tech-stack">Tech Stack</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="https://github.com/NaveenAkalanka/KokoMate/raw/master/kokomate-android/android/app/release/KokoMate.apk">⬇ Download APK</a>
</p>

![Next.js](https://img.shields.io/badge/Next.js-16-%23000000?style=for-the-badge&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-19-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v4-%2338B2AC.svg?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Capacitor](https://img.shields.io/badge/Capacitor-Android-%2300C4FF?style=for-the-badge&logo=capacitor&logoColor=white)
![License](https://img.shields.io/badge/License-CC_BY_NC_SA_4.0-orange?style=for-the-badge)

<br />

<a href="https://www.buymeacoffee.com/naveenakalanka" target="_blank">
  <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Support the Project" style="height:60px;width:217px;" />
</a>

</div>

---

## 📱 Features

| Feature | Description |
|---|---|
| ⚡ Instant Calculations | Real-time installment & surcharge breakdown as you type |
| 🔢 Smart Input | `inputMode="decimal"` triggers native number pad without input lockup |
| 🎛️ Flexible Rates | Preset merchant rates (8%, 10%, 12%) plus custom rate input |
| 📅 3 or 6 Months | Toggle split periods with a single switch |
| 🖥️ Fits Any Screen | Responsive layout that never requires scrolling |
| 📴 Offline Ready | PWA + Capacitor service worker caching for zero connectivity use |
| 📳 Haptic Feedback | Native tactile feedback on every button and input |
| 🔙 Native Navigation | Android hardware back button & swipe gestures fully integrated |

---

## ⬇️ Download

Get the latest Android APK directly:

**[⬇ Download KokoMate APK](https://github.com/NaveenAkalanka/KokoMate/raw/master/kokomate-android/android/app/release/KokoMate.apk)**

> Requires Android 7.0+ · Enable "Install from Unknown Sources" in your device settings.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 16 (Static Export, Webpack mode) |
| UI | React 19 + shadcn/ui + Tailwind CSS v4 |
| Native Bridge | Capacitor 7 (Android WebView) |
| PWA | next-pwa + Workbox Service Workers |
| Haptics | @capacitor/haptics |
| Navigation | @capacitor/app (backButton listener) |
| Icons | lucide-react |

---

## 🚀 Getting Started

```bash
# 1. Clone
git clone https://github.com/NaveenAkalanka/KokoMate.git
cd KokoMate

# 2. Install
npm install

# 3. Run dev server
npm run dev
```

### Build Android APK

```bash
# Build static export
npm run build

# Sync to Android
cd kokomate-android
npx cap sync android

# Open in Android Studio or build directly
cd android
./gradlew assembleRelease
```

---

## 📐 How the Calculation Works

```
Principal        = Store Price − Cash Down Payment
Monthly Install  = ((Principal × Rate%) / Months) + (Principal / Months)
Total Surcharge  = Principal × Rate%
Total Payable    = Store Price + Total Surcharge
Pay Today        = Cash Down Payment + Monthly Installment (first payment)
```

---

## 📄 License

This project is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)** License.

- ✅ Share & Adapt freely with attribution
- ❌ No commercial use
- 🔄 Derivatives must use the same license

See [LICENSE](LICENSE) for the full text.

> *KokoMate is not affiliated with or endorsed by Koko (Zip Co). All calculations are approximations based on publicly known merchant surcharge rates.*

---

<div align="center">

Designed & Developed with ❤️ by [Naveen Akalanka](https://github.com/NaveenAkalanka)

[☕ Support the Project](https://www.buymeacoffee.com/naveenakalanka)

</div>
