# ⚡ VibeLink — Real-Time Campus Social Network

> Stop the digital rot. Meet real people, in real life, on your campus.

VibeLink is a hyper-local, real-time social platform built for college campuses. Students broadcast time-limited "Vibes" — like a coffee run, gym session, or study break — and match with nearby peers within a 500m radius. Every connection is verified in the real world through GPS, presence tracking, and a trust-based reputation system.

---

## 🎯 Core Concept

1. **Spawn a Vibe** — Post what you're doing (Study, Coffee, Gym, Walk) with a timer (15–60 min)
2. **Peer Joins** — A nearby student swipes to join your vibe
3. **The Handshake** — Both users confirm physical arrival; the session is verified IRL
4. **Trust Earned** — Completed meetups earn trust points; ghosting costs them

---

## 🧠 Features

### Real-Time Matching & Vibes
- Time-limited vibe signals that auto-expire
- Geohash-based 9-block neighbor discovery (boundary-safe)
- Swipe-to-join slider with 95% drag threshold
- Category filters: Walk, Study, Coffee, Gym, Other
- Unique 4-character secure key per vibe

### The Handshake (Presence Verification)
- GPS-verified arrival confirmation for both users
- Synchronized mission timer that pauses until both are present
- Session completes only when both users confirm

### Trust & Reputation System
- +1 trust point per completed meetup
- -2 trust points for ghosting (reported no-show)
- Trust badges for verified users (5+ points)
- Negative trust scores block feed access

### In-Room Communication
- Real-time chat with read receipts (✓✓)
- Burning photos — self-destructing images (5s timer)
- One-click location sharing via Google Maps
- Dual-camera capture (front/back)
- Live typing indicators

### Safety & Privacy
- 🚨 Emergency SOS — broadcasts to all users within 500m with vibration alerts
- Block/unblock users
- Ghost Mode (Incognito) — hide from the radar
- Firestore security rules with role-based access
- Compressed & grayscale ID storage for minimal data exposure

### AI-Powered ID Verification
- Tesseract.js OCR scans student ID cards
- Keyword detection: `university`, `student`, `identity`, `valid`, `card`, `college`
- Images compressed to 400px @ 30% JPEG quality (Spark Plan friendly)
- Admin approval queue before campus access is granted

### Interactive Heatmap
- Leaflet.js dark-themed map with heat layer visualization
- Individual vibe node markers with activity-type icons
- Animated radar sweep overlay
- User pulse marker with GPS jitter for privacy
- Click a marker → scroll to the vibe card

### Admin Command Center
- UID-gated admin panel (`/admin-control`)
- Live stats: active vibes, total users, pending approvals
- Visual ID verification queue with approve button
- Real-time Firestore listener for pending users

### Landing Page
- GSAP scroll-triggered video mask hero animation
- Swipeable feature card stack (Framer Motion)
- Animated SVG tech-line about section
- Smooth scrolling via Lenis

### Notifications
- Firebase Cloud Messaging (FCM) with service worker
- Real-time in-app notification panel
- Auto-purge on panel close
- Types: radar (nearby), match (peer joined), safety (SOS)

### Personalization
- 7 accent color themes (neon green, gold, blue, red, pink, orange, purple)
- Guided onboarding tour (React Joyride)
- Mobile-optimized responsive UI with FAB controls

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, React Router v7 |
| UI | React Bootstrap, Lucide Icons, Custom CSS |
| Animations | GSAP + ScrollTrigger, Framer Motion, Lenis |
| Backend | Firebase (Auth, Firestore, Cloud Messaging) |
| Maps | Leaflet.js, leaflet.heat, ngeohash |
| OCR | Tesseract.js |
| Notifications | Firebase Cloud Messaging + Service Worker |

---

## ⚙️ Setup

### Prerequisites
- Node.js v18+
- A Firebase project

### 1. Clone & Install

```bash
git clone https://github.com/sanyaaroraaa/vibe-connect-app.git
cd vibe-connect-app
npm install
```

### 2. Firebase Console Setup

Go to [Firebase Console](https://console.firebase.google.com/) and:

- **Authentication** → Get Started → Enable **Email/Password** (and optionally **Google**)
- **Firestore Database** → Create Database → Start in **test mode**
- **Cloud Messaging** → Note your Sender ID
- **Project Settings** → General → Add a **Web App** → Copy the config

### 3. Environment Variables

Create a `.env` file in the project root:

```env
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_UID=your_admin_uid
```

> `VITE_FIREBASE_UID` is the UID of the admin user (found in Firebase Console → Authentication → Users). This gates access to the Admin Panel.

### 4. Messaging Service Worker

```bash
cp public/firebase-messaging-fw.js.example public/firebase-messaging-fw.js
```

Edit `public/firebase-messaging-fw.js` and replace the `PLACEHOLDER_*` values with your Firebase config.

### 5. Firestore Security Rules

Go to Firebase Console → Firestore → **Rules** tab → paste the contents of `firestore.rules` → **Publish**.

> Don't forget to replace `'Your uid daal dena'` with your actual admin UID in the rules.

### 6. Run

```bash
npm run dev
```

Open `http://localhost:5173` 🚀

---

## 📁 Project Structure

```
src/
├── components/        # Reusable UI (Heatmap, Chat, Settings, Modals, Tour)
├── config/            # Firebase initialization
├── context/           # Auth & Theme providers
├── hooks/             # Custom hooks (location, feed, active vibe)
├── pages/             # Route pages (Home, Login, Landing, Admin, ActiveMeetup)
├── section/           # Landing page sections (Hero, Features, About, Footer)
├── services/          # Business logic (auth, vibes, push notifications)
└── utils/             # Helpers (Haversine distance calculation)
```

---

## 🚀 Deployment

```bash
npm run build
firebase deploy
```

Make sure `firebase.json` is configured for hosting (points to `dist/`) and Firestore rules.

---

## 📄 License

All rights reserved. This project is for portfolio and demonstration purposes.
