[PROJECT.md](https://github.com/user-attachments/files/27279227/PROJECT.md)
# SPD Incident Tracker - Long Term Project

## Project Overview
Building a complete incident tracking and officer management system for Shrewsbury Police Department. Everything lives in a single `index.html` file that serves multiple roles depending on how it's accessed:
- **IC Dashboard** — Command center for incident commanders (login required)
- **Admin Panel** — User account management (admin login required)
- **Officer Check-In App** — Mobile check-in flow, accessed via QR code link with `?incident=` parameter

## Current Status (v20250427)
- ✅ Core architecture complete (Firebase Realtime Database backend)
- ✅ IC Dashboard with map, unit tracking, messaging
- ✅ Admin panel for IC and officer account management
- ✅ Officer check-in flow (PIN-based, QR code accessible)
- ✅ Real-time GPS tracking with map display
- ✅ Multiple map layers (Street, Satellite, Topo, Dark)
- ✅ Map opacity control
- ✅ Command Post, Staging Area, and Barricade placement on map
- ✅ Broadcast messaging (IC to all units)
- ✅ Message log
- ✅ Path/breadcrumb tracking per unit (toggleable)
- ✅ Active and closed incident selectors
- ✅ PDF export of incident report (units, messages)
- ✅ Officer map view (officer-side mini map)
- ✅ Department autocomplete suggestions on check-in form
- ✅ Existing session detection (officer can resume after page reload)
- ✅ Screen wake lock (keeps phone on during active session)
- ✅ Deployed to GitHub Pages

## Files
- `index.html` — Everything (IC dashboard, admin panel, officer check-in)
- `PROJECT.md` — This file

## Deployed URLs
- Main app / IC Login: https://aframe99.github.io/spd-tracker/
- Officer Check-In (QR): https://aframe99.github.io/spd-tracker/?incident=[INCIDENT_ID]

## Phase 1: Core Features (✅ COMPLETED)
- [x] Firebase Realtime Database setup
- [x] IC Dashboard with map
- [x] Officer check-in flow with PIN validation
- [x] Real-time unit tracking on map
- [x] Broadcast messaging
- [x] GitHub Pages deployment
- [x] Single-file architecture (no build process)

## Phase 2: Testing & Refinement (CURRENT)
- [ ] Test on real iPhone (real GPS, not simulated)
- [ ] Verify Firebase data flow end-to-end
- [ ] Test messaging between IC and officers
- [ ] Optimize mobile UI for field use
- [ ] Battery/performance testing with screen wake lock
- [ ] Test with multiple simultaneous officers
- [ ] Verify map marker updates in real-time
- [ ] Test QR code check-in flow on mobile
- [ ] Verify PDF export output is correct and readable
- [ ] Test path tracking with real movement

## Phase 3: Advanced Features (PLANNED)
- [ ] Firebase Authentication (see Deferred Items below)
- [ ] Individual officer messaging (not just broadcast)
- [ ] Message read receipts
- [ ] Officer statistics (time on scene, duration, etc.)
- [ ] Photo/document capture in officer app
- [ ] Offline mode support
- [ ] Geofencing alerts
- [ ] Heatmap of high-activity areas
- [ ] Integration with dispatch system

## Phase 4: Native Mobile Apps (FUTURE)
- [ ] iOS native app (React Native or Swift)
  - True persistent background GPS (screen off)
  - Better battery optimization
  - Push notifications
  - Home screen icon
- [ ] Android native app
- [ ] Shared Firebase backend with web apps

## Known Limitations
- **Safari on iPhone**: GPS may pause when screen locks (browser limitation, partially mitigated by wake lock)
- **Web app only**: No true background GPS without a native app
- **Messaging**: Currently broadcast only — all officers get the same message
- **Security**: `/ics` node is publicly readable in Firebase (see Deferred Items)

## Deferred Items
These were consciously set aside and should be revisited at the appropriate phase:

- **Firebase Authentication (Phase 3)** — Currently the `/ics` node is publicly readable because the app uses manual password checks instead of Firebase Auth. This is an acceptable tradeoff during development but should be addressed before any real-world deployment. Firebase Auth is free and would allow proper security rules tied to logged-in users. Deferred because it requires reworking the login flow, admin panel, and security rules — best done when the app is otherwise stable after Phase 2 testing.

## Test Credentials & Data
- **IC Login**: `admin` / `1234`
- **Default Incident PIN**: Set when creating incident (any 4 digits)

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

## Architecture
```
index.html (single file, role determined by URL/login)
  ├── Home Screen (login — IC or Admin)
  ├── Admin Dashboard
  │   ├── IC Account Management (create, delete)
  │   └── Officer Account Management (create, reset PW, activate/deactivate, delete)
  ├── IC Dashboard
  │   ├── Incident selector (active + closed)
  │   ├── New incident creation (name, type, address, PIN)
  │   ├── Map (Leaflet — Street/Satellite/Topo/Dark layers, opacity control)
  │   ├── Map markers (units, command post, staging area, barricades)
  │   ├── Unit list with real-time GPS status
  │   ├── Right panel tabs:
  │   │   ├── Info (incident details, OIC, CP, staging, QR code, close/export)
  │   │   ├── Unit (selected unit details)
  │   │   ├── Messages (broadcast send + log)
  │   │   └── Tracking (per-unit path tracking toggle)
  │   └── PDF export
  └── Officer Check-In (accessed via ?incident= URL)
      ├── PIN validation
      ├── Check-in form (name, badge, service, dept, optional: radio/cell/vehicle/notes)
      ├── Existing session detection + resume
      ├── GPS tracking (real device GPS)
      ├── Officer map view (mini map of own location)
      ├── Message receiver (broadcast from IC)
      └── Check out

Firebase Database
  └── incidents/[incidentId]/
      ├── info (name, type, location, pin, status, createdAt, etc.)
      ├── units/[badge]/ (gps, status, officer info, lastUpdatedAt)
      ├── messages/[messageId]/ (from, text, timestamp)
      ├── markers/ (commandPost, stagingArea, barricades)
      └── pathTracking/[unitId]/ (breadcrumb trail)
  └── ics/[username]/ (IC login accounts)
  └── officers/[username]/ (officer accounts)
  └── admins/ (reserved, currently unused)
```

## Development Workflow
1. Edit `index.html` locally
2. Test in browser (IC dashboard)
3. Test officer check-in by opening `index.html?incident=[id]`
4. Test on iPhone in Safari (real GPS)
5. Commit and push to GitHub
6. GitHub Pages auto-deploys

## Technical Notes
- Single HTML file — no build process, no dependencies to install
- Firebase Realtime Database for all live data
- Leaflet.js for mapping (multiple tile layer support)
- QR code generation via qrcodejs
- PDF export via html2pdf.js
- Mobile-first, dark theme optimized for field use
- Screen wake lock requested to prevent phone sleep during active session
- Department autocomplete via HTML datalist
- Officer session persisted in localStorage (survives page reload)
