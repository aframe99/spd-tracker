# SPD Incident Tracker - Long Term Project

## Project Overview
Building a complete incident tracking and officer management system for Shrewsbury Police Department. Split into two applications:
1. **IC Dashboard** (Web) - Command center for incident commanders
2. **Officer Check-In App** (Web) - Mobile app for officers in the field

## Current Status (v2)
- ✅ Core architecture complete (Firebase backend)
- ✅ IC Dashboard with map, unit tracking, messaging
- ✅ Officer Check-In App with test mode and simulated GPS
- ✅ Real-time data synchronization
- ✅ In-app messaging system (broadcast)
- ✅ Multiple test officers (4 pre-configured)
- ✅ Persistent GPS tracking with wake lock

## Files
- `index.html` - IC Dashboard (main, for GitHub Pages)
- `officer-checkin-app.html` - Officer Check-In App
- `PROJECT.md` - This file

## Deployed URLs
- Dashboard: https://aframe99.github.io/spd-tracker/
- Officer App: https://aframe99.github.io/spd-tracker/officer-checkin-app.html

## Phase 1: Core Features (✅ COMPLETED)
- [x] Firebase Realtime Database setup
- [x] IC Dashboard with map
- [x] Officer check-in flow
- [x] Real-time unit tracking
- [x] Test mode with 4 simulated officers
- [x] Broadcast messaging
- [x] GitHub Pages deployment ready
- [x] Split into two separate applications

## Phase 2: Testing & Refinement (CURRENT)
- [ ] Test on real iPhone with simulated GPS
- [ ] Verify Firebase data flow
- [ ] Test messaging between IC and officers
- [ ] Optimize mobile UI
- [ ] Battery/performance testing
- [ ] Test with multiple simultaneous officers
- [ ] Verify map updates in real-time

## Phase 3: Advanced Features (PLANNED)
- [ ] Individual officer messaging (not just broadcast)
- [ ] Message read receipts
- [ ] Incident closure workflow
- [ ] Report generation/export
- [ ] Officer statistics (time on scene, duration, etc)
- [ ] Admin panel for user management
- [ ] Photo/document capture in officer app
- [ ] Offline mode support
- [ ] Command post / Staging area management
- [ ] Barricade placement (already in old version)
- [ ] Path tracking / breadcrumb trail

## Phase 4: Native Mobile Apps (FUTURE)
- [ ] iOS native app (React Native or Swift)
  - True persistent background GPS (screen off)
  - Better battery optimization
  - Push notifications
  - Home screen icon
- [ ] Android native app
  - Same features as iOS
  - Native Material Design
- [ ] Shared Firebase backend with web apps

## Known Limitations
- **Safari on iPhone**: GPS stops when screen locks (browser limitation)
- **Web app only**: No true background GPS without native app
- **Test mode**: Simulated GPS for development (no real location data)
- **Messaging**: Currently broadcast only (all officers get same message)

## Deferred Items
These were consciously set aside and should be revisited at the appropriate phase:

- **Firebase Authentication (Phase 3)** — Currently the `/ics` node is publicly readable because the app uses manual password checks instead of Firebase Auth. This is an acceptable tradeoff during development but should be addressed before any real-world deployment. Firebase Auth is free and would allow proper security rules tied to logged-in users. Deferred because it requires reworking the login flow, admin panel, and security rules — best done when the app is otherwise stable after Phase 2 testing.

## Next Steps to Try
1. Download both HTML files
2. Upload to GitHub repo `/spd-tracker`
3. Test on iPhone:
   - Open dashboard at main URL
   - Create test incident with PIN `1234`
   - Open officer app in Safari
   - Select test officer
   - Watch GPS updates on dashboard map
   - Send/receive messages

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

## Test Credentials & Data
- **IC Login**: `admin` / `1234`
- **Default Incident PIN**: `1234`

### Test Officers (Pre-configured)
| Badge | Name | Service | Type |
|-------|------|---------|------|
| 101 | John Smith | Police | Patrol |
| 102 | Sarah Johnson | Fire | N/A |
| 103 | Mike Davis | Police | Supervisor |
| 104 | Emma Wilson | EMS | N/A |

### Test GPS Locations (around Shrewsbury, MA)
- Officer 1: 42.3601, -71.0589 (Town Center)
- Officer 2: 42.3620, -71.0610 (North)
- Officer 3: 42.3580, -71.0570 (South)
- Officer 4: 42.3590, -71.0600 (East)

## Technical Notes
- Single HTML files for easy deployment (no build process)
- Firebase Realtime Database for all data
- Leaflet.js for mapping
- Mobile-first responsive design
- Dark theme optimized for field use
- Test mode updates GPS every 10 seconds
- Real GPS uses enableHighAccuracy with 5s timeout
- Screen wake lock requested to prevent sleep

## Architecture
```
Officer App (officer-checkin-app.html)
  ├── PIN Entry
  ├── Officer Info Form
  ├── Test Mode Selector
  ├── Session Management
  ├── GPS Tracking (Real or Simulated)
  └── Message Receiver

IC Dashboard (index.html)
  ├── Login
  ├── Incident Management
  ├── Map Display with Unit Markers
  ├── Unit List with GPS Status
  ├── Message Broadcast System
  └── Incident Details Panel

Firebase Database
  └── incidents/
      ├── [incidentId]/
      │   ├── info
      │   ├── units/
      │   │   └── [badge]/
      │   │       ├── gps
      │   │       ├── lastUpdatedAt
      │   │       └── status
      │   └── messages/
      │       └── [messageId]/
      │           ├── from
      │           ├── text
      │           └── timestamp
```

## Development Workflow
1. Make changes to HTML files
2. Test locally in browser
3. Test on iPhone (officer app in Safari)
4. Commit to GitHub
5. Verify deployment to Pages

## Future Enhancements
- Real-time path tracking for each officer
- Heatmap of high-activity areas
- Incident timeline with officer movements
- Geofencing alerts
- Offline-first with sync
- Voice messages
- Photo attachments
- Integration with dispatch system
- Mobile app (native iOS/Android)

## Support/Questions
All code is self-contained in HTML files. Firebase handles backend.
No external dependencies beyond CDN-loaded libraries (Leaflet, Firebase SDK).
