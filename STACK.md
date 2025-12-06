# STACK.md
✅ CHEQUE Technical Stack  
Local-first, offline-capable, and fully functional in a standard web browser.

This document describes the complete system architecture for Cheque.dev, including the student app, teacher app, offline runtime, web runtime, local database, syncing, course packs, backend (optional), and development infrastructure.

---

# 1. Applications

## 1.1 Student App (Cheque Student)
Runs everywhere:
- Offline-first native apps (with optional cloud sync)
- Works as an installable PWA on Chromebooks (required for many schools)
- Full in-browser web version (online only)

### Technologies
- React + TypeScript  
- Next.js App Router  
- Capacitor (Native + PWA builds)  
- Tailwind CSS  
- i18next  
- Blockly (block-based coding)  
- Monaco Editor (JS/Python)  
- Pyodide (Python execution via Web Worker)  
- JavaScript sandbox (Web Worker)  
- SQLite  
  - Native SQLite (mobile/desktop/PWA)  
  - SQLite WASM (web browser)  
- Drizzle ORM  
- Service Worker + Workbox (PWA)  
- Zustand or Jotai (UI state)  

### Capabilities
- Runs fully offline (native/PWA)
- Runs fully online with feature parity (browser)
- Local lesson engine
- Local code execution (JS, Python)
- Local progress tracking
- LAN/USB sync (native/PWA)
- Cloud sync (browser/native) via optional Convex layer

---

## 1.2 Teacher App (Cheque Teacher)
Acts as the "local server" in offline schools.

### Technologies
- React + TypeScript  
- Next.js + Capacitor  
- Tailwind CSS  
- i18next  
- SQLite + Drizzle ORM  
- WebRTC/WebSocket LAN sync server (native/PWA only)  
- Optional Convex sync adapter  

### Responsibilities
- Manage students and classes  
- Import and update course packs  
- Perform offline sync with student devices  
- Merge progress  
- Sync to cloud when online  
- Provide dashboards (local or cloud-connected)  

---

## 1.3 Admin / Authoring Dashboard
Runs online only.

### Technologies
- Next.js (web)  
- React + TypeScript  
- Tailwind CSS  
- Convex backend  
- Monaco Editor (authoring autograder code)  
- Course pack builder and publisher  

---

# 2. Shared Libraries (Monorepo Packages)

- packages/
- @cheque/ui
- @cheque/i18n
- @cheque/lesson-engine
- @cheque/runtime-js
- @cheque/runtime-python
- @cheque/local-db
- @cheque/web-db
- @cheque/sync
- @cheque/blocks
- @cheque/content-schema
- @cheque/auth-local
- @cheque/web-course-pack-loader
- @cheque/web-projects  

## 2.1 @cheque/ui
Shared components, layout primitives, and styling.

## 2.2 @cheque/i18n
Shared localization setup using i18next.

## 2.3 @cheque/blocks
Custom Blockly blocks and JS/Python generators.
Supports custom block categories for generating HTML and CSS for 
student web projects. HTML/CSS is compiled into project files that 
can be previewed locally and published online.

## 2.4 @cheque/lesson-engine
Loads, evaluates, and navigates lessons. Works offline.

## 2.5 @cheque/runtime-js
Web Worker sandbox for JavaScript with:
- timeout protection  
- no global network access  
- autograder support  

## 2.6 @cheque/runtime-python
Pyodide runtime:
- Worker-based  
- restricted environment  
- custom autograder integration  

## 2.7 @cheque/local-db
SQLite (native) wrapper using Drizzle ORM.

## 2.8 @cheque/web-db
SQLite WASM + OPFS or IndexedDB handling for the browser.

## 2.9 @cheque/sync
Implements:
- student → teacher offline sync  
- teacher → cloud sync  
- timestamp-based conflict resolution  
- sync pack creation and merging  

## 2.10 @cheque/content-schema
Validates `.cheque` course packs against strict schemas.

## 2.11 @cheque/auth-local
PIN-based local authentication system for offline schools.

## 2.12 @cheque/web-course-pack-loader
Allows `.cheque` course packs to be loaded directly in the browser.

## 2.13 @cheque/web-projects
Handles HTML/CSS/JS student projects, Blockly → code generation, 
browser previews, syncing, and publishing artifacts.

Responsibilities:
- Download pack from Convex/CDN
- Decompress (JSZip/fflate)
- Store JSON into SQLite WASM
- Store assets into OPFS/IndexedDB as Blobs
- Provide asset URLs via createObjectURL

This enables a full in-browser Cheque version.

---

# 3. Local-First Architecture

## 3.1 Local Database Options

### Native / PWA
- SQLite via Capacitor SQLite plugin
- Direct filesystem access for storing course packs

### Browser (Full Web Version)
- SQLite WASM
- Storage via OPFS (preferred) or IndexedDB
- Assets stored as Blobs  
- No direct filesystem access  
- Course packs loaded through @cheque/web-course-pack-loader  

All database queries share the same Drizzle ORM schema.

---

## 3.2 Execution Engine

### Python
- Pyodide loaded in a worker  
- Cached via service worker (PWA)  
- Loaded on demand for browser version  

### JavaScript
- Worker sandbox  
- No access to global network APIs  
- Execution timeout traps  

---

## 3.3 Lesson Engine
Shared across all platforms:
- Loads lessons from SQLite  
- Tracks student attempts  
- Validates answers locally  
- Operates fully offline  

---

## 3.4 Web Runtime Considerations (Full Browser Version)

The web version supports all features with these differences:

- No LAN/WebRTC server mode (browser security restriction)  
- Sync only through Convex (teacher dashboards, progress reporting)  
- Course packs must be loaded through the Web Course Pack Loader  
- SQLite is WASM-based and stored in OPFS  

Everything else works identically to native/PWA versions.

---

# 4. Sync Architecture

Cheque supports three modes:

## 4.1 Fully Offline Schools
- Student → Teacher sync only  
- LAN/WebRTC/PWA-native sync  
- No cloud involvement  

## 4.2 Occasionally Connected Schools
- Teacher syncs with Convex when possible  
- Offline-first locally, cloud-optimal when online  

## 4.3 Fully Online (Web Browser)
- Browser communicates only with Convex  
- Sync occurs via Convex queries/mutations  
- SQLite WASM acts as optional cache  

### Note
The browser cannot act as a WebRTC/WebSocket server, so LAN sync does not exist online.

---

# 5. Backend (Optional Cloud Layer)

Convex acts as the optional online backend.

## Responsibilities
- Online teacher/admin accounts  
- Hosting course pack metadata and downloads  
- Assignments  
- Student progress aggregation  
- Real-time dashboards  
- Version distribution
- Student project storage for public webpages (HTML/CSS/JS)
- Management of public student handles (e.g., /spencer-smith)
- Providing project data to Next.js for rendering public webpages

Convex is not required for offline-only deployments.

---

# 6. Content Pipeline (.cheque Course Packs)

## 6.1 Pack Format

- course-name-vX.cheque
- manifest.json
- lessons.json
- exercises.json
- assets/
- ...
- blocks/custom_blocks.json
- runtime/python_tests.py
- runtime/js_tests.js


## 6.2 Native/PWA Loading
- File imported via filesystem  
- Unpacked locally  
- Stored in SQLite + device storage  

## 6.3 Web Loading
Handled entirely by @cheque/web-course-pack-loader:

- Download via HTTPS  
- Decompress in memory  
- Insert JSON into SQLite WASM  
- Save assets to OPFS/IndexedDB  
- Provide URLs to runtime  
- No installation required  

---

# 7. Platform Feature Parity

| Feature | Native App | PWA | Web Browser |
|--------|------------|-----|--------------|
| Blockly | Yes | Yes | Yes |
| Monaco Editor | Yes | Yes | Yes |
| Python Execution | Yes | Yes | Yes |
| JS Sandbox | Yes | Yes | Yes |
| SQLite | Native | Native | WASM |
| Course Pack Import | Yes | Yes | Yes (via loader) |
| Offline Support | Yes | Yes | Limited (requires PWA install) |
| LAN Sync | Yes | Yes | No |
| Cloud Sync | Optional | Optional | Yes |
| Teacher Dashboard | Local or Cloud | Local or Cloud | Cloud |

---

# 8. Deployment Models

## 8.1 Browser Version (Online Only)
- Full feature set  
- No installation required  
- Cloud sync only  

## 8.2 PWA Version (Critical for Chromebook Support)
The PWA exists primarily to support schools that use Chromebooks.

ChromeOS does not allow installing native apps, but PWAs:
- can be installed as standalone applications
- work offline using service workers
- store data in SQLite WASM + OPFS
- support the full Cheque experience in a browser-controlled environment

Limitations:
- No LAN sync (browsers cannot run native Capacitor plugins)
- No native SQLite engine (uses SQLite WASM instead)
- No access to native filesystem or network APIs

The PWA is ideal for Chromebook-heavy deployments, while native apps provide the full offline + LAN sync experience. 

## 8.3 Native App Version
- Full offline-first  
- Largest storage support  
- LAN sync  
- Course packs import via filesystem  

---

# 9. Summary

Cheque runs in:
- Full browser environments  
- PWAs  
- Native apps  
- Offline schools  
- Online schools  
- Mixed environments  

Core components:
- React + Next.js  
- Blockly + Monaco  
- Pyodide + JS Sandbox  
- SQLite (native or WASM)  
- Web Course Pack Loader  
- Local lesson engine  
- Offline or cloud sync  
- Optional Convex online backend  

# 10. Student Webpages (Publishing System)

Cheque supports optional public-facing student webpages such as:

  username.cheque.dev

These pages are generated from projects created inside the Cheque environment.

## Project Flow
1. Student builds project using custom HTML/CSS blocks and/or Monaco.
2. Project files are stored locally in SQLite.
3. If online, project files sync to Convex.
4. Next.js renders webpages dynamically under the route /[studentHandle].

## Rendering
- Dynamic SSR via Next.js
- Optional static generation for performance
- Asset hosting via Convex file storage

## Offline Behavior
- Students may edit projects offline.
- Publishing requires an online session.

This architecture ensures Cheque works everywhere:  
from offline village schools to fully connected web deployments.

