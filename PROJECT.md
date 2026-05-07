# SPD Incident Tracker - Long Term Project

## Project Overview
Building a complete incident tracking and officer management system for Shrewsbury Police Department. Everything lives in a single `index.html` file (IC dashboard) and a React Native iOS app (officer side).

- **IC Dashboard** — Command center for incident commanders (login required) — `index.html` on GitHub Pages
- **Admin Panel** — User account management (admin login required)
- **Officer App** — React Native/Expo iOS app, installed via EAS build / TestFlight
- **Dispatcher View** — Read-only map link (planned — see Feature Backlog)

---

## Current Status (v20250507)

### IC Dashboard (index.html) ✅
- Core architecture complete (Firebase Realtime Database backend)
- IC Dashboard with map, unit tracking, messaging
- Admin panel for IC and officer account management
- Officer check-in flow (PIN-based, QR code accessible) — web fallback
- Real-time GPS tracking with map display
- Multiple map layers (Street, Satellite, Topo, Dark) with opacity control
- Command Post, Staging Area, and Barricade placement (draggable markers)
- Broadcast messaging (IC to all units) with unread message badge on MSG tab
- Message log (newest first, 50 message limit)
- Path/breadcrumb tracking per unit (toggleable)
- Active and closed incident selectors
- PDF export of incident report (units, messages)
- Document Center (Docs tab) — IC adds name+URL links, stored in Firebase
- Auto-clears all panels and markers when incident is closed or deselected
- Firebase listeners properly cleaned up on incident switch/close

### Officer App (React Native/Expo) ✅
- Standalone iOS app (no local server needed)
- PIN-based check-in via QR code deep link
- Real-time GPS tracking (foreground + background)
- Two-way messaging (receive IC broadcast, send unit messages)
- Push notifications for IC messages (local notification on message receipt)
- Session persistence (app remembers logged-in officer across restarts)
- Officer self-registration with admin oversight
- Document Center — officers see IC-added links, tap to open in browser
- Map view with all units, CP, staging, barricades
- Map quick-zoom toolbar: 📍 Me | 🚨 Incident | 🏛️ CP | 🅿️ Stage | 👥 Units
- Unit list slide-up modal — tap any unit to zoom map to their location
- Screen wake lock
- Auto-checkout from other incidents on check-in (prevents appearing on multiple incidents)

---

## Files
- `index.html` — IC dashboard, admin panel (GitHub Pages)
- `PROJECT.md` — This file
- `C:\Users\acameron\SPDOfficerApp2\` — React Native officer app
  - `screens/SessionScreen.js` — Active session, GPS, messaging, notifications, docs
  - `screens/MapScreen.js` — Live map with quick-zoom toolbar and unit list
  - `screens/CheckinScreen.js` — Check-in form with auto-logout from other incidents
  - `screens/PinScreen.js` — QR/incident entry, PIN validation
  - `screens/LoginScreen.js` — IC and officer login
  - `screens/RegisterScreen.js` — Officer self-registration
  - `screens/IncidentSelectScreen.js` — Active incident list
  - `App.js` — Navigation, session persistence, push notification setup

---

## Deployed URLs
- IC Dashboard / Login: https://aframe99.github.io/spd-tracker/
- Officer Check-In (QR): https://aframe99.github.io/spd-tracker/?incident=[INCIDENT_ID]
- Dispatcher View (planned): https://aframe99.github.io/spd-tracker/?incident=[INCIDENT_ID]&view=dispatch

---

## Build & Deploy

### IC Dashboard
1. Edit `index.html` locally
2. Test in browser
3. Commit and push to GitHub → auto-deploys via GitHub Pages

### Officer App
```
cd C:\Users\acameron\SPDOfficerApp2
set EAS_NO_VCS=1
eas build --platform ios --profile preview
```
Build takes ~10-15 minutes. EAS provides download link for .ipa install.

### Dev Server (debugging only)
```
cd C:\Users\acameron\SPDOfficerApp2
npx expo start
```

---

## Phase 1: Core Features ✅ COMPLETED
- Firebase Realtime Database setup
- IC Dashboard with map
- Officer check-in flow with PIN validation
- Real-time unit tracking on map
- Broadcast messaging
- GitHub Pages deployment
- Single-file architecture (no build process)

## Phase 2: Testing & Refinement ✅ SUBSTANTIALLY COMPLETE
- ✅ Tested on real iPhone (real GPS)
- ✅ Firebase data flow verified end-to-end
- ✅ Messaging tested between IC and officers
- ✅ Push notifications working (local notification model)
- ✅ Two-way messaging confirmed working
- ✅ Map marker updates confirmed real-time
- ✅ QR code check-in flow tested on mobile
- ✅ Multiple simultaneous officers tested
- ✅ Document Center tested
- ✅ Map quick-zoom tested
- [ ] PDF export — verify output format
- [ ] Battery/performance testing with screen wake lock over extended session
- [ ] Path tracking with real movement

## Phase 3: Advanced Features (PLANNED — see Feature Backlog)

## Phase 4: Distribution
- [ ] TestFlight setup for coworker beta testing
- [ ] Android build
- [ ] App Store submission (future)

---

## Feature Backlog

### 🔜 Next Up
**TestFlight Distribution**
- Get app to coworkers for beta testing with minimal friction
- Testers just install TestFlight app + tap invite link — no dev tools needed
- Requires Apple Developer account ($99/yr) — Team ID 2M3WN4YMGP already exists
- Build with production profile, submit via `eas submit --platform ios`

### 📋 Queued Features

**Dispatcher View (Read-Only Map Link)**
- Web link, no app needed — designed for laptops/PCs at dispatch
- URL: `https://aframe99.github.io/spd-tracker/?incident=[ID]&view=dispatch`
- PIN-required to access (same 4-digit PIN as incident)
- Shows: unit markers, incident location, CP, staging, barricades, live GPS
- Side panel: unit list (name, badge, service, status)
- Does NOT allow: messaging, editing, marker placement, any data changes
- Auto-refreshes as units move in real time

**Individual Officer Messaging**
- IC can message a specific unit instead of broadcast only
- Unit sees it flagged differently (e.g. "DIRECT MESSAGE")

**Message Read Receipts**
- Small checkmark or "seen" indicator when officer has read a message

**Officer Statistics**
- Time on scene per officer
- Check-in/check-out log per incident
- Exportable with PDF report

**Photo Messaging**
- Camera icon in message compose area
- Photos upload to Firebase Storage
- Display inline in message thread
- Requires Firebase Storage setup

**Geofencing Alerts**
- IC sets a perimeter on the map
- Alert fires if a unit leaves the perimeter

**Offline Mode**
- Basic functionality when cell signal is lost
- Sync when connection restored

### 🔭 Future / Phase 4
- Android native app
- App Store / full public release
- Integration with dispatch CAD system
- Heatmap of unit activity over incident duration
- Firebase Authentication (replace manual password checks)

---

## Known Limitations
- **Safari on iPhone**: GPS may pause when screen locks (mitigated by wake lock + background task)
- **Messaging**: Currently broadcast only — all officers get the same message
- **Security**: `/ics` Firebase node is publicly readable (acceptable for dev, fix before production)
- **Push notifications**: Using local notification model (app must be installed and have had session) — not true remote push

## Deferred Items
- **Firebase Authentication** — Currently using manual password checks. Should be addressed before real-world deployment. Deferred until app is stable post-Phase 2.

---

## Test Credentials
- **IC Login**: `admin` / `1234`
- **Incident PIN**: Set at incident creation (any 4 digits)

---

## Firebase Configuration
```
apiKey: AIzaSyC-56rq9DEw2U0XJ3708ruje0604zAphXE
authDomain: shrewsbury-pd-track.firebaseapp.com
databaseURL: https://shrewsbury-pd-track-default-rtdb.firebaseio.com
projectId: shrewsbury-pd-track
storageBucket: shrewsbury-pd-track.firebasestorage.app
messagingSenderId: 1833033387326
appId: 1:1833033387326:web:09b8659a1004aa42bbc6ef
```

## Expo / Apple Info
```
Expo account: aframe99@gmail.com
Apple Team ID: 2M3WN4YMGP
EAS Project ID: f4ee1939-38fa-4cdf-98b3-e8aaacd0597f
Bundle ID: com.shrewsburypd.officerapp
```

---

## Firebase Security Rules (Current)
```json
{
  "rules": {
    "incidents": {
      ".read": true,
      "$incidentId": {
        ".write": true,
        "units": { "$unitId": { ".read": true, ".write": true } },
        "messages": { ".read": true, ".write": true },
        "markers": { ".read": true, ".write": true },
        "pathTracking": { ".read": true, ".write": true }
      }
    },
    "ics": {
      ".read": true,
      ".write": false,
      "$username": {
        ".read": true,
        ".write": false,
        "savedCheckinData": { ".write": true }
      }
    },
    "officers": {
      ".read": true,
      ".write": false,
      "$username": { ".read": true, ".write": true }
    },
    "admins": { ".read": false, ".write": false }
  }
}
```

---

## Architecture
```
index.html (IC dashboard + admin panel)
  ├── Home Screen (IC login / Admin login)
  ├── Admin Dashboard
  │   ├── IC Account Management
  │   └── Officer Account Management
  └── IC Dashboard
      ├── Incident selector (active + closed)
      ├── New incident creation (name, type, address, PIN)
      ├── Map (Leaflet — Street/Satellite/Topo/Dark, opacity control)
      ├── Map markers (units, CP, staging, barricades — draggable)
      ├── Unit list with real-time GPS status
      └── Right panel tabs:
          ├── Info (incident details, OIC, CP, staging, QR code, close/export)
          ├── Unit (selected unit details + force checkout)
          ├── Messages (broadcast send + log, unread badge)
          ├── Tracking (per-unit path tracking toggle)
          └── Docs (add/remove document links)

Officer App (React Native)
  ├── PinScreen — QR deep link entry + PIN validation
  ├── LoginScreen — IC and officer login
  ├── RegisterScreen — Officer self-registration
  ├── IncidentSelectScreen — Active incident list
  ├── CheckinScreen — Check-in form (auto-clears other incidents)
  ├── SessionScreen — GPS, messaging, notifications, docs, checkout
  └── MapScreen — Live map, quick-zoom toolbar, unit list modal

Firebase Database
  └── incidents/[incidentId]/
      ├── info (name, type, location, pin, status, createdAt)
      ├── units/[badge]/ (gps, status, officer info, pushToken)
      ├── messages/[messageId]/ (from, text, timestamp, type)
      ├── markers/ (CP, staging, barricades)
      ├── pathTracking/[unitId]/ (breadcrumb trail)
      └── documents/[docId]/ (name, url, addedAt, addedBy)
  └── ics/[username]/ (IC accounts + savedCheckinData)
  └── officers/[username]/ (officer accounts)
  └── admins/ (reserved)
```
